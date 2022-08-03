# Octane-install
Notes on SGI Octane network installation of Irix 6.5.30 using [unxmaal's booterizer](https://github.com/unxmaal/booterizer) from the serial console.
These notes are to help myself remember all the steps. I make them public in case it helps someone, but they are not meant to cover all cases.

## Format and test disks
At the serial console prompt enter the Command Prompt mode by entering 5

    
    
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
    fx: "device-name" = (dksc) 
    fx: ctlr# = (0) 
    fx: drive# = (1) 
    ...opening dksc(0,1,0)
    ...drive selftest...OK
    Scsi drive type == SEAGATE ST336706LC      010A
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [exi]t               [d]ebug/             [l]abel/
    [b]adblock/          [exe]rcise/          [r]epartition/
    fx> r
    
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
    fx/repartition> ro
    
    fx/repartition/rootdrive: type of data partition = (xfs) 
    Warning: you will need to re-install all software and restore user data
    from backups after changing the partition layout.  Changing partitions
    will cause all data on the drive to be lost.  Be sure you have the drive
    backed up if it contains any user data.  Continue? y                                             
    
    ----- partitions-----
    part  type        blocks            Megabytes   (base+size)
      0: xfs      266240 + 71421130     130 + 34874
      1: raw        4096 + 262144         2 + 128  
      8: volhdr        0 + 4096           0 + 2    
     10: volume        0 + 71687370       0 + 35004
    
    capacity is 71687370 blocks
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [ro]otdrive        [u]srrootdrive     [o]ptiondrive      [re]size
    fx/repartition> ..
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [exi]t               [d]ebug/             [l]abel/
    [b]adblock/          [exe]rcise/          [r]epartition/
    fx> exe
    
    ----- please choose one (? for help, .. to quit this menu)-----
    [b]utterfly            [seq]uential           [set]testpat
    [e]rrlog               [st]op_on_error        [sh]owtestpat
    [r]andom               [m]iscompares          [c]omplete
    fx/exercise> c
    
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
    fx/exercise> ..
    
    ----- please choose one (? for help, .. to quit this menu)----- 
    [exi]t             [d]ebug/           [l]abel/           [a]uto
    [b]adblock/        [exe]rcise/        [r]epartition/
    fx> exi
    
    label info has changed for disk dksc(0,1,0).  write out changes? (yes) 

    System Maintenance Menu
    
    1) Start System
    2) Install System Software
    3) Run Diagnostics
    4) Recover System
    5) Enter Command Monitor
    
    Option? 



