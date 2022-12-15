# HSRP-Configuration

Configuring Hot Standby Router Protocol

![HSRP](https://github.com/Hsnbacha/HSRP-Configuration/blob/main/HSRP.png)

First of all we did installation for our routers and Switches including a ISP and Server and 1 
pc for each switch

Second we assign IP address and Hostnames for all routers as we put the given on the each interface 

After that configure the interfaces and routings on all the routers

We assigned a Virtual ip between the R1 and R2 routers 

We use this Virtual Ip in Default gateway in 2 PC's (Note: The default gateway for Hosts 1 ,and 2 is configured with the HSRP virtual IP address (172.16.1.254, in this case))

In my case i assigned IP route in the router in the following commands:

R1= ip route 0.0.0.0 0.0.0.0 203.1.1.2

R2= ip route 0.0.0.0 0.0.0.0 202.1.1.2

The 0.0 0.0 designation has multiple meanings, but in the context of IP routing, 0.0 0.0 refers to when a data packet does not specify the destination subnet it needs to be forwarded to. It generally means that the data packet doesn't have any more different remote routers to hop across.

While for The ISP Router I assigned 2 ip route with the following command:

ISP Router= ip route 172.16.1.0 255.255.255.0 203.1.1.1

ISP Router= ip route 172.16.1.0 255.255.255.0 202.1.1.1

Then we go for checking by opening cmd in both PC and try to ping the Server with command:
ping 8.8.8.8

and all works well 

we also check what way the ping pass through tracert 8.8.8.8

For Configuring HSRP we use this commands:

For int f0/0

standby 1 ip 172.16.1.254

standby 1 priority 150 (we assigned for router 1 'priority 150' and for R2 'priority 2')

standby 1 preempt

Here we assign an active router and standby router 

R1 is the active router and tracks the R1 interface state. When R1 is the active router all the traffic from the hosts (Host 1, 2) to the servers is routed through R1.

R2 is the standby router and tracks the R2 interface state.

If the R1 interface goes down, the R1 HSRP priority is decrease. At this point the R2 HSRP priority is higher than R1, and R2 takes over as the active router.

When R2 becomes the active router all the traffic from the hosts to the servers is routed through R2.

and to check we can type the command after enabling the R1 router 

show standby brief 

and then we will have what is active and what is standby and what is the Virtual IP 

So at last we check by shutting down the interface f0/0 in R1 the open any pc and type 
Tracert 8.8.8.8 and we can see how the routing changed from R1 to R2 because the server went down.

THANK YOUU



