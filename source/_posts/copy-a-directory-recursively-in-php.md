---
extends: _layouts.post
section: content
title: 'Copy a directory recursively in php (using Standard PHP Library)'
author: "guttume"
date: 2019-10-20
---

Last time, we discussed [how to remove a non empty directory in php](https://guttu.me/blog/remove-a-non-empty-directory-in-php/) using [Standard PHP Library (SPL)](https://www.php.net/manual/en/book.spl.php). This time we will learn how to copy a directory recursively in php using SPL. By recursively, I mean we want to copy a directory which has directories inside it along with files and those directories can have more directories inside them.

This time we will use another SPL class: [`The DirectoryIterator class`](https://www.php.net/manual/en/class.directoryiterator.php). As per the documentation 
> The DirectoryIterator class provides a simple interface for viewing the contents of filesystem directories.

In simple words, it lets us iterate through the files in a given directory while providing methods to get several information about those file. We will use couple of those methods in our function but I do encourage you to look at the [documentation](https://www.php.net/manual/en/class.directoryiterator.php) for the full list of methods this class provides.

Before we get to the code, we are going to discuss, how we will approach our problem. In order to copy one directory to another with its contents, we need to copy files from our source directory to the target directory. Lets break it down into steps:

 * Iterate through each file in the source directory
 * Create a file in the target directory with the same name corresponding to each file in the source directory
 * Copy the contents of the source file and put it into the target file
 
 There you go. Three simple steps and our problem is solved. Not quite. Our steps work if there are only files inside the source directory, however, we may have several directories (containing more directories) inside the source directory. Well, the solution is very simple. While iterating through the files, we keep checking if the we got a directory and if it is a directory, we create a directory in the target directory and call our function recursively.
 
 Alright, its time for the code.
 
 ```php
function copyDir($source, $destination)
{
    if(! file_exists($destination) {
        mkdir($destination)
    }

    $directory = new DirectoryIterator($source);

    foreach ($directory as $file) {
        if ($file->isDot()) continue;
        $sourceFile = $source . '/' . $file;
        $targetFile = $destination . '/' . $file->getBasename();
        if ($file->isDir()) {
            mkdir($targetFile);
            copyDir($sourceFile, $targetFile);
        } else {
            $contents = file_get_contents($sourceFile);
            file_put_contents($targetFile, $contents);
        }
    }
}
```