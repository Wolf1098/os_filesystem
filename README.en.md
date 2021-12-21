# os_filesystem-a virtual file system (C++)

## Introduction

This is a linux-like virtual file system. The system is carried by a virtual disk file, which simulates disk read and write by file reading and writing, and does not involve underlying drivers.

To write a simple Linux-like file system, you first need to design a basic framework that includes inode, block, superblock, virtual disk layout, space allocation and other information. The beginning of the file system is a superblock, which contains important information about the system, including the number and size of inodes and blocks. For inode, generally speaking, it needs to occupy one percent of the disk space, but this is a small system with a total size of only a little more than 5M, so the space allocated to the inode area is very small, and most of the remaining space is the block area.

The overall plan of the file system is as follows:  
![](./screenshots/00.png)

Because the time is tight when writing the program, it took only 4 days to check and accept the code, so the code did not have time to optimize, and some parts would appear redundant. Don't be surprised.

Although time is limited, it also implements the function of a vi editor. The writing is relatively simple and the code is messy. There is time to improve it.

In general, the code has yet to be optimized, and you are welcome to comment and find faults.

## how to use

### step 1: Download the project

`git clone https://github.com/windcode/os_filesystem.git`

### Step 2: Open the project with VC++6.0

Double-click in the catalog**MingOS.dsw**File, or drag the file to the VC++6.0 interface.

### step 3: compile, link, run

![](./screenshots/0.png)

### or

### step 1: run directly**/Debug**Under folder**MingOS.exe**document

## characteristic

-   First run, create virtual disk file

![](./screenshots/1.png)

-   log in system

The default user is root and the password is root

![](./screenshots/2.gif)

-   Help command (help)

![](./screenshots/3.gif)

-   User add, delete, login, logout (useradd, userdel, logout)

![](./screenshots/5.gif)

-   Modify file or directory permissions (chmod)

![](./screenshots/6.gif)

-   Write and read are restricted by permissions

![](./screenshots/7.gif)

-   File/folder addition and deletion (touch, rm, mkdir, rmdir)

![](./screenshots/8.gif)

-   View system information (super, inode, block)

![](./screenshots/9.gif)

-   Imitate a vi text editor (vi)

![](./screenshots/4.gif)

-   Index node inode management file and directory information

-   use**Group Link Method**Manage the allocation of free blocks
        * **block分配过程：**
    When a block needs to be allocated, the top of the free block stack takes out a free block address as the newly allocated block.
    When the stack is empty, the stack in the free block represented by the bottom address of the stack is taken as the new free block stack.
        * **block回收过程：**
    When reclaiming a block, check whether the stack is full. If it is not full, the current stack pointer is moved up and the address of the block to be reclaimed is placed on the top of the new stack.
    If the stack is full, the block to be recycled is used as a new free block stack, and the address of the bottom element of the free block stack is set to the free block stack just now.
        * 分配和回收的同时需要更新block位图，以及超级块。

-   Inode allocation/reclamation
    -   The allocation and recovery of inodes are relatively simple, using sequential allocation and recovery.
    -   When it needs to be allocated, search for a free inode sequentially from the inode bitmap, and return the number of the inode if the search succeeds.
    -   When recycling, just update the inode bitmap.
    -   Both allocation and recovery need to update the inode bitmap.

## Notice

-   Operating environment is VC++6.0
