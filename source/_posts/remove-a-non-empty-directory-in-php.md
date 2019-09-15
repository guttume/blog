---
extends: _layouts.post
section: content
title: 'Remove a non empty directory in php'
author: "guttume"
date: 2019-09-15
---

There are lot of useful inbuilt functions that comes with PHP interpreter. One such function is `rmdir` which allows us to delete a folder/directory through php code. The documentation for the function can be found [here](https://www.php.net/manual/en/function.rmdir.php).

As useful this function is, it fails to delete a directory when the directory is not empty throwing a warning message `Directory not empty`. As a workaround, we need to first delete all files and folders recursively inside the specified directory and then delete the directory itself.

There are various ways to get all files and directories inside the specified directory. We can use `opendir()` along with `readdir()` for this purpose or just `glob()` function.

Here we will look at an alternative way, wherein we use an inbuilt utility that comes with **[SPL](https://www.php.net/manual/en/book.spl.php)**. SPL stands for Standard PHP Library which comes with loads of utilities such as `DataStructures`, inbuilt `Exception` classes, `Iterators` and others.

We will make use of two such `Iterator` classes from SPL, [`RecursiveDirectoryIterator`](https://www.php.net/manual/en/class.recursivedirectoryiterator.php) and [`RecursiveIteratorIterator`](https://www.php.net/manual/en/class.recursiveiteratoriterator.php). 

Lets look at the code first.

```php
function removeDir($target)
    {
        $directory = new RecursiveDirectoryIterator($target,  FilesystemIterator::SKIP_DOTS);
        $files = new RecursiveIteratorIterator($directory, RecursiveIteratorIterator::CHILD_FIRST);
        foreach ($files as $file) {
            if (is_dir($file)) {
                rmdir($file);
            } else {
                unlink($file);
            }
        }
        rmdir($target);
    }
```

Here, `removeDir` is a funtion which takes a path to the target directory. 

First we create a `Traversable` object using `RecursiveDirectoryIterator` which accepts one mandatory parameter, a path to a valid directory and one optional parameter, `flags`. Here, we have passed `FilesystemIterator::SKIP_DOTS` as a flag to skip the dot files.

The main component of this function is the `$files` object which is an instance of `RecursiveIteratorIterator` class. This class takes one mandatory parameter, an instance of `Traversable`, and two optional parameters, `mode` and `flags` . These optional parameters are used to change the default behaviour of the object. Here, we are passing `RecursiveIteratorIterator::CHILD_FIRST` as `mode` to get the children of the directory before getting the directory name itself.

Once we get the `$files` object, we simply traverse through each `$file` and delete them. Finally we delete the target directory.