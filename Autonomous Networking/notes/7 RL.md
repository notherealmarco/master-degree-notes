Case study: battery-free smart home
- each device produces a new data sample with a rate that depends on the environment and the user (continuously, event based / on demand...)
- a device should only transmit when it has new data
	- but in backscattering-based networks they need to be queried by the receiver

In which order should the reader query tags?
- assume prefixed time slots
- TDMA with random access performs poorly
- TDMA with fixed assignment also does (wasted queries)
- we want to query devices that have new data samples and avoid
	- data loss
	- redundant queries

Goal: design a mac protocol that adapts to all of this.
One possibility is to use Reinforcement Learning

#### Reinforcement learning
How can an intelligent agent learns to make a good sequence of decisions

- an agent can figure out how the world works by trying things and see what happens
- is what people and animals do
- we explore a computational approach to learning from interaction
	- goal-directed learning from interaction

RL is learning what to do, it presents two main characteristics:
- trial and error search
- delayed reward

- sensation, action and goal are the 3 main aspects of a reinforcement learning method
- a learning agents must be able to
	- sense the state of the environment
	- take actions that affects the state

Difference from other ML
- no supervisor
- feedback may be delayed
- time matters
- agent action affects future decisions
- a sequence of successful decisions will result in the process being reinforced
- RL learns online

Learning online
- learning while interacting with an ever changing world
- we expect agents to get things wrong, to refine their understanding as they go
- the world is not static, agents continuously encounter new situations

RL applications:
- self driving cars
- engineering
- healthcare
- news recommendation
- ...

Rewards
- a reward is a scalar feedback signal (a number)
- reward Rt indicates how well the agent is doing at step t
- the agent should maximize cumulative reward

RL based on the reward hypotesis
all goals can be described by the maximization of expected cumulative rewards

communication in battery free environments
- positive rewards if the queried device has new data
- else negative

Challenge:
- tradeoff between exploration and exploitation
- to obtain a lot of reward a RL agent must prefer action that it tried in the past
- but better actions may exist... So the agent has to exploit!

exploration vs exploitation dilemma:
- comes from incomplete information: we need to gather enough information to make best overall decisions while keeping the risk under control
- exploitation: we take advanced of the best option we know
- exploration: test new decisions

### A general RL framework
**at each timestamp the agent:**
- executes action At
- receives observation Ot
- receives scalar reward Rt

**the environment:**
- receives action At
- emits observation Ot
- emits scalar reward Rt



**agent state:** the view of the agent on the environment state, is a function of history
- the function of the history is involved in taking the next decision
- the state representation defines what happens next
- ...

#### Inside the agent
one or more of these components
- **Policy:** agent's behavior function
	- defines what to do (behavior at a given time)
	- maps state to action
	- core of the RL agent
	- the policy is altered based on the reward
	- may be
		- deterministic: single function of the state
		- stochastic: specifying probabilities for each actions
			- reward changes probabilities
- **Value function:**
	- specifies what's good in the long run
	- is a prediction of future reward
	- used to evaluate the goodness/badness of states
	- values are prediction of rewards
	- Vp(s) = Ep[yRt+1 + y^2Rt+2 ... | St = s]
- **Model:**
	- predicts what the environment will do next
	- many problems are model free

back to the original problem:
- n devices
- each devices produces new data with rate_i
- in which order should the reader query tags?
- formulate as an RL problem
	- agent is the reder
	- one action per device (query)
	- rewards:
		- positive when querying a device with new data
		- negative if it has no data
		- what to do if the device has lost data?
	- state?


### Exploration vs exploitation trade-off
- Rewards evaluate actions taken
- evaluative feedback depends on the action taken
- no active exploration

Let's consider a simplified version of an RL problem: K-armed bandit problem.
- K different options
- every time need to chose one
- maximize expected total reward over some time period
- analogy with slot machines
	- the levers are the actions
	- which level gives the highest reward?
- Formalization
	- set of actions A (or "arms")
	- reward function R that follows an unknown probability distributions
	- only one state
	- ...

Example: doctor treatment
- doctor has 3 treatments (actions), each of them has a reward.
- for the doctor to decide which action to take is best, we must define the value of taking each action
- we call these values the action values (or action value function)
- action value: ... 

Each action has a reward defined by a probability distribution.
- the red treatment has a bernoulli probability
- the yellow treatment binomial
- the blue uniform
- the agent does not know the distributions!
- the estimated action for action a is the sum of rewards observed divided by the total time the action has been taken (add formula ...)
	- 1predicate denotes the random variable (1 if true else 0)

- greedy action:
	- doctors assign the treatment they currently think is the best
	- ...
	- the greedy action is computed as the argmax of Q values
	- greedy always exploits current knowledge
- epsilon-greedy:
	- with a probability epsilon sometimes we explore
		- 1-eps probability: we chose best greedy action
		- eps probability: we chose random action

exercises ...

exercise 2: k-armed bandit problem.
K = 4 actions, denoted 1,2,3 and 4
eps-greedy selection
initial Q estimantes = 0 for all a.

Initial sequenze of actions and rewards is:
A1 = 1   R1 = 1
A2 = 2  R2 = 2
A3 = 2  R3 = 2
A4 = 2  R4 = 2
A5 = 3  R5 = 0

---
step A1: action 1 selected. Q of action 1 is 1
step A2: action 2 selected. Q(1) = 1, Q(2) = 1
step A3: action 2 selected. Q(1) = 2, Q(2) = 1.5
step A4: action 2. Q(1) = 1, Q(2) = 1.6
step A5: action 3. Q(1) = 1, Q(2) = 1.6, Q(3) = 0

For sure A2 and A5 are epsilon cases, system didn't chose the one with highest Q value.
A3 and A4 can be both greedy and epsilon case.

#### Incremental formula to estimate action-value
- to simplify notation we concentrate on a single action
- Ri denotes the reward received after the i(th) selection of this action. Qn denotes the estimate of its action value after it has been selected n-1 times (add Qn formula ...)
- given Qn and the reward Rn, the new average of rewards can be computed by (add formula with simplifications...) $Q_(n+1) = Q_{n} + \frac{1}{n}[Rn - Qn]$
	- NewEstimate <- OldEstimate + StepSize (Target - OldEstimate)
	- Target - OldEstimate is the error

Pseudocode for bandit algorithm:
```
Initialize for a = 1 to k:
	Q(a) = 0
	N(a) = 0
Loop forever:
	with probability 1-eps:
		A = argmax_a(Q(a))
	else:
		A = random action
	R = bandit(A) # returns the reward of the action A
	N(A) = N(A) + 1
	Q(A) = Q(A) + 1\N(A) * (R - Q(A))
```

Nonstationary problem: rewards probabilities change over time.
- in the doctor example, a treatment may not be good in all conditions
- the agent (doctor) is unaware of the changes, he would like to adapt to it

An option is to use a fixed step size. We remove the 1/n factor and add an $\alpha$ constant factor between 0 and 1.
And we get $Q_{n+1} = (1-\alpha)^{n}Q_1 + \sum_{i=1}^{n}{\alpha(1 - \alpha)^{(n-1)} R_i}$


... ADD MISSING PART ...