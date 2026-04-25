# Consensus

Get n processes to agree on a value.
Processes start with an input in some range and must decide inrevocably (once it decides it can not change its mind)
on an output in the same range as the input (e.g. input space - output space = {0,1} this is called binary consensus)

## Correctness and Conditions

We require 3 conditions to hold

1. Agreement: All deciding processes choose the same value
2. Termination: All non faulty processes eventually decide
3. Validity: The common output is some process input

Question: try to think of how to trivially provide an algorithm that solves any 2 conditions

## Synchronous vs Asynchronous

Synchronous consensus operates on the assumption that messages are delivered within a known, finite time bound, allowing nodes to reach agreement in distinct, coordinated rounds. In contrast, asynchronous consensus assumes messages can be delayed arbitrarily, meaning no assumptions are made about timing, only that messages will eventually arrive.

- Synchronous: Faster and simpler to design because nodes can "wait" for a specific time, but it breaks down if the network delays messages beyond that expected limit.
- Asynchronous: More robust and suitable for unpredictable networks like the internet (e.g., blockchains), as it preserves safety even if messages are severely delayed, though it is harder to achieve fast finality.

## Methods of Communication

We distinguish between two main categories of communication

1. Message Passing - processes communicate explicitly by sending and recieving messages, the FLP result shows that deterministic consensus is impossible even if one process fails undetectibaly.

2. Shared Memory - processes communicate implicitly via shared memory like a shared register in hardware. Loui and Abu proved the analogous impossibility to FLP in this case. In this model we can achieve "wait-free" (up to n-1 crashes) consensus, as you can still read the "dead" processes messages from the shared uunlike message passing

## Adversary

An adversary in the purest sense is a function from partial executions to operations, it picks which operation happens next. We will draw the distnction between a "Strong" and a "Weak" Adversary.

- Strong Adversary - Sees the full execution history e.g. past coin flips in the randomized case, process states, messages / register values.

- Weak Adversary - There are many different verstions, each takes away some power from the strong adversary, well discuss this briefly later when relevant.
