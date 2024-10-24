IoT term is used to refer to
- the resulting globlal network of connecting smart objects
- technologies needed to realize such a vision
- applications and services leveraging such technologies

required features:
- devices hetereogeneity
- scalability
- data ubiquitous data exchange
- energy-optimized solutions

key features:
- localization and tracking (many applications require position and movement tracking)
- self-organization capabilities (support ad-hoc networks)
- semantic interoperability (standard formats for data)
- embedded security and privacy-preserving mechanism

#### Backscattering
- allows devices to run without battery
- only available at research level for now
- use radio frequency signals as power source
- two types
	- ambient
	- RFID

##### Ambient backscattering
- devices harvest power from signals available in the environment
- they use existing RF signals without requiring any additional

- Performance drawbacks
	- low data rate (about 1kbps)
		- not suitable for real-time applications that continuously exchange data
	- availability of signals
		- signal may not be available indoor or not powerful enough

##### RFID backscattering
- main advantage is the availability of RFID signal. Reader is always present in a RFID
##### Battery free smart home
- in a smart home there may be a lot of smart devices
- if every one of them has a battery, it's not good for the environment
- we can deploy an RFID reader with multiple antennas that covers all the different rooms
- of course RFID sensors can have very low capabilities, but they can run sensors!

### Communication
add scheme slide

RFID tags run EPC Global Standard
- in a smart home we may want less bits dedicated to the tag id and more dedicated to the actual data
- 8 bits for ID
- 6 bits for data
- 4 for CRC

### Infrastructure based wireless networks
- base stations connected to wired backbone network
- stations choses the closest base station
- Limits
	- when no infrastructure is available
	- expensive/inconvenient to setup
	- when there is no time to set it up