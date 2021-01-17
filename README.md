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
# 1. namespace: 
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
   
Example of Network namespaces
     man ip net-ns
     Network namespace, in particular, virtualizes the network stack. Each network namespace has its own set of resources
     like network interfaces, IP addresses, routing tables, tunnels, firewalls etc. For example, iptables rules added to a
     network namespace will only affect traffic entering and leaving that namespace.
     
    for example,
      1. To add a new network interface, use ip link add <interface-name> type <interface-type> <interface-arguments>...
      2. To allocate a new IP address range to an interface (device), use ip addr add <ip-address-range> dev <device-name>
      3. To delete a route entry from the route table, use ip route del <route-ip-range> dev <device-name>

     The -n option can be used to switch the target namespace. For example, to allocate the 10.0.1.0/24 IP address range 
     to the interface veth0 within the vnet0 network namespace, use ip -n vnet0 addr add 10.0.1.0/24 dev veth0.
 
Configure the 1st Network Namespace    
     
    Our first task is to create a new pair of veth interfaces, veth0and veth1, by using the ip link add command:
       
       # create the pair of veth interfaces named, veth0 and veth1
            ip link add veth0 type veth peer name veth1

       # confirm that veth0 is created
            ip link show veth0

       # confirm that veth1 is created
            ip link show veth1
    
Let’s create our first network namespace, vnet0. Then we can assign the veth0 interface to this network namespace, and 
allocating the 10.0.1.0/24 IP address range to it:

       # create the vnet0 network namespace
            ip netns add vnet0

      # assign the veth0 interface to the vnet0 network namespace
            ip link set veth0 netns vnet0

      # assign the 10.0.1.0/24 IP address range to the veth0 interface
            ip -n vnet0 addr add 10.0.1.0/24 dev veth0

      # bring up the veth0 interface
            ip -n vnet0 link set veth0 up

      # bring up the lo interface, because packets destined for 10.0.1.0/24
      # (like ping) goes through the "local" route table
            ip -n vnet0 link set lo up 

      # confirm that the interfaces are up
            ip -n vnet0 addr show

What happens if we try to pingthe veth0 interface from both the host and vnet0 network namespaces?
 
      # veth0 no longer appears in the host network namespace
            ip link show veth0            

      # ping doesn't work from the host network namespace
            ping -c10 10.0.1.0

      # ping works from inside the vnet0 network namespace
            ip netns exec vnet0 ping -c10 10.0.1.0
            
Notice that veth0 is no longer reachable from the host network namespace.

Let’s look at the last ip netns exec command in the above code snippet. This command allows us to execute arbitrary 
commands in network namespaces.
It is comprised of two parts:
 1. ip netns exec vnet0 identifies the target network namespace
 2. ping -c10 10.0.1.0 is the command to be executed in target namespace

