---
marp: true
theme: randomizedConsensus
paginate: true
footer: Daniel & Dylan
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
