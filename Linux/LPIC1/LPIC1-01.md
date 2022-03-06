# Notes from Linux Professional Institute Certification Study Guide

## Linux Command-Line Tools

### Navigation in Shell

- Press Ctrl+A or Ctrl+E to move the cursor to the start or end of the line

### Environmental Var

- Use export to initialize a new env variable; to add this variable permanently add this line in .bashrc or .profile;
- *export* ENV=some_value

### Using Streams, Redirection, and Pipes

#### Redirecting Input and Output

- **&>** or **2>&1** Creates a new file containing both standard output and standard error.  If the specified file exists, it’s overwritten.
- **<>** Causes the specified file to be used for both standard input and standard output

### Processing Text Using Filters

#### Cat

- Display line ends ==> -E or --show-ends
- Number lines ==> -n or --number
- Minimize blank lines ===> -s or --squeeze-blank
- Display special characters The -T or --show-tabs

#### Join

> *-1* and *-2* associated with N-th felids to join with
> i.e. join -1 2 -2 3 means compare first file 2nd field with second file 3rd field

#### Paste

> merges files line by line and separates them with tabs

#### expand and unexpand

> convert tabs to spaces and vice versa with unexpand

- -t nums or --tabs=nums  *tab spacing value*

#### od

> shows octal ,hex, dec and other format of a file

#### sort

- Ignore case ===> -i or --ignore-case
- Month sort ===> -M or --month-sort
- Numeric sort ===> -n
- Reverse sort order ===> -r or --reverse
- Sort field ===> -k field or --key=field

#### split

- Split by bytes ===> -b size or --bytes=size
- Split by number of lines ===> -l lines or --lines=lines
- Split by bytes in line-sized chunks ===> -C=size or --line-bytes=size

#### tr [options] SET1 SET2

- The -t or --truncate-set1 option causes tr to truncate SET1 to the size of SET2 instead.
- -d, which causes the program to delete the characters from SET1
- shortcuts like [:alnum:] (all numbers and letters), [:upper:] (all uppercase letters), [:lower:] (all lowercase letters), and [:digit:] (all digits).

#### uniq

> removes repeated lines *(use with sort)*

### File-Formatting Commands

- fmt *Reformat Paragraphs*
- nl *Number Lines*
- pr *Prepare a File for Printing*

### File-Viewing

- head and tail (-c for number of *bytes or chars* and -n for number of *lines*)
- *tail -f* is tracks file latest changes ---> **Extremely useful**
- less and more (*only* in less you can use h for showing help)

### File-Summarizing

#### cut

- By byte ==>  -b list or --bytes=list (*same as -c*)
- By field ==> -f list or --fields=list , Note: **must be used by -d**
- -d or --delim=char for specify delimiter, don't forget about *-f*

#### wc

> output format  [lines] [words] [bytes/chars]

- -l ==> lines
- -w ==> words
- -c ==> chars

### Regex

- *Bracket expressions*: choose one of elements in brackets.
    > (for instance: b[aeiou]g ==> bag, beg, big, bog, bug)
- *Range expressions*: range expressions list the start and end points separated by a dash.
    > (for instance: a[f-u]b ==> afb, agb, ...)
- *Any single character*: The dot (.) represents any single character except for a *newline*.
    > (for instance: b.g means any 3 letter word).
- *Start and end of line*: The carat *(^)* represents the start of a line and the dollar sign *($)* denotes the end of a line.
    > (for instance: ^include means it must start with include)
- *Repetition operators*: *(\*)* denotes zero or more occurrences, a plus sign *(+)* matches one or more occurrences, and a question mark *(?)* specifies zero or one match
    > (for instance: a?b.* means a(some char)b.(any extension))
- *Multiple possible string*: The vertical bar (|) separates two possible matches.
    > (for instance: car|truck matches either car or truck).
- *Parentheses*: Ordinary parentheses () surround subexpressions.
- *Escaping*: If you want to match one of the special characters, such as a dot precede it with a backslash (\).
    > (for instance: to match a computer hostname (say, twain.example.com), you must escape the dots, as in twain\.example\.com.)

#### grep

- *Count matching lines*: displays the number of lines that match the specified pattern if you use the -c or --count option.
- *Specify a pattern input file*: The -f file or --file=file option takes pattern input from the specified file rather than from the command line.
- *Ignore case*: You can perform a case-insensitive search, rather than the default case-sensitive search, by using the -i or --ignore-case option.
- *Search recursively*: The -r or --recursive option searches in the specified directory and all subdirectories rather than simply the specified directory.
- *Use an extended regular expression*: To use an extended regular expression, you can pass the -E or --extended- regexp option.

> My favorite combination is *grep -irn "pattern" files*

#### sed

> sed [OPTION]... {script-only-if-no-other-script} [input-file]...

> (for instance: sed "s/old/new" file, replaces old with new)

### Managing Processes

#### ps [options]

> CAUTION: watchout switches with dash (-).In other words, *-x* and *x* are **not same**.

- -A and -e ==> display all the processes on the system.
- x ==> displays all processes owned by the user. Also increases the amount of information that’s displayed.
- (-u | -U | --User) User ==> Display one user’s processes. The user variable may be a *username* or a *user ID*.
- *Display extra information*: The -f, -l, j, l, u, and v options all expand the information provided in the ps output.
- *Display process hierarchy*: The -H, -f, and --forest options group processes and use indentation to show the hierarchy of relationships between processes.
- -w and w options tell ps that output *must not* exceeded 80 chars per line. (useful for saving ps output to a file)

> My favorite combination is *ps aux* and *ps fax*.

##### Interpreting ps Output

- > *Username*: The name of the user who runs the programs.
- > *PID*: The process ID (PID) is a number that’s associated with the process. (useful for modify or kill the process).
- > *PPID*: parent process ID (PPID) identifies the process’s parent.
- > *TTY*: The teletype (TTY) is a code used to identify a terminal.
- > CPU time: The **TIME** and **%CPU** headings are two measures of CPU time used.
  - > First indicates the total amount of CPU time consumed
  - > Second represents the percentage of CPU time the process is using when ps executes.
- > CPU priority (nice level): The **NI** column lists these priority codes. Default value is 0.
  - > *Positive* values represent **reduced** priority, while *negative* values represent **increased** priority.
- > Memory use
  - > **RSS** is resident set size (the memory used by the program and its data)
  - > **%MEM** is the percentage of memory the program is using
  - > **SHARE** column is memory that’s shared with other processes (such as shared libraries)
- > *Command*: is the command used to launch the process

#### top

> Switches

- *-d delay*: This specifies the delay between updates, which is normally 5 seconds.
- *-p pid*: If you want to monitor specific processes, you can list them using this option.
- *-n iter*: ou can tell top to display a certain number of updates (iter) and then quit.
- *-b*: This specifies batch mode, in which top doesn’t use the normal screen update commands

> Commands

- *h or ?*:  These keystrokes display help information
- *k*: You can kill a process with this command
- *q*: This option quits from top
- *r*: You can change a process’s priority with this command.
- *s*: This command changes the display’s update rate.
- *P*: This sets the display to sort by **CPU usage**
- *M*: You can change the display to sort by **memory usage** with this command.

#### Jobs, Foreground and Background Processes

- jobs: shows  all jobs in this terminal.
- Ctrl+Z normally pauses the program and gives you control of the terminal
- fg [num]: restore job [num to foreground]
- An alternative to launching a program, using Ctrl+Z, and typing bg to run a program in the background is to append an ampersand (&) to the command when launching the program.
