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
    