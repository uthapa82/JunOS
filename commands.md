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

* Syslog/logging 

```
    user@JunOS# run show system syslog
    user@JunOS# edit system syslog 
    user@JunOS# set file <file_name> change-log info 
        - change-log = facility 
        - info = severity 

    user@JunOS# set file messages any any 
        - severity level for the default messages file to any 

    configure the system to send logs to remote server running standard 
    system logging utility
    [edit system syslog] 
    user@JunOS# set host <ip-address> authorization info 
        - correct facility that logs access attempts on the system 

    user@JunOS# run file list /var/log/ 
        - lists log files from the system's configuration 
    
    user@JunOS# run show log <file name>

    user@JunOS> show log FILENAME | last NUM 
    - show the most recent entries 
    

```

* NTP

```
    user@JunOS# set system ntp server <ip address>
    
    configure NTP boot server 
    user@JunOS# set system ntp boot-server <ip address>
    user@JunOS# show system ntp

    manually force synchronization with the NTP server 
    user@JunOS> set date ntp 

    verify sync 
    user@JunOS> show ntp associations 

```

* SNMP 

```
    user@JunOS# set snmp community <name> clients <ip address>

    configure snmp trap group to send traps to the NMS server, whenever the interface
    transitions to a down state
    user@JunOS# set snmp trap-group <name> targets <ip address>
    user@JunOS# set snmp trap-group <name> categories link 

    testing SNMP configuration
    user@JunOS# set interface <number> disable 
    user@JunOS#  run show interface <number> terse 
    
    no shutdown 
    user@JunOS# delete interface <number> disable 

    checking the interface up down state change in snmp 
    user@JunOS> show log messages | match <interface> | match snmp 
    user@JunOS> show snmp statistics 

    user@JunOS#  show snmp mib walk jnxOperatingDescr


```
* Operational Monitoring and Maintenance 

```
    - check the status of routing protocol process(rpd)
    user@JunOS> show system processes extensive 
    user@JunOS> show system processes extensive | match "pid | rpd"

    view the protocol statistics 
    user@JunOS> show system statistics 
    user@JunOS> show system uptime 
    user@JunOS> show system users 

    force a logout for the user 
    user@JunOS> request system logout user walter 
    
    user@JunOS> show chassis routing-engine 
    user@JunOS> show chassis location 

    set the system location 
    user@JunOS# set system location bulding "DC Pod 2" floor <number>
    user@JunOS> show chassis hardware 
    user@JunOS> show interface terse 
    user@JunOS> show interface <name> extensive
    user@JunOS> clear interface statistics  <number>
    user@JunOS> show interface <number> extensive | find "traffic" 

```

* What Trap category do we enable to receive traps for an over-temperature condition
    => chassis category

* Password recovery 
    1. Reboot the system 
    2. Press space
    3. boot -s 
    4. recovery 
    5. Set root password 
    6. commit 

* Upgrading Junos 

```
    user@JunOS> request system storage cleanup or dry-run 
    user@JunOS> request system zeroize 
        - removes all configs factory default 
    user@JunOS> request system zeroize media 
        - to make removed information unrecoverable 

    check the version 
    user@JunOS> show version 

```

* Interfaces 
    - gig -> ge 
    - tenGig -> xe 
    - et -> 40/100G
    - es -> encryption interface 
    - gr -> GRE 
    - Naming convetion FPC/Slot/port example: ge-0/0/2
    - irb integrated routing and bridiging int

* LAG 
    1. Logical aggregrate ethernet interface (ae0)
        - edit chassis( add aggregated devices)
        - set aggregrated devices ethernet device count 
    
    2. Parameters associated with the ae0 are defined 

    3. Member links are associated with LAG 

* Configuration Groups 
    Features 
    - apply a group of configuration statements to different sections of configuration 
    - separate the common interface media parameters from the interface-specific addressing info 
    - create smaller, more logically constructed configuration files 

* Routing 
    - End-to-end communication path 
    - Forwarding information Base (FIB) and Routing Information Base (RIB)

    Primary 
    - inet.0 for ipv4 unicast 
    - inet6.0 for ipv6 unicast 
    - inet.1 multicast forwarding cache 
    - inet.2 multicast(MBGP) routes to provide reverse path(RPF) checks 
    - inet.3 used for MPLS path inform 
    - inet.4 multicast source discovery protocol (MSDP)
    - MPLS.0 MPLS next hop 

    **Routing Preferences**
    | Routing Protocol | Preference | 
    | -----------------|------------|
    | Direct  | 0 |
    | Local   | 0 |
    | static  | 5 |
    | OSPF internal | 10 | 
    | RIP | 100 |
    | OSPF AS external | 150 |
    | BGP (both internal and external) | 170 |



```
    user@JunOS> show route ip exact 
    user@JunOS> show route 
    user@JunOS> show route protocol ospf 
    user@JunOS> show route forwarding table 
    user@JunOS> show route instance 
    user@JunOS# set static route <ip address eg 192.168.2.2/32> qualified-next-hop <backup ip eg 172.20.77.2> 
    preference 6

    user@JunOS# delete static route <ip address  eg. 192.168.2.2/32>

    user@JunOS# delete rib inet6.0 static 
    
```

* Elements of Routing table 

    - Active, holddown, hidden
        - Active route 
        - pending state before system declares inactive 
        - cannot be used because of policy or invalid next-hop
    
    - Forwarding table 
        - stores only best route to particular destination 
    
    - dest => directly connected 
    - intf => installed as a result of interface configurations
    - perm => routes installed by kernel during initialization of routing 
    - user => routes by routing protocol process or as a result of config 

* Routing instance types 
    - evpn : EVPN routing instance 
    - evpn-vpws: EVPN VPWS routing instance 
    - mpls : internet multicast over MPLS routing instance 
    - virtual router : used for non-VPN related application such as virtualization 
        - creates a new forwarding table 

    - no-forwarding: used to separate large networks into smaller administrative entities 
        - shares the global forwarding table 

    - forwarding : used to implement FBF for common Access layer application  
    - vrf : used in L3 VPN implementation 

```
    user@JunOS> show route table <instance-name eg new-instance>.inet0

    user@JunOS> show interface terse routing-instance <name of instance >

    user@JunOS> ping <ip address> rapid count <number> routing-instance <new-instance>

    user@JunOS> traceroute <ip address> routing-instance <new instance>
```

* OSPF configuration 

```
    user@JunOS# set protocols ospf area <area eg. 0.0.0.0> interface <eg so-0/0/0> 
    [edit protocols ospf]
    user@JunOS# set area 0 interface ge-0/0/3.0 passive 
    [edit protocols ospf]
    user@JunOS# set area 0 interface lo0.0
```

* Configuring interface for ipv6 
    - enabling ipv6
        - configures the interface's link local address
        - activates ipv6 processing for that interface 

```
    user@JunOS# set family inet6 address <xxxx:xxxx::x/n>
    
    point-to-point: /127
    loopback: /128
    user@JunOS# set family inet6 ----> enables the ipv6 and adds link-local address 
    user@JunOS# set family inet6 address <ipv6 address>/64 eui-64

    user@JunOS# set rib inet6.0 static route fda9::2 next-hop fda1::2

    can use link local addres but the route type have to be fully specified 
    user@JunOS# set rib inet6.0 static route ::/0 qualified-next-hop fe80::1 interface ge-0/0/0  

```

* OSPF for ipv6 **RFC5340**
    - Fundamental mechanics of OSPF unchanged 
        | Fundamental|
        |--------|
        | Link-state Advertisement Flooding |
        |Areas |
        |Designated router elections |
        |stub|
        |Not-so-stubby(NSSA)|

    - Configuration mainly requires us to change ospf3 for ospf 

* Routing Policy
    - explicit routing poilicy 
    - `user@JunOS# edit policy-options`
    - exact, longer, orlonger, perfix-length-range /x -/y, upto
    
    1. Terminating actions 
        - accept 
        - reject 

    2. Flow control
        - next term 
        - next policy 

    3. Modifying Attributes 
        - community(add, delete and set)
        - preference
        - as path-prepend
        - next-hop 

    **Accept**
    - Causes the system to accept the packet and continue the input or output processing of the packet 

    **Discard**
    - Causes the system to discard the packet and send a message back to source address 

    - Flow Control  
        - next term 
        - next filter action doesn't exist 
    
    - Action Modifiers 
        - count, log and syslog 
        - forwarding-class and loss-priority 
        - policer 

```
    user@JunOS# edit policy-options 
    user@JunOS# edit policy-statement <name1>
    user@JunOS# set term <name2> from protocol static 
    user@JunOS# set term <name2> from route-filter 0/0 exact 
    user@JunOS# set term <name2> then accept 

    user@JunOS# top edit protocols ospf 
    user@JunOS# set export <name1>
```

* Firewall filter Use Cases 

```
    user@JunOS# commit confirmed 
    -----> when configuring the firewall filters best to use so that we don't get locked out 
    of the mgmt interface

    user@JunOS# edit firewall 

    user@JunOS> show firewall counter filter <name-of-filter> outbound-accept 
    user@JunOS> show firewall log 
    user@JunOS> clear firewall filter <filter name >
                           
```
    - [edit firewall family inet] => ensures that family inet filters are applied only to interfaces 
    running ipv4

* Policing 
    - rate-limiting 
    - enables us to limit the amount of traffic that passes into or out of an interface
    - works with firewall filters to thwart DoS attack
        - common actions include discard, specify the forwarding class and set loss-priority level 
    - interface-based policers 
        - L2VPN traffic, MPLS, IPV6 families 
    - Policing employs the token bucket algorithm 
        - enforces limit on average bandwidth
        - enabling burst upto specified maximum value 
    
    - Can configure two rate limits for the traffic 
        1. Bandwidth: the number of bits per second permitted on average 
        2. Maximum burst size: the total number of bytes the system permits in bursts of data that exceed the given bandwidth limit 

    - Maximum Burst size = Speed of the interface x Amount of time bursts 
    Eg. to permit burst on Ethernet link for 5ms 
        Burst Size = 100,000,000 bps  x (5/1000s) => 62,500bytes 
    
    - statements 
        bandwith-limit
        burst-size-limit

    - can use k, m, g for one thousand, one million and one billion bytes or bits 

* Unicast RPF (Reverse path-forwarding)
    - automates antispoofing filters based on routing table (RIB)
    - helps to validate packet receipt on the interfaces
    - strict(default) accept if :pkt source match or next-hop match 
    
    - Two modes
        - loose- accept of the pkt's src address matches a prefix in the routing table 
                 if default route is present, packets are always match loose mode 
    
    - Network where assymetric routing excists should activate following 

```

    routing-options{
        forwarding-table {
            unicast-reverse-path feasible-paths;
        }
    }

```     

    - only enable on edge router 
    - Fail filter run filter on the packet before discarding it 


* CoS 
    - mechanisms for categorizing traffic and meeting performance requirements within a network 
    - serial transmission delays 
    - Propagation delays 

    - CoS meets a network's performance requirements by:
        - Prioritizing latency-sensitive traffic such as VoIP 
        - Controlling congestion to ensure SLA maintenance 
        - Allocating bandwidth for different classes of traffic 

    ![CoS Processing](./images/cos-1.png)  
    
    - Fowarding Classes
        - identify traffic that should receive common treatment 
        - are used to assign traffic to output queues 
    
    - Packet Loss Priority (PLP)
        - identifies the priority a system should give to dropping a packet 
        - Is used to select the drop profile used in the random early detection (RED) process 
    
    - Scheduler: defines how a packet is treated in the output transmission queue 
    - Buffer Size: Defines the period  for which a packet is stored during congestion 
    - Scheduling Priority and Transmit Rate 
        - Determine the order in which a packet is transmitted 
    - Drop Profile 
        - Define how agressively a device drops packets that are using a particular scheduler 


* Junos OS Traffic Classification 
    - Multifield classifier 
    - Behavior aggregate(BA) 
        - Consistent CoS traffic  throughout the entire network 
        - Simplified task of creating and maintaining accurate multifield classifiers on each system 

    `user@JunOS# edit firewall family inet `

    `user@JunOS#  show filter <name eg. apply-cos-marking >`


    - Rewrite Rule 
        - modifies the appropriate CoS bits in an outgoing packet
        - examine the forwarding class and loss priority of packet 
        - set bits to a corresponding value specified in the rule 
        - [edit class-of-service]

    |Behavior Aggregate classifier| Multifield Classifier |
    | --------------------- | -----------------------------|
    | configured like regular firewall filters | configured by applying a rewrite rule to the outbound interface|
    | configured under [edit class-of-service interfaces] | configured under [edit firewall family inet] |

* Junos OS Traffic Queuing 
    - Fowarding classes map to queues 
    - 0: best effort 
    - 1: expedited-forwarding 
    - 2: assured-forwarding 
    - 3: network-control 

```
    user@JunOS> show class-of-service forwarding-class 
    user@JunOS# edit class-of-service 
    user@JunOS# set forwarding-classes queue 0 general-traffic 
    user@JunOS# set forwarding-classes queue 1 important-traffic 
    user@JunOS# set forwardig-classes queue 2 critical-traffic 
```

* Junos OS Traffic Scheduling 
    - Priority 
    - Transmission Rate (95% to queue 0 best-effort)
        - 5 % to network-control forwarding class queue 3
    - Buffer Size 
    - RED configuration 
    
    - Queue Priority 
        - Strict-high 
        - High
        - Medium-high
        - Medium-low 
        - Low 

    - Drop Profile 
        - queue fullness and the drop probability 

    * **Function of Scheduler and scheduler map**
        - Associate parameters with various queues 
        - Define CoS parameters for queue servicing 

    * **Scheduling components that define the storage and dropping of packets**
        - Buffer Size 
        - RED configuration


* CoS Use Case 

```
term from-professors {
    from {
        source-address {
            192.168.25.64/26;
        }
    }
    then {
        forwarding-class professors;
        accept;
    }
}
term from-students {
    from {
        source-address {
            192.168.25.128/25;
        }
    }
    then {
        policer student-policer; ## 'student-policer' is not defined
        forwarding-class students;
        accept;
    }
}
term default {
    then accept;
}

    user@JunOS> show interfaces queue ge-0/0/3
    user@JunOS> show class-of-service interface ge-0/0/3 --> displays queue stat for the specified interface 
```

* Lab
    - Configuring the best-effort or network-control forwarding classes or assigning them to their respective queues is not necessary because they are default CoS designations and assignments
    
    - RTP traffic uses udp and a port range of 16384-32767


* JTAC Support Case 
    - 'user@JunOS> request support information '

    - Priority  Critical(1), High(2), Medium(3), low(4)

    - Customer Support Login and Chassis Serial Number are required to open support case 
    - A valid maintenance contract is required to access the juniper Networks Support Services over the Web 
    
* Juniper Knowledge Base to research issue 
* Problem Report is the description of hardware or software defect 
* structre of PR: Number, title, release Note, Severity, **trigger, workaround**, status, resolved-in 
* chmod 444 to ensure all users have read access 


* **Juniper Security Concepts** 
    - Threat Surface Expanding 
    - Security Technology  Interaction 
    - Attack Frequency and Cost 
    - Security
        - Availability 
        - Scalability 
        - Manageability 
        - Performance 
        - Budget 
    - Security Management Challenges 
        - Too many isolated security appliances 
        - Many different threat scores 
        - Manual Coordination 

    - Layered stack of specialized products 
        - Intrusion Prevention
         - Endpoint protection 
         - Advanced Threat Prevention 
         - Application Security 
         - Data Loss Prevention 
        
    - Juniper Connected Security Components 
        - Operational Efficiency : centralized management 
        - Security Efficacy : must support advanced security features 
        - Business Agility : easily scale 

        - Performance, efficacy, scalability, automation, & centralized control 
        - **SRX Foundation Services**
            1. Firewall 
            2. NAT 
            3. VPN
            4. Routing 
            5. Reporting 
            6. Anakytics 
            7. Automation 
            8. Management 

        ![SRX Security](./images/srx.png)

        - Juniper Secure Connect : SSL-VPN application 
        - Juniper Identity Management Service(JIMS) windows based agent that collects and maintains a database of authenticaion information from Active Directory domains or syslog 
        - Features of JIMS
            - Centralized User Identity Data Collection : a scalable service that can take over user identity data collection 

            - Data Collection from Syslog Sources : collect event data and user information data from an event source eg. DHCP  

            - Domain PC probing : Acts as a supplement to event log reading 

            - System-Level IP address, Event and Group Filtering: Specify IP address ranges 

        ![NGFW Juniper](./images/firewall.png)
        
        - AppTrack : Analyzes application data and classifies it based on risk level, user ID, zones, source, and destination addresses 

        - ApppFW : Creates application control policies to allow or deny traffic based on application names 

        - AppQoS : Meters and marks traffic based on the application security policies set by the administrator 

        - SSL Proxy: Transparently performs SSL encryption between the client and the server 

        - IPS: Tightly integrates Juniper's application security features with the network infrastructure to provide threat mitigation and protection 

        - Unified Threat Management Services(UTM)
            - Consolidation of several security features into one device, protecting against multiple threat types  
            
        ![UTM](./images/utm.png)

        - Juniper Advanced Threat Prevention (JATP), SecIntel
        - Benefits of Juniper Connected security 
            1. Centralized Management and Visibility 
            - Visibility of North-South Traffic 
            - Ability to block East-West Traffic 


            2. Comprehensive Security 
            - Firewalls are right-sized for their application in the network 
            - Security on both public and private cloud environments 


            3. Protection from Advanced Malware 
            - Detect known and unknown threats 
            - Gathers and transforms the threat intelligence information into actionable items 


            4. Multivendor Integration 
            - Detect and enforce security across Juniper products and solutions 


            5. Access and application Mobility 
            - Consolidate threats from different sources to protect network access for all users 


            **Juniper Connected Security building blocks**
                - Management and Visibility 
                - Detection 
                - Enforcement 

* IPv6 
    - Device consider the neighbor as reachable in the process of Neighbor Discovery when
        - the device receives recent conifrmation from the neighbor recevice 
        - the received neighbor has processed IP traffic request 

    - Router solicitation messages to discover the presence and properties of on-link routers
    - EUI is a 64 bit HEX value used to identify an interface in stateless autoconfiguration 

    - `user@JunOS> show bgp summary`

    - `user@JunOS> show route table inet6.0 protocol static`

* IPV6 Over IPV4 Tunnels 
    - Approaches to Tunneling 
        - ipv4 compatbile addressing 
        - configured tunnels 
        - 6to4
        - 6over4
    - tunnel interface gr- for GRE 

```

    user@JunOS> ping <ipv6> source <ipv6> rapid count <count>
    user@JunOS> show interfaces gr-0/0/0.0 detail | find "traffic statistics"

```

**VLAN**
```
    [edit interface xe-0/0/2]
    user@JunOS# set unit 0 family ethernet-switching interface-mode {access | trunk } vlan members <vlan name>
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

    user@JunOS>  show ospf neighbor 
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

``` 
    user@JunOS# edit interfaces    
    [edit interfaces]
    user@JunOS# edit ge-0/0/0 unit 0 family inet 
    [edit interface ge-0/0/0 unit 0 family inet]
    user@JunOS#  exit 

```

* Examples of transit traffic 
    - SFTP traffic that enters and exits the same interface on the local routers 
    - SCP traffic that enters one interface and exists another interface on the local router 

* Benefits realized by the disaggregated Junos OS 
    - the platform drivers and FE are removed from the control plane to increase performance 
    - the architecture facilitates programmability through provisioning the control plane, the data path and the platform API

* A packet enters a Junos device. No matching destination entry exists in Forwarding table 
    - the PFE responds to the source with destination unreachable notification message 

* Each process operates in its own shared memory space 

* Control between Control Plane and Forwarding Plane 
    - Control traffic is preferred over exception traffic during congestion 
    - A rate limiter is configured by default 

* Reset the candidate configuration to active configuration rollback and rollback 0

* Max number of configurations recoverable using rollback command ==> 50 

* the purpose of load merge command 
    - to combine the current configuration with the configuration you load 

* Minimum system-defined user class required to issue clear commands 
    - operator 

* Ip address to use for packets sent to hosts on the same subnet
    - use  preferred 
    
    ![preferred](./images/preferred.png)


* traceroute on Junos devices
    - receives ICMP time-exceeded packets 
    - transmits UDP packets 

* Upgrading Junos OS but there is not enough space to complete the upgrade 
    - request system software add command no-copy 
    
    ![no-copy-command](./images/no-copy.png)


* Dynamic routing allows a device to automatically reroute traffic around a failure and participating devices dynamically 
learn routing information from each other 

* The purpose of discard action of a firewall filter => system silently drops the packet

* Ipv4 behavior for BGP 
    - Import all BGP routes into the inet.0 routing table 
    - Export active BGP routes to configured BGP peers 
    
    ![BGP-default-import](./images/bgp.png)

* Next Hop and the interface from routing table 
    
    ![routing-table](./images/next-hop.png)

* Discard - Silently drops 
* Reject - Drops the packet but ICMP error message is sent 
* Transit traffic : - goes in from ingress interface and out from egress interface 
* Exception traffic :- Forwarding table redirects to local RE Protocol update, ICMP( Ping, traceroute, Mgmt traffic )

* CHASSISD : starts up PFE's, interface and other hardware as well as copied run in the RE and each PFE 

* DCD : device control daemon, manages interfaces (encapsulation, timers, VLANs, IP's)
* MGD : management daemon, provides device management (ssh, telnet, web etc), handles CLI commands and configurations 
* RPD : routing protocol daemon, manages all routing protocols (BGP, OSPF, IS-IS, RIP etc), works out metrics, finds best paths etc, multithreaded process 
* SNMPD : SNMP daemon , responds to SNMP polling and sends SNMP traps 

* `show system processes [brief | extensive]`

* `show firewall log`


```
[edit firewall family inet filter BLOCK_vMX4]
term Block-ICMP {
    from {
        source-address {
            100.123.1.3/32;
        }
        protocol icmp;
    }
    then {
        log;
        reject;
    }
}
term Default-Term {
    then accept;
}

```

* Qualified next-hop (JunOS) = Floating static Route (IOS)

* `set routing-options router-id <router-id eg.1.1.1.1>`

* If we want to advertise our static routes with OSPF what type of pplicy will we use 
    - basically we're trying to redistribute static into ospf so we need to configure a policy on a router where we
    export static route into OSPF database 

```
    set protocols ospf export static-into-ospf 
    set protocols ospf area 0.0.0.0 interface em1.0
    set policy-options policy-statement static-into-ospf term static from protocol static 
    set policy-options policy-statement static-into-ospf term static then accept 
    
[edit policy-options]
jcluser@vMX1# show 
policy-statement static-into-OSPF {
    term Redistribute-static {
        from {
            protocol static;
            route-filter 0.0.0.0/0 exact;
        }
        then accept;
    }
}
[edit protocols ospf]
jcluser@vMX1# set export static-into-OSPF

jcluser@vMX1> show ospf database external extensive 

[edit policy-options]
jcluser@vMX1# show 
policy-statement static-into-OSPF {
    term Redistribute-static {
        from {
            protocol static;
            route-filter 0.0.0.0/0 exact;
        }
        then accept;
    }
    term Redistribute-static-E1 {
        from {
            protocol static;
            route-filter 0.0.0.0/0 exact;
        }
        then {
            external {
                type 1;
            }
            accept;
        }
    }
}

``` 

* Default Routing Import/Export Policy 

![Default-policy](./images/default-policy.png)

* Realtime Performance Monitoring (RPM)
    - measure performance between two endpoints using probes 
    - probes can use HTTP, ICMP, TCP and UDP 
    - one or more probes are grouped into a 'TEST'

```
[edit services rpm]
jcluser@vMX1# show 
probe Ping-GW {
    test ICMP {
        probe-type icmp-ping;
        target address 100.123.0.1;
        probe-count 10;
        probe-interval 3;
    }
}
``` 

* ping force and egress interface 

    `ping <IP address> bypass-routing`
    
* Monitor a Path 

    `traceroute monitor <ip address`


