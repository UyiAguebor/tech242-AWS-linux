# <u>Processes</u>

## What is a process in Linux

### A process is something that is in memory and looks like its possibly using the CPU
A single cpu means really the cpu can only run one process at a time

**concurrent processing**
With a single core it switches between different processes

`man` before any command shows the manual for the command.

## Two types of processes

- <u>user processes (view using `ps`)</u> 
  - every process is given a process id.
  - TTY is related to a process that is associated with a terminal.
- <u>system processes (to view system processes use the `` command to manage use the `sudo systemctl` command)</u>
  - System processes are not linked to a terminal session.
  - Most dont provide an application or interface for the end user to view they provide things like logging or print services.
  - If it runs in the background its a system process.
  - a lot of system processes end in d which stands for deamon(which means a process that runs in the background).


## Remember these commands:
-  `ps aux` shows
-  `top` show the top processes and refreshes every 3 seconds in order of cpu usage
-  If you need a delay before you run your script command `sleep {seconds}` add an `&` to make it run in the background
-  `jobs` to check the processes that we are running
-  To kill a process we do `kill {process ID}`
   -  The first stage of killing is to terminate or kill a process 
   -  Some processes are stubborn and are still there if you run `kill` the strongest kill command is `kill -9 {process ID}` a brute force kill(only do it if you know what you're doing a last resort)!


Parent processes - a process that starts other processes
Child processes - are started by parent process

## things to keep in mind!
[Diagram](../readme-images/parentprocess.png)
You may use something to run your app and that then starts other processes e.g. maven is responsible fo0r starting and stopping the java apps.
what may happen - if you want to stop a child process but you cant you can kill it.
If you kill a parents its child processes become zombies which still take memory(Last resort).
If a parent is responsible for running an app if you kill the app the parent may just keep recreating the process.