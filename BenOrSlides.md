---
marp: true
theme: randomizedConsensus
paginate: true
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
