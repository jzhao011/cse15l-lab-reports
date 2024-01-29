# Week 1 Lab Report

## cd

**no arguments**
```
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$ 
```
The current working directory is `/home/lecture1/`. Typing the `cd` command with no arguments will change you to the home directory, `/home/`, from any directory.

**path to directory**
```
[user@sahara ~]$ cd lecture1/
[user@sahara ~/lecture1]$
```
The current working directory is `/home/`. There is a folder under `/home/` called `lecture1`, so the command moved the working directory to that folder.

**path to file**
```
[user@sahara ~/lecture1]$ cd Hello.java
bash: cd: Hello.java: Not a directory
[user@sahara ~/lecture1]$
```
The current working directory is `/home/lecture1/`, which contains `Hello.java`. `cd` is for changing directories, so it can't move into a file. So, it gives an error.

## ls

**no arguments**
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
[user@sahara ~/lecture1]$
```
`ls` is used to list the folders and files in a directory, so typing the command with no arguments listed all the folders and files in the current directory, `/home/lecture1/`.

**path to directory**
```
[user@sahara ~/lecture1]$ ls messages
en-us.txt  es-mx.txt  fr-ca.txt  zh-cn.txt
[user@sahara ~/lecture1]$
```
The current directory is `/home/lecture1/`, which contains the folder `messages`. When given the path to a directory, `ls` will list the folders and files in that directory. `messages` is the relative path to `/home/lecture1/messages/` from the current directory, so it listed all of the files in `messages`.

**path to file**
```
[user@sahara ~/lecture1]$ ls Hello.java
Hello.java
```
The current directory is `/home/lecture1/`, which contains the file `Hello.java`. Within a file, there is only one folder/file `ls` can list, and that is the file itself. Thus, `ls Hello.java` prints `Hello.java`.

## cat

**no arguments**
```
[user@sahara ~/lecture1]$ cat
hello
hello
hi
hi
^C
[user@sahara ~/lecture1]$
```
The current directory is `/home/lecture1/`. The command was unresolved and I had to stop the process using `Ctrl-C`. This is probably because `cat` is designed to work with one or more arguments. Without any arguments, it doesn't know what to output. After retesting I found that, if I type something and press enter, whatever I typed would be echoed back in the terminal. This is likely because `cat` is printing out the content of what I typed.

**path to directory**
```
[user@sahara ~/lecture1]$ cat messages/
cat: messages/: Is a directory
```
The current directory is `/home/lecture1/`, which contains the folder `messages`. The command returned an error because `cat` is meant to output the contents of files. Giving it a directory as its argument won't work because, though directories may contain other files and folders, a directory itself does not have any data.

**path to file**
```
[user@sahara ~/lecture1]$ cat messages/en-us.txt 
Hello World!
[user@sahara ~/lecture1]$
```
The current directory is `/home/lecture1/`. The command printed out "Hello World!" because that is the content of the `en-us.txt` file, whos relative path to the current directory was given as the argument.
