---
extends: _layouts.post
section: "content"
title: "PHP match expressions"
author: "guttume"
date: 2021-07-13
---

PHP introduced match expressions with the release of PHP 8.0.0. The PHP match expressions can be used as an alternative to switch statements most of the time. Later in this article, we will see how match expressions in PHP differs from PHP switches. First, let us see how a match expression in PHP looks like.

```php
<?php
   
    $x = 1;

    echo match ($x) {
      1 => "one",
      2 => "two",
      3 => "three",
      4 => "four",
      5 => "five",
      default => "something else",
    };

    // outputs: one
```

A match expression has a subject expression, result of which is compared to multiple conditions based on which the suitable expression is returned. The conditional expression and the corresponding result expression is separated by the _fat arrow_ symbol (`=>`).

There can be multiple conditioal expression with their corresponding result expression. Each combination is known as match arms and they are separated by comma. 

The final match arm must be a default pattern which will match anything that has not matched previously. Multiple default patterns will raise a E_FATAL_ERROR error.

The match expression must be terminated by semicolon. 

Though it is not necessary to use the result of a match expression, you will always use it.

Unlike a switch statement, the comparison of the value of subject expression and the conditional expression is an identity check (===) rather than a weak check (==), which means `1` will not be equal to `'1'`.

## Implementing logical OR in a match expression
If we want to compare a conditional expression with multiple values and wants to execute same piece of code for each of them, we can do that in a match expression by using multiple expressions separted by comma in a single match arm.

**Example**

```php
<?php

$integer = 1;

echo match ($integer) {
  1, 3, 5 => "Odd number",
  2, 4, 6 => "Even number",
  default => "Out of range"
};

// outputs: Odd number
```

## Handling non identity conditonal cases
It is possible to use a match expression to handle non-identity conditional cases by using true as the subject expression.

**Example**

```php
<?php
    
    $age = 42;

    echo match (true) {
        $age <= 13 => 'Kid',
        $age <= 18 => 'Teen',
        $age <= 30 => 'Adult',
        $age <= 45 => 'Senior'
        default => 'Old'
    };

    // outputs: Senior
```

## Differences between Match expression and Switch statement in PHP
* A PHP match is an expression which means it returns a value. Though it is not necessary to use its returned value, you will be using its returned value all the time. PHP switch statement is a block statement, just like an if statement.

* Comparison in a match expression is an identity check (===) while a switch statement does a weak comparison (==). For example, when comparing `2` with `'2'` in a switch statement will result in true, however, it will return false in a match expression.

* As it is an expression, a match statement must be terminated by semicolon.

* A switch statement in PHP can omit a default case, however, a match statement is required to have the default pattern. Hence, a match expression must be exhaustive.

* PHP switch statement will raise `E_COMPILE_ERROR` error when multiple default cases are provided, whereas, a match expression with multiple default patterns will raise an `E_FATAL_ERROR` error.