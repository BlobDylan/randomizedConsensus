---
marp: true
theme: randomizedConsensus
paginate: true
---

<!-- _class: title -->

# Rolling the Dice on Agreement

## Randomized Asynchronous Consensus

A 120-minute deep dive into overcoming the FLP impossibility wall.

---

## Part 1: The Consensus Problem & The FLP Wall

### Understanding the fundamental limits of agreement

- Introduction to the Consensus Problem
- The Three Pillars of Correctness
- The Interactive 2-out-of-3 Exercise
- The FLP Impossibility Theorem

---

## The Consensus Problem

### Getting $n$ processes to agree on a single value

- **Goal:** Reach a common decision in a distributed system.
- **Binary Consensus:** The input range is strictly $\{0, 1\}$.
- **The Three Pillars**:
  1. **Agreement:** All deciding processes choose the same value.
  2. **Termination:** All non-faulty processes eventually decide.
  3. **Validity:** The output must be an input of some process.

---

## Interactive Exercise: 2-Out-Of-3 Ain't Bad

### Can we solve it by sacrificing one condition?

- **Agreement + Termination:**
  - _Solution:_ Hardcode every process to output 0.
  - _Failure:_ Fails **Validity** (if everyone's input was 1).
- **Agreement + Validity:**
  - _Solution:_ Wait for everyone to broadcast the same value.
  - _Failure:_ Fails **Termination** if even one process stalls.

---

## The Crushing Reality: FLP Impossibility

### The "No-Go" Theorem for Distributed Systems

- **The Theorem:** No deterministic protocol satisfies agreement, termination, and validity in an asynchronous system with even one failure.
- **Proof Intuition (Valence):**
  - **Univalent:** The system is guaranteed to decide a specific value.
  - **Bivalent:** The outcome is still undecided; an adversary can steer it.
- **The Trap:** An adversary can infinitely delay processes to keep the system bivalent.

---

<!-- _class: split -->

<div class="col">

## The Minds Behind FLP

- **Michael J. Fischer** (Yale): Pioneer in distributed computing and complexity theory.
- **Nancy Lynch** (MIT): Author of "Distributed Algorithms," 2007 Knuth Prize winner.
- **Michael S. Paterson** (Warwick): Renowned for algorithms and "Big O" notations.

</div>

<div class="col">

![width:150px](fischer_portrait.jpg) ![width:150px](lynch_portrait.jpg)
![width:150px](peterson_portrait.jpeg)

</div>

---

## Part 2: Cheating the System with Randomization

### Escaping the FLP trap by changing the rules

- Timing Assumptions & Failure Detectors
- Strong Primitives vs. Randomization
- Embracing Probability
- Ben-Or’s Pioneering Protocol

---

## Four Ways to Cheat

### Extending the model to find a solution

1. **Timing Assumptions:** Adding synchrony to exclude "bad" executions.
2. **Failure Detectors:** Mechanisms to notify when a process has likely failed.
3. **Strong Primitives:** Using shared-memory like test-and-set.
4. **Randomization:** Providing probabilities for transitions (our focus).

---

## Embracing Probability & The Adversary

### Shifting from Determinism to Likelihood

- **Rule Change:** Termination is weakened to "occurs with probability 1."
- **The Adversary Types:**
  - **Strong Adversary:** Sees history and internal states, but cannot predict future coin flips.
  - **Weak Adversary:** Restricted vision (e.g., cannot see message contents).

---

<!-- _class: split -->

<div class="col">

## Ben-Or's Protocol: The Pioneer

- **Michael Ben-Or** (Hebrew University):
  - Proved that randomization solves consensus.
  - **Resilience:** Tolerates $t < n/2$ crash failures.
  - **The Catch:** Termination can take exponential time in the worst case.

</div>

<div class="col">

![width:300px](michael_ben_or_portrait.jpeg)

</div>

---

## Ben-Or's Two-Stage Rounds

### How the protocol breaks symmetry

- **Stage 1: Voting**
  - Processes broadcast preferences and wait for $n - t$ messages.
- **Stage 2: Ratification**
  - If a majority is seen, propose ratification.
  - If no majority, **flip a fair coin** for the next round's preference.

---

## Break

### 15 Minutes

---

## Part 3: The Shared Memory Race

### Wait-free protocols and the voting power of $n^2$

- Wait-Free Shared Memory
- The Chor-Israeli-Li Race
- Weak Shared Coins
- Bracha & Rachman's $O(n^2 \log n)$ protocol

---

## A Different Playing Field: Shared Memory

### Wait-Free Protocols ($n-1$ resilience)

- **The Advantage:** Data survives in memory even if processes crash.
- **Goal:** Drive shared memory into a state that unambiguously determines the decision.
- **Independence:** Any process can finish in isolation without waiting for others.

---

## The Chor-Israeli-Li Race

### Advancement via Randomness

- **The Metaphor:** Processes run a race; slow ones adopt the preference of leaders.
- **Advancement:** Flip a coin with $1/2n$ probability to move to the next round.
- **The Flaw:** A strong adversary can force a "lockstep execution," preventing any leader from emerging.

---

## The Ultimate Weapon: Weak Shared Coins

### Defeating the Strong Adversary

- **Definition:** A minimum probability $\epsilon$ that all processes agree on a coin value.
- **Bracha & Rachman Protocol:**
  - Processes generate $n^2$ "common votes."
  - Majority becomes so large that malicious "hidden votes" cannot flip the outcome.
  - **Expected Work:** $O(n^2 \log n)$.

---

## Part 4: Perfectionism Doesn't Pay

### Honoring Michael O. Rabin and Nature's Solution

- The _Myrmoyssus_ Mite Consensus
- Choice Coordination Problem (C.C.P.)
- Symmetry Breaking with 8-bit Alphabets

---

## A Bug in the System: Nature's Consensus

### The _Myrmoyssus_ Mite Dilemma

- **The Threat:** Infecting both moth ears makes the host deaf to bats, leading to death for all.
- **The Protocol:** Mites use chemical trails to coordinate.
- **Result:** They collectively agree to occupy only one ear membrane.

---

## The Choice Coordination Problem (C.C.P.)

### Coordinating without names

- **Setup:** $n$ processes must choose one of $k$ alternatives.
- **Deterministic Wall:** To guarantee 100% success, memory must scale with $n$ ($O(\log n)$ bits).
- **Randomization Pivot:** Using randomness allows a **fixed alphabet** to coordinate any number of processes.

---

## Rabin's Design Philosophy

### Breaking the symmetry of scale

- **The Efficiency:** An 8-bit alphabet can coordinate $n$ processes with error probability $< 2^{-127}$.
- **Core Lesson:** Demanding zero failure probability forces complexity to scale with $n$.

> "Perfectionism, it seems, does not pay!"
> — **Michael O. Rabin**
