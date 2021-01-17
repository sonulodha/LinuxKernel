# LinuxKernel
    The Linux kernel is the main component of a Linux operating system (OS) and is the core interface between a computer’s 
    hardware and its processes.It communicates between the 2, managing resources as efficiently as possible.

# What the kernel does
    1. Memory management: Keep track of how much memory is used to store what, and where
    2. Process management: Determine which processes can use the central processing unit (CPU), when, and for how long
    3. Device drivers: Act as mediator/interpreter between the hardware and processes
    4. System calls and security: Receive requests for service from the processes
    5. File system
    6. TCP/IP Networking Stacks
    7. It Implements Access Control based on process identity and file permission.
Kernel Space/ Kernel Mode (Privileged Mode ), Direct access the Hardware.
 
User Space/ User Mode ( Unprivileged Mode ).

    SHELL
    Command-Line-Tool
    Application


    UserMode
    -------------------		switching->context-switching
    Kernel Mode		

# System calls
 System calls provide an interface to the services made available by an Operating System.
 System call is the programmatic way in which a computer program requests a service from the kernel of the operating system.
 These calls are generally available as routines written in C and C++

# Linux Kernel Features
 
    Kernel namespaces (ipc, uts, mount, pid, network and user)
    Apparmor and SELinux profiles
    Seccomp policies
    Chroots (using pivot_root)
    Kernel capabilities
    CGroups (control groups)
