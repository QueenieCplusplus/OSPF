# OSPF

Open Shortest Path Forward

------------------------------------------------------------------------------

* main part:

> Single Area & Mulitple Area & Stub Area (omit)

> ROUTER ID cmd

> Default GW Route

> Link-state DB


------------------------------------------------------------------------------

* LB

------------------------------------------------------------------------------

* virtual part:

> Connection to Remote Area


------------------------------------------------------------------------------

* NBMA, fully meshed

------------------------------------------------------------------------------

* cmd part:

> show ip ospf interface 

> show ip ospf neibor

> show ip ospf border-routers

> show ip ospf database

> show ip ospf vitual-links

------------------------------------------------------------------------------
# OSPF Hierarchical Routing



                   ---------------------------
                  |                           |
    Internet  -- (R) ---- (R) Backbone(area0) |
                  |                           |
                  |------(R)----------(R)-----
                  |             |             |
                  |    area1    |    area2    R --- Internet
                  |             |             |
                  -----------------------------
------------------------------------------------------------------------------
# OSPF Router ID

Cisco IOS provides a method for assigning an OSPF ID equal to the desirable OSPF ID.

    $router-id <OSPF ID> 
    # OSPF id ranging from 0-255

(to be continued)

------------------------------------------------------------------------------
# Default GW Route

https://github.com/QueenieCplusplus/GW

------------------------------------------------------------------------------
# Link-state DB

to see router's link-state db
by entering cmd without param

    r3$show ip ospf database
    
    > output
    
    OSPF Router wiht ID (10.0.0.3) (PID 1)
    
    
             Router Link States (Area 0)
     Link ID       ADV Router     Age      Seq$       Checksum    LinkCount
     10.0.0.2      10.0.0.2                                          6
     10.0.0.3
     "10.0.0.4"
                                   ...(omit)
     192.168.255.5 192.168.255.5                                     2
     
                         
             Net Link States (Area 0)
      Link ID       ADV Router     Age      Seq$       Checksum
     10.0.5.2      10.0.0.4       173     bits_val     hex_val
     "10.2.0.2"    10.0.0.3
     
 to see cmd output with param 10.0.0.4
 
      r3$show ip ospf database 10.0.0.4
      
      > output
    
      OSPF Router wiht ID (10.0.0.3) (PID 1)
    
    
             "Router" Link States (Area 0)

 to see cmd output with param 10.2.0.2
 
      r3$show ip ospf database 10.2.0.2
      
      > output
    
      OSPF Router wiht ID (10.0.0.3) (PID 1)
    
    
             "Net" Link States (Area 0)
      
             
   
------------------------------------------------------------------------------
# LB == Parallel Routes

OSPF, like all other routing protocol (sources) available on Cisco Rputers, can install multi-routes for the same mask.

This feature enable LB of traffic destined for the same mask (network prefix).

         $maximum paths <# of paths> 
         # of paths, shall be under OSPF config
         # max num is 4
         # if param is 1 in value, then it means no LB is enable.

------------------------------------------------------------------------------
# Virtual Connection to Remote Areas

Vitual Links are established on a PPP basis between 2 Routers there are connected to common area, one of which shall be connected to OSPF Area0.

                       ---------Area 0--------
                       |      BackBone       |
                       |                     |
                       |      segment1       |
                       |     10.0.1.0/24     |
                       |                     |
                       |     |        |      |
                       |    e0        e0     |
                       ----(R)-------(R)------
         -------------------|  Area10  |------------------
         |                  s0        s0                 |
         |                                               |
         |                  |         |                  |
         |                  |         |                  |
         |                                               |
         |                 s0        s0                  |
         |                  |         |                  |
         |                 (R)       (R)                 |
         |                  |         |                  |
         |                 e0         e0                 |
         |                                               |
         |                   Segment10                   |
         |                  10.10.4.0/24                 |
         |                                               |
         |                  |         |                  |
         |                  |         |                  |
         |                                               |
         |                 e0         e0                 |
         |                  |         |                  |
         -------------------(R)------ (R)-----------------
                            |         |
                 --------- to0----  --- to0 --------
                 |           |   |  |    |          |
                 |   segement100 |  | segement200   |
                 | 10.100.1.0/24 |  | 10.200.1.0/24 |
                 |               |  |               |
                 |    Area 100   |  |   Area 200    |
                 |               |  |               |
                 ----------------   -----------------

Router N's config

Each Router must under OSPF cmd config, like

     Rn$ area <area> virtual-link <router ID>
     
area, these 2 routers connected to, and the virtual-link is establish thru.

router ID, the OSPF ID of the counterpart router.

lo == loopback

e == thernet

s == serial

    ip subnet-zero
    
    > interface lo0
    > ip address 10.0.0.1 255.255.255.0
    
    > interface e0
    > ip address 10.0.1.1 255.255.255.0
    
    > interface s0
    > ip address 10.10.255.5 255.255.255.252
    
    bandwidth 64
    
    router ospf 1
    
    > network 10.0.0.0. 0.0.255.255 area0
    > network 10.10.0.0 0.0.255.255 area10
    > area0 range 10.0.0.0. 255.255.0.0
    > area0 range 10.10.0.0 255.255.0.0
    > area10 virtual-link 10.0.0.5
    
    ip classless
    
