### JunOS commands 
```
    user@JunOS> show version brief 
    user@JunOS> show ospf neighbor
    user@JunOS> show route 
    user@JunOS> show route forwarding-table 
    user@JunOS> show system processes extensive 

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