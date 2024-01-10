# Week 1 Lab Report

## cd

**no arguments**
```
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$ 
```
Typing the cd command with no arguments will change you to the home directory.

**path to directory**
```
[user@sahara ~]$ cd lecture1/
[user@sahara ~/lecture1]$
```
There is a folder under the home directory called lecture1, so the command moved the working directory to that folder.

**path to file**
```
[user@sahara ~/lecture1]$ cd Hello.java
bash: cd: Hello.java: Not a directory
[user@sahara ~/lecture1]$
```
cd is for changing directories, so it can't move into a file. So, it gives an error.

## ls

**no arguments**
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
[user@sahara ~/lecture1]$
```
ls is used to list the folders and files in a directory, so typing the command with no arguments listed all the folders and files in the current directory, lecture1.

**path to directory**
```
[user@sahara ~/lecture1]$ ls messages
en-us.txt  es-mx.txt  fr-ca.txt  zh-cn.txt
[user@sahara ~/lecture1]$
```
if given the path to a directory, ls will list the folders and files in that directory.

**path to file**
```
[user@sahara ~/lecture1]$ ls Hello.java
Hello.java
```
Within a file, there is only one folder/file ls can list, and that is the file itself.

## cat

**no arguments**
```
[user@sahara ~/lecture1]$ cat
^C
[user@sahara ~/lecture1]$
```
The command was unresolved and I had to stop the process. This is probably because cat is designed to work with one or more arguments. Without any arguments, it doesn't know what to output.

**path to directory**
```
[user@sahara ~/lecture1]$ cat messages/
cat: messages/: Is a directory
```
The command returned an error because cat is meant to output the contents of files. Giving it a directory as its argument won't work because, though directories may contain other files and folders, a directory itself does not have any data.

**path to file**
```
[user@sahara ~/lecture1]$ cat messages/en-us.txt 
Hello World!
[user@sahara ~/lecture1]$
```
The command printed out "Hello World!" because that is the content of the en-us.txt file, which was given as the argument.
