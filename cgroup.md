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


Step 1 — Installation
         yum install  libcgroup-tools

Step 2 — Starting the Service
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

     
Step 3 — Configuration
        
 In this section, we will create example cgroups and set some resource limits for those cgroups. 
 The cgroup configuration file is /etc/cgconfig.conf. Depending on the contents of the configuration file, 
 cgconfig can create hierarchies, mount necessary file systems, create cgroups, and set subsystem parameters 
 (resource limits) for each cgroup.


 A hierarchy is a set of cgroups arranged in a tree, such that every task in the system is in exactly one of 
 the cgroups in the hierarchy. In a default CentOS 7 configuration, each subsystem is put into its own hierarchy.


 Let us first create a few cgroups named limitcpu, limitmem, limitio, and browsers. The /etc/cgconfig.conf file
 contains two major types of entries — mount and group. Lines that start with group create cgroups and set subsystem 
 parameters. Edit the file /etc/cgconfig.conf and add the following cgroup entries at the bottom:


lsblk ( to check blkio like: 252:0)

vim /etc/cgconfig.conf

    group limitcpu{
           cpu {
                  cpu.shares = 400;
             }
     }

    group limitmem{
          memory {
                  memory.limit_in_bytes = 512m;
          }
      }

    group limitio{
          blkio {
                  blkio.throttle.read_bps_device = "252:0         2097152";
          }
    }

    group browsers{
         cpu {
                 cpu.shares = 200;
         }
         memory {
                 memory.limit_in_bytes = 128m;
         }
    }



In the limitcpu cgroup, we are limiting the cpu shares available to processes in this cgroup to 400. cpu.shares
specifies the relative share of CPU time available to the tasks in the cgroup.

In the limitmem cgroup, we are limiting memory available to the cgroup processes to 512MB.

In the limitio cgroup, we are limiting the disk read throughput to 2MiB/s. Here we are limiting read I/O to the 
primary disk, /dev/vda, with major:minor number 252:0 and 2MiB/s is converted to bytes per second (2x1024x1024=2097152).

In the browsers cgroup, we are limiting cpu shares to 200 and available memory to 128MB.

We need to restart and enable  the cgconfig service for the changes in the /etc/cgconfig.conf file to take effect:
       
       systemctl restart cgconfig.service  
       systemctl enable cgconfig.service  



Our next goal is to add the processes (tasks) for which we wish to limit resources to the cgroups we created earlier.

Cgred (control group rules engine daemon) is a service that moves tasks into cgroups according to parameters set in 
the /etc/cgrules.conf file. Entries in the /etc/cgrules.conf file can take one of the two forms:

    user subsystems control_group
    or
    user:command subsystems control_group


user refers to a username or a groupname prefixed with the “@” character. subsystems refer to a comma-separated list
of subsystem names. control_group represents a path to the cgroup, and command stands for a process name or a full 
command path of a process. Entries in the /etc/cgrules.conf file can include the following extra notations:


    @ — indicates a group instead of an individual user. For example, @admin indicates all users in the admin group.

    * — represents “all”. For example, * in the user field represents all users.

    % — represents an item the same as the item in the line above.


Now let us add the programs/processes we wish to limit. Edit /etc/cgrules.conf and add the following at the bottom:


vim /etc/cgrules.conf

    *:firefox       cpu,memory      browsers/
    *:hdparm        blkio   limitio/
    student   blkio   limitio/
    @admin:memhog  memory  limitmem/
    *:cpuhog        cpu     limitcpu/
    
    
We need to start the cgred service for the cgrules configuration changes to take effect, do this using the command:
  
    systemctl start cgred
    systemctl enable cgred
    
    
In the above lines, we are setting the following rules:

    firefox processes run by any user will be automatically added to the browsers cgroup and limited in cpu 
    and memory subsystems.

    hdparm processes run by any user will be added to the limitio cgroup and will be limited in blkio subsystem 
    according to the parameter values specified in that cgroup.
  
    All processes run by user student will be added to the limitio cgroup and limited in blkio subsystem.

    memhog processes run by anyone in the admin group will be added to the cgroup limitmem and limited in memory subsystem.
    
    cpuhog processes run by any user will be added to the cgroup limitcpu and limited in cpu subsystem.    

SELinux

    setenforce 0


Step 4 — Testing

    sudo yum install hdparm
    sudo hdparm --direct -t /dev/vda
    see ouput
    systemctl stop cgred
    systemctl stop cgconfig.service  
    sudo hdparm --direct -t /dev/vda
    see output

   
  



