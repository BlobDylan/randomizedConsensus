# Rolling the Dice on Agreement: Randomized Asynchronous Consensus

**Duration:** 120 Minutes (105 mins content, 15 min break)

---

### Part 1: The Consensus Problem & The FLP Wall

**Time:** 30 Minutes

## The Consensus Problem

- **The Goal:** Getting a group of $n$ processes in a distributed system to agree on a single value.
- **Binary Consensus:** The input range is restricted strictly to $\{0,1\}$.
- **The Three Pillars of a Correct Protocol**:
  1.  **Agreement:** All processes that decide must choose the same value.
  2.  **Termination:** All non-faulty processes eventually decide.
  3.  **Validity:** The common output value must be an input value of some process.

---

## Interactive Exercise: 2-Out-Of-3 Ain't Bad

_How trivially could we solve this if we only needed to guarantee TWO of the three conditions?_

- **Agreement + Termination:**
  - _Solution:_ Hardcode every process to always output 0.
  - _Failure:_ Fails Validity (if everyone's initial input was 1).
- **Agreement + Validity:**
  - _Solution:_ Wait until everyone broadcasts the exact same initial value. If there's a mismatch, halt forever.
  - _Failure:_ Fails Termination.

---

## The Crushing Reality: FLP Impossibility

- **The Theorem:** There is no deterministic protocol that satisfies agreement, termination, and non-triviality for an asynchronous message-passing system in which any single process can fail undetectably.
- **Proof Intuition - Valence:**
  - _Univalent:_ The system is guaranteed to eventually decide 0, or eventually decide 1.
  - _Bivalent:_ The outcome is still completely up in the air; the adversary can force either a 0 or 1 decision.
- **The Trap:**
  - There must be an initial bivalent configuration.
  - If a step is about to push the system into a univalent (deciding) state, the adversary simply delays that process and schedules a different one.
  - Because asynchronous events can commute, the adversary can infinitely steer around deciding steps.

---

### Part 2: Cheating the System with Randomization

**Time:** 25 Minutes

## Four Ways to Cheat

To escape the FLP result, we must extend the model.

1.  **Timing Assumptions:** Adding synchrony to exclude bad executions.
2.  **Failure Detectors:** Mechanisms to notify other processes that a process has failed.
3.  **Strong Primitives:** Stronger shared-memory primitives like test-and-set or read-modify-write.
4.  **Randomization:** Providing probabilities for some transitions. _(Our focus today)._

---

## Embracing Probability & The Adversary

- **Changing the Rules:** We weaken the termination requirement to permit non-terminating executions, provided they collectively occur with a probability of zero.
- **Meet the Adversary:**
  - **Strong Adversary:** Observes the entire history, internal states, past coin-flip outcomes, and memory, but cannot predict future coin flips.
  - **Weak Adversaries:** Restricted vision. For example, a "content-oblivious adversary" cannot see the contents of messages or registers.

---

## Ben-Or's Protocol: The Pioneer

- **First of its kind:** Achieves consensus with probabilistic termination against a strong adversary.
- **Resilience:** Tolerates $t < n/2$ crash failures.
- **Two-Stage Rounds:**
  1.  **Voting:** Transmit current preference and wait for $n - t$ messages.
  2.  **Ratification:** If a majority is seen, propose ratification. If no majority is seen, flip a fair coin to choose a new preference for the next round.
- **The Catch:** The probability of termination in a given round might be exponentially small, requiring exponential expected time in the worst case.

---

# Break

**Time:** 15 Minutes

---

### Part 3: The Shared Memory Race

**Time:** 40 Minutes

## A Different Playing Field: Wait-Free Shared Memory

- **Wait-Free Protocols:** Tolerate up to $n-1$ crash failures. Any process can finish in isolation without waiting for others.
- **The Shared Memory Advantage:** Data survives in memory even if processes crash, eliminating the danger of network partitions.
- **The New Goal:** Get the shared memory into a state that unambiguously determines the decision value, even for processes that slept through the protocol.

---

## The Chor-Israeli-Li Race

- **The Metaphor:** Processes run a race, advancing through rounds. Slow processes adopt the preferences of faster ones.
- **Winning:** If a leader pulls far enough ahead of disagreeing processes, everyone decides on the leader's preference.
- **Advancement:** Processes flip coins with a probability of $\frac{1}{2n}$ to advance to the next round.
- **The Flaw:** A strong adversary easily defeats this by constructing a "lockstep execution," stopping any advancing process until everyone else catches up.

---

## The Ultimate Weapon: Weak Shared Coins

- To defeat a strong adversary, we need a **weak shared coin**.
- **Definition:** Regardless of adversary behavior, there is a constant minimum probability $\epsilon$ that all processes agree on a coin value (0 or 1).
- **Bracha and Rachman's Voting Protocol:** \* Processes collectively generate a massive volume of "common votes" ($n^2$).
  - The adversary can hide up to $n-1$ votes by delaying processes.
  - _Result:_ The common votes create a majority so large that neither random extra votes nor malicious hidden votes are likely to flip the outcome. Expected total work drops to $O(n^2 \log n)$.

---

### Part 4: Perfectionism Doesn't Pay – Honoring Michael O. Rabin

**Time:** 10 Minutes

## A Bug in the System: Nature's Consensus

- **The _Myrmoyssus_ Mite:** Parasites the ear membrane of moths.
- **The Threat:** If mites infect _both_ ears, the moth goes deaf, fails to hear predatory bat sonar, and the moth (plus the mites) are eaten.
- **The Solution:** Mites employ a chemical trail choice coordination protocol to ensure they collectively agree to infect only one ear.

---

## The Choice Coordination Problem (C.C.P.)

- **The Setup:** $n$ processes must agree on exactly one of $k$ alternatives.
- **The Catch:** They do not share a common naming system for the alternatives and must coordinate purely via shared variables.
- **The Deterministic Wall:** To mathematically guarantee 100% success without randomization, the coordination alphabet (memory) must grow as the number of processes grows. Specifically, $O(\log n)$ bits are necessary and sufficient.

---

## Breaking Symmetry with Randomization

- **Rabin's Pivot:** By utilizing randomizing protocols, we break the symmetry and no longer need memory to scale with $n$.
- **Fixed Alphabet:** An 8-bit (256-letter) alphabet can coordinate any number of processes with a non-termination probability of less than $2^{-127}$.
- **The Design Philosophy:** Demanding absolute zero probability of failure forces complexity to scale with $n$. In distributed systems, as in life...
  > _"Perfectionism, it seems, does not pay!"_ - Michael O. Rabin
