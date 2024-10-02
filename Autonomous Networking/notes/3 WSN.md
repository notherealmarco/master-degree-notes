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
### CSMA/CA
![[Pasted image 20241002114133.png]]

IFS is random, so hopefully only a node starts transmitting at the same time.