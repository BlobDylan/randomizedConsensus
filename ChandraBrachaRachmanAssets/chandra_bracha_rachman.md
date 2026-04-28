# Chandra's Protocol with Bracha-Rachman Shared Coin

This is two nested algorithms: Chandra's consensus framework calls Bracha-Rachman's shared coin as a subroutine.

---

## Part A — Chandra's Consensus Framework

### 1. Settings

- **Communication:** Shared memory. Two arrays of multi-writer bits: mark[0][r] and mark[1][r] for each round r.
- **Consensus type:** Binary.
- **Fault tolerance:** Wait-free — tolerates up to n−1 crash failures.
- **Subprotocol required:** A weak shared coin for each round.

### 2. Adversary

**Strong adversary.** Sees everything — process states, register contents, past coin-flip outcomes.

### 3. Complexity

- **Operations per round:** O(1) — a few reads/writes of mark bits.
- **Expected rounds to converge:** O(1/ε) = O(1), where ε is the shared coin's agreement probability.
- **Total cost dominated by the shared coin subroutine.**

### 4. Algorithm

**Data structure:** mark[b][r] is a multi-writer bit. Setting mark[b][r] = true means "some process with preference b has reached round r." Initially mark[0][0] = mark[1][0] = true, everything else false.

These bits are **monotonic** (only transition: false → true). Concurrent writes are safe — everyone writes "true", so order doesn't matter and no information is lost.

Each process maintains preference p and round r.

#### Phase 1 — Announce

Set mark[p][r] ← true.

**Intuition:** You declare "preference p has a representative at round r." This is immediately visible to all processes. Unlike Chor-Israeli-Li where you read n registers to figure out where each side's frontrunner is, here you just check a few bits — much cheaper.

#### Phase 2 — Compare Positions

Read mark[1−p] at rounds r+1, r, r−1 (in that order). Four cases based on how far ahead or behind you are relative to the opposing side's frontrunner:

**Case 1 — Behind** (mark[1−p][r+1] is set):
→ p' ← 1−p. Switch to opponent's preference.

**Intuition:** The other side has someone a full round ahead of you. You're losing — give up and join them.

**Case 2 — Tied** (mark[1−p][r] is set, but not r+1):
→ p' ← SharedCoin_r(). Use the shared coin to pick a new preference.

**Intuition:** The other side is at the same round as you. Neither side is winning. This is where randomization enters. With a shared coin, all tied processes agree with constant probability ε — exponentially better than independent local coins (which would give 1/2^Θ(n) agreement probability, like Ben-Or). This is exactly why the protocol is polynomial instead of exponential.

**Case 3 — One round ahead** (mark[1−p][r−1] is set, but not r or r+1):
→ p' ← p. Keep your preference but do NOT decide.

**Intuition:** You're ahead but not safely. An opponent at round r−1 might be about to write mark[1−p][r] and think it's tied with you. If you decided now, that opponent could flip the shared coin and land on the wrong value, breaking agreement. Wait one more round to be sure.

**Case 4 — Two+ rounds ahead** (none of the above are set):
→ **return p. Decide and exit.**

**Intuition:** No opponent has reached round r−1 yet. When any opponent Q eventually gets there, Q will see your mark at round r, putting Q in Case 1 — Q switches to your preference. Every even slower process will similarly get pulled along. It's safe to commit.

#### Phase 3 — Safety Check

Check if mark[p][r+1] is already set (an ally with your preference already advanced to the next round).

- If yes → discard p', keep old preference p.
- If no → adopt p' as your new preference.

**Intuition:** This prevents your own side from splitting. Suppose you're about to flip the shared coin and switch preference. But an ally already moved ahead with your old preference. If you switch, you'd diverge from your ally. By deferring to the ally (keeping the old preference), you stay aligned with the fastest member of your own team.

Advance r ← r+1, go back to Phase 1.

### 5. Correctness

#### Agreement

**Deciding process is safe (Case 4):** If P decides p at round r, no opponent has reached round r−1. When any opponent Q eventually reaches r−1, Q sees mark[p][r] and falls into Case 1 — Q switches to p. So Q never decides anything other than p.

**Same-side processes don't diverge (Phase 3):** The safety check ensures that if an ally is already ahead, you follow them rather than flipping the coin and potentially splitting. This prevents a subtle bug where P decides p, but a slower ally P' hits a tie in an earlier round, flips the coin, and switches away.

**Key subtlety:** Correctness depends on the precise order of reads in Phase 2 — checking r+1 first, then r, then r−1. If you checked r−1 first, you might think you're ahead when you're actually behind (because the opponent wrote mark[1−p][r+1] between your reads). The descending order ensures you always see the worst case first.

#### Validity

No process changes its preference unless it sees a mark from the opposing side (meaning someone actually has a different input). If all inputs are the same value v, mark[1−v] is never set beyond round 0. Every process immediately satisfies Case 4 (two+ rounds ahead of nonexistent opponents) and decides v.

**Intuition:** The protocol only changes preferences in response to disagreement. No disagreement → no change → immediate decision on the common input.

#### Termination

In each round, every process falls into one of two categories:

- **(a) Leaders** — processes in Cases 1, 3, or 4. These processes agree with each other (they either decided, kept their preference, or switched to the winning side).
- **(b) Tied** — processes in Case 2. These flip the shared coin.

Group (a) already agrees. Group (b) will agree with group (a)'s value with probability ≥ ε (shared coin guarantee). So with probability ≥ ε, everyone aligns after this round. Expected rounds to consensus: O(1/ε) = O(1).

**Subtlety:** Some processes might skip the coin — they read mark[1−p][r] just before an opponent writes it, so they think they're ahead (Case 3) when they're actually tied. But these processes keep their preference, and they agree with each other. The coin-flippers just need to match them, which happens with probability ≥ ε.

---

## Part B — Bracha-Rachman Shared Coin

### 1. Settings

- **Communication:** Shared memory. Each process has a single-writer register r[p] holding a pair (flips, ones), initially (0,0).
- **Purpose:** Implements the weak shared coin subroutine used by Chandra's framework.
- **Output:** Each process outputs 0 or 1. Guarantee: for each value v, Pr[all processes output v] ≥ ε for some constant ε > 0.

### 2. Adversary

**Strong adversary.** The adversary's attack: between flipping a coin (Line 1) and writing the result (Line 2), the adversary can see the coin outcome and selectively delay the process to **hide** that vote. This is the "hidden votes" mechanism.

### 3. Complexity

- **Total votes cast (all processes):** O(n²).
- **Batch size:** n/log(n) flips between termination checks.
- **Termination check cost:** O(n) reads.
- **Amortized cost per vote:** O(log n) — the O(n) read cost is spread over n/log(n) flips.
- **Total expected work:** O(n² log n).
- **Combined with Chandra's framework:** O(n² log n) total, since the framework adds only O(1) overhead per round.

### 4. Algorithm

#### Phase 1 — Vote in Batches

Repeat the following:

1. Flip n/log(n) coins, one at a time. For each coin flip:
   - c ← CoinFlip() (Line 1)
   - Write updated (flips+1, ones+c) to your register r[p] (Line 2)
2. Read all n registers (Line 3). Sum everyone's flip counts into total.
3. If total > n² → exit loop and go to Phase 2. Otherwise → do another batch.

**Intuition:** You're casting votes (fair coin flips) and writing running totals to your own register. The batching is the key innovation: you do n/log(n) cheap flips (O(1) each) before paying for one expensive termination check (O(n) reads). This amortizes the read cost so each flip pays O(log n) amortized instead of O(n). Without batching, checking termination after every flip would cost O(n) per flip × n² total flips = O(n³). The batching brings this down to O(n² log n).

The tradeoff: batching creates more "extra votes" (up to n/log(n) per process instead of 1), but the analysis shows this is tolerable since extra votes are unbiased. Hidden votes are the real threat, and those are capped at 1 per process regardless of batch size.

#### Phase 2 — Read and Decide

1. Read all registers one final time (Line 4).
2. Compute total = sum of all flips, ones = sum of all ones.
3. If ones/total ≥ 1/2 → return 1. Otherwise → return 0.

**Intuition:** Take a final snapshot, compute majority of all visible votes, output it.

### 5. Correctness (Shared Coin Agreement Guarantee)

The argument classifies votes into three categories:

**Common votes:** All votes generated before global total flips exceeds n². Between n²+1 and n²+n such votes. These are unbiased fair coins — the adversary controls scheduling but not outcomes.

**Extra votes (per process P):** Votes generated after the n² threshold but before P's final read in Line 4. Each process Q contributes at most n/log(n) extras (one batch worth, because Q would have noticed termination at the next check). Also unbiased fair coins. More numerous than hidden votes but less dangerous since unbiased.

**Hidden votes (per process P):** Votes flipped by process Q but not yet written to r[Q] when P reads r[Q]. At most n−1 total (one per other process). This is the adversary's weapon — it sees the coin result and can selectively delay the write to suppress unfavorable votes.

**What P actually sees:** (common votes) + (extra votes for P) − (hidden votes for P).

**Why it works — the probability argument:**

1. The n² common votes are fair coins. By the normal approximation (std dev ≈ n/2), the net majority for 1 exceeds 3n with some constant probability p. (A 3n majority out of n² votes is only a few standard deviations — constant probability.)

2. The n²/log(n) extra votes per process are also fair. The probability that a random walk of length n²/log(n) ever drops below −2n is bounded by 1/n² (standard random walk extremum bound). Union bound over n processes: probability any process's extras cause trouble ≤ 1/n.

3. So with probability ≥ p(1 − 1/n) ≈ p/2, common + extra votes give a majority of at least n for ALL processes simultaneously.

4. The adversary hides at most n−1 votes per process. Since n−1 < n, hidden votes can't flip the majority. Every process sees a positive net vote for 1. Everyone outputs 1.

By symmetry, the same argument holds for value 0. The coin guarantees ε ≥ p/2 = Ω(1) agreement probability per value.

**Intuition:** The core insight is that n² fair votes build a Θ(n)-sized majority with constant probability. The adversary's only tool — hiding votes — can remove at most n−1 votes, which isn't enough to overcome a Θ(n) majority. Extra votes add noise but are unbiased, so they don't systematically erode the majority. The adversary can choose WHEN to schedule coin flips, but not WHAT they return — and with n² flips, the law of large numbers works in the protocol's favor.
