---
layout: default
title: Unix Shell Command
permalink: /shell
---

## Shortcut for Shell Command

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
Command	| Example 				| Description | 
---		| ---					| ---
`cat`	| `cat this_file.txt`	| print out the contents of the file `this_file.txt`.
`less`	| `less this_file.txt`	| print out the *few* contents of the file `this_file.txt`.
`head` 	| `head this_file.txt`	| display the first few lines of the file `this_file.txt`.
`head`  | `head -n 10 this_file.txt`| display the first **10** lines of the file `this_file.txt`.

### Work with data
Command	| Example 				| Description | 
---		| ---					| ---
`cut`	| `cut -d , -f 2 this_data.csv` | select the second **column** (**field**) of the data `this_data.csv` using **delimiter** `,`.
`grep`	| `grep hello this_data.csv`	| print out the lines that contain the word `hello` in the file `this_data.csv`.
`grep`	| `grep -v hello this_data.csv`	| print out the lines that **do not** contain the word `hello` in the file `this_data.csv`.
`grep`	| `grep -c hello this_data.csv`	| count how many lines that contain the word `hello` in the file `this_data.csv`.
`paste`	| `paste -d , this_data.csv that_data.csv` | combine `this_data.csv` with `that_data.csv` using **delimiter** `,`.

