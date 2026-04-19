# Cisco Packet Tracer : Homework 1
Date : 16 Mar 2026
Packet Tracer Version : 9.0.0+

## Objective
Create a chain of 2911 Routers using the RIP protocol until you exceed RIPs maximum hop count.


## Purpose
Become familiar with the basic process and commands for configuring Cisco routers.

## Notes
For each device:
1. Add the device 
    1. Drag a 2911 router to the logical diagram.
    2. Rename the router in logical view.
    3. Use the automatic connection to connect the new router to the previous router.
2. Configure The router
    ```
    > n     ## avoid startup config.
    > enable
    # config term 
    ```
    1. Change the Hostname.
        Adhere to common practice of capitalizing words and using dashes to separate parts of the hostname.
        ```
        (config)# hostname Router-X
        ``` 
    2. Configure the interface `gig0/0`.
        ```
        (config)# int gig0/0
        ```
        1. Change the IP address of the interface `gig0/0`.
            ``` 
            (config-int)# ip addr A.B.C.D 255.255.255.0
            ```
        2. Change the interface to up.
            ```
            (config-int)# no shut
            (config-int)# do show ip int brief      ## Verify int state and IP.
            ```
    3. Configure the interface `gig0/1`.
        ```
        (config)# int gig0/1
        ```
        1. Change the IP address of the interface `gig0/1`.
            ``` 
            (config-int)# ip addr A.B.C.D 255.255.255.0
            ```
        2. Change the interface to up.
            ```
            (config-int)# no shut
            (config-int)# do show ip int brief      ## Verify int state and IP.
            ```
    4. Configure the loopback interface interface
        ```
        (config)# int lo 0
        ```
        1. Set the IP address of the interface `lo0`.
            ```
            ip addr A.B.C.D 255.255.255.0
            ```
    5. Configure the RIP protocol.
        ```
        (config)# router rip
        ```
        1. Set version 2
            ```
            (config-router)# version 2
            ```
        2. Disable Auto-Summary
            ```
            (config-router)# no auto-summary
            ```
        3. Announce the IP spaces on RIP
            ```
            (config-router)# do show ip route       ## Show IP spaces to add
            (config-router)# net 10.0.C1.0          ## IP of gig0/0
            (config-router)# net 10.0.C2.0          ## IP of gig0/1
            (config-router)# net 1.1.X.0            ## IP of lo0
            (config-router)# do ping 1.1.(X-1).1    ## Verify connectivity
            ```
    6. Test ping to lo0 of Router-1
        ```
        # ping 1.1.1.1
        ```
    7. Copy running configuration to memory
        ```
        # write memory      
        # copy running-config startup-config        ## write memory does this under the hood
        ```