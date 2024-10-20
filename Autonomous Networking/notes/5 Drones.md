#### Unmanned Aerial Vehicle (UAV)
- UAV, commonly known as a Drone, is an aircraft without a human pilot aboard (unmanned or uncrewed)
- it operates
	- under remote control by a human
	- autonomously by on board computers

**Weight:** from 0.5g 15000kg
**Maximum speed:** up to 11265Kph
**Propellant:**
- fossil fuel
- battery

#### Usages
- can provide timely disaster warnings
- medical supplies
- dangerous situations
- traffic monitoring, wind estimation, remote sensing...

I many scenarios, UAVs need to exchange a relatively large amount of data among themselves and/or a control station. In many case there isn't any network infrastructure available. Drones can also used to expand terrestrial communication networks.

Drones can be equipped with several standard radio modules:
- Wi-Fi
- Cellular
- LPWAN (low power wide area network, es. LoRa)

## Routing
may require multi-hop data connections
#### Comparison WSN with Dronets

|                    | **WSN**                           | **Dronet**                       |
| ------------------ | --------------------------------- | -------------------------------- |
| **Mobility**       | none or partial                   | high, even 3D                    |
| **Topology**       | random, star, ad-hoc node failure | mesh                             |
| **Infrastructure** | absent                            | absent                           |
| **Energy source**  | battery                           | battery (very limited)           |
| **Typical use**    | environmental monitoring          | rescue, monitoring, surveillance |

**Goals of routing protocols:**
- increase delivery ratio
- loop freedom
- low overhead
- reduce delays
- energy consumption
- scalability

### Proactive routing protocols
Are they suitable for UAV networks?
- slow reaction to topology changes, will cause delays
- bandwidth constraints

Protocols:
- OLSR - Optimize Link State Routing
- DSDV - Destination-Sequenced Distance Vector
- B.A.T.M.A.N. - Better Approach to Mobile Ad Hoc Network

### Reactive protocols
- DSR - Dynamic Source Routing
- AODV - Ad hoc On Demand Distance Vector

#### Hybrid protocols
- ZRP - Zone Routing Protocol
- TORA - Temporarily Ordered Routing Algorithm

### B.A.T.M.A.N.
A proactive, distance-vector routing protocol for Mobile Ad-hoc Networks (MANETs) and Mesh Networks. Designed for decentralized decision-making and self-organizing networks.

**Key features:**
- decentralized routing
	- no node has global knowledge of the entire network
- next-hop based
	- nodes only know their best-hop neighbor for reaching a destination
- link quality driven
	- decisions are based on the quality of the link between nodes
- self-healing
	- adapts to changes automatically

**How batman works**
- Originator messages (OGMs):
	- broadcast to announce its presence
	- OGMs are forwarded by neighbors to propagate through the network
	- each OGM carries a sequence number to ensure the information is up-to-date and avoid routing loops
	- nodes evaluates how frequently they receive OGMs to their neighbors to determine link quality
	- each node maintains a routing table with the best next-hop neighbor based on link quality

- fields inside OGM
	- originator address
	- sequence number
	- TTL (hop limit)
	- LQ (quality between the sender and the originator)
	- hop count

Asimmetry problem:
If A can reach B well, B thinks it can reach A well too. But it may not be the case.
To overcome the issue there is the Transmit Quality (TQ) algorithm.

B transmits RQ (receive quality) packet to A. A counts them to know the link quality.
A knows the echo quality by counting the rebroadcasts of its own OGMs from its neighbors.
Dividing echo quality by receiving quality, A can calculate the transmit quality.

**propagation**
A when originates the OGMs, it sets TQ to 100%. The neighbor computes their own local link quality into the received TQ value and rebroadcast the packet.
- $TQ = TQ_{incoming} * TQ_{local}$

![[Pasted image 20241017154152.png]]

### Geographic protocols
the geographical position information of the nodes is utilized for forwarding decisions.
Nodes knows their position by GPS.

Geographic routing schemes don't need the entire network information
- no routing discovery
- no routing tables
- forward packet based on local information
- less overhead, bandwidth and so energy consumption
- for routing decisions a drone needs only the neighbors and destination position

Every node has coordinates of neighbors.

**Dead end problem**
several techniques have been defined in sensor networks to recover from a dead end but they are often not applicable to dronets.

Geo routing is based on three main approaches

##### Greedy forwarding
as stated before

#### Store-carry and forward
When the network is intermittently connected, forwarder nodes do not have any a solution to find a relay node. Not possible to forward any data packet to a predefined node which is not in range. So the current node will carry the packet until it meet another node or the destination target itself.

#### Prediction
based on geographical location, direction, and speed to predict the future position of a given node. They will predict the position of a next relay node.

### DGA algorithm
![[Pasted image 20241017161724.png]]
![[Pasted image 20241017161747.png]]

![[Pasted image 20241017161803.png]]

