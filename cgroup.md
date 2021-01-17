# cgroup
Control Groups provide a mechanism for aggregating/partitioning sets of tasks, and all their future children,
into hierarchical groups with specialized behaviour.

In short limits how much you can use.
  
Cgroups involve resource metering and limiting:

 1. memory
 2. CPU
 3. block I/O
 4. network
 
# How To Limit Resources Using cgroups on CentOS 7

    # Step 1 — Installation
         yum install  libcgroup-tools

    # Step 2 — Starting the Service
         systemctl start cgconfig.service  

You could also run the `lscgroup’ command to verify:
     lscgroup
     
     
# System Resources 
The system resources are known as subsystems, and each subsystem has several parameters to which we could
assign values. CentOS 7 provides ten cgroup subsystems


1. blkio — this subsystem sets limits on input/output access to and from block devices such as physical drives (disk, solid state, USB, etc.).

2. cpu — this subsystem sets limits on the available CPU time

3. cpuacct — this subsystem generates automatic reports on CPU resources used by tasks in a cgroup

4. cpuset — this subsystem assigns individual CPUs (on a multicore system) and memory nodes to tasks in a cgroup

5. devices — this subsystem allows or denies access to devices by tasks in a cgroup

6. freezer — this subsystem suspends or resumes tasks in a cgroup

7. memory — this subsystem sets limits on memory use by tasks in a cgroup and generates automatic reports on memory resources used by those tasks

8. net_cls — this subsystem tags network packets with a class identifier (classid) that allows the Linux traffic controller (tc) to identify packets originating from a particular cgroup task

9. net_prio — this subsystem provides a way to dynamically set the priority of network traffic per network interface

10. ns — this is the namespace subsystem     

     
        # Step 3 — Configuration
        
        In this section, we will create example cgroups and set some resource limits for those cgroups. 
        The cgroup configuration file is /etc/cgconfig.conf. Depending on the contents of the configuration file, 
        cgconfig can create hierarchies, mount necessary file systems, create cgroups, and set subsystem parameters 
        (resource limits) for each cgroup.


        A hierarchy is a set of cgroups arranged in a tree, such that every task in the system is in exactly one of 
        the cgroups in the hierarchy. In a default CentOS 7 configuration, each subsystem is put into its own hierarchy.



