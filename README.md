# Copy On Write on xv6

## Table of contents
* [About The Project](#about-the-project)
* [Requirements](#requirements)
* [How To Run](#how-to-run)
* [General Notes](#general-notes)
* [File kalloc.c](#file-kallocc)
    * [Function freerange()](#function-freerange)
    * [Function kfree()](#function-kfree)
    * [Function kalloc()](#function-kalloc)
* [File trap.c](#file-trapc)
    * [Function usertrap()](#function-usertrap)
* [File vm.c](#file-vmc)
    * [Function uvmcopy()](#function-uvmcopy)
    * [Function copyout()](#function-copyout)
<br/><br/>

## About The Project
Bla
<br/><br/>

## Requirements
* Compiler with support for C 11 or newer
  ```sh
  sudo apt install gcc
  ```
* You will need a RISC-V "newlib" tool chain from https://github.com/riscv/riscv-gnu-toolchain, and qemu compiled for riscv64-softmmu. Once they are installed, and in your shell search path, you can run "make qemu".
<br/><br/>

## How To Run 
When on the [xv6-project-2021](https://github.com/chrisioan/Copy-On-Write-on-xv6/tree/main/xv6-project-2021) directory of the project:
```sh
make qemu
```
Then execute the tests `cowtest` and `usertests`:
```sh
cowtest
```
```sh
usertests
```
To exit from `qemu`, press `Ctrl+A` then `X`.
<br/><br/>

## General Notes
* In the file [spinlock.h](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/spinlock.h), the structure of the `extern struct ref_spinlock` is defined, which is used to hold a `reference counter` for each `physical page`, as stated in the [assignment](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/K22_CW2_2021-2022.pdf), through the integer array `reference_count`. In the file [kalloc.c](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/kalloc.c), this global struct is created, named `ref_c`.
<br/><br/>

## File kalloc.c
Source code can be found here: [kalloc.c](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/kalloc.c)

### Function freerange()
In the existing for loop, the initialization of the reference counter of the current iteration's page was added with the value 1 (this counter will be reset to 0 through kfree()). This action is necessary because later kfree() is called, which decrements the counter - so initialization must be done first.

### Function kfree()
When kfree() is called, firstly the reference counter of the given page is decremented. Then, a check is performed to see if this counter is equal to 0. In this case, it means that no process is using the specific page, so it can be freed; the page is returned to the list of available pages (following the code as it was initially in kfree()). If it's not 0, then no further action is needed.

### Function kalloc()
The initialization of the reference counter of the specific page with the value 1 was added (lines 101-104).
<br/><br/>

## File trap.c
Source code can be found here: [trap.c](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/trap.c)

### Function usertrap()
An `else if(r_scause() == 15)` was added at line 68 to recognize page fault 15 - Store/AMO page fault. When page fault with code 15 occurs, it must be recognized as a valid CoW fault, meaning it is a valid page, user-level, etc. First, it checks if the virtual address (obtained by calling r_stval()) is permissible. Then, it retrieves the page table entry (PTE) of the specific page using the walk() function. It checks if the PTE obtained is valid and user-level. The physical address of the PTE is stored in the variable pa0. As mentioned in the assignment, a new page is created via kalloc() and the old page is copied to the new one via memmove(). Finally, the flags of the PTE of the old page are retrieved, the PTE is made to point to the physical address of the new page, and the PTE_W bit is set to 1. kfree() is called for the previous (initial) read-only page.
<br/><br/>

## File vm.c
Source code can be found here: [vm.c](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/vm.c)

### Function uvmcopy()
The uvmcopy() function is modified appropriately so that new pages are not created but existing pa pages are used when mappages() is called (line 317), in order to map the physical memory pages of the parent to the child. Additionally, the PTE_W flag in both the parent's and child's PTEs should be cleared. This is achieved on lines 316 and 320 using bitwise AND. In each iteration of each page, the reference counter is incremented.

### Function copyout()
The purpose of walkaddr() is to traverse and directly retrieve the physical address from a virtual address without going through the hardware MMU, which means that no page fault has occurred. So, what we need to do is modify copyout() to use the same mechanism as page faults when encountering a CoW page. Just like in the usertrap() function in trap.c, all necessary checks and procedures are performed. That is, if the virtual addresses are permissible and do not exceed MAXVA, we obtain the PTE, perform checks if it's valid and user-level, and store the physical address of the PTE in the variable pa0. Then, it checks if the PTE is read-only, i.e., if the PTE_W bit is equal to 0 (line 388). If it is, then the entire procedure followed in usertrap() must be followed, and then the previous tasks must be carried out (in the subsequent memmove (line 411), the old page is replaced with the new one), otherwise it remains unchanged, and there is no need to create a new page, copy the old one to the new one, etc.
<br/><br/>
