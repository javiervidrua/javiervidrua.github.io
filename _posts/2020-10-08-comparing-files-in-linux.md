---
layout: post
title:  "Comparing files in Linux"
date:   2020-10-08 00:00:00 +0200
categories: jekyll update
---
If you plan to work as a *system administrator*, *pentester*, *network engineer* *or related*, **there are lots of situations when you'll need to compare files between each other.**

In Linux, it's relatively easy to do, since there are various utilities for this type of job. 

These two are one of the most used ones:
* **Comm**: Compares two files and outputs the content by columns. Let's show an example:
```
javier@torre:~$ cat before
1
2
3
4
5
6
javier@torre:~$ cat after
1
2
3
4
6
7
javier@torre:~$ comm before after
                1
                2
                3
                4
5
                6
        7
```
You can see that the program outputs the lines that are the same in the last column, and the lines that are different in their respective columns, the first one for the first file passed to the tool.
* **Diff**: It works in a similar way to the *Comm* command, but it's more complex and has a lot of formatting options. These are two examples:

  With the "*-c*" parameter you tell *diff* that you want to see the contexts:

  ```
  javier@torre:~$ diff before after -c
  *** before      2020-10-08 14:07:28.820000000 +0200
  --- after       2020-10-08 14:08:11.260000000 +0200
  ***************
  *** 2,6 ****
    2
    3
    4
  - 5
    6
  --- 2,6 ----
    2
    3
    4
    6
  + 7
  ```
  With the "*-u*" parameter you tell *diff* that you want to see the output in unified format, that is, showing a mix of both files.
  ```
  javier@torre:~$ diff before after -u
  --- before      2020-10-08 14:07:28.820000000 +0200
  +++ after       2020-10-08 14:08:11.260000000 +0200
  @@ -2,5 +2,5 @@
   2
   3
   4
  -5
   6
  +7
  ```

As you can see, the output of the *comm* tool is more simple, so it's good for begginers and for simple tasks, while the *diff* is more complex and is used for the more serious jobs.