1. shared memory vs message passing - wait free (protocols that tolerate up to n-1 crash failures) are only possible with shared memory and not message passing
   page 12 of the review
2. the setting of the problem
3. adversary types
4. Background about Michael O. Rabin
5. after bracha rachman, further improvements in the strong adversary model stall
6. aspnes showed, there is a lower bound on shared coins, which is n^2/log^2(n) expected rounds
   after that, peopole pivoted to constructing protocols for weaker adversaries.

FLP Theorem
Deterministic consensus is impossible in the asynchronous model even for binary inputs and at most one crash failure.
We show: If there is a critical configuration, crashing a single node can create a bivalent leaf.
