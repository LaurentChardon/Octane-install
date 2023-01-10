# Octane-install
Notes on SGI Octane and Octane 2 network installation of Irix 6.5.30 using [unxmaal's booterizer](https://github.com/unxmaal/booterizer) from the serial console.
These notes are to help myself remember all the steps. I make them public in case it helps someone, but they are not meant to cover all cases.

## Clear PROM password (optional)
Turn the Octane off and remove the main board. Change the configuration of the password jumper very near the CPU. The first boot will be very slow after changing the jumper configuration. 
Turn on the Octane. Go into maintenance mode and enter the Command Prompt mode by entering 5. Type:

    resetpw
    resetenv
    setenv console d
 
Shutdown the Octane and reset the password jumper to its initial position. 
 

## Format and test disks
Turn on the Octane. At the serial console prompt enter the Command Prompt mode by entering 5

    
    
                             Running power-on diagnostics...
    
    
    
                               Starting up the system...
    
                   To perform system maintenance instead, press <Esc>


    System Maintenance Menu
    
    1) Start System
    2) Install System Software
    3) Run Diagnostics
    4) Recover System
    5) Enter Command Monitor
    
    Option? 

Press 5 and enter.
Set the network info, start the partitioner

    setenv netaddr 192.0.2.2
    bootp():/6.5.30/Overlay/disc1/stand/fx.64 -x

Then input and output should be as follows:

    Setting $netaddr to 192.0.2.2 (from server )
    Obtaining /6.5.30/Overlay/disc1/stand/fx.64 from server
    2112+112288Setting $netaddr to 192.0.2.2 (from server )
    +27757+10880Setting $netaddr to 192.0.2.2 (from server )
     entry: 0xa80000005fd3f0f0
    SGI Version 6.5 ARCS BE64  Jul 20, 2006
    
````
fx: "device-name" = (dksc) 
fx: ctlr# = (0) 
fx: drive# = (1) 
````
    ...opening dksc(0,1,0)
    ...drive selftest...OK
    Scsi drive type == SEAGATE ST336706LC      010A
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [exi]t               [d]ebug/             [l]abel/
    [b]adblock/          [exe]rcise/          [r]epartition/
```    
fx> r
```    
    ----- partitions-----
    part  type        blocks            Megabytes   (base+size)
      0: xfs     1052672 + 12288000     514 + 6000 
      1: raw        4096 + 1048576        2 + 512  
      6: xfs    13340672 + 12288000    6514 + 6000 
      7: xfs    25628672 + 46058698   12514 + 22490
      8: volhdr        0 + 4096           0 + 2    
     10: volume        0 + 71687370       0 + 35004
    
    capacity is 71687370 blocks
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [ro]otdrive        [u]srrootdrive     [o]ptiondrive      [re]size

```
fx/repartition> ro
fx/repartition/rootdrive: type of data partition = (xfs) 
```
    Warning: you will need to re-install all software and restore user data
    from backups after changing the partition layout.  Changing partitions
    will cause all data on the drive to be lost.  Be sure you have the drive
````
backed up if it contains any user data.  Continue? y                                             
````    
    ----- partitions-----
    part  type        blocks            Megabytes   (base+size)
      0: xfs      266240 + 71421130     130 + 34874
      1: raw        4096 + 262144         2 + 128  
      8: volhdr        0 + 4096           0 + 2    
     10: volume        0 + 71687370       0 + 35004
     
    capacity is 71687370 blocks
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [ro]otdrive        [u]srrootdrive     [o]ptiondrive      [re]size
```
fx/repartition> ..
```    
    ----- please choose one (? for help, .. to quit this menu)-----
    [exi]t               [d]ebug/             [l]abel/
    [b]adblock/          [exe]rcise/          [r]epartition/
```    
fx> exe
```    
    ----- please choose one (? for help, .. to quit this menu)-----
    [b]utterfly            [seq]uential           [set]testpat
    [e]rrlog               [st]op_on_error        [sh]owtestpat
    [r]andom               [m]iscompares          [c]omplete
```
fx/exercise> c
```    
     * * * * * W A R N I N G * * * * *
    about to destroy data on disk dksc(0,1,0)! ok? y
    sequential pass 1: scanning [0, 71687370] (71687370 blocks) 
    
    0%..........10%..........20%..........30%..........40%..........50%..........
    60%..........70%..........80%..........90%..........100%
    butterfly pass 1: scanning [0, 71687370] (71687370 blocks)
    0%..........10%..........20%..........30%..........40%..........50%..........
    60%..........70%..........80%..........90%..........100%
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [b]utterfly            [seq]uential           [set]testpat
    [e]rrlog               [st]op_on_error        [sh]owtestpat
    [r]andom               [m]iscompares          [c]omplete
```
fx/exercise> ..
```    
    ----- please choose one (? for help, .. to quit this menu)----- 
    [exi]t             [d]ebug/           [l]abel/           [a]uto
    [b]adblock/        [exe]rcise/        [r]epartition/
```        
fx> exi
label info has changed for disk dksc(0,1,0).  write out changes? (yes) 
```
    System Maintenance Menu
    
    1) Start System
    2) Install System Software
    3) Run Diagnostics
    4) Recover System
    5) Enter Command Monitor
    
```
Option? 2
```

                         Installing System Software...

                       Press <Esc> to return to the menu.



    1) Remote Directory  X) Local CD-ROM  

    Enter 1-2 to select source type, <esc> to quit,
```
or <enter> to start: 1
```

```
Enter the name of the remote host: 192.0.2.99
Enter the remote directory: 6.5.30/Overlay/disc1/dist
```

    1)[Remote Directory]  X) Local CD-ROM  
          *a) Remote directory 6.5.30/Overlay/disc1/dist from server 192.0.2.99.

    Enter 1-2 to select source type, a to select the source, <esc> to quit,
```
or <enter> to start: 
```
At this stage, it's possible to get an error like `A 000: *** TLB Refill Exception on node 0`. This happens when performing an install through the serial console when the console is set to graphical in the nvrom. To fix, `setenv console d` at the monitor prompt. Don't forget to set it back after the install with `setenv console g`.

    Setting $netaddr to 192.0.2.2 (from server )
    Setting $netaddr to 192.0.2.2 (from server )
    Setting $netaddr to 192.0.2.2 (from server )
    Copying installation program to disk.
    Setting $netaddr to 192.0.2.2 (from server )
    ......... 10% ......... 20% ......... 30% ......... 40% ......... 50% 
    ......... 60% ......... 70% ......... 80% ......... 90% ......... 100% 

    Copy complete
    Setting $netaddr to 192.0.2.2 (from server )
    Setting $netaddr to 192.0.2.2 (from server )
    IRIX Release 6.5 IP30 Version 07202013 System V - 64 Bit
    Copyright 1987-2006 Silicon Graphics, Inc.
    All Rights Reserved.

    root on /hw/node/xtalk/15/pci/0/scsi_ctlr/0/target/1/lun/0/disk/partition/1/block ; dumpdev on /dev/swap ; boot swap file on /dev/swap swplo 81000
    Creating miniroot devices, please wait...

    Current system date is Tue Jan 10 03:57:10 PST 2023

    Mounting file systems:

    /dev/dsk/realroot: Invalid argument
    No valid file system found on: /dev/dsk/realroot
    This is your system disk: without it we have nothing
    on which to install software.


    Make new file system on /dev/dsk/realroot [yes/no/sh/help]: yes

    About to remake (mkfs) file system on: /dev/dsk/realroot
    This will destroy all data on disk partition: /dev/dsk/realroot.
```
            Are you sure? [y/n] (n): y
```
```
            Block size of filesystem 512 or 4096 bytes? 512
```
Select the block size according to the typical file size. For the OS drive, and for development, 512 is good.

    Doing: mkfs -b size=512 /dev/dsk/realroot
    meta-data=/dev/rdsk/realroot     isize=256    agcount=16, agsize=4463820 blks
             =                       sectsz=512   attr=0, parent=0
    data     =                       bsize=512    blocks=71421120, imaxpct=25
             =                       sunit=0      swidth=0 blks, unwritten=1
             =                       mmr=0
    naming   =version 2              bsize=4096   mixed-case=Y
    log      =internal log           bsize=512    blocks=8718, version=1
             =                       sectsz=512    sunit=0 blks, lazy-count=0
    realtime =none                   extsz=65536  blocks=0, rtextents=0

    Trying again to mount /dev/dsk/realroot on /root.

    UX:make: INFO: `scsi' is up to date.
    NOTICE: XVM mirrors enabled
    NOTICE: XVM snapshot enabled
    xvminit complete
        /dev/miniroot            on  /
        /dev/dsk/realroot        on  /root
        
    Invoking software installation.
```
    What is the hostname (system name) of your machine? octane2
    What is the network address of octane2? 192.0.2.2                                                     
    What is the netmask for 192.0.2.2?
    Press Enter for the IP class default [0xffffff00]: 
    Starting network with hostname: octane2, at ip address: 192.0.2.2
```
    Default distribution to install from: 192.0.2.99:6.5.30/Overlay/disc1/dist

    For help on inst commands, type "help overview".


    Inst 4.1 Main Menu

    1. from [source ...]            Specify location of software to be installed
    2. open [source ...]            Specify additional software locations
    3. close [source ...]           Close a software distribution location
    4. list [keywords] [names]      Display information about software subsystems
    5. go                           Perform software installation and removal now
    6. install [keywords] [names]   Select subsystems to be installed
    7. remove [keywords] [names]    Select subsystems to be removed
    8. keep [keywords] [names]      Do not install or remove these subsystems
    9. step [keywords] [names]      Interactive mode for install/remove/keep
    10. conflicts [choice ...]       List or resolve installation conflicts
    11. help [topic]                 Get help in general or on a specific word
    12. view ...                     Go to the View Commands Menu
    13. admin ...                    Go to the Administrative Commands Menu
    14. quit                         Terminate software installation

```
Inst> admin load 192.0.2.99:selections
```
Press `q` a few times to get through all the READMEs. Then

    Select appropriate software stream to install.
    -------------------------------------------------
    Before you install an intermediate release, you must select
    between two streams of installable software: the maintenance
    stream, which contains accumulated bug fixes and any required software
    for new hardware components; or the feature stream, which contains
    accumulated bug fixes, any required software for new hardware
    components, and new hardware and software features.  After your
    selection, inst automatically identifies the modules in the stream
    that you selected and loads them with no further prompting.
    The currently loaded distributions contain maintenance and feature
    stream products.
    Select the maintenance stream if you simply want to maintain the
    current hardware and software functions on your system.  The
    maintenance stream insures compatibility of new hardware components.

    Select the feature stream if you want to upgrade hardware and
    software with new features.  Occasionally, the feature stream
    introduces incompatibilities between old and new hardware components.

    To change the stream selection between maintenance and feature,
    reset the stream preference to the new setting.  In some cases,
    resetting the stream preference and installing products from the
    new stream requires reinstalling portions of the IRIX operating system.
    -------------------------------------------------
    1. Place me on the maintenance stream.
    2. Place me on the feature stream.
    3. Cancel
```
Please enter a choice [1]: 2
```
    Reading product descriptions .. 100% Done.
    WARNING: This software distribution is not meant to install on 
    the version of IRIX currently running on this machine.  Sorry.


    This distribution contains overlay products.  You may need to specify
    additional distributions that contain the base software for these overlays.
    If you do not load all the related distributions now, "Conflict" messages
    later in the installation will prompt you to load any other necessary CDs.
    (See the "overlay" help topic for more information.)

    You can enter the name of a different distribution and press "Enter."

    Or, enter "done" if you are ready to proceed with the installation now.

    1 192.0.2.99:6.5.30/Overlay/capps/dist
    2 192.0.2.99:6.5.30/Overlay/apps/dist
    3 192.0.2.99:Development/devlibs/dist
    4 192.0.2.99:Development/devfoundations/dist
    5 192.0.2.99:Foundation/disc2/dist
    6 192.0.2.99:Foundation/disc1/dist
    7 192.0.2.99:6.5.30/Overlay/disc3/dist
    8 192.0.2.99:6.5.30/Overlay/disc2/dist
    9 192.0.2.99:6.5.30/Overlay/disc1/dist
    10 done (distribution information read, return to inst prompt)

```
Install software from: [192.0.2.99:6.5.30/Overlay/capps/dist] done
```
    Importing selections from 192.0.2.99:selections.

```
Inst> admin source 192.0.2.99:commands
```
    return is not an item on this menu.
    type ? or <Enter> to display menu

    Inst> No conflicts

And then installation should proceed

## Post install tips

To set prom from OS command line, man nvram
To set resolution from OS command line, man setmon
Resolution can also be set with GUI util "Display Properties in menu bar"
Resolution can also be set in PROM with `setenv monitor l` for 1024x768 resolution, or `setenv monitor h` for 1280x1024

