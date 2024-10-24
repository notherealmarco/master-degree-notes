The main difference between an RFID network and a WSN is that nodes:
- are battery powered
- can sense the environment
- can listen to the channel (carrier sense) and transmit spontaneously
- can make more complex computation
- can send packets to other nodes (e.g. for multi-hop communication)

#### Roles of partecipants in WSN
- Sources of data: measure data, report them somewhere
- Sinks of data: interested in receiving data from WSN
- Actors/actuators: control some devices based on data

#### Deployiment options
- Random deployiment
	- dropped from an aircraft
	- usually uniform random distribution for nodes over finite area is assumed
- Regular deployment
	- wel planned, fixed
	- not necessarily geometric structure, but that is often a convenient assumption
- Mobile sensor nodes
	- Can move to compensate for deployment shortcomings
	- Can be passively moved by some external force (wind, water)
	- Can actively seek out "interesting" areas
#### Characteristics of WSN
- Scalability
	- they need to support **large number of nodes**
	- performance should not degrade with increasing number of nodes
- Wide range of densities (very application dependent)
- Limited resources for each device
	- low amount of energy
	- low cost, size and weight
	- nodes may not have a global ID (e.g. an IP)
- Mostly static topology

- Service in WSN (not simply moving bits like traditional networks)
	- in-network processing
	- provide answers
	- comunication is triggered by events
	- asymmetric flow of information (from sensors to sink)
- QoS
	- traditional metrics do not apply
- Fault tollerance
	- be robust against node failure
	- running out of energy, physical destruct
- Lifetime
	- the network should fulfill as long as possible
	- lifetime of individual nodes relatively unimportant
	- but if a critical node dies, the network dies
- Programmability
	- being able to re-program nodes on-field, to improve flexibility
- Maintainability
	- WSN has to adapt to changes

#### Typical Adopted Mechanisms
- Multi-hop wireless communication
- Energy-efficient operation (both for computation, sensing, actuation)
- Self-configuration
- Collaboration & in-network processing
	- the nodes in the network collaborate towards a joint goal
	- pre-processing the data before sending it to the sink, to improve efficiency

#### Mechanism to meet requirements
- Data centric networking
	- focussing network design on data, not on node identifiers
- Locality
	- do things locally as far as possible
- Exploit tradeoffs
	- e.g between invested energy and accuracy

> [!PDF|yellow] [[3 WSN.pdf#page=29&color=yellow|3 WSN, p.29]]
> > WSN: reasoning of existence
> 
> collect, couple, provide, establish
#### Main sensor node components
- antenna and RF transceiver
- memory unit
- CPU
- sensor unit (i.e. thermostat)
- power source (typ. battery)
- operating system
	- TinyOS

sensing, processing and networking is all done by the sensor node.

#### WSN vs conventional networks

| **Conventional networks**                                           | **WSN**                                                   |
| ------------------------------------------------------------------- | --------------------------------------------------------- |
| general purpose design                                              | serving a single application or a bouquet of applications |
| network performance and latency                                     | energy is the primary challenge                           |
| devices and networks operate in controlled / mild environments      | unattended, harsh conditions & hostile environments       |
| global knowledge is feasible and centralized management is possible | localized decisions - no support by central entity        |
#### Wireless signal issues
- **Attenuation**: the strength of the signal decreases rapidly over distance
- **Multi-path propagation**:
	- when a radio wave encounter an obstacle, all or part of the wave is reflected, with a loss of power
	- a source signal can arrive, to successive reflections, to reach a station through multiple paths
- **Interference:**
	- from the same source (multi-path propagation): signal arrives multiple time
	- from multiple sources: more stations transmit simultaneously

We use **SNR** to measure the ratio of good to bad signal (signal to noise). Higher is better.

> [!PDF|yellow] [[3 WSN.pdf#page=49&selection=77,0,77,15&color=yellow|3 WSN, p.49]]
> > Synchronization
> 
> nodes have clocks but they may not be synchronized!

To address these issues, we use MAC protocols. We need a protocol suitable for wireless networks, which emphasize energy-efficient operation.

##### MAC protocols:
- controls how the shared medium is used by different devices
- controls when to send a packet and when to listen for a packet
- avoids collisions, reducing retransmissions
- increases energy efficiency, aviding idle listening
- allows scalability, lower latency, fairness and better throughput

### Reasons of energy waste
- Collision: they need to be discarded and retransmitted
- Overhearing: node receiving packet destined to other nodes
- Overhead caused by control-packets
- Idle listening
- Overemitting: destination not ready

#### Techniques for WSN MAC
- Contention based
	- on-demand allocation for those that have frames for transmission
	- sensing the carrier before transmitting
	- scalable, no center authority
	- idle listening / interference / collisions / traffic fluctutations -> higher energy consumption
	- hidden/exposed terminal problem
- Scheduled based:
	- schedule that specifies when, and for how long, each node may transmit over the shared medium
	- energy efficient
	- interference, collisions are not a problem
	- synchronization is a problem
	- there is a central authority
### CSMA/CA
In wireless network we cannot detect collisions and interrupt an ongoing transmission. But we can try to avoid them as much as possible.


![[Pasted image 20241002114133.png]]

Contention window is random, so hopefully only a node starts transmitting at the same time. The backoff clock is randomly chosen between [0, CW-1], where CW represents a contention window.

The first station whose clock expires starts transmission. Other terminals sense the new transmission and freeze their clocks to be restarted after the completion of the current transmission in the next contention period. Length of backoff time is exponentially increased (doubled) as the station goes through successive retransmissions.

![[Pasted image 20241002234319.png]]

By using different IFS sizes, we can prioritize some packets.
- SIFS is used for immediate responses (ACK, CTS, polling response)
- PIFS (Point coordination function IFS): medium priority, for real time services, is SIFS + 1 time slot
- DIFS (Distributed coordination funcion IFS): lowest priority, for async data service, is SIFS + 2 time slots

#### DFC CSMA/CA with ACK
- stations has to wait for DIFS before sending data
- receiver ACKs immediately (after waiting for SIFS) if the packet was received correctly (CRC is valid)
- ACK is transmitted without sensing the medium
- if ACK is lost, retransmission will be done

![[Pasted image 20241002235016.png]]


#### Problems
- Hidden terminal
- Exposed terminal

We fix both of them with RTS/CTS:
- transmitter sends an RTS (request to send) after medium has benn idle for time interval more than DIFS
- receiver responds with CTS (clear to send) after medium has been idle for SIFS
- data is transmitted
- RTS/CTS is used for **reserving channel** for data transmission so that the collision can only occur in control message.
![[Pasted image 20241002235259.png]]

Both of the problems are solved!

However, collisions are still possible as RTS packets can collide. But RTS packets are small, RTS collisions are not as bad as data collisions.

### Network Allocation Vector (NAV)
- provides *Virtual Carrier sensing*
- most 802.11 frames carry a duration field
- transmitter sets the NAV to the time for which it expects to use the medium
- other stations starts counting down from NAV to 0
- If NAV > 1, transmission is delayed. When NAV = 0, carrier is sensed.

![[Pasted image 20241002235837.png]]

##### CSMA/CA with RTS/CTS (NAV)
- receiver receives RTS and sends CTS after SIFS
- CTS again contains duration field
- all the stations receiving the CTS need to adjust their NAV
- sender sends data after SIFS
- receiver sends ACK after SIFS

### Communication patterns
- **Broadcast**
	- a broadcast pattern is generally used by a base station (sink) to transmit information to all sensor nodes of the network
- **Convergecast or data gathering (all/many to 1)**
	- all or a group of sensors comunicate to the sink
	- typically used to collect sensed data

## S-MAC protocol
S-MAC: Sleep MAC

As idle listening consumes significant energy (50-100% of the energy required for receiving), the solution is to periodic listen and sleep, with a listen duty cycle of about 10% (e.g. listen 200ms and sleep 2s).

![[Pasted image 20241011175026.png]]

- Duration of sleep and listen cycles are the same for all nodes
- All nodes are free to choose their own listen/sleep schedules
- to reduce control overhead, **neighbor nodes are syncronized together**
- neighboring nodes form **virtual clusters** so as to set up a common sleep schedule
	- each node maintaines a table with neighbors’ schedule
	- table entries are filled when the node receives sync packets
- SYNC packets are exchanged periodically to maintain schedule synchronization
	- they are sent every SINCHRONYZATION PERIOD
- Receivers will adjust their timer counters immediately after they receive the SYNC packet

- If there are no neighbors, the node will chose a random schedule. These nodes will be called **synchronizers**, nodes who receive a schedule are called **followers**.

- In a large network we cannot guarantee that all nodes follow the same schedule
- node on the border will follow both schedules
	- they need to broadcast packet twice, for schedule 1 and 2
	![[Pasted image 20241011180413.png]]


#### Collision avoidance
- RTS/CTS with duration is used (so NAV is used for backoff)
- carrier sense before initiating a transmission
- neighbor nodes of both sender and receiver sleeps during transmission to save power

- listen time is divided into minislots
	- sender selects a minislot to end carrier sense
	- if channel is free it transmits SYNC in the next minislot

### S-MAC Performance evaluation
- Topology: Two-hop network with two sources and two sinks
- Sources periodically generate a sensing message which is divided into fragments
- Traffic load is changed by varying the inter-arrival period of the messages: (for inter-arrival period of 5s, a message is generated every 5s by each source. Here it varies between 1-10s)
![[Pasted image 20241011182036.png]]

- In each test, there are 10 messages generated on each source node
- Each message has 10 fragments, and each fragment has 40 bytes (200 data packets to be passed from sources to sinks)
- The total energy consumption of each node is measured for sending this fixed amount of data
![[Pasted image 20241011182343.png]]

- S-MAC consumes much less energy than 802.11-like protocol without sleeping
- At heavy load, idle listening rarely happens, energy savings from sleeping is very limited
- At light load, periodic sleeping plays a key role

Conclusions:
- A mainly static network is assumed
- Trades off latency for reduced energy consumption
- Redundant data is still sent with increased latency