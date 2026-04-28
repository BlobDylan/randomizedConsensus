# Chor-Israeli-Li Protocol

## 1. Settings

- **Communication:** Shared memory. Each process has a single-writer multi-reader register.
- **Consensus type:** Arbitrary input range (not just binary).
- **Fault tolerance:** Wait-free — tolerates up to n−1 crash failures. Any process can finish in isolation by reading registers left by dead processes.

## 2. Adversary

**Weak adversary.** Cannot observe the internal state of a process between when it generates a random value and when it writes that value to its register. In other words, the adversary can't see a coin-flip result until it appears in shared memory.

## 3. Complexity

- **Expected iterations until leader emerges:** O(n).
- **Work per iteration:** O(n) — one write + n reads.
- **Total expected work:** O(n²).
- **Why not exponential:** The weak adversary can't see which process chose to advance, so it can't construct a lockstep execution. A leader naturally emerges through random advancement.
- **Fails against strong adversary:** A strong adversary sees the advancement decision before the write, freezes the advancing process, lets everyone else catch up → lockstep forever, no leader emerges, no decision.

## 4. Algorithm

Each process maintains a **preference** (initially its input) and a **round number** (initially 1). Each iteration has three phases.

### Phase 1 — Announce and Observe

1. Write (preference, round) to your own register.
2. Read all n registers.
3. Compute maxround = highest round seen among all registers.

**Intuition:** You publish your state and take a snapshot of everyone else's state. The register is your only communication channel — it replaces message-passing. Because shared memory survives crashes, even a process running in isolation can read what dead processes left behind, which is why wait-freedom is possible here but not in message-passing.

### Phase 2 — Check if the Race is Won

Look at all processes "near the front" — those with round ≥ maxround − 1.

- If they ALL share the same preference v → **decide v and exit.**
- Otherwise → don't decide, move to Phase 3.

**Intuition:** If all frontrunners agree and nobody close behind disagrees, it's safe to commit. Any process further behind hasn't reached Phase 3 at this round yet, and when it does, it will see the leaders' preference and adopt it. The gap of 2 rounds provides the safety margin — a process can only influence the outcome if it's within 1 round of the lead.

### Phase 3 — Adopt Leader's Value and Randomly Advance

Two sub-steps:

1. **Adopt leader's value:** If all processes at maxround agree on v, set preference ← v.

   **Intuition:** Slow processes get pulled toward the leaders' opinion. This is how the race propagates — once a leader is ahead, everyone behind eventually adopts their value as they catch up.

2. **Random advancement:** With probability 1/(2n), set round ← max(round+1, maxround−2). With probability 1 − 1/(2n), stay put.

   **Intuition:** Low advancement probability means most processes stay still on any given iteration. Occasionally one process gets a lucky streak and pulls ahead while opponents don't. The jump to maxround−2 prevents processes from falling hopelessly behind. With n processes each advancing with probability 1/(2n), we get roughly one advancement every 2 iterations system-wide, and a leader emerges after O(n) iterations on average.

Go back to Phase 1.

## 5. Correctness

### Agreement

If process P decides v, it means all processes with round ≥ maxround−1 had preference v at the time P read the registers. Any slower process Q hasn't decided yet. When Q eventually reaches Phase 3, it sees the leaders' preference v and adopts it. When Q eventually reaches Phase 2, either the leaders still agree on v (Q decides v), or Q adopts again and keeps carrying v forward. No divergence is possible because the decision condition requires unanimity among all near-front processes.

**Intuition:** The race structure ensures one-directional influence — fast processes pull slow ones, never the reverse. Once the frontrunners agree, that agreement propagates backward through the field.

### Validity

No process changes its preference unless it sees a faster process with a different preference (Phase 3 adoption). If all inputs are the same value v, no process ever has a reason to switch away from v. The Phase-2 decision condition is immediately satisfied: all frontrunners agree on v. Everyone decides v in the first iteration.

**Intuition:** The protocol only changes preferences by copying from leaders. If leaders already agree with you, nothing changes.

### Termination

Each iteration, each process advances with probability 1/(2n). Eventually, by chance, one process gets several consecutive advances while its opponents don't. Once it's 2 rounds ahead of all dissenters, the Phase-2 decision condition triggers. The expected number of iterations until this happens is O(n).

**Intuition:** Think of it as a random race. n runners, each moves forward with probability 1/(2n) per step. After O(n) steps, one runner is likely to be 2 positions ahead of all runners on the opposing team. The weak adversary can handicap runners (by choosing who runs next), but it can't identify which runners moved (because it can't see the advancement choice before the write), so it can't specifically target the leaders.

**Why it fails under a strong adversary:** The strong adversary can see that a process chose to advance (internally, before writing). It freezes that process and lets every other process run and catch up to the same round. Result: all processes stay at the same round forever, no leader emerges, no decision. This lockstep attack is impossible for the weak adversary because it can't see the advancement decision until after the write.
