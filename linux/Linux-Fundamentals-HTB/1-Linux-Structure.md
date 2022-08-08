# Linux Structure

## Philosophy

Linux follows five core principles:

![c807039562c4a5a8628ccdefa6c31dc7.png](images/c807039562c4a5a8628ccdefa6c31dc7.png)

## Components

![3b88ae788bd07480185e8a7f7ef23245.png](images/3b88ae788bd07480185e8a7f7ef23245.png)

## Linux Architecture

The Linux operating system can be broken down into layers:

![ba8d363b7b91338e0f150d193d681ae4.png](images/ba8d363b7b91338e0f150d193d681ae4.png)

## File System Hierarchy

The Linux operating system is structured in a tree-like hierarchy and is documented in the [Filesystem Hierarchy](http://www.pathname.com/fhs/) Standard (FHS). Linux is structured with the following standard top-level directories:

![e0931eb9990d193a780f0056aca2b31a.png](images/e0931eb9990d193a780f0056aca2b31a.png)

| Path   | Description                                                                                                                                              |
|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| /      | the root, contains all files needed to boot. After boot, all of the other filesystems are mounted at standard mount points as subdirectories of the root |
| /bin   | contains essential command binaries                                                                                                                      |
| /boot  | static bootloader + kernel executable + files needed 4 boot                                                                                              |
| /dev   | device files                                                                                                                                             |
| /etc   | Local system config files, may also have installed applications config                                                                                   |
| /home  | Each user on the system has a sub-dir here                                                                                                               |
| /lib   | shared library files reqd 4 system boot                                                                                                                  |
| /media | Externel removable media devices, e.g. USB                                                                                                               |
| /mnt   | Temporary mount points for regular FSs                                                                                                                   |
| /opt   | Optional files, e.g. 3rd party tools                                                                                                                     |
| /root  | the home directory of the root user                                                                                                                      |
| /sbin  | contains exes for sys admins                                                                                                                             |
| /tmp   | temporary files, cleared upon system boot                                                                                                                |
| /usr   | contains user's exes, libs, man files                                                                                                                    |
| /var   | variable data files such as log files, email in-boxes, web-app related data, cron files and more                                                         |
