### Communication patterns
- **Broadcast**
	- a broadcast pattern is generally used by a base station (sink) to transmit information to all sensor nodes of the network
- **Convergecast or data gathering (all/many to 1)**
	- all or a group of sensors comunicate to the sink
	- typically used to collect sensed data

We need to define a good MAC protocol for wireless sensor networks, the following attrivutes must be considered:
- energy efficiency
- scatability and adaptability to changes
Latency, throughput and bandwidth utilization may be secondary, but desirable.

### Reasons of energy waste
- Collision: they need to be discarded and retransmitted
- Overhearing: node receiving packet destined to other nodes
- Control-packet overhead
- Idle listening
- Overemitting: destination not ready

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

### Routing
- routing technique is needed to establish multi-hop communication
- the routing strategy should ensure
	- mminimun energy consumption
	- maximization of the network lifetime

#### Ad Hoc Routing Protocols – Classification
- **network topology**
	- flat
	- hierarchical
- **which data is used to identify nodes**
	- arbitrary identifier
	- the position of a node
		- can be used to assist in geographical routing problems to decide next hop
		- scalable and suitable for sensor networks

##### Flat routing protocols
Three main categories

- Proactive protocols (table driven)
	- always tries to keep routing data up-to-date
	- active before tables are actually needed
		- routes are always already known
		- more bandwidth and energy usage
- Reactive protocols
	- route determined only when needed
	- operates on demand
		- when a route is needed, a kind of global search is started
			- causes delays if routes are not already cached
- Hybrid protocols
	- combination of these behaviors

### Destination Sequence Distance Vector (DSDV)
- based on bellman-ford algorithm
- proactive protocol
- add aging information to avoid routing loops
- on topology change, send incremental route updates
- unstable route updates are delayed

![[Pasted image 20241011191033.png]]

- to avoid loops, DSDV adds a **sequence number** to each routing table entry which is periodically updated. Routes with higher sequence number are preferred

##### Reactive protocols

### Flooding
- copies of incoming packets are sent by every link except the one by which the packet is arrived
- generates a lot of superfluous traffic
- flooding is a reactive technique, and does not require costly topology maintenance and complex route discovery algorithms

Characteristics:
- derivery is guaranteed (e grazie al cazzo)
- one copy will arrive by the quickest possible route (wow)

Drawbacks:
- implosion: duplicated messages are broadcasted to the same node
- overlap: if two nodes share the same under observation region, both of them may sense the same stimuli at the same time. As a result, neighbor nodes receive duplicated messages
- resource blindness (no knowledge about the available resources)
- does not take into consideration all the available energy resources
- consumes a lot of energy

### Gossiping
- nodes send the incoming packages to a randomly selected neighbor
- avoids implosion, but it takes long to propagate the message

### Dynamic Source Routing (DSR)
- Source routing: Each data packet sent carries in its header the complete, ordered list of nodes through which the packet will pass
- The sender can select and control the routes used for its own packets and supports the use of multiple routes to any destination


not finished yet