# cs-bash <img src="./figures/project-logo.png" width="100" align="right">

A primer on BASH scripting and Linux systems, now a course on: [derecksnotes.com](https://www.derecksnotes.com/courses/displayEntry.php?entry=intro-unix-shell)

## About this course 

This course was prepared by Dereck de Mezquita, inspired by a number of different resources online and personal experience with SHELL scripting. This document is prepared in an `.Rmd` file with `knitr`; let's start by clearing all files in the `./playground` directory.


```bash
rm -r playground
mkdir playground
```

In this course we will be using the `iris` and `mtcars` datasets. This is saved in a `.csv` file in the data directory:


```bash
head data/iris.csv
```

```
## "sepal.length","sepal.width","petal.length","petal.width","variety"
## 5.1,3.5,1.4,.2,"Setosa"
## 4.9,3,1.4,.2,"Setosa"
## 4.7,3.2,1.3,.2,"Setosa"
## 4.6,3.1,1.5,.2,"Setosa"
## 5,3.6,1.4,.2,"Setosa"
## 5.4,3.9,1.7,.4,"Setosa"
## 4.6,3.4,1.4,.3,"Setosa"
## 5,3.4,1.5,.2,"Setosa"
## 4.4,2.9,1.4,.2,"Setosa"
```


```bash
head data/mtcars.csv
```

```
## model,mpg,cyl,disp,hp,drat,wt,qsec,vs,am,gear,carb
## Mazda RX4,21,6,160,110,3.9,2.62,16.46,0,1,4,4
## Mazda RX4 Wag,21,6,160,110,3.9,2.875,17.02,0,1,4,4
## Datsun 710,22.8,4,108,93,3.85,2.32,18.61,1,1,4,1
## Hornet 4 Drive,21.4,6,258,110,3.08,3.215,19.44,1,0,3,1
## Hornet Sportabout,18.7,8,360,175,3.15,3.44,17.02,0,0,3,2
## Valiant,18.1,6,225,105,2.76,3.46,20.22,1,0,3,1
## Duster 360,14.3,8,360,245,3.21,3.57,15.84,0,0,3,4
## Merc 240D,24.4,4,146.7,62,3.69,3.19,20,1,0,4,2
## Merc 230,22.8,4,140.8,95,3.92,3.15,22.9,1,0,4,2
```

*Note you might often see the use of a pipe and head. This is to reduce the output's size.*

You can download the data for this course by searching in the data directory of the associated github repo: [dereckdemezquita/cs-bash](https://github.com/dereckdemezquita/cs-bash).

# Bash quick start; basic commands

BASH is a 0 indexed language. This means that arrays start counting from 0. Thus, the first item in a list is 0, then 1, then `n - 1` for the ending item.

*Note you can use the tab key to autocomplete file and directory names.*

The filesystem manages files and directories in the computer system. These are defined by absoulute paths to the system's root. The root being the lowest level directory holding all other system files designated by `/`.

A file and directory can be defined as such: 

- `/home/usr/username/Documents/`
- `/home/usr/username/Documents/somefile.txt`

- Never use spaces in file names; use underscores `_`.
- `history` prints a list of recent commands; then use `!n` to re-use a specific command by index.

Quotes are understood a little different depending on which kind are used: double `""` or single `''`. 

- `'sometext'` single quotes means the SHELL understands this literally.
- `"sometext"` double quotes means the SHELL understands this literally expcept when it sees a `$` or backticks \`\`.

Backticks create a SHELL within a SHELL. This syntax can be very powerful and useful.

- \`some_command\` the SHELL runs the command inside and captures the STDOUT (output) back into the above SHELL as a variable.
    - A newer syntax for this same method is `$(some_command)`.
    
<br>

- BASH is indexed at 0.
- `#!/bin/bash` A "shebang" should be included in each script file to point to the interpreter.
    - Or use: `bash somescript.sh`.
- `'sometext'` single quotes means the SHELL understands this literally.
- `"sometext"` double quotes means the SHELL understands this literally expcept when it sees a `$` or backticks \`\`.
- \`some_command\` the SHELL runs the command inside and captures the STDOUT (output) back into the above SHELL as a variable.
    - A newer syntax for this same method is `$(some_command)`.
- `date` returns the current date and time: `date Sat 13 Mar 2021 15:56:49 PST`.
- All variables in BASH are global by default.
    - `local var="Something"` restrict to local scope

<br>

- Check BASH version with `bash --version`.

- assignment
	- `=`
	- `+=`
	
- comparators
	- `==`
	- `!=`
	- `>`
	- `<`
	- `<=`
	- `>=`
	
- arithmetic
	- `+`
	- `-`
	- `*`
	- `/`



```bash
w # shows list of connected users.
whoami # shows the name of the current user.
uname -a # shows kernal information (Linux, Darwin for OSX).
date # shows current date and time.

# ctrl + c abort execution.
# ctrl + z pause execution.
```

```
## 20:08  up 24 mins, 4 users, load averages: 2.01 2.12 2.61
## USER     TTY      FROM              LOGIN@  IDLE WHAT
## work     console  -                19:44      23 -
## work     s000     -                19:45      22 ping google.com
## work     s005     -                19:45      23 -bash
## work     s006     -                19:45      23 -bash
## work
## Darwin derecks-MBP-2.attlocal.net 19.6.0 Darwin Kernel Version 19.6.0: Thu Oct 29 22:56:45 PDT 2020; root:xnu-6153.141.2.2~1/RELEASE_X86_64 x86_64
## Mon Mar 15 20:08:24 PDT 2021
```


```bash
history # prints a list of recent commands
    !2 # re-runs command n2 from the history
chmod # modify permissions on a file/directory
pwd # find current directory
cd # change directory
ls # list contents of a given directory
cp original.txt copy.txt # copy files
mv original-location/text.txt new-location/text.txt # move/rename files
rm text.txt text-1.txt # delete files
rmdir directory-name # delete directory
mkdir directory-name # create directory
touch file-name.txt # create file
cat file-name.txt # print file contnets
less file-name.txt # print large file contnets in pages
cut # print certain columns from text file such as csv
grep # match a string/regex
sed # regex string replacement
ls -la > list-of-files.txt # redirect outputs
wc -l text.csv # count word/characters/lines
sort -r -n -b # sort in reverse numerical order and ignore leading white spaces
uniq # gets unique non-adjacent data
echo $SHELL # print a variable (preceed with $ sign)
test="A value or file input of some sort." # define a variable
for item in  list1 list2 list3; do echo $item; done # for loop
# multi-line for loop
for item in $@
do
    echo "Testing: $item"
done
# a script with multiple variables
echo '{
    echo "Username: $1";
    echo "Age: $2";
}' > script.sh
declare -a my_array # creates an empty array
my_array=(1 2 3) # creates an array of 1, 2, 3
echo ${my_array[@]} # prints all elements
echo ${#my_arary[@]} # prints the length 
array[@]:n:m # slice an array
echo ${!friends_list[@]} # return all keys of associative list
# if statements
if [ boolean_condition ]; then
    # CODE
else
    # OTHER CODE
fi # finishes an if statement
```

`|` the pipe command separator allows the passsage of one output to another command. Chains can be built in this way for example: `ls -l | grep ".*\.file-extension` that command allows one to find all files from `ls -l` that have the given extension.

## Getting help

Use the `man command-name` command to get help for a specific command. For example `man ls` would get you the documentation in a paginated view for `ls`. 

Note the following for help:

- `NAME` tells you what the command does.
- `SYNOPSIS` list all possible flags.
- `[...]` are optional flags.
- `|` separates alternatives.
- `...` arguments that can be repeats.

# Introduction to BASH scripting

A UNIX shell is a command line interface/interpreter or shell that allows one to use a command line to manipulate UNIX-like operating systems. It is a both an interactive command language and a scripting language. You can both use it as a console, and write and save scripts to be run later.

Before computers had GUIs they had command-line interfaces. The shell is an interface through which we can run other programmes and points their output to a human readable form; it then prompts for the next command. 

What is BASH and why use it? BASH stands for "Bourne Again Shell". It is a shell that was developed in the 80's. BASH is used by the operating system to control the execution of shell scripts. Operating systems like, OSX and Linux are based on UNIX. The operating system manages memory and computing power (CPU, GPU), devices (screen, keyboard), and manages users. Operating systems allows us to run computer programmes and the graphical user interface (GUI).

**MacOS has recently switched to zsh - a variant of BASH. - Note that in MacOS's `zsh` implementation `zsh` is 1 indexed.**

**We will learn: directory structure, shell (command lin interface), philosphy (multiuser/multitasking), flexibility; everything is a file.**

BASH can be used to run programmes and data pipelines or automate repetitive tasks. All major cloud computing systems have a CLI to their products: AWS, Google Cloud, Microsoft Azure. Learning and being capable of using SHELL scripting and BASH is an essential skill.

BASH scripting is the process whereby a programmer writes a BASH script in a file and saves it. This file can then be executed and even repeatedly used for convenience. 

## History

The origins of UNIX start with multics created at MIT in the 60s, this was then licenced by AT&T in the 70s. The GNU project in the 80s developed the project. Finally Linux was created by Linus Torvalds in the 90s.

The Linux operating system has by far gone on to become the most succesful and widely used operating system in the world. Today it runs on anything from personal machines, industrial machines, and a majority of internet servers.

## Architecture of UNIX

<p align="center">
    <img src="build-Rmd/figures/1-os-structure.jpg">
</p>

Computer programmes, including GUI ones, access the computer's hardware through the kernal and system calls.

<p align="center">
    <img src="build-Rmd/figures/2-file-structure.jpg">
</p>

The computer's storage is the separated into various parts and compartments, some accessible to a basic user and some not. The `/bin`, `/dev`, `/etc`, are mostly files used by the system and the programmes installed on the machine. `/usr` contains common user system files for programmes installed on the machine; this reduces redundancies and saves storage. Finally the `/home` directory contains the different user's files; on MacOS machines.

There is only one root in UNIX. Called by the `/`. Every path starts from the root.

## Absolute vs relative paths

There are two main kinds of paths in the filesystem. Absolute and relative. These are defined by the first character of the path; paths begining with `/` are absolute. These paths can be from anywhere in the file system and will always use as a starting point the root directory of the computer. A relative path may be defined by either `./` or a directory name at the begining of the path. These will only work when inside the directory above the rest of the path.

## The SHELL

In essence the shell is a programme like any other, but this particular programme allows us access to the machine's hardware through commands and scripts.

**You can launch the terminal in mac by searching your machine for an application named "Terminal". It is often found in the applications directory in the folder Utilities.**

It is an interface where we can give commands, a command line type of interface. The command line interface interprets what the user types. A command can be: a built-in shell command (`ls`, `pwd`, `mkdir`, etc...), an executable script, or even a compiled programme.

<p align="center">
    <img src="build-Rmd/figures/3-mac-terminal.png">
</p>

As stated before there are different implementations of the shell; `sh`, `BASH`, `csh`, `tsh`, `zsh`...

**The Mac terminal uses the `zsh` implimentation, as seen in the terminal's name up top in the info bar.**

## Logging in and out

You might find yourself using `BASH` or other on a local machine as well as remotely. Logging and logging out might be required follow these steps to do so:


```bash
login # logs in locally

ssh user@server # ssh command for server use
logout # logs you out of your session
```

## Running and managing running commands

Here are some basic commands that are often used. These get you some information about the system you're currently logged into.

- `w` shows list of connected users.
- `whoami` shows the name of the current user.
- `uname -a` shows kernal information (Linux, Darwin for OSX).
- `date` shows current date and time.


**To quit a command or abort execution you can used: `ctrl + c`.**

**To pause execution of a command you can use: `ctrl + z`.**

To go back to the programme type, `fg`, to let the programme resume in the background type `bg`. This allows it to push the execution to the background and let you use the shell for other things meanwhile.

So to restate: pause then send to background, or type the command and leave a space and type `-&` this will directly send the execution to the background. Look to the command below to view all commands currently running:


```bash
ps -u "Dereck" | head # this command shows all programmes currently running for a given user
```

```
##   UID   PID TTY           TIME CMD
##   501   751 ??         0:00.01 /usr/sbin/cfprefsd agent
##   501   752 ??         0:00.08 /usr/libexec/lsd
##   501   753 ??         0:00.06 /usr/libexec/secd
##   501   754 ??         0:00.02 /usr/sbin/distnoted agent
##   501   755 ??         0:00.23 /usr/libexec/trustd --agent
```


If you want to distinguish the programmes running on your user then do `ps -u $(whoami)`; more modern syntax shown below. This will show the ID "PID" of the programme and the time also with the command. As seen above a quick example of what was currently running on my machine.


```bash
ps -u `whoami` | head
```

```
##   UID   PID TTY           TIME CMD
##   502   319 ??         0:00.07 /System/Library/Frameworks/LocalAuthentication.framework/Support/coreauthd
##   502   320 ??         0:01.19 /usr/sbin/cfprefsd agent
##   502   322 ??         0:01.02 /usr/libexec/UserEventAgent (Aqua)
##   502   324 ??         0:00.78 /usr/sbin/distnoted agent
##   502   325 ??         0:00.24 /usr/sbin/universalaccessd launchd -s
##   502   326 ??         0:00.02 /System/Library/PrivateFrameworks/CloudServices.framework/Helpers/com.apple.sbd
##   502   327 ??         0:00.57 /usr/libexec/lsd
##   502   328 ??         0:00.83 /usr/libexec/knowledge-agent
##   502   329 ??         0:05.07 /usr/libexec/trustd --agent
```

To kill the execution of a specific command use the above to find the "PID" then use it as follows.


```bash
kill -9 PID # kills programme with given PID

# Flag: -9 KILL (non-catchable, non-ignorable kill)
```

## File permissions

On a LINUX system permissions are granted to specific users. Here is a quick summary of the different possibilities. This is seen when we do `ls -l` this will give the permissions and details. They are separated into three groups.


```bash
touch playground/test_permissions.txt
ls -l | head
chmod u+x playground/test_permissions.txt
ls -l | head
```

```
## total 112
## drwxr-xr-x  5 work  staff    160 Mar 15 18:21 build
## -rw-r--r--  1 work  staff  46332 Mar 15 20:08 build-Rmd.Rmd
## -rw-r--r--@ 1 work  staff    204 Mar 15 19:45 build-Rmd.Rproj
## drwxr-xr-x  9 work  staff    288 Mar 15 19:50 data
## drwxr-xr-x+ 8 work  staff    256 Mar 15 19:38 figures
## drwxr-xr-x  3 work  staff     96 Mar 15 20:08 playground
## -rw-r--r--  1 work  staff     71 Mar 15 14:36 styles.css
## total 112
## drwxr-xr-x  5 work  staff    160 Mar 15 18:21 build
## -rw-r--r--  1 work  staff  46332 Mar 15 20:08 build-Rmd.Rmd
## -rw-r--r--@ 1 work  staff    204 Mar 15 19:45 build-Rmd.Rproj
## drwxr-xr-x  9 work  staff    288 Mar 15 19:50 data
## drwxr-xr-x+ 8 work  staff    256 Mar 15 19:38 figures
## drwxr-xr-x  3 work  staff     96 Mar 15 20:08 playground
## -rw-r--r--  1 work  staff     71 Mar 15 14:36 styles.css
```

- `r` is readable.
- `w` is writable.
- `x` is executable.

These options can be used with the `chmod` command shown above. Here is a run down which are available and of what they do.

- `+x` adds execution.
- `-x` removes execution rights.
- `+r` adds reading rights.
- `+w` adds writing rights.
- `+rwx` adds all of the above.
- `u+x` change rights for current user; execution.
- `g+x` change rights for current group; execution.
- `o+x` change rights for other users; execution.
- `a+x` change the rights for everyone; execution.

**`chmod a-rwx filename.txt` removes all permissions to all users.**

```bash
chown Work playground/test_permissions.txt # sets the ownership of file to a user.

groups

chgrp admin playground/test_permissions.txt # sets the ownership of file to a group.
```

```
## staff everyone localaccounts _appserverusr admin _appserveradm _lpadmin com.apple.sharepoint.group.2 _appstore _lpoperator _developer _analyticsusers com.apple.access_ftp com.apple.access_screensharing com.apple.access_ssh com.apple.access_remote_ae com.apple.sharepoint.group.1
```

## Wildcards

Wildcards allow you to match all files in a directory by use of `*`; you can also use regex in your bash commands to read in certain files:


```bash
ls -l ./data/*\.csv # this will print all files that are .csv extensions
```

```
## -rw-r--r--@ 1 work  staff  3975 Feb  5  2014 ./data/iris.csv
## -rw-r--r--@ 1 work  staff  1700 Jul 31  2014 ./data/mtcars.csv
```

## Zipping and unzipping files

These commands allow you to zip and unzip; compress files. There are two main utilities for zipping and unzipping: `gzip` and `tar`. You will often come across these file types when downloading content.


```bash
touch playground/test_zip.txt
ls -l playground > playground/test_zip.txt # create a file and add content
ls -l playground

cat playground/test_zip.txt # show the original file
gzip playground/test_zip.txt # this will compress the file the original will no longer be available, there is an option to keep both
cat playground/test_zip.txt.gz # show the file was zipped

gunzip playground/test_zip.txt.gz # will unzip the file back to the original.
```

For the curious this is what happens when you try to gzip an already compressed file:


```bash
ls -l playground/ > playgound/test_zip.txt 

gzip playground/test_zip.txt
gzip playground/test_zip.txt.gz
```

The interest of `tar` vs `gzip` is that it works from a directory, unlike gzip which works from files. This allows us to zip multiple files at once easily. Note that the original file is not deleted with tar.


```bash
ls -l playground
tar -cf playground/Archive.zip figures/*
ls -l playground

cat playground/Archive.zip
```

You can also use tar for compression, but the syntax is backwards. Here with tar we give the options then the file we want to create or the archive.tar as the first argument, and the names of the files to compose the archive as the second.

## Change directories

Changing directories can be done with the command `cd` followed by either a relative or absolute path to go to. Once done you can use `pwd` to verify you've navigated correctly.

Use `cd ..` in order to move up one directory. The `..` designates the above directory.

## Copying files

The command `cp` can be used to copy a file. It takes two arguments the first being the original file and the following the copy. Note that this overwrites any existing files. The `cp` command can also take multiple files and if the last argument is a directory it will copy all files to that directory as shown below.


```bash
touch playground/original.txt
mkdir playground/backupdir
cp playground/original.txt playground/copy.txt
cp playground/original.txt playground/copy.txt playground/backupdir
```

## Moving a file

In order to move a file you can use the `mv` command. It has a similar syntax to the `cp` command except it deletes the original file. This command `mv` can also move multiple files in the same way as `cp`. Note `mv` can also rename files.

## Deleting/creating files/directories

Use the `rm` to remove or delete files. It takes as many names of files as you want. This command can even be done recursively: `rm text.txt`. 

Note that there is a separate command for deleting directories; or you can use the recursive option on `rm -r`. Use `rmdir` to delete directories.

In order to create directories use the `mkdir directory-name` command. In order to create files use the `touch file-name.txt` command.

## Temporarily storing files

When working on a project you often generate intermediate files. One place to temporarily store files is the `tmp` directory right below the root: `/tmp`.

## Printing a file's content

Use the `cat` command to print the file contents to the terminal. Sometimes you may have a very large file and printing with `cat` can cause issues. Instead use `less`. This command will display some of the content on one page and you can use the spacebar to page down and `q` command to quit.

In order to print just the top of a file use the `head` command. Use the arugments `head -n 5 filename.txt` to specify the amount of lines to print.

### `head`/`tail`

In order to get rows or columns from a file use the `cut` command. It has several arguments but a common use looks something like this:


```bash
cut -f -1-2,5 -d , data/iris.csv | head -n 5 # head used to reduce print size
```

```
## "sepal.length","sepal.width","variety"
## 5.1,3.5,"Setosa"
## 4.9,3,"Setosa"
## 4.7,3.2,"Setosa"
## 4.6,3.1,"Setosa"
```

This can be translated as select columns 2 to 5 and columns 8 using a comma as a separator. The `-f`argument means fields, it is used to specify columns and `-d` means delimiter or separator.

Note that cut doesn't work well with quoted text, this because there could be commas inside the quoted text. Such as:

```csv
name,age
"Dereck, Mezquita", 28
```

This would print one column (2):


```bash
cut -f 2 -d , data/iris.csv | head -n 5
```

```
## "sepal.width"
## 3.5
## 3
## 3.2
## 3.1
```

### `head`/`tail` the middle of a file

This trick might be useful to get lines in an interval. Use `tail` to get the last lines `n` to `...`. Then use a subsequent `head` to get the top of that:


```bash
tail -n 20 data/iris.csv | head -n 3
```

```
## 7.4,2.8,6.1,1.9,"Virginica"
## 7.9,3.8,6.4,2,"Virginica"
## 6.4,2.8,5.6,2.2,"Virginica"
```

## `paste` combine data

Can be used to combine data files. 

## `grep` regex selection

In order to select text that contains a certain string use the command `grep`. This can be used with a quoted or unquoted argument which is the string to match as such:


```bash
grep "Virginica" data/iris.csv | head -n 5
```

```
## 6.3,3.3,6,2.5,"Virginica"
## 5.8,2.7,5.1,1.9,"Virginica"
## 7.1,3,5.9,2.1,"Virginica"
## 6.3,2.9,5.6,1.8,"Virginica"
## 6.5,3,5.8,2.2,"Virginica"
```

Note that the above command does nothing as it has no text to select from. This command is often used in conjunction with another that has some kind of text or string output. This command can also be used by itself for printing content:


```bash
grep -c "Virginica" data/iris.csv data/mtcars.csv # -c prints number of matches
ls -l data | grep ".*\.csv"
```

```
## data/iris.csv:50
## data/mtcars.csv:0
## -rw-r--r--@ 1 work  staff     3975 Feb  5  2014 iris.csv
## -rw-r--r--@ 1 work  staff     1700 Jul 31  2014 mtcars.csv
```

That command allows us to get all files in a given directory that have the extension `.csv`.

Here are some common flags with `grep`:

- `-q` make grep return a boolean `true`/`false`.
- `-c` print the number of matching lines per file.
- `-h` don't print names of files when searching.
- `-i` ignore capitalisations ("Name" and "name" treated as matches).
- `-l` print the names of files containing matches but not the matches.
- `-n` print line numbers for matching lines.
- `-v` invert the match; only show lines that don't match.

## `sed` regex string replacement

The `sed` command allows you to replace a matching string with another. The syntax requires a regular expression as follows:


```bash
echo "The duck went to the market." | sed "s/duck/fox/g"
```

```
## The fox went to the market.
```

## Outputing to a file aka redirecting outputs

In BASH you don't necessarily have an output argument. Instead the output, which would and could normally go to the console/terminal, can be redirected to a file with use of `>` as a connector; right side argument is a file name:


```bash
ls -la > playground/list-of-files.txt
cat playground/list-of-files.txt | head
```

```
## total 136
## drwxr-xr-x  12 work  staff    384 Mar 15 20:08 .
## drwxr-xr-x  13 work  staff    416 Mar 15 19:59 ..
## -rw-r--r--@  1 work  staff   6148 Mar 15 20:02 .DS_Store
## -rw-r--r--   1 work  staff     58 Mar 15 19:41 .Rhistory
## drwxr-xr-x   4 work  staff    128 Mar 14 15:06 .Rproj.user
## drwxr-xr-x   5 work  staff    160 Mar 15 18:21 build
## -rw-r--r--   1 work  staff  46332 Mar 15 20:08 build-Rmd.Rmd
## -rw-r--r--@  1 work  staff    204 Mar 15 19:45 build-Rmd.Rproj
## drwxr-xr-x   9 work  staff    288 Mar 15 19:50 data
```

## `wc` word/character/line count

This command will let you count the number of works, characters, or lines in a document. Use the appropriate arguments:

- `-w` word count.
- `-c` character count.
- `-l` line count.


```bash
wc -l data/iris.csv
```

```
##      150 data/iris.csv
```

## `sort` data

In order to sort use the command `sort` with flags:

- `-f` ignore case.
- `-n` sort numerically.
- `-r` sort in reverse order.
- `-b` ignore leading blanks.

## `uniq` removing duplicates

### Aside: `awk`

This joins two `.csv` files by column names; we will use this for generating test data:


```bash
awk 'NF' data/mtcars.csv data/mtcars.csv | sort | head -n 5

awk 'NF' data/mtcars.csv data/mtcars.csv > playground/duplicated.csv
```

```
## AMC Javelin,15.2,8,304,150,3.15,3.435,17.3,0,0,3,2
## AMC Javelin,15.2,8,304,150,3.15,3.435,17.3,0,0,3,2
## Cadillac Fleetwood,10.4,8,472,205,2.93,5.25,17.98,0,0,3,4
## Cadillac Fleetwood,10.4,8,472,205,2.93,5.25,17.98,0,0,3,4
## Camaro Z28,13.3,8,350,245,3.73,3.84,15.41,0,0,3,4
```

This command removes adjacent duplicated lines. Bellow is an example:


```bash
cut -d , -f 1 playground/duplicated.csv | uniq | head -n 10
```

```
## model
## Mazda RX4
## Mazda RX4 Wag
## Datsun 710
## Hornet 4 Drive
## Hornet Sportabout
## Valiant
## Duster 360
## Merc 240D
## Merc 230
```



```bash
cut -d , -f 1 playground/duplicated.csv | uniq | head -n 10
```

```
## model
## Mazda RX4
## Mazda RX4 Wag
## Datsun 710
## Hornet 4 Drive
## Hornet Sportabout
## Valiant
## Duster 360
## Merc 240D
## Merc 230
```

Note that as state above this only works on adjacent lines so note that Tiger is left in duplicate where duplicate lines are not adjacent; sort to get strictly unique.

# BASH scripting

Here we get into actually creating more permanent scripts and code; not just typing single commands into the terminal.

## Manually editing a file

There are number of different editors that can be used in `Unix`: `nano` and `vim` just to name two very popular ones.

To use them use the command name and a file as an argument:


```bash
nano data/iris.csv
```

You can then navigate and edit with:

- `Ctrl + K` deletes a line.
- `Ctrl + U` undo line deletion.
- `Ctrl + O` save file ("o" for "output").
- `Ctrl + X` exit the editor.

## Printing variables

In order to store information you can use environment variables; you are storing information in these pre-existing variables that is. These are named as such with all capitals. 

- HOME User's home directory `/home/repl`
- PWD Working directory	`pwd`
- SHELL Which shell program	`/bin/bash | /bin/zsh`
- USER User ID `repl`

Type `set` in the terminal to get a full list (there are many):


```bash
set | head
```

```
## BASH=/bin/bash
## BASH_ARGC=()
## BASH_ARGV=()
## BASH_EXECUTION_STRING='set | head'
## BASH_LINENO=()
## BASH_SOURCE=()
## BASH_VERSINFO=([0]="3" [1]="2" [2]="57" [3]="1" [4]="release" [5]="x86_64-apple-darwin19")
## BASH_VERSION='3.2.57(1)-release'
## CLICOLOR_FORCE=1
## COMMAND_MODE=unix2003
```

You can print the value of a variable, even user defined ones, by using the `echo` command and the variable preceded by a dollar sign `$`.


```bash
echo $SHELL
```

```
## /usr/local/bin/bash
```

## Anatomy of a BASH script

The first part of a BASH script is a "shebang" often seen as this: `#!/bin/bash` on the very first line. This is so that the interpreter knows to use BASH and where it's located on your machine.

If you are having issues try to check where it's located using the command: `which bash`.

The next part of the script is your code. 


```bash
#!/bin/bash

# your code goes here
for item in item1 item2 item3
do
    echo "Print: $item"
done
```

These files are typically used with the extension `.sh`. Be sure to read the next section on execution permissions.

Run your script by using the command: `bash script.sh`. Or if the file contains the "shebang" on the first line the no need to define the interpreter simply use `./script.sh`.

## Saving and running scripts

In order to write shell scripts in a file and use them use the `.sh` extension on a file name. Be sure to change the permissions on the file you created to allow for execution. This may require use of `chmod` as follows:


```bash
touch playground/script_0.sh
ls -l playground | grep "script_0.sh"

chmod u+x playground/script_0.sh

ls -l playground | grep "script_0.sh"
```

```
## -rw-r--r--  1 work  staff     0 Mar 15 20:08 script_0.sh
## -rwxr--r--  1 work  staff     0 Mar 15 20:08 script_0.sh
```

You can then run the file using the following syntax in the terminal:


```bash
echo 'echo "testing; echoing from script"' > playground/script_0.sh
bash playground/script_0.sh
```

```
## testing; echoing from script
```

## Pass filenames to a script

In order to have a script use files we give it as an argument in the terminal we can use the following syntax inside the script: `$@`. This allows the script to the use our arguments in the CLI.


```bash
echo 'sort $@ | uniq -c' > playground/script_1.sh
cat playground/script_1.sh
```

```
## sort $@ | uniq -c
```


```bash
bash playground/script_1.sh playground/*\.csv
```

```
##    2 AMC Javelin,15.2,8,304,150,3.15,3.435,17.3,0,0,3,2
##    2 Cadillac Fleetwood,10.4,8,472,205,2.93,5.25,17.98,0,0,3,4
##    2 Camaro Z28,13.3,8,350,245,3.73,3.84,15.41,0,0,3,4
##    2 Chrysler Imperial,14.7,8,440,230,3.23,5.345,17.42,0,0,3,4
##    2 Datsun 710,22.8,4,108,93,3.85,2.32,18.61,1,1,4,1
##    2 Dodge Challenger,15.5,8,318,150,2.76,3.52,16.87,0,0,3,2
##    2 Duster 360,14.3,8,360,245,3.21,3.57,15.84,0,0,3,4
##    2 Ferrari Dino,19.7,6,145,175,3.62,2.77,15.5,0,1,5,6
##    2 Fiat 128,32.4,4,78.7,66,4.08,2.2,19.47,1,1,4,1
##    2 Fiat X1-9,27.3,4,79,66,4.08,1.935,18.9,1,1,4,1
##    2 Ford Pantera L,15.8,8,351,264,4.22,3.17,14.5,0,1,5,4
##    2 Honda Civic,30.4,4,75.7,52,4.93,1.615,18.52,1,1,4,2
##    2 Hornet 4 Drive,21.4,6,258,110,3.08,3.215,19.44,1,0,3,1
##    2 Hornet Sportabout,18.7,8,360,175,3.15,3.44,17.02,0,0,3,2
##    2 Lincoln Continental,10.4,8,460,215,3,5.424,17.82,0,0,3,4
##    2 Lotus Europa,30.4,4,95.1,113,3.77,1.513,16.9,1,1,5,2
##    2 Maserati Bora,15,8,301,335,3.54,3.57,14.6,0,1,5,8
##    2 Mazda RX4 Wag,21,6,160,110,3.9,2.875,17.02,0,1,4,4
##    2 Mazda RX4,21,6,160,110,3.9,2.62,16.46,0,1,4,4
##    2 Merc 230,22.8,4,140.8,95,3.92,3.15,22.9,1,0,4,2
##    2 Merc 240D,24.4,4,146.7,62,3.69,3.19,20,1,0,4,2
##    2 Merc 280,19.2,6,167.6,123,3.92,3.44,18.3,1,0,4,4
##    2 Merc 280C,17.8,6,167.6,123,3.92,3.44,18.9,1,0,4,4
##    2 Merc 450SE,16.4,8,275.8,180,3.07,4.07,17.4,0,0,3,3
##    2 Merc 450SL,17.3,8,275.8,180,3.07,3.73,17.6,0,0,3,3
##    2 Merc 450SLC,15.2,8,275.8,180,3.07,3.78,18,0,0,3,3
##    2 Pontiac Firebird,19.2,8,400,175,3.08,3.845,17.05,0,0,3,2
##    2 Porsche 914-2,26,4,120.3,91,4.43,2.14,16.7,0,1,5,2
##    2 Toyota Corolla,33.9,4,71.1,65,4.22,1.835,19.9,1,1,4,1
##    2 Toyota Corona,21.5,4,120.1,97,3.7,2.465,20.01,1,0,3,1
##    2 Valiant,18.1,6,225,105,2.76,3.46,20.22,1,0,3,1
##    2 Volvo 142E,21.4,4,121,109,4.11,2.78,18.6,1,1,4,2
##    2 model,mpg,cyl,disp,hp,drat,wt,qsec,vs,am,gear,carb
```

## Process arguments in a script

In order to have more complex scripts with more than one input you can use the syntax of `$n`; where n is the number of the argument. Note that an order is applied. The arguments in a script are processed in the same order in which they are sent. So if your script and command are as such then expect the following:


```bash
echo '{
    echo "Username: $1";
    echo "Age: $2";
}' > playground/script_2.sh
```



```bash
bash playground/script_2.sh 28 John

bash playground/script_2.sh John 28
```

```
## Username: 28
## Age: John
## Username: John
## Age: 28
```

## Special arguments array from script

When passing multiple arguments to a script - these are saved to a `ARGV` variable. This has some special properties and can even be printed out.

- `$@`/`$*` prints out all arguments array.
- `$#` prints the length of the array of arguments

## Streams in BASH

There are three kinds of streams of data in BASH:

1. `STDIN` standard input going into a programme.
1. `STDOUT` standard output going out of a programme.
1. `STDERR` standard errors from the programme.

Sometimes in a programme you can see the following lines at the end of a script:


```bash
2> /dev/null # redirects STDERR to be suppressed
1> /dev/null # STDOUT is being suppressed
```



```bash
cat data/iris.csv 1> playground/std-output-redirection.txt # redirects the standard output to a file
cat playground/std-output-redirection.txt | head -n 5
```

```
## "sepal.length","sepal.width","petal.length","petal.width","variety"
## 5.1,3.5,1.4,.2,"Setosa"
## 4.9,3,1.4,.2,"Setosa"
## 4.7,3.2,1.3,.2,"Setosa"
## 4.6,3.1,1.5,.2,"Setosa"
```

## Arithmetic and numbers in BASH

Numbers are not natively supported in BASH; in R or JS for example you could type something like: `3 + 2` in a console and the arithmetic would return a result. In BASH this does not work. 

In order to do arithmetic you can use the utility `expr` with the following syntax: `expr 2 + 2`.

Note that `expr` cannot handle decimals. In order to solve this you could instead use `bc`, it invokes an interactive calculator. However you can pipe values to it and use it in a similar way to `expr` but with decimals: 


```bash
expr 1 + 6.2
```

```
## expr: not a decimal number: '6.2'
```


```bash
echo "1 + 6.2" | bc
```

```
## 7.2
```

Note `bc` also takes an argument for stipulating how many decimal places to return - `scale`. By default it returns whole numbers rounding.

When using numbers for example assigning them to a variable name be sure not to assign them as a string with quotes. Keep them as numbers in order to be able to use them as number types later on.

*You might see some people use a double paranethesis method for invoking `expr`: `$((1 + 2))`.*


## Objects in BASH: storing variables

In order to create a variable use the following syntax. Note you set the variable with no spaces between the variable name and the value: `var_name=value`. Then you can call the variable by use of a dollar sign `$` as such:


```bash
test="A value or file input of some sort."
echo $test
```

```
## A value or file input of some sort.
```

## Objects in BASH: arrays

There are two types of arrays in BASH: numerical-indexed arrays or list with key value pairs.

### Numerical-indexed array

There are two ways to create a numerical-indexed arrays:


```bash
declare -a my_array # creates an empty array
my_array=(1 2 3) # creates an array of 1, 2, 3

echo ${my_array[@]}
```

```
## 1 2 3
```

Note the separation with spaces, *not* commas.

In order to print you array you can print all elements by use of the `{array[@]}` syntax. Like so:


```bash
declare -a my_array
my_array=(1 2 3)

echo ${my_array[@]} # prints all elements
echo ${#my_array[@]} # prints the length 
```

```
## 1 2 3
## 3
```

In order to access elements of array use `[]` brackets. As stated before BASH is 0 indexed. Elements can be changed by using the bracket notation and setting the value.


```bash
declare -a my_array
my_array=(1 2 3)

echo ${my_array[2]} # prints 3

my_array[1]=10
echo ${my_array[@]}
```

```
## 3
## 1 10 3
```

**Again please note that BASH is often 0 indexed, but MacOS's recent implementation of `zsh` is indeed 1 indexed.**

In order to slice a part of an array or get things in a certain interval use the syntax: `array[@]:n:m`.


```bash
declare -a my_array
my_array=(1 2 3 4 5 6 7 8 9 10)

echo ${my_array[@]:2:5}
```

```
## 3 4 5 6 7
```

Note that `m` is the number of elements to return after the starting index `n`.

Appending to an array uses the operator `+=`:


```bash
declare -a my_array
my_array=(1 2 3 4 5 6 7 8 9)
my_array+=(10) # note the use of parenthesis

echo ${my_array[@]}

my_array+=10 # not using parenthesis yields different results

echo ${my_array[@]}
```

```
## 1 2 3 4 5 6 7 8 9 10
## 110 2 3 4 5 6 7 8 9 10
```

Note the difference in results; however, this does not occur in `zsh`. In `zsh` you get the same result as when using parenthesis: `1 2 3 4 5 6 7 8 9 10`.

Here is a quick example using ararys, backticks (SHELL in SHELL), pipes, and arithmetic all in one:


```bash
declare -a my_array_a
my_array_a=(1 2 3 4 5 6 7 8 9 10)
declare -a my_array_b
my_array_b=(10 11 12 13 14 15 16 17 18 19 20)

declare -a my_array_final=`echo "scale=3; (${my_array_a[1]} + ${my_array_b[2]}) + ${my_array_b[3]} / 3" | bc`

echo ${my_array_final[@]}
```

```
## 18.333
```

### Associative arrays; key-value paired

*Note this feature is only available in BASH v4+.*


```bash
bash --version
```

```
## GNU bash, version 5.1.4(1)-release (x86_64-apple-darwin19.6.0)
## Copyright (C) 2020 Free Software Foundation, Inc.
## License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
## 
## This is free software; you are free to change and redistribute it.
## There is NO WARRANTY, to the extent permitted by law.
```

To create an associative array you must use the `declare` syntax. Either declare an empty array first and then add key-values or declare with key-value all at once.

Set keys with square brackets `[]` and values after the equals `=` sign. Multiple elements at once can be declared.


```bash
declare -A friends_list # declare first
friends_list_2_step=([name]="Guy" [age]=21) # add elements
echo ${friends_list_2_step[@]}
```

```
## bash: line 0: declare: -A: invalid option
## declare: usage: declare [-afFirtx] [-p] [name[=value] ...]
## 21
```



```bash
declare -A friends_list_1_step=([name]="Guy" [age]=21) # all at once
echo ${friends_list_1_step[@]}
```

```
## bash: line 0: declare: -A: invalid option
## declare: usage: declare [-afFirtx] [-p] [name[=value] ...]
## 21
```

Associative arrays' keys can all be got with the following syntax `!`:


```bash
declare -A friends_list_1_step=([name]="Guy" [age]=21) # all at once
echo ${!friends_list[@]} # return all keys
```

```
## bash: line 0: declare: -A: invalid option
## declare: usage: declare [-afFirtx] [-p] [name[=value] ...]
```

## `for` loops

The structure and syntax of a for loop in BASH is as follows:


```bash
for item in list1 list2 list3; do echo $item; done

for item in playground/*; do ls -l $item; done
```

```
## list1
## list2
## list3
## total 0
## -rw-r--r--  1 work  staff  0 Mar 15 20:08 copy.txt
## -rw-r--r--  1 work  staff  0 Mar 15 20:08 original.txt
## -rw-r--r--  1 work  staff  0 Mar 15 20:08 playground/copy.txt
## -rw-r--r--  1 work  staff  3400 Mar 15 20:08 playground/duplicated.csv
## -rw-r--r--  1 work  staff  682 Mar 15 20:08 playground/list-of-files.txt
## -rw-r--r--  1 work  staff  0 Mar 15 20:08 playground/original.txt
## -rwxr--r--  1 work  staff  36 Mar 15 20:08 playground/script_0.sh
## -rw-r--r--  1 work  staff  18 Mar 15 20:08 playground/script_1.sh
## -rw-r--r--  1 work  staff  49 Mar 15 20:08 playground/script_2.sh
## -rw-r--r--  1 work  staff  3975 Mar 15 20:08 playground/std-output-redirection.txt
## -rwxr--r--  1 work  admin  0 Mar 15 20:08 playground/test_permissions.txt
```

Note that a variable is defined and then called.

A multi-line syntax for `for` loops in a script is as follows:


```bash
for item in playground/* # this * syntax explained later
do
    echo "Testing: $item"
done
```

```
## Testing: playground/backupdir
## Testing: playground/copy.txt
## Testing: playground/duplicated.csv
## Testing: playground/list-of-files.txt
## Testing: playground/original.txt
## Testing: playground/script_0.sh
## Testing: playground/script_1.sh
## Testing: playground/script_2.sh
## Testing: playground/std-output-redirection.txt
## Testing: playground/test_permissions.txt
```

To create a numeric range to iterate through you can use something called brace extension. This is done with curly brackets: `{start..stop..increment}` by default this increments by 1 if you do not specify the third argument.

You can also use three expression syntax using parenthesis: `((x=2;x<=10;x+=3))`. This might seem very familiar if you are experiences with `JavaScript`.


```bash
# brace extensions
for x in {1..10..3}
do
    echo $x
done

# three expression
for ((x=2;x<=10;x+=3))
do
    echo $x
done
```

```
## {1..10..3}
## 2
## 5
## 8
```

You can also use something called glob expansions to iterate in place, this uses the `*` asterisk. This can be used for example to read in all files from a directory at once:


```bash
# read in all files from directory
for file in playground/*\.txt # only matches .txt files
do
    cat $file | head -n 5
done
```

```
## total 136
## drwxr-xr-x  12 work  staff    384 Mar 15 20:08 .
## drwxr-xr-x  13 work  staff    416 Mar 15 19:59 ..
## -rw-r--r--@  1 work  staff   6148 Mar 15 20:02 .DS_Store
## -rw-r--r--   1 work  staff     58 Mar 15 19:41 .Rhistory
## "sepal.length","sepal.width","petal.length","petal.width","variety"
## 5.1,3.5,1.4,.2,"Setosa"
## 4.9,3,1.4,.2,"Setosa"
## 4.7,3.2,1.3,.2,"Setosa"
## 4.6,3.1,1.5,.2,"Setosa"
```

A nested SHELL cna also be used to create a `for` loop. This can be useful for matching specific files on a condition or otherwise then passing the result to the `for` above loop for more work:


```bash
for file in `ls playground/ | grep -i 'duplicated'`
do
    echo $file
done
```

```
## duplicated.csv
```

## `while` loops

These are similar to `for` loops but they have a condition that is tested at each iteration. This is useful if you have a variable that is being modified each loop that needs to be checked. Iterations continue until the conidition is no longer met:


```bash
x=1
while [ $x -le 10 ]
do
    echo $x
    ((x+=1))
done
```

```
## 1
## 2
## 3
## 4
## 5
## 6
## 7
## 8
## 9
## 10
```

**BEWARE OF INFINITE LOOPS; MODIFY YOUR CONDITIONS INSIDE WHILE LOOPS**

## `if` statements and conditional flow

An if statement can be written with the following syntax; note that when using a numerical condition use double parenthesis `(())` or you can use `[]` brackets with a flag:


```bash
if [ boolean_condition ]; then # spaces!!! [ logical ];
    # CODE
else
    # OTHER CODE
fi # finishes an if statement
```



```bash
var="Yes"
if [ $var == "No" ]; then
    echo "You are authorised."
else
    echo "You may not proceed."
fi

var=20
if (( $var > 10 )); then
    echo "It is larger."
else
    echo "It is smaller."
fi

var=20
if [ $var -gt 10 ]; then
    echo "It is larger."
else
    echo "It is smaller."
fi
```

```
## You may not proceed.
## It is larger.
## It is larger.
```

The possible flags for use as comparators are:

- -eq equal to.
- -ne not equal to.
- -lt less than.
- -le less than or equal to.
- -gt greater than.
- -ge greater than or equal to.

You can also file related flags for checking things in the system:

- -e does a file exist.
- -s if the file has a size larger than 0.
- -r if the file is readable.
- -w if the file is writable.

You can use multiple conditions with and/or notations:

- `&&`
- `||`

Here is a demonstration of some chained conditions; note you can also use a double bracket syntax for a more simple expression:


```bash
var=1
if [ $var -gt 1 ] && [ playground/file.txt -w ]; then
    echo "Memory states greater than 1 with a value of $var \n and the files are writable."
fi

if [[ 2 -gt 1 && playground/file.txt -w ]]; then
    echo "Memory states greater than 1 with a value of $var \n and the files are writable."
fi

# you can also use other CLI programmes inside
if grep -q expression file.txt; then
    echo "Expression found inside."
fi

# a SHELL can be used inside the statement too
if `grep -q expression file.txt`; then
    echo "Expression found inside."
fi
```

## `case` statements

Case statements are light in syntax than `if` statements. This is often useful when you have multiple conditions and actions to take. These can technically be done with `if` statements but case statements are useful.


```bash
# case statement
case "string_or_var" in # pass string or variable; even a nested SHELL
    CONDITION_1)
    code_command;;
    CONDITION_2)
    code_command;;
    *)
    DEFAULT_CONDITION code_command;;
esac # case backwards
```

Here is an example demonstrating an `if` statement refactored as a `case` statement:


```bash
# complicated if statement
if grep -q "House Cat" $1; then
    mv $1 mammal/
fi
if grep -q "Crocodile" $1; then
    mv $1 reptile/
fi
if grep -q "Bald Eagle" $1; then
    mv $1 bird/
fi

# case statement
case `cat $1` in
    "House Cat")
    mv $1 mammal/;;
    "Crocodile")
    mv $1 reptile/;;
    "Bald Eagle")
    mv $1 bird/;;
    *)
    mv $1 other/;;
esac
```



```bash
# case statement matching days of week
case "Tuesday" in
  # weekdays
  Monday|Tuesday|Wednesday|Thursday|Friday)
  echo "Weekday";;
  # weekend
  Saturday|Sunday)
  echo "Weekend";;
  # default
  *)
  echo "Unrecognised";;
esac
```

```
## Weekday
```

## `function`s in BASH

Functions allow you to write code that can be re-used - modular code. There are two main syntax for functions in BASH as follows:


```bash
func_name () {
    # code goes here
    return # something
}

function func_name () { # parenthesis optional here
    # code goes here
    return # something
}
```

In order to pass arguments into functions is very simple. Use the dollar `$` sign:

The same system as shown above applies:

- `$1 ... $n`
- `$@`/`$*` arguments in `ARGV`.
- `$#` number of elements.

Note that when building functions you must take into consideration the scope of your code; local and global. All variables in BASH are global by default. Beware as this might be unfamiliar.

To restrict a scope use the `local` keyword:


```bash
function a_func () {
    local var="Something" # restrict to local scope
}
```

### `return` keyword

In BASH the function returns based on if the function was successfully executed `O` or not `1-255`. This is captured in the global variable `$?`.

We have some options:

1. Assign results to a global variable.
1. `echo` in the last line and capture it in a variable using a nested SHELL.


```bash
function func {
    fake_command # this should err
}

func # called
echo $? # print the err returned

# bash: fake_command: command not found
echo $? # print the err returned
127 # 127 means the code called does not exist

# return with a nested SHELL
function func_2 {
    echo `echo "scale=2; ($1 - $2) / 2 * 100" | bc`
}

saved_var=`func_2 21 5`
echo "The result is $saved_var"
```

```
## bash: line 1: fake_command: command not found
## 127
## 0
## bash: line 9: 127: command not found
## The result is 800.00
```

# Automation and `cron` jobs

You can schedule a script to be run on a schedule. This is done through a utility called `cron`. This helps to optimise your time and resources. The use `cron` is an essential skill.

`cron` has been a part of the `unix` systems since about the 70's. The name cron comes from the Greek "chronos".

It works with something called a `crontab`. This is a file that contains jobs called `cronjobs` which directs the programme as to what to run and when.

Call `crontab -l` in order to see what jobs are schedule by the user.

You can get an idea of how `cronjobs` work by the following structure:


```txt
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * <command to execute>
```

*Source: [wikipedia - cron](https://en.wikipedia.org/wiki/Cron)*

**Note in some `unix` systems Sunday can be designated as 0 not 7.**

In a `crontab` file you have many jobs - one per line. The starts are one for each unit of time: min, hour, day of month, month, day of the week. Note that you replace a placeholder start with what values you desire. Here is a quick example:


```bash
15 2 * * * bash script.sh
```

The minutes star is set to fifteen, this means fifteen minutes after the hour has set 0. Hours star is set to two, this means the second hour after 0 *eg.* 0200 (2 a.m.) The last are not set and left to stars so that means every day and month.

This means that the job runs every day at 0215 (2:15 a.m.).

You can run programmes at `t` time intervals:


```bash
10,20,40 * * * * bash script.sh
```

The above job would run at 10, 20, and 40 minutes every hour. You can also use a `/` for every `t` increment.


```bash
*/5 * * * * bash script.sh
```

The above would run every five minutes every day every hour.

## To schedule a `cron` job

1. Use the command `crontab -e` to edit the `cronjobs` list.
1. Create the job's command: `*/5 * * * * bash script.sh`.

To check that the job is scheduled and running check with `crontab -l`.


# Note to MacOS users

After around v3.0 of BASH Mac switched to using `zsh`. This because of licencing issues. If you wish to update your BASH to v4+ then see the following stackoverflow post: [stackoverflow](https://apple.stackexchange.com/questions/193411/update-bash-to-version-4-0-on-osx).

# Note to .Rmd MacOS users

In order to change the version that is used of BASH in R-Studio you can change the path in the preferences: `CMD + ,`. However, if you wish to change the version that is used in the chunks inside inline execution and when kiting you need to use the following guide found here: [bookdown](https://bookdown.org/yihui/rmarkdown-cookbook/eng-bash.html).

Change BASH version inside R-Studio chunks explained:

1. Create a `.bash_profile` file.
1. Export the path with the following syntax inside this file: `export PATH="$PATH:/usr/local/bin/bash"`.
1. Use the `engine.opts='-l'` in your chunk as shown below:


```bash
bash --version
```

```
## GNU bash, version 5.1.4(1)-release (x86_64-apple-darwin19.6.0)
## Copyright (C) 2020 Free Software Foundation, Inc.
## License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
## 
## This is free software; you are free to change and redistribute it.
## There is NO WARRANTY, to the extent permitted by law.
```

# Data processing with BASH

In this section we cover a practical approach to simple yet powerful data specific processing skills. We will cover things from downloading data, transforming, cleaning, and even SQL CLI database commands. 

## Downloading data

We will start with using `CURL`. `CURL` stands for Client for URLs. It is a UNIX based CLI tool to transfer data to and from servers; can be used to download from http, https, ftp, sftp.

It is a utility, if you do not have it installed try using the `brew` package manager: `brew install curl`.

The syntax to use curl is: `curl -options URL`.


```bash
curl -o playground/iris_download.csv https://gist.githubusercontent.com/netj/8836201/raw/6f9306ad21398ea43cba4f7d537619d0e07d5ae3/iris.csv
```

- `-O` saves the file with the original name.
- `-o` saves the file with the argument given.

Note that you can also download multiple files with a wildcard: `https://sitename.com/path/*\.txt`. Or you can use a glob: 


```bash
curl -O https://sitename.com/path/filename[01-20]*\.txt
```

# Exercise: gene sequence processing pipeline

Let's start by downloading a gene file from rscb.org. Chose a gene any really and download the fasta file.

<p align="center">
    <img src="build-Rmd/figures/4-download-fasta.png">
</p>

We will be working with the following files:


```bash
cat data/rcsb_pdb_6CLZ.fasta | head
```

```
## >6CLZ_1|Chain A|Matrix metalloproteinase-14|Homo sapiens (9606)
## PNICDGNFDTVAMLRGEMFVFKERWFWRVRNNQVMDGYPMPIGQFWRGLPASINTAYERKDGKFVFFKGDKHWVFDEASLEPGYPKHIKELGRGLPTDKIDAALFWMPNGKTYFFRGNKYYRFNEELRAVDSEYPKNIKVWEGIPESPRGSFMGSDEVFTYFYKGNKYWKFNNQKLKVEPGYPKSALRDWMGCPSG
## >6CLZ_2|Chains B,C|Apolipoprotein A-I|Homo sapiens (9606)
## STFSKLREQLGPVTQEFWDNLEKETEGLRQEMSKDLEEVKAKVQPYLDDFQKKWQEEMELYRQKVEPYLDDFQKKWQEEMELYRQKVEPLRAELQEGARQKLHELQEKLSPLGEEMRDRARAHVDALRTHLAPYSDELRQRLAARLEALKENGGARLAEYHAKATEHLSTLSEKAKPALEDLRQGLLPVLESFKVSFLSALEEYTKKLNTQ
```


```bash
cat data/rcsb_pdb_6E5B.fasta | head
```

```
## >6E5B_1|Chains A,O|Proteasome subunit alpha type-2|Homo sapiens (9606)
## MAERGYSFSLTTFSPSGKLVQIEYALAAVAGGAPSVGIKAANGVVLATEKKQKSILYDERSVHKVEPITKHIGLVYSGMGPDYRVLVHRARKLAQQYYLVYQEPIPTAQLVQRVASVMQEYTQSGGVRPFGVSLLICGWNEGRPYLFQSDPSGAYFAWKATAMGKNYVNGKTFLEKRYNEDLELEDAIHTAILTLKESFEGQMTEDNIEVGICNEAGFRRLTPTEVKDYLAAIA
## >6E5B_2|Chains B,P|Proteasome subunit alpha type-4|Homo sapiens (9606)
## MSRRYDSRTTIFSPEGRLYQVEYAMEAIGHAGTCLGILANDGVLLAAERRNIHKLLDEVFFSEKIYKLNEDMACSVAGITSDANVLTNELRLIAQRYLLQYQEPIPCEQLVTALCDIKQAYTQFGGKRPFGVSLLYIGWDKHYGFQLYQSDPSGNYGGWKATCIGNNSAAAVSMLKQDYKEGEMTLKSALALAIKVLNKTMDVSKLSAEKVEIATLTRENGKTVIRVLKQKEVEQLIKKHEEEEAKAEREKKEKEQKEKDK
## >6E5B_3|Chains C,Q|Proteasome subunit alpha type-7|Homo sapiens (9606)
## MSYDRAITVFSPDGHLFQVEYAQEAVKKGSTAVGVRGRDIVVLGVEKKSVAKLQDERTVRKICALDDNVCMAFAGLTADARIVINRARVECQSHRLTVEDPVTVEYITRYIASLKQRYTQSNGRRPFGISALIVGFDFDGTPRLYQTDPSGTYHAWKANAIGRGAKSVREFLEKNYTDEAIETDDLTIKLVIKALLEVVQSGGKNIELAVMRRDQSLKILNPEEIEKYVAEIEKEKEENEKKKQKKAS
## >6E5B_4|Chains D,R|Proteasome subunit alpha type-5|Homo sapiens (9606)
## MFLTRSEYDRGVNTFSPEGRLFQVEYAIEAIKLGSTAIGIQTSEGVCLAVEKRITSPLMEPSSIEKIVEIDAHIGCAMSGLIADAKTLIDKARVETQNHWFTYNETMTVESVTQAVSNLALQFGEEDADPGAMSRPFGVALLFGGVDEKGPQLFHMDPSGTFVQCDARAIGSASEGAQSSLQEVYHKSMTLKEAIKSSLIILKQVMEEKLNATNIELATVQPGQNFHMFTKEELEEVIKDI
## >6E5B_5|Chains E,S|Proteasome subunit alpha type-1|Homo sapiens (9606)
## MFRNQYDNDVTVWSPQGRIHQIEYAMEAVKQGSATVGLKSKTHAVLVALKRAQSELAAHQKKILHVDNHIGISIAGLTADARLLCNFMRQECLDSRFVFDRPLPVSRLVSLIGSKTQIPTQRYGRRPYGVGLLIAGYDDMGPHIFQTCPSANYFDCRAMSIGARSQSARTYLERHMSEFMECNLNELVKHGLRALRETLPAEQDLTTKNVSIGIVGKDLEFTIYDDDDVSPFLEGLEERPQRKAQPAQPADEPAEKADEPMEH
```


```bash
cat data/SC.gtf | head
```

```
## #!genome-build R64-1-1
## #!genome-version R64-1-1
## #!genome-date 2011-09
## #!genome-build-accession GCA_000146045.2
## #!genebuild-last-updated 2011-12
## IV	SGD	gene	1802	2953	.	+	.	gene_id "YDL248W"; gene_name "COS7"; gene_source "SGD"; gene_biotype "protein_coding";
## IV	SGD	transcript	1802	2953	.	+	.	gene_id "YDL248W"; transcript_id "YDL248W"; gene_name "COS7"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "SGD"; transcript_biotype "protein_coding";
## IV	SGD	exon	1802	2953	.	+	.	gene_id "YDL248W"; transcript_id "YDL248W"; exon_number "1"; gene_name "COS7"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "SGD"; transcript_biotype "protein_coding"; exon_id "YDL248W.1";
## IV	SGD	CDS	1802	2950	.	+	0	gene_id "YDL248W"; transcript_id "YDL248W"; exon_number "1"; gene_name "COS7"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "SGD"; transcript_biotype "protein_coding"; protein_id "YDL248W";
## IV	SGD	start_codon	1802	1804	.	+	0	gene_id "YDL248W"; transcript_id "YDL248W"; exon_number "1"; gene_name "COS7"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "SGD"; transcript_biotype "protein_coding";
```

## Refresher: commands and regex

Here are some commands we will be working with:

- `cat file1 file2` prints the contents of the file(s).
- `head -5 file` prints the first n (eg 5) lines of a file.
- `tail -5 file` print the last n (eg 5) lines of a file.
- `tail -n +5 file` skip the 5 first lines of a file and prints the rest.
- `wc -l file` print the number of lines in a file.
- `wc -c file` print the number of characters in a file.
- `zcat or gzcat file.gz` prints the content of zipped files.
- `less file` browse a file page by page.

Here is some review on regular expressions:

This is the syntax for using this grep command:
 
```... | grep options pattern | ...```
 
- Important options
	- `-P` Allows Perl style and special characters
	- `-v` Negative grep, exludes lines with the pattern given
 
- Some patterns
	- `text` Lines containing "text".
	- `^text` Lines starting with "text".
	- `text$` Lines finishing by "text".
	- `text.abc` Lines containing "text" other characters, then "abc".
	- `[axz]` Lines containing characters, a, x, or z.
	- `[a-z]` Lines containing lower case characters.
	- `[^0-9]` Lines not containing numerical characters.

## Processing through UNIX pipeline

This means we can chain commands together with a pipe character `|`. In this example we take our file and reformat it, then pipe its output to a new file called: "newfile.txt.gz", as we a re compressing it with `gzip`.


```bash
cat data/rcsb_pdb_6E5B.fasta | tail -n +1 | cut -c4- | sort -k1 | uniq | awk '{print $1 "\t" $4}' | head

cat data/rcsb_pdb_6E5B.fasta | tail -n +1 | cut -c4- | sort -k1 | uniq | awk '{print $1 "\t" $4}' | gzip > playground/newfile.txt.gz
```

```
## 5B_10|Chains	beta
## 5B_11|Chains	beta
## 5B_12|Chains	beta
## 5B_13|Chains	beta
## 5B_14|Chains	beta
## 5B_1|Chains	alpha
## 5B_2|Chains	alpha
## 5B_3|Chains	alpha
## 5B_4|Chains	alpha
## 5B_5|Chains	alpha
```

Here is part of our file printed out, we wanted just the transcripts. To print just these we used a regular expression with the command `grep`.

Here is a command plus some review on regex:


```bash
cat data/SC.gtf | grep -e "SGD\ttranscript" | head -5
```

```
## IV	SGD	transcript	1802	2953	.	+	.	gene_id "YDL248W"; transcript_id "YDL248W"; gene_name "COS7"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "COS7"; transcript_source "SGD"; transcript_biotype "protein_coding";
## IV	SGD	transcript	3762	3836	.	+	.	gene_id "YDL247W-A"; transcript_id "YDL247W-A"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_source "SGD"; transcript_biotype "protein_coding";
## IV	SGD	transcript	5985	7814	.	+	.	gene_id "YDL247W"; transcript_id "YDL247W"; gene_name "MPH2"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "MPH2"; transcript_source "SGD"; transcript_biotype "protein_coding";
## IV	SGD	transcript	8683	9756	.	-	.	gene_id "YDL246C"; transcript_id "YDL246C"; gene_name "SOR2"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "SOR2"; transcript_source "SGD"; transcript_biotype "protein_coding";
## IV	SGD	transcript	11657	13360	.	-	.	gene_id "YDL245C"; transcript_id "YDL245C"; gene_name "HXT15"; gene_source "SGD"; gene_biotype "protein_coding"; transcript_name "HXT15"; transcript_source "SGD"; transcript_biotype "protein_coding";
```


**Note that when ^ is used inside the [] brackets it means excludes whatever is inside.**

Let's try another command and try to get back just select columns from our file.


```bash
cat data/SC.gtf | grep -e "transcript\t" | cut -f 1,4,5,6,10 | head
```

```
## IV	1802	2953	.
## IV	3762	3836	.
## IV	5985	7814	.
## IV	8683	9756	.
## IV	11657	13360	.
## IV	16204	17226	.
## IV	17577	18566	.
## IV	18959	19312	.
## IV	20635	21006	.
## IV	22471	22608	.
```

Now we'll start trying to change the format of our file just to practice our commands. We will invert the order of two of the columns and add an empty column as well:


```bash
cat data/SC.gtf | grep -e "transcript\t" | awk '{print $1"\t"$4"\t"$5"\t"$10"\t0\t"$7}' | head
```

```
## IV	1802	2953	"YDL248W";	0	+
## IV	3762	3836	"YDL247W-A";	0	+
## IV	5985	7814	"YDL247W";	0	+
## IV	8683	9756	"YDL246C";	0	-
## IV	11657	13360	"YDL245C";	0	-
## IV	16204	17226	"YDL244W";	0	+
## IV	17577	18566	"YDL243C";	0	-
## IV	18959	19312	"YDL242W";	0	+
## IV	20635	21006	"YDL241W";	0	+
## IV	22471	22608	"YDL240C-A";	0	-
```

Now let's try and replace an element with another. We'll use the `sed` command. We'll try to substitute both semi-colon and quotes with nothing.

So we're using the sed command coupled with the regular expression: `s/[;\*]//g` between simple quotes `''`. The g is for global at the end, if not included then it would only work on the first instance.

Compare the results to the above print:


```bash
cat data/SC.gtf | grep -e "transcript\t" | awk '{print $1"\t"$4"\t"$5"\t"$10"\t0\t"$7}' | sed 's/[;\*]//g' | sed 's/["\*]//g' | head
```

```
## IV	1802	2953	YDL248W	0	+
## IV	3762	3836	YDL247W-A	0	+
## IV	5985	7814	YDL247W	0	+
## IV	8683	9756	YDL246C	0	-
## IV	11657	13360	YDL245C	0	-
## IV	16204	17226	YDL244W	0	+
## IV	17577	18566	YDL243C	0	-
## IV	18959	19312	YDL242W	0	+
## IV	20635	21006	YDL241W	0	+
## IV	22471	22608	YDL240C-A	0	-
```


Finally let's sort our outputs. Sorted the data by the numerical order of the second column. Command was sort `-k 2`. To sort by multiple criteria, we just repeat the `-k` option for sort and give it another argument.


```bash
cat data/SC.gtf | grep -e "transcript\t" | awk '{print $1"\t"$4"\t"$5"\t"$10"\t0\t"$7}' | sed 's/[;\*]//g' | sed 's/["\*]//g' | sort -k 2  | head
```

```
## XV	1000	1338	YOL166C	0	-
## IV	1000104	1002023	YDR266C	0	-
## XII	1000342	1001703	YLR431C	0	-
## VII	1000927	1002240	YGR254W	0	+
## XV	1001147	1003225	YOR354C	0	-
## V	100133	100204	tM(CAU)E	0	+
## I	100225	101145	YAL025C	0	-
## IV	1002510	1003502	YDR267C	0	-
## VI	100252	100605	YFL019C	0	-
## VII	1002523	1003962	YGR255C	0	-
```

