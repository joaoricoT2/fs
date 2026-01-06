# CEO Code Rules for AI-Assisted Workflows

## Part I: The Original Rules (Victor Cheng)

These rules, from Victor Cheng (CaseInterview.com), describe how to communicate effectively with executives. The insight: the system should prompt the human user this way.

### Rule #1: Problems Come With Solutions
Never mention a problem without proposing a specific solution or a next step to finding a solution.

### Rule #2: Facts Before Opinions
Always separate facts from opinions. Always present facts first.

### Rule #3: Anticipate Questions
Always anticipate the 5 most logical and intuitive questions a CEO is likely to ask. Proactively mention each question and answer it before you are asked verbally.

### Rule #4: Structured Proposals
When you make a proposal to a CEO, state what you want to do, your three reasons why you want to do it, and what you need from them to proceed.

---

## Part II: Extended Analysis — Applying CEO Code to AI Workflows

### 2.1 Why This Matters for Formoso e Seguro

The "CEO Code" rules aren't just executive communication tips — they're principles for **efficient information exchange under cognitive constraints**.

A CEO's situation:
- Limited time and attention
- Must make many decisions
- Can't go deep on every issue
- Needs to trust the presenter's judgment

A human using an AI workflow has the same constraints:
- Limited cognitive bandwidth
- Many decisions per session
- Can't verify every AI output in depth
- Needs to trust the process

**The insight:** AI outputs should be structured like CEO briefings — concise, actionable, pre-answered, and solution-oriented.

### 2.2 Rule-by-Rule Application

#### Rule #1 Applied: AI Outputs Should Be Actionable

**Bad AI output:**
> "There's a potential issue with the caching layer."

**Good AI output (CEO Code compliant):**
> "The caching layer has a race condition in invalidation (lines 142-156 of cache.ts). Proposed fix: add mutex lock before invalidation. Alternative: switch to write-through caching. Recommendation: mutex lock (simpler, lower risk). Next step: approve approach, then I'll implement."

**Workflow implication:** Every phase output should end with:
- What was found
- What's proposed
- What decision the human needs to make

#### Rule #2 Applied: Separate Observations from Interpretations

**Bad AI output:**
> "This design is risky because it might have performance issues."

**Good AI output (CEO Code compliant):**
> "**Fact:** The design uses nested loops over unbounded collections (lines 45-60). Current test data is 100 items; production data is 10,000+.
>
> **Interpretation:** This will likely cause O(n²) performance degradation at production scale.
>
> **Recommendation:** Refactor to hash-based lookup (O(n) → O(1) inner loop)."

**Workflow implication:** Critiques should clearly label:
- What is observed (code, spec, behavior)
- What is inferred (risk, impact, cause)
- What is recommended (action, priority)

#### Rule #3 Applied: Pre-Answer Predictable Questions

When presenting a design decision, the human will predictably ask:
1. Why this approach over alternatives?
2. What are the risks?
3. How long will it take?
4. What could go wrong?
5. Is it reversible?

**CEO Code compliant output:**
> "**Proposed:** Use Redis for session storage.
>
> **Why not alternatives?** In-memory doesn't persist across restarts. PostgreSQL adds latency. Redis is the standard for this use case.
>
> **Risks:** Redis is a new dependency; adds operational complexity.
>
> **Reversibility:** High — interface is abstracted; can swap storage backend later.
>
> **What could go wrong:** Redis unavailability would break sessions. Mitigation: fallback to in-memory with degraded UX."

**Workflow implication:** RFC and Spec templates should require these sections by default. Don't wait for the human to ask.

#### Rule #4 Applied: Clear Ask Structure

**Bad AI output:**
> "I think we should maybe consider refactoring the auth module if you think it makes sense."

**Good AI output (CEO Code compliant):**
> "**Proposal:** Refactor auth module to use JWT instead of session tokens.
>
> **Three reasons:**
> 1. Enables stateless scaling (current bottleneck for Phase 2)
> 2. Reduces database load by 40% (session lookups eliminated)
> 3. Industry standard for our API consumers
>
> **What I need from you:**
> - Confirm this is worth 3-4 days of work
> - Decide: break backward compatibility or maintain both flows temporarily?"

**Workflow implication:** Every gate that requires human approval should present:
- The proposal (what)
- The justification (why, in threes)
- The specific decision needed (binary or small choice set)

### 2.3 Implications for Question Funnel Design

The Question Funnel (Phase 1b-1d) is where the AI asks the human for input. CEO Code applies in reverse — the AI is now the "briefer" asking for the executive's decision.

**CEO Code for questions:**

| Rule | Application to Questions |
|------|-------------------------|
| #1 (Solutions) | Don't just ask — propose a default answer and ask for confirmation or override |
| #2 (Facts first) | Explain why the question matters before asking it |
| #3 (Anticipate) | Group related questions; don't ask one at a time |
| #4 (Structured ask) | Make the decision space clear (Option A / Option B / Other) |

**Bad question:**
> "What database should we use?"

**Good question (CEO Code compliant):**
> "**Question:** Which database for user data?
>
> **Why it matters:** Affects scaling strategy, query patterns, and operational complexity.
>
> **Options:**
> - **PostgreSQL** (recommended): Team expertise, relational integrity, proven at our scale
> - **MongoDB**: Flexible schema, but we don't need it; adds operational burden
> - **SQLite**: Only if this stays single-instance forever
>
> **Default if you don't have preference:** PostgreSQL
>
> **Your answer:** [Confirm default / Choose alternative / Provide context]"

### 2.4 Implications for Gate Design

Gates are where the human makes go/no-go decisions. CEO Code makes gates efficient.

**Bad gate:**
> "Spec is ready. Approve? [Yes/No]"

**Good gate (CEO Code compliant):**
> "**GATE: Spec Approval (Phase 2g)**
>
> **Summary:** Caching layer design with Redis backend, LRU eviction, 15-minute TTL.
>
> **Key decisions embedded:**
> - Redis over in-memory (scalability)
> - TTL over size-based eviction (predictability)
> - No cache warming (YAGNI for now)
>
> **Top risks identified by Red Team:**
> 1. Cache stampede on cold start — mitigated by staggered TTL
> 2. Redis failure — mitigated by fallback to origin with circuit breaker
>
> **What I need from you:**
> - [ ] Confirm Redis is acceptable operationally
> - [ ] Confirm 15-minute TTL is appropriate for your freshness requirements
> - [ ] Approve to proceed, OR specify concerns
>
> **Approve / Request changes / Need more info**"

### 2.5 The Meta-Principle: Respect the Human's Scarcity

CEO Code exists because executives' attention is scarce and valuable. The same is true for any human in an AI workflow:

- **Attention is limited** — Don't waste it on things the AI can decide
- **Decisions are costly** — Minimize decision count, maximize decision quality
- **Trust must be earned** — Structured output signals competence
- **Time is money** — Get to the point

**The workflow should feel like a well-prepared briefing, not a meandering conversation.**

### 2.6 Anti-Patterns to Avoid

| Anti-Pattern | CEO Code Violation | Fix |
|--------------|-------------------|-----|
| "What do you think?" without context | No facts, no options | Provide context + options + recommendation |
| Listing problems without solutions | Rule #1 violation | Every problem has a proposed fix |
| Mixing observations with speculation | Rule #2 violation | Label facts vs. interpretations |
| Asking questions one at a time | Inefficient; breaks flow | Batch related questions |
| Vague approval requests | Rule #4 violation | Specific ask with clear options |
| Burying the lead | Wastes attention | Key point first, details after |

### 2.7 Implementation in Prompt Templates

Every prompt template in the workflow should encode CEO Code principles:

```markdown
## Output Format Requirements

1. **Lead with the key finding or recommendation**
2. **Separate facts (observed) from interpretation (inferred)**
3. **For each issue identified, propose a specific fix**
4. **Anticipate and pre-answer the 5 obvious questions:**
   - Why this approach?
   - What are the alternatives?
   - What are the risks?
   - What's the cost/effort?
   - Is it reversible?
5. **End with a clear, structured ask:**
   - What you're proposing
   - Your top 3 reasons
   - What you need from the human to proceed
```

---

## Summary

The CEO Code rules are a compression algorithm for human-AI communication:

| Rule | Core Principle | Workflow Application |
|------|---------------|---------------------|
| #1 | Problems need solutions | Every issue comes with a proposed fix |
| #2 | Facts before opinions | Label observations vs. interpretations |
| #3 | Anticipate questions | Pre-answer the obvious 5 |
| #4 | Structured proposals | What + 3 reasons + specific ask |

The human's time and attention are the scarcest resources in the workflow. CEO Code ensures they're spent on decisions that matter, not on extracting information that should have been provided upfront.

**The goal:** Every AI output should be ready for a busy, intelligent person to review and decide in minimal time.
