---
layout: post
title:  "Hello! Let's start with CLion"
date:   2016-05-18 13:58:24 +0300
categories: setting up environment
---

Hello! My name is Vladyslav Batyrenko, I'm a student living in Kyiv, Ukraine.
This is my first GSoC blog post about my participating in Ark project.
In this post I would like to tell you how I managed to set up my environment
for coding with CLion, utilizing it not only as an editor but also as a
debugging tool. You may think that's a trivial task almost every IDE can handle - 
and this is true, but until your project requires a non-trivial deployment
process before it is able to start. So if you want to know how to build,
deploy and run/debug your executable with pressing only `(SHIFT+F10)` buttons,
you may find this post useful.

This was a thorny way with doing a lot of workarounds...

## Getting started
CLion is an IDE from JetBrains, it has a very rich set of features.
You can easily get it from its [official site][clion-official-site].
Here I'll use Ark project I'll be working on as an example for setting up CLion.

`git clone git://anongit.kde.org/ark`

Ark uses CMake for building and it's a fortune because so does CLion.
After opening the project in the IDE and trying to run it
`(Run -> Run)` or `(SHIFT+F10)` you must get "Unable to find Ark's KPart
component, please check your installation." error. That's it. We'll want
to run `make install` for built project and to run **installed** executable.
For accomplishing this task CLion provides us with `External tool` functionality.

After spending much time on researching, how exactly I can make CLion
run **installed** executable I came up with the following solution...
If someone has another ideas please feel free to share them, that's only
how I myself managed to workaround the situation.

## Setting up the project in CLion
`External tool` allows us to run a custom bash script before/after build step.

**Firstly**, I though, I needed to somehow make CMake communicate with
my bash script in order to get my script to know where the build directory is.
CLion generates random path (for our perception) for its CMake binary dir...
I had, for instance, `/home/mvlabat/.CLion2016.1/system/cmake/generated/ark-e86190eb/e86190eb/Debug`.

So I've ended up with adding to Ark target's CMakeList.txt file the following
lines:

{% highlight cmake %}
set(ARK_INSTALL /path/mvlabat/gsoc/ark_install) # Use your path where you want to keep your installed files.
add_custom_command(TARGET ark
    PRE_BUILD
    COMMAND echo "${CMAKE_BINARY_DIR}" > "${ARK_INSTALL}/cmake_binary_dir"
)
{% endhighlight %}

After each build command this will create the file called "cmake_binary_dir"
with a path to CMAKE_BINARY_DIR.

**Secondly**, I had to set the environmental variables for building and
running processes. On [Ark page in KDE Community Wiki][kde-community-ark-page]
it is stated that we have to set these variables:

{% highlight bash %}
# Install prefix, replace with any folder you want
export KF5=~/foo/whatever

export XDG_DATA_DIRS=$KF5/share:$XDG_DATA_DIRS
export XDG_CONFIG_DIRS=$KF5/etc/xdg:/etc/xdg
export PATH=$KF5/bin:$PATH
export QT_PLUGIN_PATH=$KF5/lib/plugins:$QT_PLUGIN_PATH
{% endhighlight %}

Alright, said - done. We have to go to both CLion CMake options<BR>
`(File -> Settings (CTRL+ALT+S) -> Build, Execution, Deployment -> CMake)`<BR>
and target's build options<BR>
`(Run -> Edit Configurations...) (ALT+SHIFT+F10 and then press 0)`,<BR>
where we'll set them.

![Environment Variables]({{ site.url + site.baseurl }}/assets/2016-05-18/environment-variables.png)

Be aware! CLion, unfortunately, doesn't allow to use bash variables here,
so you won't be able to write something like "/custom/folder/bin:$PATH".
So check you variables from terminal with `echo $VARIABLE` and paste needed
values manually.<BR>
Correct me if it is/will be wrong, I'll become happy.

**Finally**, we can add our bash script. Don't leave target's build options.
At the bottom you'll see the pane, which contains "Build" command running
before launch. Here we can add our external tool, which will be a bash script.

![After Install script]({{ site.url + site.baseurl }}/assets/2016-05-18/after-install-script.png)

(On the screenshot you may see a little difference, I just pressed edit
buttons in order to show you my existing External Tool, while you'll
have to add your own.)<BR>

And there are the contents of the script:

{% highlight bash %}
#!/bin/bash
working_dir=/home/mvlabat/gsoc/ark_install
cmake_binary_dir=$(</home/mvlabat/gsoc/ark_install/cmake_binary_dir)

cd "$cmake_binary_dir"
rm CMakeCache.txt
# The first argument is the path to the project sources
cmake ~/ClionProjects/ark/ -DCMAKE_INSTALL_PREFIX=/home/mvlabat/gsoc/ark_install -DKDE_INSTALL_LIBDIR=lib -DCMAKE_BUILD_TYPE=Debug
cd -

# '8' should be replaced with your CPU's thread count.
make -j8 -C $cmake_binary_dir install

app_path="$cmake_binary_dir/app/ark"
rm $app_path
ln -s $working_dir/bin/ark $app_path
{% endhighlight %}

P.S. Remember to replace the according strings with your install path.

## Running
Now we can start our happy debuggin! :)

Set your break points, then just press `ALT+SHIFT+F9`, select Ark target
(later you can just use `SHIFT+F9`) and enjoy! I hope everything will
work on your local machines.

If you have any problems, questions, marks, feel free to comment this post.

Thank you.

[clion-official-site]: https://www.jetbrains.com/clion/
[kde-community-ark-page]: https://community.kde.org/KDE_Utils/Ark
