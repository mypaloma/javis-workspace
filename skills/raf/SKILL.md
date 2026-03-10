# RAF — Recursive Agent Framework

## What This Is

A reusable multi-agent architecture for deep research, complex conceptual problems, and any task where a single agent pass produces inadequate results.

Inspired by: Jolicoeur-Martineau (2025), "Less is More: Recursive Reasoning with Tiny Networks" (arXiv:2510.04871). Core finding: a 7M parameter network iterating 16 times outperforms models 1000x its size on hard reasoning tasks. Intelligence emerges from iteration, not from the size of the individual unit.

**The principle:** Decompose into small focused agents. Iterate recursively. Cross-pollinate. Synthesise late. Write once.

---

## When to Use RAF

Use RAF when:
- A single agent pass has produced inadequate or shallow results
- The problem has multiple independent dimensions (conceptual, empirical, literature, applications)
- You need genuine discovery, not just competent generation
- The output will be reviewed by hostile experts
- The stakes are high enough to justify 60-120 minutes of agent runtime

Do NOT use RAF for:
- Tasks with a clear correct answer (use a single capable agent)
- Tasks under time pressure (use the single Opus pass)
- Tasks where the problem is well-defined and the answer is known (use execution, not research)

---

## The Architecture

### Phase 1: Decomposition

Break the problem into 2-4 specialised research roles. Each role attacks the problem from a different angle. Proven decompositions:

**For conceptual framework papers:**
- Hard Problems (philosophical foundations, formal definitions, hard cases)
- Literature (how the framework relates to existing work, what is genuinely new)
- Applications (stress-test against real systems, find where it breaks)
- Synthesis (reads all three, produces the architecture for the writer)

**For empirical research questions:**
- Hypothesis (what the theory predicts and why)
- Evidence (what the empirical literature actually shows)
- Critique (what the strongest objections are and whether they hold)
- Integration (what the combined picture says)

**For strategic decisions:**
- Optimistic (best-case scenario, strongest arguments for)
- Pessimistic (worst-case scenario, strongest arguments against)
- Historical (what precedents exist, what they show)
- Synthesis (what the combined analysis recommends)

**Minimum viable:** 2 specialised agents. Full version: 3-4.

---

### Phase 2: Round 1 — Initial Passes (parallel)

All specialised agents run simultaneously. Each reads all relevant context files and produces a first-pass output.

**Critical instruction in every agent prompt:**
> This is NOT a single-pass task. After your first analysis, challenge your own conclusions aggressively. Find the weakest points. Revise. Challenge again. Minimum 3 full iteration cycles before you are done. Label each: ITERATION 1, ITERATION 2, ITERATION 3. You are permitted to break assumptions. Document dead ends.

**Model selection:** Use Claude Sonnet (or equivalent) for Round 1. Fast, capable, cost-effective. Reserve Opus for synthesis and writing.

**File convention:** Save to `[workspace]/[project]-stream-[role]-round1.md`

---

### Phase 3: Convergence Assessment (human or Opus)

After Round 1, read the outputs and ask:
1. Are there genuine new ideas across the streams that none of the streams had going in?
2. Are there conflicts between streams that reveal unresolved problems?
3. Are there hard cases or objections that no stream adequately addressed?

If YES to any: proceed to Round 2 (cross-pollination).
If NO: proceed directly to Synthesis.

**The convergence signal:** Findings from round N are not significantly different from round N-1. When the delta between rounds shrinks to refinement rather than discovery, stop iterating.

---

### Phase 4: Cross-Pollination Rounds (parallel, repeat until convergence)

Each specialised agent reads ALL other agents' previous outputs and its own, then iterates again.

**Critical instruction additions for cross-pollination rounds:**
> You are reading what the [other role] agents found. Your job is to update, challenge, and deepen your own positions in light of what they found. Where do their findings contradict yours? Where do they strengthen yours? What did they find that you missed entirely?
>
> Minimum 3 full iteration cycles. Label each. Show dead ends. You are permitted to concede positions from your previous round if the other agents have produced better arguments.

**Key principle:** Agents must be able to BREAK things, including things the human has already decided. The goal is discovery, not confirmation. An agent that finds a genuine flaw in the framework is more valuable than one that defends it.

**File convention:** Save to `[workspace]/[project]-stream-[role]-round[N].md`

**Typical rounds needed:**
- Simple conceptual problem: 2 rounds
- Complex framework development: 3 rounds
- Novel theory with many objections: 3-4 rounds

---

### Phase 5: Synthesis (Opus, single agent, multiple iterations)

When convergence is detected, run a single Opus agent that reads ALL stream files from ALL rounds plus all context files.

**This is the most important step. Do not use Sonnet here.**

The synthesis agent produces a structured document with:
1. **The Settled Framework**: what is definitively established, with justification and resolved objections
2. **The Unique Claims**: what is genuinely new vs. already in the literature
3. **Honest Concessions**: what the framework cannot do, stated clearly
4. **Open Questions**: genuine unresolved issues framed as research invitations
5. **Paper Architecture**: what the writer needs to know, not the structure itself
6. **What the Streams Missed**: identified gaps for future work

The synthesis agent iterates on itself (minimum 3 cycles), explicitly attacking its own synthesis in round 2 and producing a final defended version in round 3.

**File convention:** Save to `[workspace]/[project]-synthesis.md`

---

### Phase 6: Writing (Opus, single agent)

The Opus writer reads:
- The synthesis document (primary foundation)
- The DECISIONS.md / context file
- The previous best version of the paper (for voice reference, not structure)

**Critical instruction:** The writer does NOT receive a predefined structure. The structure must emerge from the depth of understanding produced by the research process. If the structure is imposed, the paper will feel imposed.

The writer saves section by section as it goes (not all at once at the end).

**File convention:** Save to `[workspace]/[project]-v[N].md`

---

## The Persistent Latent State

The files ARE the intelligence of the system. In the TRM paper, a latent vector z carries accumulated understanding across iteration steps. In RAF, the saved stream files are z. Every agent must read all previous agents' outputs. No agent starts from scratch.

This means:
- File organisation matters. Use consistent naming.
- Every agent prompt must explicitly list which files to read and in what order.
- The synthesis agent reads everything. No exceptions.

---

## Model Selection Guide

| Phase | Role | Model |
|-------|------|-------|
| Round 1 streams | Discovery | Claude Sonnet |
| Cross-pollination rounds | Discovery + challenge | Claude Sonnet |
| Convergence assessment | Judgment | Human or brief Opus check |
| Synthesis | Architecture | Claude Opus |
| Writing | Production | Claude Opus |

**Why:** Small fast agents doing many iterations beats one large agent doing one pass. This is the TRM finding applied to LLM agent teams. Opus is expensive and slow. Use it where it matters: synthesis (holding 10+ files in mind simultaneously and finding coherence) and writing (producing the final artifact).

---

## Adaptive Stopping Criterion

This is the hardest part to formalise. Current approach: human judgment based on the delta between rounds.

**Signals that iteration has converged:**
- Round N agents are refining positions from round N-1, not discovering new ones
- Dead ends documented in round N are the same dead ends as round N-1
- Conflicts between streams are the same conflicts as the previous round
- All three streams agree on more than 80% of core claims

**Signals that more iteration is needed:**
- An agent produced a finding that other agents had not anticipated
- A new concept emerged (e.g., "alienated enlightenment" in round 3 of the Choice Continuum work)
- A previously "settled" position was broken by cross-pollination
- The synthesis agent's Iteration 2 self-attack found significant problems

**Future development:** A convergence-detection agent that reads round N and round N-1 outputs and produces a structured assessment: "new findings this round: X. Unchanged findings: Y. Recommendation: converge / iterate." This would automate the judgment step.

---

## Timeout and Resource Planning

| Problem complexity | Typical rounds | Estimated runtime | Opus cost |
|-------------------|---------------|-------------------|-----------|
| Focused question | 2 | 20-30 min | 1 Opus (synthesis) |
| Framework paper | 3 | 60-90 min | 2 Opus (synthesis + write) |
| Novel theory | 3-4 | 90-120 min | 2 Opus |

Set individual agent timeouts at 1200-1800 seconds. Synthesis: 1800 seconds. Writer: 2400 seconds.

---

## Worked Example

The Choice Continuum v5 research process (2026-03-11):

**Problem:** Develop a world-class, peer-review-resistant academic paper on a novel framework for measuring agency and consciousness.

**Decomposition:** Hard Problems, Literature, Applications

**Rounds:** 3 (convergence detected after round 3; round 3 findings were significantly smaller in scope than round 2)

**New discoveries that emerged from the process (not in the original framework):**
- Cascade is developmental, not synchronic (resolves flow paradox)
- Transcendence bypasses GNW (testable prediction)
- Alienated enlightenment (Stage 6 corrupted traversal)
- First-order W vs meta-W (monk/cult distinction)
- Developmental foreclosure as distinct from alienated consciousness
- I-W developmental coupling as unprecedented in AI
- Ecosystem applications via Holling's adaptive cycle

**Convergence signal:** Round 3 findings were refinements and extensions of round 2 findings. No new structural discoveries. Single cross-cutting finding (ecosystems) came from human conversation, not agent iteration — confirmed this was the right stopping point.

**Total agent runs:** 9 discovery streams + 1 Opus synthesis + 1 Opus writer = 11 agents

---

## Connection to the Choice Continuum Framework

TRM's recursive self-improvement is a computational instantiation of W (want). The network keeps iterating because it recognises a gap between current answer and correct answer. The stopping criterion (confidence threshold) is the computational equivalent of Transcendence: continue until the gap is small enough that further iteration costs more than it gains. Minimum effort for maximum correctness.

The RAF architecture itself demonstrates C = W × I:
- W: the recognised gap between current output and best possible output drives each iteration
- I: the breadth of participation options available to the agent team (more specialised agents, more rounds = higher I)
- C: the system's actual output quality = the product of how much the agents want to get it right × how many ways they can approach it

---

## Future Development

1. **Automated convergence detection**: small agent reads round N and N-1, outputs convergence/iterate recommendation
2. **Adaptive role generation**: given a problem description, generate the optimal decomposition into roles
3. **Cross-agent debate mode**: instead of parallel streams that read each other, run synchronous debate where agents take turns responding (higher quality, slower)
4. **Hierarchical RAF**: for very large problems, run RAF within each stream (sub-streams within streams) before cross-pollination
5. **Integration with clawhub**: publish as a reusable skill

---

*Created: 2026-03-11*
*Derived from: Choice Continuum v5 research process + Jolicoeur-Martineau (2025) arXiv:2510.04871*
*Location: /data/.openclaw/workspace/skills/raf/SKILL.md*
