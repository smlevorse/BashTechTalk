# BashTechTalk
Resources for an Introduction to Bash tech talk for RIT's Society of Software Engineers

## Getting Started with Bash
[Bash](https://www.gnu.org/software/bash/) is the Bourne Again SHell, a shell program meant to incorporate useful features from [Korn Shell](http://www.kornshell.org/) and the [C Shell](https://en.wikipedia.org/wiki/C_shell). It provides important functionality that lets you interact with your operating system and run programs, as well as automate tasks. It is installed on many devices from personal computers, servers, and embedded systems. 

### MacOS and Linux
Chances are you already have Bash installed. You should be able to just open your terminal and be in Bash. If for some reason you aren't, you should be able to type `bash` and enter the shell.

### Windows
For Windows devices, you have a couple of options:
- SSH into another machine
- Install the [Windows Subsytem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
- Install [MinGW](http://mingw.org/) or [Cygwin](https://www.cygwin.com)
- You might have GitBash installed if you installed it when you install Git

## Basic Commands
To run a command on Bash, simply type it into the terminal and press Enter. Bash runs commands and programs out of a working directory. To find out what the current working directory is, run the `pwd` (Print Working Directory) command.

```
$ pwd
/home/usr/username
$
```

You can create files in a directory using the `touch` command. `touch` modifies a files timestamp. If the file exists, it will make the timestamp the current time. Otherwise, it creates the file. 

```
$ touch file1
$
```

You can make subdirectories using the `mkdir` (MaKe DIRectory) command. This will create an empty directory at the path specified.

```
$ mkdir docs
$
```

You can see what is currently in the directory with the `ls` (LiSt) command. 

```
$ ls
file1 docs
$
```

When you run a command, by default it runs as the user that is logged in (you). However, some commands require permissions to run. In order to escalate the priveleges for a certain command, use `sudo` (Super User DO).

```
$ apt-get install vim
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?
$ sudo apt-get install vim
[sudo] password for username: 
...
$
```

> NOTE: When you use `sudo`, you are granting a command or program access to your computer. Make sure you trust commands that your un with `sudo`

If you don't know how to use a command, or need more information about how a command works, use `man` to bring up the manual for a command. This will open the manual in a new process. To return to the terminal window, press `Q`.

```
$ man ls
LS(1)                                         User Commands                                         LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
```

## Command Parameters
Most commands take parameters. Each parameter is separated by a space. If you need a space inside a parameter, surround the parameter with quotes. There is a general format for command parameters. Take for example the following command:

```
$ ls -a -l --color=auto /var
```
The command is `ls`, and the parameters are what follow.
- `-a` is a _flag_ in _short form_ that tells `ls` to include hidden files
- `-l` is a _flag_ in _short form_ that tells `ls` to use a list forat for output
- `--color` is a _flag_ in _long form_ that tells `ls` what mode to use when coloring the text
- `=auto` is the parameter for a flag to say that when coloring, use automatic mode
- `/var` is the paramter that tells `ls` what directory it should list

## Wildcards
Most bash programs support wildcard characters. These characers take the place of a character or substring to match on, giving the user a way to specify to the program how it should search or match something.
- `*` Matches on any string, including an empty string. `*abc*` would match any string that contains "abc"
- `?` matches on any single character. `file?.txt` would match files like `file1.txt`, `file2.txt`, `file3.txt`, etc

This is useful for doing things in bulk:
```
$ git add *.java
```
Would add any java file in the project to staging

## History
Bash keeps track of a history of the commands that you have enterd during a session. It then lets you go back in that history to reuse commands. If you use the arrow keys, you can scroll up or down through the commands you have recently entered. This is useful for rerunning commands, or going back to fix mispelled commands:

```
$ gvv -o myProgramWithALongName myProgramWithALongName.c myProgramWithALongName.h
bash: gvv: command not found
*** Press up arrow key and fix command ***
$ gcc -o myProgramWithALongName myProgramWithALongName.c myProgramWithALongName.h
```

You can also search you history for a command. Simply press `Ctrl + R` and type the command you want to search for. Then use the arrow keys to scroll through your results.

You can use `!` to refer to commands in your history.
- `!!` Copies the last command you entered
- `!-3` Copies the command you ran 3 commands ago
- `!22` Copies the command at index 22 in your history

This is very useful for when you forget to use `sudo`
```
$ apt-get install vim
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?
$ sudo !!
sudo apt-get install vim
[sudo] password for username: 
...
$
```

At the end of your session, your bash history is flushed to the `~/.bash_history` file in your home directory.

## Piping and Streaming
Piping and Streaming lets you string together commands

### Piping
Piping allows you to take the output from one command and use it as the input to the next command. You can string together as many commands as you would like. Piping uses the `|` character.
```
$ cat log.txt | grep error
```
This line is useful in that instead of printing the entire log file to the console window, it outputs it to the `grep` command which filters out any lines that do not match the pattern. Therefore, the only lines you would see are lines that contain errors. 

### Streaming
Streaming allows you to intercept input and output for a command and stream them to somewhere else like a file. Streaming uses the `<` and `>` characters. 

```
$ netstat -anp > port_usage.txt
```
This will take the output from the `netstat` command and write it to a file called `port_usage.txt`

There are three standard streams that can be intercepted:
- 0 stdin - This stream is the input to the program that usually you unter in the console window
- 1 stdout - This stream is the output that would be written to the console window
- 2 stderr - This stream is any error output that usually is written to the console window

This allows you to do something like:
```
$ myProgram 0<input.txt 1>log.txt 2>errors.txt
```
In this case, the program is now reading from an input file and writing any log statements to the log file. However, when it encounters an error, it outputs that to an error file for easy aggregation.

## Variables and Aliases
Bash lets you write to an read from variables. Use `=` to set a variable. Spaces are not allowed, unless quotes are used
```
$ var=hello
$ echo $var
hello
$ var="hello world"
$ echo $var
hello world
$
```

You can also create aliases to commands
```
$ alias lsl='ls -l'
$ lsl
total 0
drwxrwxrwx 1 username username 4096 Oct  4 17:44 docs
-rwxrwxrwx 1 username username    0 Oct  4 17:44 file1
$
```

## Scripting
(Coming Soon)

## Cool Tricks
`&&` and `||` let you break on failure or recover from failures.
```
$ ping nitron.se.rit.edu && ssh sml2565@nitron.se.rit.edu
```
In this case, if the ping fails, then the ssh won't be attempted

```
rm file1 || rm ../file1
```
In this case, if the `rm` failed to delete `file1`, it tries again in the parent directory

`Ctrl + L` – Clears the screen

`Ctrl + U` – Clears your current input

Interpolation lets you run commands within a command:
```
ls $(cat directory.txt)
```
`Ctrl + C` – Kill current process
`Ctrl + Z` – Put process in background
`fg` – pulls process from the background to the foreground

## Resources
- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)
- [Learn X in Y Minutes](https://learnxinyminutes.com/docs/bash/) <- This has a lot more in depth scripting techniques
- [Codecademy Bash Walkthrough](https://www.codecademy.com/catalog/language/bash)

## Fun stuff
- [Bash one liners](http://www.bashoneliners.com/)
- [Web Server Written in Bash](https://github.com/avleen/bashttpd)
- [Bash Game](http://play.bash.academy/)
