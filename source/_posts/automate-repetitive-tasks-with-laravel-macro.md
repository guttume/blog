---
extends: _layouts.post
section: content
title: "Automate repetitive tasks with Laravel Macro"
author: "guttume"
date: 2019-12-03
---

Laravel macros are a way to extend Laravel core classes, without writing our own class. In other words, it allows us to add new methods to the core Laravel classes. In this discussion, we will look at one of the several practical scenarios, where I use Laravel macro at my work. Then we will drill down into how it works internally.

At my work, most of the master tables we create in our application database have few common columns such as '**active**', '**created_by**', '**updated_by**', '**created_at**', '**updated_at**'. Laravel schema builder already provides us with one useful function to add '**created_at**' and '**updated_at**' columns with one single method, `$this->timestamps()`. However, we still need to add other columns and it is really frustrating to type the same column names over and over for different table schemas. So, we use Laravel macro to encapsulates all these common columns into one simple function. Lets see how we implement this.

First thing that we need to make sure that the class, we are trying to extend, `uses Macroable trait`. In our case, we are trying to extend the `Illuminate\Database\Schema\Blueprint` class and it uses Macroable trait, so we can use macro function in order to add our own little function to `Blueprint` class.

So we open `AppServiceProvider.php` class and add our own little macro function for the `Blueprint` class in its boot method.

```php
<?php

namespace App\Providers;

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
   /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Blueprint::macro('masters', function(){
            $this->boolean('active')->default(true);
            $this->string('created_by');
            $this->string('updated_by');
            $this->timestamps();
        });
    }

}
```

As you can see, macro is a static function which takes two arguments. The first argument expects a name and the second one is a callable function. In our the case, our custom method simply calls the Blueprint class's other methods, in order to create our desired columns for the database. Here I have added only one custom method, however, we can add as many custom methods to a class as we want. Lets see how we use this macro.

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreatePermissionsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('permissions', static function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('name')->unique();
            $table->string('description')->nullable();
            $table->masters();
        });
    }

```

While creating our migration file, we simply call our custom method (which was named **masters** in the macro definition) on the Blueprint object just like we use other inbuilt methods. Now, I can use this one method to define all of those common columns just using one line. How cool is that.

Now, lets have a look at that `Macroable` trait that Laravel is using to do this magic. Here, I have included the complete `Macroable` trait, with few non related parts omitted.

```php
<?php

namespace Illuminate\Support\Traits;

use Closure;
use ReflectionClass;
use ReflectionMethod;
use BadMethodCallException;

trait Macroable
{
    /**
     * The registered string macros.
     *
     * @var array
     */
    protected static $macros = [];

    /**
     * Register a custom macro.
     *
     * @param  string $name
     * @param  object|callable  $macro
     *
     * @return void
     */
    public static function macro($name, $macro)
    {
        static::$macros[$name] = $macro;
    }

    /** --- Other non related stuffs --- **/

    /**
     * Checks if macro is registered.
     *
     * @param  string  $name
     * @return bool
     */
    public static function hasMacro($name)
    {
        return isset(static::$macros[$name]);
    }

    /**
     * Dynamically handle calls to the class.
     *
     * @param  string  $method
     * @param  array  $parameters
     * @return mixed
     *
     * @throws \BadMethodCallException
     */
    public static function __callStatic($method, $parameters)
    {
        if (! static::hasMacro($method)) {
            throw new BadMethodCallException(sprintf(
                'Method %s::%s does not exist.', static::class, $method
            ));
        }

        $macro = static::$macros[$method];

        if ($macro instanceof Closure) {
            return call_user_func_array(Closure::bind($macro, null, static::class), $parameters);
        }

        return $macro(...$parameters);
    }

    /**
     * Dynamically handle calls to the class.
     *
     * @param  string  $method
     * @param  array  $parameters
     * @return mixed
     *
     * @throws \BadMethodCallException
     */
    public function __call($method, $parameters)
    {
        if (! static::hasMacro($method)) {
            throw new BadMethodCallException(sprintf(
                'Method %s::%s does not exist.', static::class, $method
            ));
        }

        $macro = static::$macros[$method];

        if ($macro instanceof Closure) {
            return call_user_func_array($macro->bindTo($this, static::class), $parameters);
        }

        return $macro(...$parameters);
    }
}

```

We can see that this trait implements a static macro method, which we used in order to register our own custom method. As you can see, this method stores all defined macro methods in an array (`$macros`). Now, when we try to use our custom function, the `__call()` method is invoked as our function was not defined in the class. `__call()` is a magic method which is used to overload methods in PHP. You can find more about it [here](https://www.php.net/manual/en/language.oop5.magic.php). In this method, Laravel is first checking if the function we are trying to call is defined in the `$macros` array, if not it throws an exception. If it finds our method, it again checks if it is an instance of `Closure` or not and executes our custom method accordingly with provided arguments.

Since, this trait also implement `__callStatic` method in the same way it does `__call()` method, we can call our own custom methods statically as well. Also, we can create and register as many custom methods to a Laravel macroable class as we want, since they are stored in an array.
