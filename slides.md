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

<!-- _class: split -->

<div class="col">

# FLP Theorem

### Fischer Lynch Peterson

Deterministic consensus is impossible in the asynchronous model even for binary inputs and at most one crash failure.

</div>

<div class="col">

![width:150px](/FLPAssets/fischer_portrait.jpg)
![width:150px](/FLPAssets/lynch_portrait.jpg)
![width:150px](/FLPAssets/peterson_portrait.jpeg)

</div>

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

## Recap — Part 1

### Consensus & The FLP Wall

- **The Goal:** Agreeing on a single value (Agreement, Termination, Validity).
- **Asynchrony:** No timing assumptions; messages can be delayed indefinitely.
- **FLP Impossibility:** No deterministic solution with even 1 crash failure.

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

---

## Recap — Part 2

### Cheating the System

- **Breaking the Wall:** Using randomization to achieve "Probabilistic Termination."
- **Adversary Models:** Strong (sees everything) vs. Weak (blind to internal coin flips).
- **Ben-Or's Protocol:**
  - $t < n/2$ resilience in message-passing.
  - Two-stage rounds (Voting & Ratification) + local coins.
  - **The Limit:** Can require exponential expected time to terminate.

---

## Lecture Agenda

### Rolling the Dice on Agreement

- ~~**Part 1: The Consensus Problem & The FLP Wall**~~
  - Defining the three pillars and the deterministic impossibility.
- ~~**Part 2: Cheating the System with Randomization**~~
  - Models of the adversary and Ben-Or's pioneering protocol.
- **Part 3: The Shared Memory Race**
  - Wait-free protocols and defeating adversaries with shared coins.

---

<!-- _class: title -->

# Part 3

## The Shared Memory Race

Three algorithms, one consistent template.

---

## A different playing field

### Wait-free shared memory

- Tolerate up to $n - 1$ crashes — any process finishes alone.
- Memory survives the processes that wrote it.
- No network. No partitions.
- Goal: drive shared state into a configuration that **announces the decision**.

**Intuition:** dead processes still "speak" through the registers they left behind. A late-arriving process can read those registers and decide without waiting for anyone.

---

## How we'll cover each algorithm

### One template, three times

For **Chor–Israeli–Li**, **Chandra**, and **Bracha–Rachman** we cover, in order:

1. **Settings** — communication model and registers.
2. **Adversary** — what it can see and do.
3. **Complexity** — work, rounds, probability.
4. **Algorithm** — phase by phase, each with intuition.
5. **Correctness** — Agreement, Validity, Termination, each with intuition.

---

<!-- _class: title -->

# Algorithm 1

## Chor–Israeli–Li

A wait-free race through rounds.

---

## CIL — Settings

### Communication model

- **Shared memory**, not message passing.
- One **single-writer multi-reader register** per process.
- Register holds the pair $(\text{preference}, \text{round})$.
- Inputs may take **any value** — not restricted to $\{0, 1\}$.
- Wait-free up to $n - 1$ crashes.

**Intuition:** the register is the only channel. Writing to it is broadcast; reading $n$ of them is a snapshot. Dead writers' registers remain readable.

---

## CIL — Adversary

### Weak adversary

- Schedules messages, steps, and process delays.
- **Cannot** see a coin-flip result until it appears in shared memory.
- It chooses _who runs next_ without knowing what they will write.

**Intuition:** the adversary plays blind for one move at a time. It can stall a process, but it cannot tell _whether the process is about to advance_ — so it cannot specifically target winners.

---

## CIL — Complexity

### Cost in iterations and total work

- **Iterations until a leader emerges:** $O(n)$ in expectation.
- **Work per iteration:** one write + $n$ reads = $O(n)$.
- **Total expected work:** $O(n^2)$.
- **Termination probability:** $1$ (eventually, with probability $1$).

**Intuition:** $n$ runners each step forward with probability $\tfrac{1}{2n}$. After $O(n)$ iterations, one runner gets a $2$-step lead with constant probability — that triggers a decision.

---

## CIL — Algorithm overview

### State and loop

Each process keeps:

- $\text{preference}$ — initially the input value.
- $\text{round}$ — initially $1$.

It runs a loop of three phases until it decides.

**Intuition:** _announce → check → adopt-or-advance._ The race never speeds up — it just keeps cycling until a leader pulls clearly ahead.

---

<!-- _class: split -->

<div class="col">

## CIL - Algorithm

![width:500](/ChorIsraeliLiAssets/chor_israeli_li_algorithm.png)

</div>

<div class="col">

![](/ChorIsraeliLiAssets/chor_israeli_li_visualization.png)

</div>

---

## CIL — Phase 1: Announce & Observe

### Steps

1. Write $(\text{preference}, \text{round})$ to your register.
2. Read all $n$ registers.
3. Compute $\text{maxround}$ = highest round seen.

**Intuition:** publish your state, then take a snapshot of everyone else's. After this phase you know who is at the front, who is behind, and what they think.

---

## CIL — Phase 2: Has the race been won?

### Steps

- Look at the **frontrunners**: every process with $\text{round} \geq \text{maxround} - 1$.
- All share preference $v$ → **decide $v$ and exit.**
- Otherwise → continue to Phase 3.

**Intuition:** if the leaders unanimously agree, it is safe to commit. The 2-round window is the safety margin — anyone $2$ or more rounds behind cannot influence the outcome before adopting.

---

## CIL — Phase 3: Adopt + maybe advance

### Steps

1. **Adopt:** if all processes at $\text{maxround}$ agree on $v$, set $\text{preference} \leftarrow v$.
2. **Advance:** with probability $\tfrac{1}{2n}$, set $\text{round} \leftarrow \max(\text{round} + 1, \text{maxround} - 2)$. Else stay.
3. Loop back to Phase 1.

**Intuition:** slow processes get pulled toward leaders' opinion. The jump to $\text{maxround} - 2$ keeps stragglers in the race.

---

## CIL — Correctness: Agreement

### Argument

- A decider sees frontrunners unanimous on $v$.
- Any slower process eventually reaches $\text{maxround}$, sees the leaders, adopts $v$.
- Once carrying $v$, it can only decide $v$ or adopt $v$ again.

**Intuition:** influence is **one-way** — fast processes pull slow ones. Once the front locks onto a value, that value propagates backwards through the field. Disagreement cannot survive.

---

## CIL — Correctness: Validity

### Argument

- A process changes preference only by copying a leader (Phase 3 adopt).
- If all inputs are $v$, no leader ever holds anything else.
- The Phase-2 unanimity check fires immediately.

**Intuition:** the protocol has no way to _invent_ a value. It only copies. If everyone starts with $v$, $v$ is the only thing in the system.

---

## CIL — Correctness: Termination

### Argument

- Each iteration, each process advances with probability $\tfrac{1}{2n}$.
- Expected $O(n)$ iterations until one process is $2$ rounds ahead of all dissenters.
- That triggers Phase-2 decision.

**Intuition:** the weak adversary can choose _who runs_ but not _who wins the flip_. It cannot freeze the leader because it cannot identify the leader before the write.

---

<!-- _class: invert title -->

# Lockstep.

## What if the adversary is strong?

---

## CIL fails under a strong adversary

### The lockstep attack

- Strong adversary sees the advancement flip **before** the write.
- It freezes the lucky process and lets every other process catch up.
- All processes stay at the same round forever.
- No leader emerges. No decision.

**Intuition:** the moment the adversary can read the future, "who is about to advance" becomes targetable. CIL's randomness leaks before it can be cashed in.

We need a primitive the adversary cannot freeze.

---

<!-- _class: title -->

# The weak shared coin

## A coin that _sometimes agrees with itself_.

---

## Weak shared coin — definition

### Specification

- For each value $v \in \{0, 1\}$, no matter what the adversary does:
  - $\Pr[\text{every process outputs } v] \geq \epsilon$
  - for some constant $\epsilon > 0$.

**Intuition:** not a fair coin. Not a perfect coin. Just a primitive where, with constant probability, _all_ processes see the same value. That is enough to crack lockstep — disagreement is broken with probability $\epsilon$ per call.

---

<!-- _class: title -->

# Algorithm 2

## Chandra

A wait-free binary consensus **framework** that calls a weak shared coin once per round.

---

## How Chandra and Bracha–Rachman fit together

### Two nested algorithms

- **Chandra** is a _consensus framework_. Each round, processes that disagree call a subroutine: $\text{SharedCoin}_r()$.
- **Bracha–Rachman** is a _weak shared coin_ — one specific implementation of that subroutine.
- Plug BR into Chandra → wait-free binary consensus, strong adversary, $O(n^2 \log n)$.

**Intuition:** Chandra is the chassis; Bracha–Rachman is the engine. Either is useless alone. We cover them in that order.

---

## Chandra — Settings

### Communication model

- **Shared memory**, not message passing.
- Two arrays of **multi-writer bits**: $\text{mark}[0][r]$ and $\text{mark}[1][r]$ for each round $r$.
- Bits are **monotonic** — only flip false → true.
- **Binary** consensus (inputs in $\{0, 1\}$).
- Wait-free up to $n - 1$ crashes.

**Intuition:** "preference $b$ has reached round $r$" is a single global fact, written once and visible to everyone. Concurrent writes are safe — everyone writes "true."

---

## Chandra — Adversary

### Strong adversary

- Sees process states, registers, past coin outcomes, and the schedule.
- **Cannot** predict future outputs of $\text{SharedCoin}_r$.

**Intuition:** Chandra confronts the worst case. Its only edge is that the _next_ shared-coin call is unpredictable to the adversary, even when everything else is exposed.

---

## Chandra — Complexity

### Cost per round and overall

- **Operations per round:** $O(1)$ (a few mark-bit reads and writes).
- **Expected rounds to converge:** $O(1/\epsilon) = O(1)$.
- **Probability of termination:** $1$.
- Total cost is dominated by the **shared coin subroutine**.

**Intuition:** the framework itself is cheap. Each round it makes one expensive call (the coin) and a handful of free bit operations. So the coin's cost = the algorithm's cost.

---

## Chandra — Algorithm overview

### State and loop

Each process keeps:

- $p$ — preference, initially the input.
- $r$ — round, initially $0$.

Init: $\text{mark}[0][0] = \text{mark}[1][0] = \text{true}$. Loop the three phases until you decide.

**Intuition:** _plant your flag → look around → maybe flip the coin and check you didn't lap your own team._

---

<!-- _class: split -->

<div class="col">

## Chandra - Algorithm

![width:450px](/ChandraBrachaRachmanAssets/chandra_algorithm.png)

</div>

<div class="col">

![](/ChandraBrachaRachmanAssets/chandra_visualization.png)

</div>

---

## Chandra — Phase 1: Announce

### Steps

- Set $\text{mark}[p][r] \leftarrow \text{true}$.

**Intuition:** you declare "preference $p$ has a representative at round $r$." That single bit is now visible to every other process. Cheaper than CIL's $n$-register snapshot — the cost of binary consensus paid in advance.

---

## Chandra — Phase 2: Compare

### Read $\text{mark}[1-p]$ at $r+1$, $r$, $r-1$ — in that order

- **Case 1 — Behind** ($\text{mark}[1-p][r+1]$ set): $p' \leftarrow 1 - p$. Switch.
- **Case 2 — Tied** (only $\text{mark}[1-p][r]$): $p' \leftarrow$ **SharedCoin$_r$()**.
- **Case 3 — One ahead** (only $\text{mark}[1-p][r-1]$): keep $p$. Do **not** decide.
- **Case 4 — Two+ ahead** (none set): **return $p$. Decide and exit.**

**Intuition:** descending read order means you always see the worst news first. The four cases form a ladder from "I lost" to "I won."

---

## Chandra — Phase 3: Safety check

### Steps

- Read $\text{mark}[p][r+1]$.
  - **Set** → discard $p'$, keep old $p$.
  - **Not set** → adopt $p'$ as new preference.
- Advance $r \leftarrow r + 1$, return to Phase 1.

**Intuition:** if an ally already moved ahead carrying the old preference, do **not** flip the coin and split your own team. Defer to your fastest teammate.

---

## Chandra — Correctness: Agreement

### Argument

- A decider (Case 4) sees no opposing mark at $r-1$, $r$, $r+1$.
- Any future opposing process reaching $r-1$ will see $\text{mark}[p][r]$ → falls into Case 1 → switches to $p$.
- The Phase-3 safety check prevents same-side splits.

**Intuition:** Case 4 is a fortress: by the time anyone could threaten it, they will see your flag and join you. The safety check guards the back door — your own teammates cannot defect via the coin.

---

## Chandra — Correctness: Validity

### Argument

- Preference changes only when an opposing mark is observed (Case 1 or 2).
- All inputs $v$ → $\text{mark}[1-v]$ is never set beyond round $0$.
- Every process satisfies Case 4 at round $1$ → decides $v$.

**Intuition:** disagreement is the only trigger for change. No disagreement → no change → immediate decision on the common input.

---

## Chandra — Correctness: Termination

### Argument

Per round, partition processes:

- **Group A** (Cases 1, 3, 4): already agree on a single value.
- **Group B** (Case 2, tied): call SharedCoin$_r$.
- Group B matches Group A's value with probability $\geq \epsilon$.
- Once the system unifies, the next round triggers Case 4 for everyone.

**Intuition:** the coin's job is to drag the tied minority onto the leaders' value. It only has to succeed _once_ — and it succeeds with constant probability per round.

---

<!-- _class: title -->

# Algorithm 3

## Bracha–Rachman

The weak shared coin Chandra calls every round.

---

## BR — Settings

### Communication model

- **Shared memory**, not message passing.
- One **single-writer register** per process: $r[p] = (\text{flips}, \text{ones})$, init $(0, 0)$.
- $\text{flips}$ counts coin flips so far; $\text{ones}$ counts how many came up $1$.
- **Output:** each process returns $0$ or $1$.

**Intuition:** every process maintains a public running tally of its own contributions. The collective output is a vote count, not a leader election.

---

## BR — Adversary

### Strong adversary — with one specific weapon

- Sees full state, all past coin outcomes, the schedule.
- **Crucial gap:** between flipping a coin and writing it, the adversary can see the outcome and selectively delay the write to **hide** the vote.

**Intuition:** the adversary cannot bias coins, but it can decide _which fair coins reach the public ledger_. "Hidden votes" are its only weapon.

---

## BR — Complexity

### Cost and probability

- **Total votes cast:** $\Theta(n^2)$.
- **Batch size:** $n / \log n$ flips per termination check.
- **Termination check cost:** $O(n)$ reads.
- **Amortized cost per vote:** $O(\log n)$.
- **Total expected work:** $O(n^2 \log n)$.
- **Agreement probability:** $\epsilon = \Omega(1)$ for each value.

**Intuition:** without batching, checking after every vote would cost $O(n^3)$. Batching trades off slightly more "extra" votes for vastly cheaper bookkeeping.

---

## BR — Algorithm overview

### State and structure

Each process repeatedly votes (Phase 1) until a global threshold is crossed, then takes one final reading (Phase 2) and outputs a majority bit.

**Intuition:** generate so many fair votes that the law of large numbers builds a $\Theta(n)$-sized majority — too large for the adversary's $n - 1$ hidden votes to flip.

---

<!-- _class: split -->

<div class="col">

## BR - Algorithm

![width:450px](/ChandraBrachaRachmanAssets/bracha_rachman_algorithm.png)

</div>

<div class="col">

![](/ChandraBrachaRachmanAssets/bracha_rachman_visualization.png)

</div>

---

## BR — Phase 1: Vote in batches

### Steps

Repeat:

1. Flip $n / \log n$ coins. For each $c$:
   - Write $(\text{flips} + 1, \text{ones} + c)$ to $r[p]$.
2. Read all $n$ registers; sum into $\text{total}$.
3. If $\text{total} > n^2$ → exit. Else another batch.

**Intuition:** cheap flips, expensive checks — so do many flips between checks. The $O(n)$ check is amortized over $n / \log n$ flips, giving $O(\log n)$ per vote.

---

## BR — Phase 2: Read and decide

### Steps

1. Read all $n$ registers one final time.
2. $\text{total} \leftarrow \sum \text{flips}$, $\text{ones} \leftarrow \sum \text{ones}$.
3. Return $1$ if $\text{ones} / \text{total} \geq 1/2$, else $0$.

**Intuition:** snapshot the vote ledger, count the ones, output whichever side has the majority. The argument that this matches across processes is the correctness proof.

---

## BR — Correctness: the three vote types

### What every process sees

- **Common votes** — cast before $\text{total} > n^2$. About $n^2$ total. Unbiased.
- **Extra votes** — cast after the threshold but before $P$'s final read. Up to $n^2 / \log n$ per process. Unbiased.
- **Hidden votes** — flipped but unwritten when $P$ reads. At most $n - 1$. The adversary's weapon.

What $P$ sees = $\text{common} + \text{extra}_P - \text{hidden}_P$.

**Intuition:** common votes are everyone's shared truth. Extras are noise. Hidden are sabotage. Only hidden are _biased_ — and they are bounded.

---

## BR — Correctness: Agreement (the ε-guarantee)

### Why all processes output the same bit

1. $n^2$ fair common votes build a $\Theta(n)$ majority for $1$ with constant probability $p$ (normal approximation).
2. Each process's extra-vote walk stays within $\pm 2n$ with probability $\geq 1 - 1/n^2$. Union bound: works for **all** processes with probability $\geq 1 - 1/n$.
3. Hidden votes per process $\leq n - 1$ — cannot flip a $\Theta(n)$ majority.
4. Every process sees a positive net vote for $1$ → all output $1$.

**Intuition:** the common votes set the verdict. Extras and hidden votes are perturbations bounded below the verdict's margin. By symmetry, the same holds for $0$. So $\epsilon = \Omega(1)$.

---

## BR — Correctness: Validity & Termination

### The two remaining properties

- **Validity:** $0$ and $1$ are both possible outputs, by symmetry of the fair coin flips. (A weak shared coin must not collapse to a constant.)
- **Termination:** the global $\text{flips}$ counter is monotonic and grows by at least $n / \log n$ per batch — exits in $O(n \log n)$ batches almost surely.

**Intuition:** validity is automatic from fair flips; termination is automatic from monotonic progress. The hard property is agreement — that is where the $n^2$-vote argument lives.

---

<!-- _class: title -->

# $O(n^2 \log n)$

## Chandra + Bracha–Rachman: the first sub-exponential wait-free consensus against a strong adversary.

---

## Recap — Part 3

### Three algorithms, one template

- **Wait-free shared memory** trades messages for crash-proof state.
- **Chor–Israeli–Li** — weak adversary, race metaphor, $O(n^2)$, dies under lockstep.
- **Weak shared coin** — $\epsilon$-agreement against any adversary.
- **Chandra** — binary consensus _framework_ calling a coin per round, $O(1)$ rounds.
- **Bracha–Rachman** — the coin: $n^2$ fair votes drown $n - 1$ hidden ones.
- **Together:** $O(n^2 \log n)$ wait-free binary consensus, strong adversary.

---

<!-- _class: title -->

# Thank You!

## Any Questions?

**Daniel & Dylan**
Overcoming impossibility through randomization
