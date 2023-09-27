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

**Loading Configuration File**

```
    user@JunOS# run file list ---> equivalent of UNIX ls -l command 
    user@JunOS# run file show <file_name> --> cat equivalent of UNIX 
    user@JunOS# load merge <file_name>
    user@JunOS# show | display set 
    user@JunOS# show | display set relative 
    user@JunOS# load override reset  
```

**Deactivating Configuration**

```
    user@JunOS# run show route 100/24
    user@JunOS# edit routing-options static 
    user@JunOS# deactivate route ip/prefix
    user@JunOS# show 
    user@JunOS# run show route protocol static 
    *********Shutdown equivalent of Cisco******
    user@JunOS# edit int xe-0/0/0
    user@JunOS# set disable 
    user@JunOS# run show interface terse <interface_name>

    **Deactivating and disabling are totally different thing**
    user@JunOS# clear system commit ? 
    
```
**Cutting and Pasting Configuration/General Configuration**

```
    user@JunOS# save terminal 
    user@JunOS# load merge terminal relative 
    user@JunOS# show configuration | compare rollback <number> 
        ==> Compare rollback with current active configuration 

    user@JunOS> file compare <file-before-config> <file-after-config>

    user@JunOS> request system zeroize 
    user@JunOS# load factory-default 
    user@JunOS# show system root-authentication
    user@JunOS# show system services 
    user@JunOS# set system host-name <hostname>

    user@JunOS# help aprops archieve 
    user@JunOS# set system services set web-mangement http interface <name eg: fxp0.0>
    user@JunOS# up <number eg 2>
    user@JunOS# show | display set 
    user@JunOS# commit synchronize --> if multiple RE are installed 
    user@JunOS> show system commit 
    user@JunOS# commit comment "Text " 
    user@JunOS# commit prepare 
    user@JunOS# commit activate 
    
    user@JunOS# request system configuration rescue save 
    user@JunOS# rollback rescue  
    user@JunOS# save <filename>

    user@JunOS# save <path/filename>
    user@JunOS# save ftp://user:password@router/<path/filename>
    user@JunOS# save scp://user@router/<path/filename>

    user@JunOS# load factory-default  
    user@JunOS# show syatem authentication-order 
    user@JunOS# show radius-server 
    user@JunOS# show tacplus-server 
    user@JunOS# show login user lab 
    user@JunOS# edit system archival 
        - transfer-on-comit 

    user@JunOS> restart routing 
    - restarts the routing protocols process(rpd), 
    which can be useful when troubleshooting routing problems
```

* User Authentication

```
    user@JunOS# set system radius-server <ip-address> secret <secret>
    user@JunOS# set system authentication-order radius 
    user@JunOS# 
    - if an authentication method is unavailable because of network or server outage,
    the software automatically consults the local password database 

```

* Archival configuration 

```
    user@JunOS# edit system archival configuration 
    user@JunOS# set archieve-sites "ftp://ftp@172.25.11.254/archive" password ftp 
    user@JunOS# set transfer-on-commit 
    
    user@JunOS# set system syslog file messages any any 
    
    user@JunOS> show log messages | match transfer 
    - the transfer is logged in the /var/log/messages file 

```

* OSPF configuration 

```
    user@JunOS# set protocols ospf area <area eg. 0.0.0.0> interface <eg so-0/0/0> 
```
**Practice Questions**


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

* Real time syslog messages on the terminal 

    `user@JunOS> monitor start message`

* All traffic dropped and an ICMP notification sent back to the source 
    ```
    user@JunOS> show configuration interface ge-0/0/1.0
    family inet {
        filter{
            input input-ff;
        }
        address 10.10.102.1/24;
    } 
    user@JunOS> show configuration firewall filter input-ff
    term t1{
        then{
            reject;
        }
    } 
    ```

* Show the options we can use with syslog UI_DBASE_LOGOUT_EVENT

    `user@JunOS> help syslog UI_DBASE_LOGOUT_EVENT`

* virtual Router: Similar to a VPN routing and forwarding instance type,but used for non-VPN realted 
applications like virtualization. 

    - l2vpn: Used in Layer 2 VPN implementation 
    - no-forwarding: Used to separate large networks into smaller administrative entities 
    - vpls: Used for point-to-multipoint LAN implementations between a set or sites in a VPN 
    - vrf: Used in Layer 3 VPN implementations 

* Transport Layer => flow control & Segmentation 

* Displays the available options and a brief description of usage guidelines 

    `user@JunOS> help topic interface ? `
    
* Gigabit interface regex matching on FPC 2 will inherit the family inet configuration 

    ```
    [edit groups]
    user@JunOS# show 
    ge-int {
        interfaces {
            <ge-2/*>{
                unit 0{
                    family inet {

                    }
                }
            }
        }
    }
    ```

* Input firewall filters to control incoming traffic on a specific interface,
output firewall filter to control outgoing traffic on a specific interface 

* Separation of the control plane and forwardinf plane of a JunOS device 
    - graceful Routing Engine Switchover 
    - deep packet inspection 
    - in-service software upgrades 

* Transit traffic is only processed by the forwarding plane 

* Order of authenticaion, attempt to authenticate using the local database if RADIUS and TACACS+ fails 
    
    ```
    [edit]
    user@JunOS> show system authentication-order 
    authentication-order [ radius tacplus];

    ```

* Apply the policy as an export policy within the OSPF configiration 

    ```
    [edit policy-options]
    user@JunOS# show 
    policy-statement BGP-to-OSPF{
        term 10 {
            from protocol bgp;
            then accept;
        }
    }
    ```

* Login classes, permissions can be overridden for certain commands & define access privileges for a user 

* Exception traffic processing, 
    - By default a rate-limiter for exception traffic exists on the internal link between the control plane and the forwarding plane 
    - when congestion occurs, only exception traffic is given preference 

* Asterisks in a traceroute indicate a response timeout 

```
    user@JunOS> traceroute 10.1.15.2
    traceroute to 10.1.15.2 (10.1.15.2), 30 hops max, 40 byte packets 
    1 10.1.36.1 (10.1.36.1) 0.651ms 7.882ms 0.568ms
    2 10.1.23.1 (10.1.23.1) 0.6ms 0.8ms 0.6ms
    3 * * *
    4 * * *
    5 * * *
```

* Protocol updates and system management is done by Routing Engine(RE) 

* Only allow 10.10.10.0/24 route and reject others

```
    user@JunOS# show policy-options 
    policy-statement block-routes {
        term 1{
            from {
                route-filter 10.10.10.0/24 longer;
            }
            then reject;
        }
    }
```

* Junos Software upgrade requires a reboot of the device 

* An Export policy determines the routes in the local routing table that are advertised to peers 
and  an import policy is used to control routes that are accepted by the local routing table 
