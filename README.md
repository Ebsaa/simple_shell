0x16. C - Simple Shell

ALX  Simple_shell Team project

This is shell Team project at ALX SE to create a simple shell.
Our shell shall be called hsh

Project was covered using :- Shell, C-language & Betty linter

General Requirements

Allowed editors: vi, vim, emacs
~All files will be compiled on Ubuntu 20.04 LTS using gcc, using the options -Wall -Werror -Wextra -pedantic -std=gnu89
~All  files should end with a new line
~A README.md file, at the root of the folder of the project is mandatory
~ use the Betty style. It will be checked using betty-style.pl and betty-doc.pl
~shell should not have any memory leaks
~No more than 5 functions per file
~All header files should be include guarded
~Write a README with the description of the project
~ Have an AUTHORS file at the root of your repository

Description

hsh is a simple UNIX command language interpreter that reads commands from either a file or standard input and executes them

How hsh works

~Prints a prompt and waits for a command from the user
reates a child process in whic~Checks for built-ins, aliases in the PATH, and local executable programs
~The child process is replaced by the command, which accepts arguments
~When the command is done, the program returns to the parent process and prints the prompt
~The program is ready to receive a new command
~To exit: press Ctrl-D or enter "exit" (with or without a status)
~Works also in non interactive mode

Compilation

shell will be compiled by this way:

~gcc -Wall -Werror -Wextra -pedantic -std=gnu89 *.c -o hsh

Invocation
Usage: hsh [filename]

To invoke hsh, compile all .c files in the repository and run the resulting executable.

hsh can be invoked both interactively and non-interactively. If hsh is invoked with standard input not connected to a terminal
it reads and executes received commands in order.
Eg.
$ echo "echo 'hello'" | ./hsh
'hello'
$

If hsh is invoked with standard input connected to a terminal (determined by isatty (3)), an interactive shell is opened. when executing interactively
hsh displays the prompt $ when it is ready to read a command.
Eg.
$ ./hsh
$

Alternatively, if command line argument is inputted upon invocation, hsh treats the first argument as a file from which to read commands
The file should contain one command per line when supplied.
Each of the commands contained in the file is run by hsh before exiting.
Eg.
$ cat test
echo 'hello'
$ ./hsh test
'hello'
$

Environment
Upon, invocation hsh recieves and copies the environment of the parent process in which it was executed.
This environment is an array of name value strings describing variables in the format NAME = VALUE.
A few key environmental variables are:

HOME
The home directory of the current user  and the default directory argument for the cd builtin command.

$ echo "echo $HOME" | ./hsh
/home/projects

PWD

The printing(current) working directory as set by cd command.

$echo "echo $PWD" | ./hsh
/home/projects/alx/simple_shell

OLDPWD

The previous working directory as set by cd command.

$echo "echo $OLDPWD" | ./hsh
/home/projects/alx/printf

PATH

A colon separated list of directories in which the shell looks for commands. A null directory name in the path (represented by any of two adjacent colons
initial otr trialing colon) indicates the current directory.

$echo "echo $PATH" | ./hsh

Command execution
After receiving  a command hsh tokenizes it into words using " " as a delimiter.  The first word is considered the command & all remaining words are
considered arguments to that command.hsh then proceeds with the following actions:
1~. If the first char of the command is none of slash  ( \ ), dot (.), nor builtin, hsh searches each elements  of the  PATH environmentals variable
	for a directory containing an executable file by that name.
2~. If the first char of the command is neither a slash ( \ ) nor dot ( . ) the shell searches for it in the list of shell builtins.
	If there exists a builtin by that name the builtin is invoked.
3~. If the first char of the command is a slash ( \ ) or dot ( . ) or either of it the searches was successful & the shell executes
	the named programm with any remaining arguments in a separate execution enviroment.

Exit Status
hsh returns the exit status of the last command executed, with zero indicating success and non-zero indicating failure.

If a command is not found, the return status is 127; if a command is found but is not executable, the return status is 126.

All builtins return zero on success and one or two on incorrect usage

Signals

While running in interactive mode, hsh ignores the keyboard input ctrl+c. Alternatively an input of end-of-file ctrl+d will exit the program.
~ Hints ctrl+d in the third line
$ ./hsh
$ ^C
$ ^C
$

Variable Replacements

hsh interpretes the $ character for variable replacement

$ENV_VARIABLE

ENV_VARIABLE is substitued with its value.
Eg.
$echo "echo $PWD" | ./hsh
/home/projects/alx/simple_shell
$?

? is substituted with the return value of the last project executed.
Eg.
$ echo "echo $?" | ./hsh
0
$$

The second $ is substituted with the current process ID.
Eg.
$ echo "echo $$" | ./hsh
5656

Comments

hsh ignores all words and characters preceeded by a # character on a line

$ echo "echo 'hello' #this wil be ignored!" | ./hsh
'hello'

Operators

hsh specially interprets the following operator characters:
;- Command separator
Commands separated by a ; are executed sequentially.
Eg.
$ echo "echo 'hello'; echo 'world'" | ./hsh
'hello'
'world'

&& - AND Logical Operator

command1 && command2: command2 is executed if and only if command1 returns an exit status of zero.
Eg:
$ echo "error! && echo 'hello'" | ./hsh
./hsh: 1: error!: not found
$ echo "echo 'all good' && echo 'hello'" | ./hsh
'all good'
'hello'

|| - Logical Operator

command1 && command2: command2 is executed if and only if command1 returns non-zero exit status.
Eg.

$ echo "error! || echo 'but still runs'" | ./hsh
./hsh: 1: error!: not found
'but still runs'

The Operator s && and || have equal precedence followed by ;.

hsh Builtin Commands
cd
~Usage: cd [Directory]
~changes the current directory of the process to Directory.
~If no argument is given, the command is interpreted as cd $HOME
~If the argument - is given, the command is interpreted  as cd $OLDPWD and the path name of the new working Directory is printed to stdout.

Eg:

$ ./hsh
$ pwd
/home/projects/alx/simple_shell
$ cd ../
$ pwd
/home/projects/alx
$ cd -
$ pwd
/home/projects/alx/simple_shell

Alias

Usage: alias [NAME[='VALUE'] ...]
Handles aliases.
alias: Prints a list of all aliases, one per line, in the form NAME='VALUE'.
alias NAME [NAME2 ...]: Prints the aliases NAME, NAME2, etc. one per line, in the form NAME='VALUE'.
alias NAME='VALUE' [...]: Defines an alias for each NAME whose VALUE is given. If name is already an alias, its value is replaced with VALUE.

Exit
~Usage: exit [STATUS]
~Exit the shell
~The status argument is the integer used to exit the shell
~If no argument is given the command is interpreted as exit 0.
Eg:
$ ./hsh
$ exit

Env

~Usage: env
~ Prints the current environment
Eg:
$ ./hsh
$ env
NVM_DIR=/home/projects

setenv

~Usage: setenv [VARIABLE][VALUE]
~Initializes a new environmental variable or modifies an existing one
~upon failure prints message to stderr
Eg:
$ ./hsh
$ setenv NAME Poppy
$ echo $NAME
Poppy

unsetenv

Usage: unseten [VARIABLE]
~Removes an environmental variables
~Upon failure, prints a message to stderr

Eg:
$ ./hsh
$ setenv NAME Poppy
$ unsetenv NAME
$ echo $NAME

$

What we have learned from the project:

~How a shell works and finds commands
~Executing a program from another program
~Handling dynamic memory allocation in a large program
~Creating, forking and working with processes
~Building a test suite to check our own code
~Pair programming and team work

Authors:
 ~Ebsau Adem [ebsaumar200@gmail.com]
 ~Melese Assefa [getmesy@gmail.com]
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

