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
# namespace: 
    Namespaces are one of the most important methods for organizing and identifying software objects.A namespace wraps a
    global system resource (for example a mount point, a network device, or a hostname) in an abstraction that makes it 
    appear to processes within the namespace that they have their own isolated instance of the global resource.
    In short  limits what you can see (and therefore use)

Namespaces provide processes with their own view of the system

    There are 6 types of namespaces:
      1. mount ns - for file system.
      2. UTS(Unique time sharing) ns - which checks for different hostnames of running containers
      3. IPC ns - interprocess communication
      4. Network ns- takes care of different ip allocation to different containers
      5. PID ns - process id isolation
      6. user ns- different username(uid)
For more information about namespaces, see the ( man namespaces 7 )
   
   # Example of Network namespaces
     Network namespace, in particular, virtualizes the network stack. Each network namespace has its own set of resources
     like network interfaces, IP addresses, routing tables, tunnels, firewalls etc. For example, iptables rules added to a
     network namespace will only affect traffic entering and leaving that namespace.
