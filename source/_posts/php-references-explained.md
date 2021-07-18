---
extends: _layouts.post
section: content
title: 'PHP References explained'
author: "guttume"
date: 2021-07-17
---

 

PHP references are another name to access a value which is already associated with another variable. They are nothing like pointers in C and you should not confuse it with them.

### **So what are references in PHP**

PHP references are symbol table aliases. If you have never heard about symbol table, don't worry. We are going to learn about them by understanding how variables  work in PHP. Let us consider the below code:

```php
<?php
    
    $var = "some value";
```

In the above code sample, we have created a variable and assigned a value to it. The type of the variable is string and the value is "some value". Let us now understand, what happens under the hood, when PHP engine executes the above code.

Whenever a new variable is created and a value is assigned to it, PHP engine creates a new 'zval' with the information about its value and its type. This zval is kept in the symbol table along with its reference count and the variable name mapped to it. PHP maintains a symbol table to store information about all the variables created in a program's lifetime. The reference count has the count of total variables pointing towards the value stored in this zval. Now, let us consider the below code:

```php
<?php
    
    $var = "some value";
    unset($var);
```

Once again we have created and assigned a new variable on the first line. We have then destroyed the same variable using `unset` function. We have already seen about what happens when a variable is assigned a value. Now, let us look at what happens when a variable is unset. Before we proceed further, let me ask you a question, why the function is named as unset and not delete when we are destroying the variable. Well, the answer is in the name of the function itself. When we unset a variable, the variable name is simply destroyed from the symbol table and the reference count is decremented. The value is not removed at that point of time. The value is removed from the memory when the reference count reaches zero and the garbage collector claims the memory.

Now, that we understand how variables work in PHP, let us now move on to references. We will again start by looking at a code.

```php
<?php
    
    $var = "some value";
    $anotherVar =& $var;
    unset($var);
    echo $anotherVar; // prints "some value"
```

On the first line, we have created a variable and assigned a value to it. In the next line, we have created a reference to the previous variable. A reference to a variable is created using the ampersand symbol. Once the reference is created, we now have two variables pointing to the same value. We have then unset the original variable. Next, we have printed the value of the reference we created earlier. We see that the value of the original variable is printed even though the original variable was destroyed using `unset`.

### **Where to use a PHP reference variable**

There are few uses to a PHP reference variable such as passing by reference, returning reference, creating global variable, passing object and `$this` keyword.

#### **Passing by reference**

While making a function call, sometimes, we want the function to change the variable that we are passing as a parameter to the function. However, as we know that the a function creates a copy of the variable that is provided as a parameter leaving the original variable untouched. Instead, while accepting a parameter in a function, if we create a reference to it and not a copy, we can make the function change the original variable. We can simply do it by simply placing an ampersand before the argument. Let us see the code for it.

```php
<?php
    
    function changeVar(&$var) {
        // make changes to $var here
    }
```

#### **Returning references from functions**

If we want to return a reference to a variable instead of returning a copy from a function, we can do that by placing an ampersand symbol in front of the function name. Example:

```php
<?php
    
    function &returnARef() {
        // return any variable here
    }
```

#### **Creating global variable**

If you have ever created a global variable in PHP, you have already used references.

```php
<?php
    
    global $var;
```

The above code is similar to the below code.

```php
<?php
    
    $var =& $GLOBAL['var'];
```

#### **Passing Objects**

Whenever we create a copy of an object or an object is passed as parameter to a function, PHP creates a reference rather than creating a copy of the object. This helps in saving memory as well as execution that would have been spent in creating a copy of the object. Though, we can create a true copy of an object by using clone keyword. There are inbuilt functions in PHP such as `sort`,`array_unshift`,  `array_shift` and others, which are already using references.

#### **$this** **keyword**

$this keyword is used to access an object's property and methods, which is again a reference to the object.

### **Destroying a reference**

Now that we know how to create a reference in PHP, we will see how to destroy a reference. A reference can be destroyed using unset function. However, this does not destroy the value associated with it.

```php
<?php
    
    $var = "some value";
    $anotherVar =& $var;
    unset($anotherVar);
    
```
