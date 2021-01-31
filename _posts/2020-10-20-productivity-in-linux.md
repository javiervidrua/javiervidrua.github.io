---
layout: post
title:  "Productivity in Linux"
date:   2020-10-20 00:00:00 +0200
categories: jekyll update
---
<!--
https://www.youtube.com/watch?v=IlK6FNxmRMY
https://www.youtube.com/watch?v=6OOqROUQ60s
https://www.youtube.com/watch?v=W8ykZNSLDqE
https://www.youtube.com/watch?v=azcrPFhaY9k
-->
Here are some tricks to increase your productivity when using Linux.

## Tab completion
Whenever you're typing a command, if you hit `<tab>`, it will try to autocomplete the command. If nothing happens, try hitting it twice, to make it show you all the possibilities.

## Circle through the history of commands with the arrow keys
This isn't a Linux specific productivity habit, since it works in every operating system, but I felt that it belongs here.

You can use the "*up*" and "*down*" arrow keys to navigate through the history of commands.

## Perform a reverse search
If you want to search for a command you ran a long ago, you can hit `ctrl+r` and type some part of it.

Once you get the command, hit `<tab>`, `<esc>` or the arrow keys to edit it, or `<enter>` to run it.

## Readline arguments
If you really want to know about this, check **[this](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Readline-Arguments)** out.
If you want to repeat `Z` 10 times:
```bash
alt+1 0 Z
```
It will output `ZZZZZZZZZZ`

If you want to delete the next 5 characters:
```bash
alt+- 5 ctrl+d
```
It will output `ZZZZZ`

## Redo the last command as root user
If you type a long command, and when you press enter you get a `Permission denied` error message, instead of hitting "*up*" and going to the start of the command to type "*sudo*", you can run this instead:
```bash
sudo !!
```

## Fix a really long command that you messed up in your favourite editor
If you realize that you have to fix major part of the last command that you typed, you can fix it in your favourite editor and run it with the command:
```bash
fc
```
Alternatively, you can use the following key combination:
```bash
ctrl+x+e
```

## Don't add command to the history
To do this you just need to append a space at the start of the command (note the space at the start):
```bash
 echo 'test'
```

## Go to the previous directory
You can type the following command to go to the previous directory:
```bash
cd -
```

## Write to file as root when using vim
Using vim, if you find yourself editting a file and when you try to write the changes you get a `File not writable` error message, you can do this:
```bash
:w !sudo tee %
```

## Write to a file in between pipes
If you want to log a command to a file, at the same time as keeping the stdout, you can do this:
```bash
echo 'hola' | tee -a log.txt
```

## Close a terminal but keep all the processes running
Let's say that you have used the `at` command to program something but now you want to close the terminal. If you do so, the `at` command won't work.

What you can do is make the process of the terminal disown all of its child processes with the command:
```bash
disown -a && exit
```

## Reset a terminal that has gone crazy
Just type:
```bash
reset
```

## Create multiple folders and subfolders
Let's say that you wanted to create two directories with 5 folders inside each one. You can do so like this:
```bash
mkdir -p {one,two}/{1,2,3,4,5}
```
Note: The `-p` parameter is to tell `mkdir` to create the parent directory if it does not exist.

## Quickly create a sequence of numbers
There are two main, quick ways of doing so:
```bash
{1..100}
```

```bash
seq 1 100
```

## Quickly cut and yank part of the commands
Let's say that you have a long command:
```bash
ls | tee -a files.txt | xargs ls -l | tee files-precise.txt | wc -l
```
And you want to only get this:
```bash
ls | tee -a files.txt
```
* You can navigate to that position and hit `ctrl+k` to cut the text from there to the end of the line.
* You can yank it back using `ctrl+y`.
* If what you wan to do is cut from there to the start of the line you can hit `ctrl+u`
* If you want to kill backwards, word by work, you can hit `ctrl+w`

## Using less to view the last lines of a file (useful for logs)
Instead of using `tail -f <file>` you can use:
```bash
less +F <file>
```
This will start `less` at the end of the file.
* If you want to be able to move up or down, you can hit `ctrl+c`.
* If you want to attach less to the end of the file again, hit `shift+f`
* If you didn't run `less +F`, you can hit `shitf+g` to go to the end of the file.

## Quickly access the arguments
You can hit `atl+.` to circle through the arguments used in previous commands instead of typing them.

This is useful, for example, if you typed a really long path to change directories.

## Go to the beginning of the line
You can hit:
```bash
ctrl+a
```
And this will bring the cursor to the beginning of the line.

## Piping the output of grep to less
In many situations you'll find yourself grepping the output of a command, and sometimes it can be a little bit too much. You can pipe it to less so you can scroll up and down.
```bash
cat /var/log/dpkg.log | grep 'installed' | less
```

## Searching for files or directories using find and grep
If you need to search for somthing you can use `find` and `grep` to do so.

Let's say that you want to find a file named "*secretFile*"
```bash
find . -type f | grep 'secretFile'
```

Note: The first argument is the directory that contains what you want to search.

If you want to search for a directory named "*secretDirectory*", you can do so like this:
```bash
find . -type d | grep 'secretDirectory'
```

## Using the man pages
There are man pages for almost every command and for many system functions in Linux, so if you don't know how to use some tool or C function, try using the `man` command.

If you don't remember the name of the command you want to use, you can use `man` for that:
```bash
man -k compare strings
```

## Use regular expressions
These are some of the most common flavours of regular expressions:
```bash
( )   -> Grouping
( | ) -> Grouping with or.
[]    -> Character classes.
*     -> Zero or more (lazy).
+     -> One or more (greedy).
.     -> Any character but newline.
$     -> End of line.
^     -> Beginning of line.
```

## Use the search mode
When using `less`, you can hit `/` and enter the search mode.

Then, you can hit `n` and `shift+n` to go to the next or the previous occurrence of the text you searched.

## Learn to solve a problem
1. Understand the problem. Drawing is a good way to make this step easier. Trying to explain the problem to other person is a good way too.
1. Divide the problem into smaller problems. Make a check list. Make sure the problems are small enough, don't make them too big.
1. If you get stuck, take it as a challenge.
1. If all of this doesn't work, try starting over. You don't necessarily need to delete everything, you can comment everything.
1. Practise allday everyday.

## Run a command for each line of the output of another command
You just need to pipe the output into `xargs <command>`.
```bash
find . | grep .png | xargs ls -l
```

## Use git for source control
If you dont know what `git` is, drop everything you're doing right now and learn it immediately.
