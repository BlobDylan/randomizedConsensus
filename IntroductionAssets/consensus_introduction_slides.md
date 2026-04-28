---
marp: true
theme: randomizedConsensus
paginate: true
footer: Daniel & Dylan
---

<!-- _class: title -->

# Introduction to Consensus

## Reaching Agreement in Distributed Systems

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

## </div>
