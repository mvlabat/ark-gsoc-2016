---
layout: post
title:  "Investigating archive operations"
date:   2016-08-07 20:23:00 +0300
categories: researches
---

In this post I'm going to tell you about some advanced using of terminal utilities for zip, 7z and rar archive types. It'll cover the following operations: add files to specific folder in archive, move or rename files and copy files within archive. There will be described both solutions with using only classic command line programs for mentioned archive formats and programmed workarounds for solving the problems, if those tools are inefficient.

## Adding files to specific directory within the archive
Unfortunately, there are no options and switches for command line tools for doing this. So we have to do it manually somehow...

The idea is to recreate destination folder structure and place symlinks of the added files into those directories, then we can add the first created directory to the archive so their contents will be merged.

So if we wanted to add, for instance, entries "some_dir/" and "some.txt" to "some/dir/inside/", bash solution would look like this:
{% highlight bash %}
cd $TMP_DIR
mkdir some
mkdir some/dir
mkdir some/dir/inside
ln -s $SRC_DIR/some_dir $TMP_DIR/some/dir/inside/some_dir
ln -s $SRC_DIR/some.txt $TMP_DIR/some/dir/inside/some.txt
# and here we can add "some" folder to the archive with our favorite tool
{% endhighlight %}
Btw, in Qt we could utilize `QDir::mkpath`, which could create "some/dir/inside" directories in one call.

## Rename/move entry

### 7z and rar
7z and rar command line tools allow us to move files with using only one command! It's very simple:
{% highlight bash %}
7za(/rar) rn <archive_name> <src_file_1> <dest_file_1> [ <src_file_2> <dest_file_2> ... ]
{% endhighlight %}
### Zip
Unfortunately, if we use zip tool, it won't be so easy... The solution is to extract wanted files, rename or move them, recreating a new directory structure if needed, delete old entries and add the new ones as we do it while adding entries to specific directory within the archive.

# Copy files within the archive
There is no support for it at all... Looks like nobody needs copying within the archive in real life. :D But if you're developing a universal archive manager like Ark, you might need it. :) So the idea is completely the same as we move files with zip command line tool, we only do not delete copied entries.

So, that's all... This post turned out to be not very long, but I hope it was useful or at least interesting to you.
