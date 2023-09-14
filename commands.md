### JunOS commands 
```
    user@JunOS> show version brief 
    user@JunOS> show ospf neighbor
    user@JunOS> show route 
    user@JunOS> show route forwarding-table 
    user@JunOS> show system processes extensive 
    user@JunOS> show interfaces ge-0/0/0 extensive 

```

* UNIX Shell 

    ` user@JunOS> start shell `

* Configure vs configure private 

    Makes change to the active cofiguration 

    ` user@JunOS> configure `


    Makes changes in candidate configuration which is separate file 

    ` user@JunOS> configure private `

```
    user@JunOS> show system commits
    user@JunOS> rollbacks ?
    [edit]
    user@JunOS# set system host-name <NAME>
    user@JunOS# edit routing-options static
    user@JunOS> edit interface ge-0/0/0
    [edit interface ge-0/0/0]
    user@JunOS# set.....
    user@JunOS# del.....
    user@JunOS# top --------> similar to do command 
    user@JunOS# rollback <version number> 
    user@JunOS> help topic routing-options static 
    user@JunOS> help reference routing-options static 


```

* Copy and Replace configs 

    `user@JunOS# copy et-0/0/0 to xe-0/0/0`

    `user@JunOS# replace pattern 0.100 with 0.200`

    `user@JunOS# top show | compare `


*  Real-time statistics of all active interfaces 

    `user@JunOS> monitor interface traffic `

* Display output with no page breaks 

    `user@JunOS> show interfaces | no-more `

*  Route Engine(RE) 

    - Calculate the best network path to each individual subnet based on input from various routing protocols and static  routes 

    - Process management traffic to help monitor and manage the network devices 

```

    user@JunOS>  shwo ospf neighbor 
    Address         Interface       State   ID              Pri     Dead
    172.25.1.9      ge-0/0/1.0      Full    192.168.100.3   128     38
    172.25.1.2      ge-0/0/2.0      Full    192.168.100.2   128     35

```
- Router Id of the neighbor connected to the ge-0/0/1.0 interface is 192.168.100.3

- Router has two OSPF adjacencies 


* `user@JunOS> show chassis routing-engine`

* while configuring non-root user in JunOS class must be defined 

* Configuration archival feature on device 
    
    - Occurs when a user performs a commit opertaion 
    - When a specific time interval passes 


* Static Routing Configuration parameter **no-readvertise** allows us to prevent the static route to be redistributed using a dynamic routing protocol, when using this static route to provide connectivity to mgmt network 


