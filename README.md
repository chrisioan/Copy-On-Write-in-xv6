# Copy On Write on xv6

## Table of contents
* [About The Project](#about-the-project)
* [Requirements](#requirements)
* [How To Run](#how-to-run)
* [General Notes](#general-notes)
* [File kalloc.c](#file-kallocc)
* [Function freerange()](#function-freerange())
* [Function kfree()](#function-kfree())
* [Function kalloc()](#function-kalloc())
* [File trap.c](#file-trapc)
* [Function usertrap()](#function-usertrap())
* [File vm.c](#file-vmc)
* [Function uvmcopy()](#function-uvmcopy())
* [Function copyout()](#function-copyout())
<br/><br/>

## About The Project
Bla
<br/><br/>

## Requirements
* Compiler with support for C 11 or newer
  ```sh
  sudo apt install gcc
  ```
<br/><br/>

## How To Run 
Bla
<br/><br/>

## General Notes
* In the file [spinlock.h](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/spinlock.h), the structure of the `extern struct ref_spinlock` is defined, which is used to hold a `reference counter` for each `physical page`, as stated in the [assignment](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/K22_CW2_2021-2022.pdf), through the integer array `reference_count`. 
In the file [kalloc.c](https://github.com/chrisioan/), this global struct is created, named `ref_c`.
<br/><br/>

## File kalloc.c
Source code can be found here: [kalloc.c](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/kalloc.c)

Bla
<br/><br/>

## File trap.c
Source code can be found here: [trap.c](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/trap.c)

bla
<br/><br/>

## File vm.c
Source code can be found here: [vm.c](https://github.com/chrisioan/Copy-On-Write-on-xv6/blob/main/xv6-project-2021/kernel/vm.c)

bla
<br/><br/>
