# Pset 5
## Mason + Dhilan
Grammar  
commandline ::= list (command is a list)  
commandline ::= conditional (command is a conditional)  
...  

- parsing command line 
- precedence from top to bottom of grammar

execvp: replaces current process image with a fresh  process image found via input string  
waitpid ==> waits for child to exit and takes in status and parent can be blocked, returns pid of child and exit status (useful for false in conditionals)  
blocking = block parent from running until child exits  
WNOHANG = not blocking  

### Step 2 - Background Command
alter parse_line to check for &, and & indicates background command  
parse_line used to create linked list  
- make background boolean attribute in struct
- change parse_line to check for &, once that token is found then you can set certain command to be background in struct (use headers)  
- within run_list, use waitpid to determine background? the return value of waitpid is important...

### Step 3 - Command Lists
traverse command line once to create linked list. When you perform commands, you need to iterate through linked list multiple times.  

### Step 4 - Conditionals
Interested in exit status of waitpid.

### Step 5 - Pipelines
pipe hygiene involves opening/closing multiple pipes. Make sure you close a pipe on both sides.

### Step 6 - Zombie Processes
clean up process

### Step 7 - Redirections
taking stdio, stderr and redirection to a new location. easy EC

### Step 8 - Interruption
