#----------------------------------------------------------
# NAME: Jennifer Wang 
# SID: 1775380
# CCID: jennif11
#----------------------------------------------------------

Design Choices

Built-in Commands (cd, pwd, exit):
For the built-in commands, I this felt fairly straightforward. Commands like cd directly change the working directory of the shell, so running them in the same process instead of forking. 
With exit, I terminate any background processes and calculate time.

External Commands Execution:
For external commands, I used fork() and execve() to run them in a separate process. If the user runs a command without the &, I wait for it to finish before returning control to the shell. When running commands in the background with &, I chose not to redirect output to /dev/null, as the assignment clarified that background processes may print to the terminal.

Signal Handling:

Ctrl+C (SIGINT): I made sure the shell itself wouldn’t terminate when you send a SIGINT. Only the foreground process should handle that.

Ctrl+Z (SIGTSTP): Similar logic, but instead of killing the process, it suspends the foreground process. I added a message to inform the user that the process was suspended, and the shell keeps running.

Redirection and Piping:
For redirection, I used dup2() to manage input and output redirection. This allows redirecting file descriptors to files for commands like cat < file.txt or echo test > output.txt.
I used pipe() and fork() to pass the output of one command to another. I only handle one-level pipes, like echo hello | grep h.

Background Processes:
The shell tracks background processes using &, and I track them in an array of process IDs. I handle background process termination using the SIGCHLD signal handler, where I get the system resource usage for each process that terminates.

System Calls Used
fork(): create child processes for external commands
execve(): executes external programs by replacing the child process with the new program.
wait4(): waits for child processes to finish and retrieves resources. Used for both foreground and background processes.
dup2(): redirects input and output file descriptor
pipe(): piping between two commands 
kill(): kills background processes when the shell exits
tcgetpgrp() and tcsetpgrp(): terminal control, makes it such that the shell regains control after a foreground process terminates or suspends
signal(): Handles signals like SIGINT, SIGTSTP, and SIGCHLD when interrupting or suspending processes

Testing Strategy

I basically just looked through the assignment descript, rubric and skimmed through the discussion forum. Not much else. I basically tried it to see if it works, and made test cases for myself, although not sure if comprehensive enough. 

Resources: 
Used linux man pages, and as well as in class discussion forum on edge cases signal handling and file redirection
In-class discussions on signal handling and terminal control
and https://tldp.org/LDP/abs/html/io-redirection.html
office hours and lab