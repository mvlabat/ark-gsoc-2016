---
layout: post
title:  "Finishing my GSoC project"
date:   2016-08-17 10:06:00 +0300
categories: tasks
---

Hello! I'm GSoC student. I've finished working on my project - KDE GUI archive manager called Ark. My main task was to implement add **to**, move to, copy to (within an archive) functionality.

Here are two screenshots to summarize all my done work.

Before:<BR>
![Ark before]({{ site.url }}{{ site.baseurl }}/assets/2016-08-17/ark-before.png)

After:<BR>
![Ark after]({{ site.url }}{{ site.baseurl }}/assets/2016-08-17/ark-after.png)

## Results
- For the first two months, while learning the codebase, also dealing with getting my Bachelor's diploma at the university (that's my excuse I was a little bit slow), I refactored ArchiveNode and ArchiveDirNode classes into Archive::Entry class. Previously metadata for archive entries was stored inside QMap with predefined fields in enum. Now QMap is replaced with QProperty system, and the two mentioned classes were merged into one (what is rather good than bad, because they have the same data and very few differences in methods).
- For the following three weeks interfaces and functionality for adding to, moving, copying and renaming were implemented, and the appropriate tests were written.
- The last week I implemented the GUI part: new actions were added and also checks if user is overwriting existing files were provided. 

Now I'm going to create my project page on <a href="https://community.kde.org/GSoC/2016/StatusReports">https://community.kde.org/GSoC/2016/StatusReports</a>

## The End
So... Shortly, that was a great experience. :) I'm very glad I was selected for Google Summer of Code. It gave me valuable experience in working on real open source products, developed both my hard and soft skills and gave me new contacts. Hello to my new 10 friends in Facebook from India, adding a guy you haven't even known (no, that's good (: ). You guys are very open, communicable and nice. )

I want to thank my mentor, Elvis Angelaccio. I'm very grateful to you for answering all my questions, being tolerant and nice and for your support. :) Ragnar Thomsen, thanks to you also for helping with debugging and fixing actual bugs on master. Mr. Bug Fixer :p

I hope my work will find its use, and my code will be reliable. :)
