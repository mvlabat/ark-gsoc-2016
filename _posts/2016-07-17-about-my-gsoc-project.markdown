---
layout: post
title:  "About my GSoC project"
date:   2016-07-18 20:06:00 +0300
categories: tasks
---

I think it's <del>slowpoke</del> time to present my project finally. Better late than never. :)

I'm a GSoC student. My name is Vladyslav Batyrenko, I'm from Ukraine.

My project is related to Ark - GUI archive manager. Shortly, my main task is to implement add **to**, move to, copy to (within an archive) functionality.
For now Ark can only create archives, add files and folders (preserving directory structures for inner entries) to root, extract and delete them.

## My progress
My mentor gave me a task (it was one of the first ones) to refactor QMap structure, which was holding archive entries metadata, into an Archive::Entry class, which would use QProperty system. Such refactoring would bring more extensibility and allow to pass and manipulate data in more convenient ways. And of course QProperty is a big step forward for possible future using QtQuick and other nice modern Qt stuff. Today I'm finished with it. That was a huge amount of work in order to complete that task. It was not hard itself, but rather routine, because this structure was used by large part of code. Though after that I faced a tough challenge to fix all the bugs I've done with that refactoring. Now I'm happy I can proceed to other important things.

Today I also started expanding adding functionality. Interfaces are refactored and the tests are written. The small part is left - implement it. :D

## My remained tasks
- First of all, I still have to research the ways to add **to**, move to, copy to via libarchive, zip, 7zip and rar command line tools.
- After that, besides refactoring and writing tests, I, obviously, have to implement all that functionality in code.
- And finally I'll have to bind that code to new GUI elements.

So... There are four weeks left. I'll do my best to complete the tasks. Time management, strength of will, don't screw me. )

I think my next post will be about my experience and researches about QProperty system. I've found out some interesting details which were not clearly documented in Qt. So if you are interested, stay tuned. :)
