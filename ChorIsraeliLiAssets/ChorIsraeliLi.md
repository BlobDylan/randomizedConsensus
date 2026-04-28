OK. The protocol is a single loop, but each iteration has three logical phases:

**Phase 1 (Announce):** Write your (preference, round) to your own register, then read everyone else's registers. Now you have a snapshot of where everyone is and what they prefer. Compute maxround = the highest round you see.

**Phase 2 (Check if race is won):** Look at all processes that are "near the front" — specifically those with round ≥ maxround−1. If they ALL share the same preference v, the race is over — decide v and exit. The logic: nobody close to the lead disagrees, and anyone further behind will adopt the leaders' preference by the time they catch up (because of phase 3 below). If there's still disagreement among the frontrunners, don't decide — move on.

**Phase 3 (Catch up + random advance):** Two things happen:

- **Adopt the leaders' value:** If all processes at maxround agree on some value v, set your preference to v. This is how slow processes get pulled toward the leader's opinion.
- **Random advancement:** With probability 1/(2n), increment your round (jumping to at least maxround−2 so you're never too far behind). With probability 1 − 1/(2n), stay where you are.

Then go back to phase 1.

**Why agreement holds:** The decision condition in phase 2 requires ALL near-front processes to agree. If process P decides v, it means everyone within 1 round of the lead preferred v at the time P read the registers. Any slower process Q that hasn't decided yet will eventually reach phase 3, see that the leaders all prefer v, adopt v, and either decide v itself or carry v forward.

**Why validity holds:** No process ever changes its preference unless it sees some other process with a different preference at a higher round (phase 3 adoption). If everyone starts with the same input v, nobody ever switches away from v, so everyone decides v.

**Why it terminates:** Each iteration, each process advances with probability 1/(2n). Most of the time nothing happens — processes stay at their current rounds. But occasionally, one process gets a streak of advances while its opponents don't. Once that process is 2 rounds ahead of all dissenters, the decision condition triggers. Expected iterations until a leader emerges: O(n). Each iteration costs O(n) reads, so O(n²) total work.

**Why it breaks under a strong adversary:** The strong adversary can see that a process chose to advance (internally, before writing). It freezes that process, lets every other process run and catch up to the same round, then unfreezes it. Result: nobody ever gets ahead, no leader emerges, no decision. The weak adversary can't do this because it can't see the advancement decision until it's written out — by then it's too late.
