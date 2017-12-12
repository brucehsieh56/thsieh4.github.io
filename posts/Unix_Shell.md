---
layout: default
title: Unix Shell Command
permalink: /shell_command
---

<h2 style="text-align: center;">Unix Shell Command</h2>

### Basic Command
- `whoami` find out the user
- `pwd` print the current working directory	
- `ls`	list files
- `cd`	change directory
- `cd ..` move up a directory
- `cd ~` home directory

### Special Command

Command	| Description
---		| ---
`~`		| the shortcut for your home directory
`/tmp`	| the directory where you or your system keep the file temporary
`!` 	| re-run the most recent use of that command
`history`  | look at what command you have used


- `cp original.txt duplicate.txt` copy file
- `mv file_a.txt this_dir` move file `file_a` to directory `this_dir`
- `mv file_a.txt file_b.txt` rename the file `file_a.txt` to `file_b.txt`
- `rm file_a.txt` remove the file `file_a.txt`
- `rmdir this_dir` remove the directory `this_dir` when `this_dir` is empty
- `mkdir that_dir` creat a new directory `that_dir`


### Look at data

Command	| Example 				| Description
---		| ---					| ---
`cat`	| `cat file1.txt`		| print out the contents of the file `file1.txt`.
`less`	| `less file1.txt`		| print out the *few* contents of the file `file1.txt`.
`head` 	| `head file1.txt`		| display the first few lines of the file `file1.txt`.
`head`  | `head -n 10 file1.txt`| display the first **10** lines of the file `file1.txt`.


### Work with data

Command	| Example 					| Description
---		| ---						| ---
`cut`	| `cut -d , -f 2 data1.csv` | select the 2nd **column** (**field**) of `data1.csv` with **delimiter** `,`.
`grep`	| `grep hello data1.csv`	| print out the lines that contain the word `hello` in `data1.csv`.
`grep`	| `grep -v hello data1.csv`	| print out the lines that **do not** contain the word `hello` in `data1.csv`.
`grep`	| `grep -c hello data1.csv`	| count how many lines that contain the word `hello` in `data1.csv`.
`paste`	| `paste -d , data1.csv data2.csv` | combine `data1.csv` with `data2.csv` by inserting **delimiter** `,`.

