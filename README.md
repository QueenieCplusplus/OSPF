# OSPF

Open Shortest Path Forward

* main part:

> Single Area & Mulitple Area & Stub Area

> ROUTER ID cmd

> Default GW Route

> Link-state DB

LB

* cmd part:

> show ip ospf interface 

> show ip ospf neibor

> show ip ospf border-routers

* virtual part:

> Connection to Remote Area

* NBMA, fully meshed

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

