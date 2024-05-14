# Copy On Write on xv6

## Table of contents
* [About The Project](#about-the-project)
* [Requirements](#requirements)
* [How To Run](#how-to-run)
* [General Notes](#general-notes)
* [File kalloc.c](#file-kalloc.c)
* [Function freerange()](#function-freerange())
* [Function kfree()](#function-kfree())
* [Function kalloc()](#function-kalloc())
* [File trap.c](#file-trap.c)
* [Function usertrap()](#function-usertrap())
* [File vm.c](#file-vm.c)
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
* In the file `spinlock.h`, the structure of the `extern struct ref_spinlock` is defined, which is used to hold a `reference counter` for each `physical page`, as stated in the assignment, through the integer array `reference_count`. 
In the file [kalloc.c](https://github.com/chrisioan/), this global struct is created, named `ref_c`.
<br/><br/>

## File kalloc.c
Source code can be found here: [kalloc.c](https://github.com/chrisioan/)

Bla
<br/><br/>

## File trap.c
Source code can be found here: [trap.c](https://github.com/chrisioan/)

bla
<br/><br/>