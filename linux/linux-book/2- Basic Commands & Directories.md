# Basic Commands and Directory Hierarchy

## Listing Processes 

use `ps` command 

possible options to are `ps aux` and `ps auxw`

## Killing Processes

use `kill <pid>`

The `kill` command uses many types of signals. The default is `TERM` or terminate.

We can freeze a process by using the signal `kill -STOP <pid>`

We can unfreeze a process by using `kill -CONT <pid>`

Numbers can also be used, so `TERM` has an alternative of `kill -9 <pid>`

## Background Processes

we can use `&` to send a process to the background, instead of blocking the current shell

e.g. `gunzip file.gz &`

The shell should respond by printing the PID of the new background process, and the prompt should return immediately so that you can continue working. 

The process will continue to run after you log out.