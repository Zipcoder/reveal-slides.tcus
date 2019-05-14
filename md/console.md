#Introduction to the Terminal

Aka console, CLI, command line, shell, etc.

-
-

## What we'll cover

<p class="fragment fade-up">Your Shell Terminal</p>
<p class="fragment fade-up">Terminology</p>
<p class="fragment fade-up">Useful commands for navigating and manipulating your file system</p>

-
-
##Some terminology

- Command Line Interface (CLI) - the text-based interface of a computer
- Console - See Command Line Interface
- Terminal - See Console
- Shell - See Terminal*

*A shell is the interactive program that runs on the terminal, but the word is often used interchangeably to refer to both.

-
## Some more terminology
`*nix` - any Unix or Linux operating system. Examples:

- Solaris (Unix)
- Ubuntu (Linux)
- Red Hat (Linux)
- BSD (Unix)
- Apple OS X (Unix)
- Android (Linux)
- Chrome OS (Linux)

-
-
## Getting to Terminal on a Mac

- Press `command` + `space`
- Type 'terminal' and hit `enter`

**OR**

- Open Applications
- Go to the Utilities folder
- Double-click Terminal

-
-
## Getting Familiar with the Prompt

-
### What the prompt looks like

Simple examples: `$`, `>`, `>>>` or `->`  
Highly customizable.  
Default OS X prompt:  
`Computername:directory username$` eg: `Valhalla:HallOfOdin thor$`  
Idioms: `/`= root; `~`=home

-
### Executing commands

Commands are executed when you hit enter  
Can chain commands with semicolons  
eg: `echo "hello"; echo "goodbye"`  
Obviously incomplete commands will provide intermediate prompt (>)

```
$ echo "hello
> world"
hello
world
```


-
-
# Navigating the file system

-
### File system structure

*nix file systems are a tree with a root named `/`  
Everything is within / or a descendant directory  
directories are delimited with / character  
This means we can list the exact position of a dir using its name (called an absolute path)  
eg: /usr/bin/, /dev/null, /Users/loki

-
### Getting your bearings (basic commands)

- pwd - Print Working Directory aka. "Where am I?"  
- ls - list directory contents
- cd - change directory (explained on next slide)


-
### Moving around

- cd - change directory  
  Must provide a destination as relative or absolute path  
  From home directory (aka `~` or `/Users/thor`)  
  change to Desktop (~/Desktop) using one of these commands:  
  `cd Desktop`  
  `cd /Users/Thor/Desktop`  
  `cd ~/Desktop`

<!--
#### Optional whiteboard navigation exercise

Ask students to speak commands to:

- find out the current directory
- list files
- get to a goal file
-->

-
### Notable directory names and idioms:

- `/` root directory
- `~` home directory
- `.` this dir
- `..` parent directory
- `-` used with cd to mean "go to where I just was" (`cd -` twice and you haven't moved)
- Go up a directory with `cd ..`
- Use these idioms to save time.  
  eg: from Desktop use `cd ../Documents` or `cd /etc/lib` rather than multiple `../` to get to root first

-
### Class Exercise

- In terminal, navigate to the highest level directory you can reach.
- Write down the name of that directory
- Navigate back to your home directory, list each directory along the way and how many items it contains

-
### Class Exercise 2

- Find the `bin` directory and list its contents
- Identify at least one item you recognize in `bin`
- Now find the `Applications` directory
- List the contents of the directory
- Open the directory in finder with `open .` to see the same list of files in finder

-
-
## Inspecting files and directories

-
#### File contents

- View file contents with `cat` or `less`
- use `grep` find specific pattern/text  
Patterns are defined by regular expressions (more on that in later lessons)

-
#### Permissions

`ls -l` shows file permissions

```
-rw-r--r--  1 thor  staff  0 Jan 10 16:04 file
-rw-r--r--  1 root  staff  0 Jan 10 16:04 rootFile
```

-
### Running commands as superuser (aka root)

`sudo` command - Do something as Superuser (requires password)

```bash
$ cat file.txt
cat: file.txt: Permission denied
$ sudo cat file.txt
this is a file
```

-
-
###Creating, copying, moving, renaming, deleting files and directories

-
### Three ways to create a file:

- `touch` command ( `touch newfile` )
- Using a text editor
- Redirect output to a file eg: `echo "hello" >file.txt`

-
###  Create directories with mkdir

- `mkdir dirname`
- Can use paths, but will not crate other dirs implicitly  
  eg: `mkdir dir1/dir2` fails if dir1 doesn't exist.

-
-
### Editing files

Many terminal-based editors out there. We recommend VIM (explained in console lab 2).  
Other options include:

- emacs - Popular and comparable to VIM in power
- nano - Very simple, easy for beginners but limited functionality

-
-
### The art of Redirection

Every command has access to at least three "file descriptors" (input and output sources). By default these are all routed to the command line

-
#### Standard file descriptors

- STDIN (fd 0) - Whatever you type
- STDOUT (fd 1) - Normal command output
- STDERR (fd 2) - Error output


-
#### Redirecting File Descriptors

File descriptors can be rerouted using these commands:

- `<` - Redirect STDIN to specified file.
- `>` - Redirects STDOUT to the specified file (if the file exists its contents are erased first)
- `2>` Same as `>` but for STDERR
-  `&>` - Redirects STDERR and STDOUT to the same place
- `>>` or `2>>` - Appends output to the end of the specified file instead of overwriting it
- `|` - redirects the output from the previous command to be the input of the next command.  
  Eg: `ps aux | grep bash`

-
### Backgrounding

Long running tasks can prevent use of the command prompt.  
Run a command in the background with the `&` character eg: `find / -name foo &`  
Send an already running foreground command to the background with `^z` `bg` (`^z` suspends the current job, `bg` resumes it in the background)  
It's often wise to redirect the output when you background something

-
#### Useful examples

- `python -m SimpleHTTPServer &>http.log &` - tiny local web server
