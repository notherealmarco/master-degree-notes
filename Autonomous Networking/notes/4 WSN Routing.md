
-  routing technique is needed to establish multi-hop communication
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
- Including the route in the header of each packet, helps other nodes forwarding the packet to cache the routing information for future use

**DSR is composed by two main mechanism:**
- **Route Discovery**
	- mechanism by which a node S obtains the route to a destination node D
	- used only if S doesn't already know the route
	- every request contains an unique ID and a route record
		- S sends a broadcast Route Request packet
		- every node broadcasts the packet (with the same ID) appending their own address to the route record
		- when D receives the request, it sends a Route Reply back to the initiator S, with a copy of the route record
			- more than a route can be returned, making the protocol more resistent to changes in network topology
- **Route Maintenance**
	-  mechanism by which a node S can detect (while sending a packet to D) if the network topology has changed and the route can't be used anymore

![[Pasted image 20241012174130.png]]

### Ad-hoc On Demand Distance Vector routing (AODV)
- a mix between DSR and DSDV
- nodes maintain routing tables
- sequence numbers added to handle stale caches (when routing info is too old)
- nodes remember from where a packet came and populate routing tables

We can see it as an improved DSDV, as it minimizes the number of required broadcast by creating routes on a demand basis.
- if the source node S does not have the route to D, S initiates a path discovery process to locate D
- the route request is broadcasted to the neighbors
- when a node receives a broadcast RREQ, it records in their table the address of the neighbor who sent the request
- when the destination or an intermediate node that can reach the destiation is reached, it replies with a route reply (RREP) packet back to the neighbor from which it first received the RREQ
- the RREQ is routed back along the reverse path
- nodes along the path updates their table

![[Pasted image 20241012175224.png]]

## Geographical routing
- routing tables contain information to which next hop a packet should be forwarded
- this can be explicitly constructed, or implicitly inferred from physical placement of nodes
	- by knowing the position of nodes, we can send the packet to a neighbor in the right direction
	- we can also do *geocasting*: sending to any node in a given area
	- to map a node ID to the node position we might need a location service

Strategies
- **Most forward within range r**
	- send to that neighbor that realizes the most forward progress towards destination, but staying in a range r (quello che stando nel range r si avvicina di più geograficamente parlando)
- **Nearest node with (any) forward progress**
	- the opposite as the previous strategy
	- minimizes transmission power
- **Directional routing**
	- choose next hop that is angularly closest to destination (closest to the connecting line to destination)
	- come se tracciassi una linea tra S e D e prendessi gli hop più vicini alla linea
	- problem: might result in loops!

#### Dead ends problem
![[Pasted image 20241012182403.png]]

## Conclusion
- there are many other protocols
- the best solution depends on network characteristics
	- mobility
	- node capabilities
- geographic approach allows to save more energy
- proactive approach is fast, but involves overhead
- reactive approach generate much less overhead, but it is slower