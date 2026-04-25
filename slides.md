---
marp: true
theme: randomizedConsensus
paginate: true
footer: Daniel & Dylan
---

<!-- _class: title -->

# Randomized Consensus

## By Daniel & Dylan

Overcoming impossibility using randomization

---

## Lecture Agenda

### Rolling the Dice on Agreement

- **Part 1: The Consensus Problem & The FLP Wall**
  - Defining the three pillars and the deterministic impossibility.
- **Part 2: Cheating the System with Randomization**
  - Models of the adversary and Ben-Or's pioneering protocol.
- **Part 3: The Shared Memory Race**
  - Wait-free protocols and defeating adversaries with shared coins.

---

<!-- _class: title -->

# Part 1

## The Consensus Problem & The FLP Wall

Foundational concepts and challenges

---

## What is Consensus?

### Defining the Goal

- **The Objective**: Get $n$ processes to agree on a single value
- **Inputs & Outputs**: Processes start with an input and must decide on an output in the same range
- **Binary Consensus**: When the input/output space is restricted to $\{0, 1\}$
- **Irrevocability**: Once a process decides, it can never change its mind

---

## Correctness Conditions

### The Three Pillars

1. **Agreement**: All deciding processes choose the same value
2. **Termination**: All non-faulty processes eventually decide
3. **Validity**: The common output must be an input from at least one process

---

## Thinking Critically

### Trivial Solutions for 2/3 Conditions

- **Agreement + Validity (No Termination)**: Processes wait forever for a value that never arrives.
- **Agreement + Termination (No Validity)**: Everyone decides "0" regardless of their inputs.
- **Termination + Validity (No Agreement)**: Every process just decides its own input value.

---

## The Adversary

### Modeling the Environment

The adversary is a function from partial executions to operations, choosing what happens next.

- **Strong Adversary**:
  - Sees full execution history (coin flips, states, messages)
  - Can make the "worst" possible scheduling decisions
- **Weak Adversary**:
  - Restricted power (e.g., cannot see future coin flips or internal states)
  - Used to analyze randomized algorithms under fairer conditions

---

## Communication Models

### How Processes Interact

- **Message Passing**:
  - Processes communicate explicitly by sending/receiving messages
  - **FLP Result**: Deterministic consensus is impossible with even one undetectable failure
- **Shared Memory**:
  - Implicit communication via shared registers (hardware)
  - Analogous impossibility results by Loui and Abu to FLP
  - **Wait-Free**: Can achieve consensus for up to $n-1$ crashes

---

<!-- _class: split -->

<div class="col">

## Synchronous

- **Timing**: Known, finite time bounds for message delivery
- **Coordination**: Operations occur in distinct, coordinated rounds
- **Trade-off**: Faster and simpler, but fragile if network delays exceed limits

</div>

<div class="col">

## Asynchronous

- **Timing**: Messages can be delayed arbitrarily
- **Robustness**: No assumptions about timing; messages / reads from registries
- **Trade-off**: More robust for unpredictable networks (e.g., Internet)

</div>

---

<!-- _class: title -->

# The Impossibility of Consensus

Breaking Down the FLP Theorem

---

# FLP Theorem

Deterministic consensus is impossible in the asynchronous model even for binary inputs and at most one crash failure.

---

<!-- _class: split -->

<div class="col">

## Configuration

- **Configuration:** The collective local state of all nodes + all messages currently in transit.

</div>

<div class="col">

![](/FLPAssets/slide1.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Configuration

- **Configuration:** The collective local state of all nodes + all messages currently in transit.

</div>

<div class="col">

![](/FLPAssets/slide2.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Configuration

- **Configuration:** The collective local state of all nodes + all messages currently in transit.

</div>

<div class="col">

![](/FLPAssets/slide3.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Configuration

- **Configuration:** The collective local state of all nodes + all messages currently in transit.
- **Univalent State:** The final decision is mathematically guaranteed.
  - _1-valent:_ System will definitely decide 1.

</div>

<div class="col">

![](/FLPAssets/slide4.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Configuration

- **Configuration:** The collective local state of all nodes + all messages currently in transit.
- **Univalent State:** The final decision is mathematically guaranteed.
  - _1-valent:_ System will definitely decide 1.
  - _0-valent:_ System will definitely decide 0.

</div>

<div class="col">

![](/FLPAssets/slide5.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Configuration

- **Bivalent State:** The outcome is undecided. The final decision depends entirely on the order of future messages.
- **Goal:** Successfully transition from a bivalent state into a univalent state.

</div>

<div class="col">

![](/FLPAssets/slide6.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Initial Bivalent Configuration Exists

- Assume all possible **initial** configurations, ranging from "all nodes = 0" to "all nodes = 1".
- There must be a specific point where the system flips from 0-valent to 1-valent. Denote that index by $i$

</div>

<div class="col">

![](/FLPAssets/slide7.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Bivalent Configuration Exists

- If the single node responsible for that flip crashes or delays, the remaining nodes must decide without it leading to a contradiction.

</div>

<div class="col">

![](/FLPAssets/slide8.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Bivalent Configuration Exists

- If the single node responsible for that flip crashes or delays, the remaining nodes must decide without it leading to a contradiction.

</div>

<div class="col">

![](/FLPAssets/slide9.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Bivalent Configuration Exists

- If the single node responsible for that flip crashes or delays, the remaining nodes must decide without it leading to a contradiction.

- **Conclusion:** Initial bivalent configurations must exist.

</div>

<div class="col">

![](/FLPAssets/slide10.png)

</div>

---

<!-- _class: split -->

<div class="col">

## The Critical Configuration

- **Definition:** A bivalent state where the _very next_ processed message permanently locks the system into a univalent state.

</div>

<div class="col">

![](/FLPAssets/slide11.png)

</div>

---

<!-- _class: split -->

<div class="col">

## The Critical Configuration

- If the system only transitions between **non-critical** states, the algorithm runs infinitely and never terminates.
- What happens if we reach a **Critical Configuration**

</div>

<div class="col">

![](/FLPAssets/slide12.png)

</div>

---

<!-- _class: split -->

<div class="col">

## The Critical Configuration

- Let's denote the transitions as a tuple, $(u,m)$ denoting node $u$ recieved message $m$

</div>

<div class="col">

![](/FLPAssets/slide13.png)

</div>

---

<!-- _class: split -->

<div class="col">

## The Critical Configuration

- Suppose a divergence happens because of different nodes.
- Then eventually they must meet at some future configuration

</div>

<div class="col">

![](/FLPAssets/slide14.png)

</div>

---

<!-- _class: split -->

<div class="col">

## The Critical Configuration

- Therefore, any divergence must be caused by the same node

</div>

<div class="col">

![](/FLPAssets/slide15.png)

</div>

---

<!-- _class: split -->

<div class="col">

## The Critical Configuration

- Therefore, any divergence must be caused by the same node
- Our critical configuration's children locking to a univalent state must be results of actions on the same node

</div>

<div class="col">

![](/FLPAssets/slide16.png)

</div>

---

<!-- _class: split -->

<div class="col">

## The Critical Configuration

- If this single node crashes we will have a bivalent leaf, meaning any algorithm will either stay in this state indefinately or decide without actuall agreement

</div>

<div class="col">

![](/FLPAssets/slide17.png)

</div>

---

# Moving Forward: Escaping Impossibility

**Where do we go from here?**

- The FLP Theorem proves deterministic consensus is impossible under these strict asynchronous conditions.
- **The Solution:** We must relax our assumptions.
- _Next up:_ Exploring **Randomized Consensus** (e.g., Ben-Or's Algorithm) to bypass the FLP impossibility and achieve consensus with high probability.

---

## Lecture Agenda

### Rolling the Dice on Agreement

- ~~**Part 1: The Consensus Problem & The FLP Wall**~~
  - Defining the three pillars and the deterministic impossibility.
- **Part 2: Cheating the System with Randomization**
  - Models of the adversary and Ben-Or's pioneering protocol.
- **Part 3: The Shared Memory Race**
  - Wait-free protocols and defeating adversaries with shared coins.

---

<!-- _class: title -->

# Part 2

## Cheating the System with Randomization

---

<!-- _class: split -->

<div class="col">

## Michael Ben-Or

- Professor at the **Hebrew University**
- Recognized for his foundational research across theoretical computer science, cryptography, and quantum computing

</div>

<div class="col">

![](/BenOrAssets/michael_ben_or_portrait.jpeg)

</div>

---

<!-- _class: split -->

<div class="col">

## Ben-Or's Algorithm

- **First of its kind:** Achieves consensus with probabilistic termination against a strong adversary.
- **Resilience:** Tolerates $t < n/2$ crash failures.
- We'll see the algorithm through an example
</div>

<div class="col">

![](/BenOrAssets/ben_or_algorithm.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Ben-Or Example

- **Setting:** Consider a network of $5$ nodes, in this case we tolerate $f=2$ crash failures

</div>

<div class="col">

![](/BenOrAssets/slide1.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 1

- **Step 1 - Voting:** Each node broadcasts its current value to all other nodes

</div>

<div class="col">

![](/BenOrAssets/slide2.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 1

- **Step 1 - Voting:** Each node broadcasts its current value to all other nodes
- Each node collects the first $n-f=3$ values

</div>

<div class="col">

![](/BenOrAssets/slide3.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 1

- **Step 1 - Voting:** Each node broadcasts its current value to all other nodes
- Each node collects the first $n-f=3$ values
- If all values are the same (majority) the node proposes it, otherwise it does not propose a value

</div>

<div class="col">

![](/BenOrAssets/slide4.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 1

- **Step 1 :** Each node broadcasts its current value to all other nodes
- Each node collects the first $n-f=3$ values
- If all values are the same (majority) the node proposes it, otherwise it does not propose a value

</div>

<div class="col">

![](/BenOrAssets/slide5.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 1

- **Step 1 :** Each node broadcasts its current value to all other nodes
- Each node collects the first $n-f=3$ values
- If all values are the same (majority) the node proposes it, otherwise it does not propose a value

</div>

<div class="col">

![](/BenOrAssets/slide6.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 1

- **Step 2 :** If no proposal is recieved the node flips a coin

</div>

<div class="col">

![](/BenOrAssets/slide7.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 1

- **Step 2:** If no proposal is recieved the node flips a coin
- In our example assume the following happens

</div>

<div class="col">

![](/BenOrAssets/slide8.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 2

- **Step 1:** Each node once again broadcasts, and then collects $3$ values
- In this case we have a proposal from the top right node

</div>

<div class="col">

![](/BenOrAssets/slide9.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 2

- **Step 1:** Each node once again broadcasts, and then collects $3$ values
- In this case we have a proposal from the top right node

</div>

<div class="col">

![](/BenOrAssets/slide10.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 2

- **Step 1:** Each node once again broadcasts, and then collects $3$ values
- In this case we have a proposal from the top right node
- Recall some messages may be delayed or crash

</div>

<div class="col">

![](/BenOrAssets/slide11.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 2

- **Step 1:** Each node once again broadcasts, and then collects $3$ values
- In this case we have a proposal from the top right node
- Recall some messages may be delayed or crash

</div>

<div class="col">

![](/BenOrAssets/slide12.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 2

- **Step 2:** Again, nodes with no proposals flip a coin, the others set their value to the proposal for next round

</div>

<div class="col">

![](/BenOrAssets/slide13.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 2

- **Step 2:** Again, nodes with no proposals flip a coin, the others propose the majority for the next round
- Suppose these nodes change values in our example

</div>

<div class="col">

![](/BenOrAssets/slide14.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 3

- We continue our execution
- If a node recieves all of the same value it decides it but continues participating in the algorithm

</div>

<div class="col">

![](/BenOrAssets/slide15.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 3

- We continue our execution
- If a node recieves all of the same value it decides it but continues participating in the algorithm

</div>

<div class="col">

![](/BenOrAssets/slide16.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Example - Round 4

- Eventually all nodes decide
- Now let's see a quick sanity check that our algorithm answers the requirements

</div>

<div class="col">

![](/BenOrAssets/slide17.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Correctness

- What if a node recieves a proposal like the following
- Is this case possible?

</div>

<div class="col">

![](/BenOrAssets/slide18.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Correctness

- What if a node recieves a proposal like the following
- Is this case possible?
- No. Because a proposal requires a majority

</div>

<div class="col">

![](/BenOrAssets/slide18.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Correctness

- Trivially, if all nodes start with the same value they immediately agree on said value

</div>

<div class="col">

![](/BenOrAssets/slide19.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Correctness

- If there is no proposal for some node because it didn't recieve all 0's or all 1's we flip a coin
- It is possible all nodes get the same value randomly

</div>

<div class="col">

![](/BenOrAssets/slide20.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Correctness

- If there is no proposal for some node because it didn't recieve all 0's or all 1's we flip a coin
- It is possible all nodes get the same value randomly
- If some nodes already have a proposal we showed its impossible to have a conflict so we still have a chance the rest fit randomly

</div>

<div class="col">

![](/BenOrAssets/slide21.png)

</div>

---

<!-- _class: split -->

<div class="col">

## Can we do better?

- **No:** Randomized consensus is impossible in the **Asynchronous** model if there can be at least $n/2$ failures
- To terminate each half must decide, but if they decide they violate agreement

</div>

<div class="col">

![](/BenOrAssets/slide22.png)

</div>
