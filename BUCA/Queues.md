Challenges
- Atomicity
	- you write to the queue first
	- you write to the db first
	- we need a transaction!
- Idempotence
	- we need locking mechanism on the queue, to avoid multiple process to process the same element
	- solution: replay protection (lastUpdate in the db), but still I have the problem of multiple process reading the same element

## Data replication
- Waiting for all replicas
	- definitely slower
	- safer for the data
- Waiting only for the primary replica
	- faster but...
	- replica may fail
	- not getting always the same value
	- ok for maybe Instagram feed, but not for e-mails

## Paxos
Paxos overview:
- partecipnats are: Writers & Replicas
- 2 phase protocol:
	- **prepare** reserves right to supply value, and robs other
	- **accepts** supplies the value **v**
	- replicas are stateful
	- communication is ordered by "proposal number" **n**
	- uses "viral propagation"

#### Algorithm
- Prepare phase
	- Success if replica hasn't prepared a higher proposal
	- Fail otherwise
	- Success with value if we have accepted another value (viral property)

Majority succeded -> Go to Accept Phase

- Accept phase
	- Success only if replica hasn't Prepared a higher proposal (same is ok)

### Mesh
- full mesh
- subsetting: not every node connected to every other node: in case of a query of death, only connected nodes will fail
- proxying: adding a layer of proxies (load balancers), that use a lookup service to route the request

### Strong stickiness
Some applications like chat, online game servers, video calls ecc. need a very strong stickiness.
In these cases, we may not want to use consistent hashing. Maybe we can use it to pick a server and then stick with it.

Also, it can happen that some users are not able to reach a specific server, but other does.
Using leases might be a solution.

- Request from Alice
- lookup server response is cached
- we have a failure
- we call Set to change the server, but the call stalls until the lease expires. Also, the lookup server might be able to still reach the server and renew the lease