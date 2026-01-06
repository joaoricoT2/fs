# The Complete "Formoso e Seguro" Workflow

## Version 3.0 — Multi-Model, File-Based, Question-Gated

---

# Part I: Philosophy & Foundations

## 1.1 Core Principles

### Principle 1: Double-Blind Before Convergence

No model sees another's work until it has produced its own:
- Design (RFC)
- Self-critique
- Edge-case register
- Implementation sketch

This preserves creative breadth and makes critiques grounded in experience, not reaction.

### Principle 2: Spec is the Contract

Code is a translation of the Spec. If reality forces a change:
1. STOP
2. Propose Spec amendment
3. Get approval
4. Update Spec
5. Then continue

The Spec is never silently violated. This is the **"Stop-the-Line" rule**.

### Principle 3: Multi-Pass Hardening

There's always one more thing. Each pass catches a different class of issue:
- **In-memory code** catches design-level mistakes
- **Cross-review** catches blind spots
- **Code on disk** catches integration issues
- **Final review** catches reality vs. intent gaps
- **Hardening** catches the stragglers

### Principle 4: Questions Before Design

Models should not guess at human decisions. All questions that require human judgment are:
1. Collected independently
2. Merged and categorized
3. Answered once by the human
4. Locked as constraints

This prevents late-stage surprises and decision drift.

### Principle 5: Tier A Messy, Tier B Crisp

- **Tier A (Working Memory)**: Verbose, exploratory, ephemeral. Lives in `.task/`, gitignored. Deleted when done.
- **Tier B (Permanent Record)**: Minimal, precise, lasting. Lives in `docs/`, committed. The code is the primary log; docs capture only the non-obvious.

### Principle 6: Same Process, Different Deliverable

The workflow applies at three levels:
- **L3 (Phase Planning)**: Deliverable is `phases.md`
- **L2 (Task Planning)**: Deliverable is `task_list.md`
- **L1 (Task Execution)**: Deliverable is `code`

The process is recursive. Each level uses the same pattern of diverge → question → converge → verify.

---

## 1.2 The Three Levels of Work

```
┌─────────────────────────────────────────────────────────────────┐
│  L3: PHASE PLANNING                                             │
│  ───────────────────                                            │
│  Question: "What are the major phases of this project?"         │
│  Frequency: Rare (project start, major pivot, quarterly review) │
│  Deliverable: phases.md                                         │
│  Stakes: High (shapes everything downstream)                    │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  L2: TASK PLANNING                                              │
│  ─────────────────                                              │
│  Question: "What tasks should we do in this phase?"             │
│  Frequency: Periodic (start of phase, weekly/biweekly)          │
│  Deliverable: task_list.md                                      │
│  Stakes: Medium (shapes near-term work)                         │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  L1: TASK EXECUTION                                             │
│  ─────────────────                                              │
│  Question: "How do we implement this specific task?"            │
│  Frequency: Frequent (daily)                                    │
│  Deliverable: Code                                              │
│  Stakes: Variable (depends on task)                             │
└─────────────────────────────────────────────────────────────────┘
```

---

## 1.3 The Core Pattern

Every level follows the same pattern:

```
DIVERGE (uncontaminated) → QUESTION GATE → CROSS-CRITIQUE → CONVERGE → VERIFY → HARDEN
```

Expanded:

```
1. ORIENT      — Understand context
2. QUESTION    — Surface unknowns, get human answers
3. DIVERGE     — Independent proposals (double-blind)
4. SELF-DOUBT  — Each critiques their own work (still blind)
   ════════════ CONTAMINATION BARRIER ════════════
5. CROSS-CRIT  — Each critiques the others
6. RED TEAM    — Adversarial stress test
7. CONVERGE    — Synthesize into single artifact
8. APPROVE     — Gate check before execution
9. EXECUTE     — Do the work (with multi-pass for L1)
10. VERIFY     — Final audit
11. HARDEN     — Fix issues, document learnings
12. CLOSE      — Archive working memory, commit permanent record
```

---

## 1.4 Model Roles

When using 2 models:

| Model | Primary Role | Strengths |
|-------|--------------|-----------|
| **Claude** | Architect & Synthesizer | Long context, nuanced reasoning, good at merging |
| **GPT** | Red Team & Auditor | Finding bugs, security issues, edge cases |

When using 3 models:

| Model | Primary Role | When to Deploy |
|-------|--------------|----------------|
| **Claude** | Architect & Synthesizer | Always |
| **GPT** | Red Team & Auditor | Always |
| **Gemini** | Lateral Thinker & Judge | High-stakes tasks, tie-breaking, fresh perspective |

### When to use 3 models:

**Worth it:**
- Phase Planning (L3) — stakes are high
- High-risk tasks — security, breaking changes, core logic
- When 2 models converge suspiciously fast — echo chamber risk
- Maximum divergence in Idea Harvest

**Not worth it:**
- Full independent implementation for all three (expensive, merge hell)
- Simple tasks
- When 2 models already disagree strongly (3rd voice adds noise)

### Best use of Model #3:

- **Early**: Idea Harvest, edge-case brainstorm (divergence)
- **Middle**: Tie-breaker when Claude and GPT disagree on approach
- **Late**: Fresh red-team on final spec (hasn't seen the process, pure outsider)

---

# Part II: Infrastructure

## 2.1 Folder Structure

```
project/
├── .task/                          # TIER A: Working Memory (gitignored)
│   ├── _template/                  # Copy this to start new task
│   │   ├── 00_status.md
│   │   ├── 01_brief.md
│   │   ├── 02_questions/
│   │   ├── 03_design/
│   │   ├── 04_reviews/
│   │   ├── 05_implementation/
│   │   └── 06_final/
│   │
│   └── TASK_NAME/                  # Current task workspace
│       ├── 00_status.md            # Current step, next step, blockers
│       ├── 01_brief.md             # Initial task description
│       │
│       ├── 02_questions/
│       │   ├── claude_questions.md
│       │   ├── gpt_questions.md
│       │   ├── gemini_questions.md # If using 3 models
│       │   ├── consolidated.md     # Merged questions
│       │   └── answers.md          # Human answers
│       │
│       ├── 03_design/
│       │   ├── claude/
│       │   │   ├── idea_harvest.md
│       │   │   ├── rfc.md
│       │   │   └── self_critique.md
│       │   ├── gpt/
│       │   │   ├── idea_harvest.md
│       │   │   ├── rfc.md
│       │   │   └── self_critique.md
│       │   ├── gemini/             # If using 3 models
│       │   │   └── ...
│       │   └── synthesis/
│       │       ├── spec_v1.md
│       │       ├── spec_v2.md      # Iterations if needed
│       │       └── red_team.md
│       │
│       ├── 04_reviews/
│       │   ├── claude_reviews_gpt.md
│       │   ├── gpt_reviews_claude.md
│       │   ├── murder_board.md     # Combined critique synthesis
│       │   └── ...
│       │
│       ├── 05_implementation/
│       │   ├── claude/
│       │   │   ├── code.md         # In-memory implementation
│       │   │   └── difficulties.md
│       │   ├── gpt/
│       │   │   ├── code.md
│       │   │   └── difficulties.md
│       │   ├── synthesis_decision.md
│       │   └── spec_amendments.md  # If spec needed updates
│       │
│       └── 06_final/
│           ├── final_review.md
│           ├── hardening_notes.md
│           └── retro.md
│
├── .project/                       # PROJECT MEMORY (persists across tasks)
│   ├── phases.md                   # L3: Phase definitions
│   ├── phases_history/             # Previous versions
│   ├── current_phase/
│   │   ├── task_list.md           # L2: Current task list
│   │   ├── task_list_history/
│   │   └── completed.md
│   ├── decisions_log.md            # Running log of human decisions
│   └── learnings.md                # Cross-task learnings
│
├── docs/                           # TIER B: Permanent Record (committed)
│   ├── briefs/                     # Task briefs (only if lasting value)
│   ├── specs/                      # Frozen specs (condensed)
│   ├── adr/                        # Architecture Decision Records
│   ├── footguns/                   # Real traps for future devs
│   └── process/                    # The workflow itself
│       ├── protocol.md             # This document
│       └── templates/
│
└── src/                            # THE CODE (Ultimate Truth)
```

### .gitignore additions:

```gitignore
# Working memory (ephemeral)
.task/

# Project memory (optional - you may want to commit this)
# .project/
```

---

## 2.2 File Naming Convention

Files are numbered to show sequence. Within each numbered folder, files are named by model or purpose.

```
02_questions/
├── claude_questions.md     # Claude's raw questions
├── gpt_questions.md        # GPT's raw questions
├── consolidated.md         # Merged list
└── answers.md              # Human's answers

03_design/
├── claude/
│   ├── idea_harvest.md
│   ├── rfc.md
│   └── self_critique.md
└── synthesis/
    ├── spec_v1.md          # First synthesis attempt
    ├── spec_v2.md          # After addressing review feedback
    └── spec_final.md       # Approved version
```

---

## 2.3 The Status File

Every task has `00_status.md` — a one-pager showing current state:

```markdown
# Task: [TASK_NAME]

## Current Phase
Phase 2d: Cross-Critique

## Current Step
Waiting for GPT to review Claude's RFC

## Completed
- [x] 1a: Orientation (both)
- [x] 1b: Questions (both)
- [x] 1c: Questions consolidated
- [x] 1d: Human answers
- [x] 2a: Idea Harvest (both)
- [x] 2b: RFC (both)
- [x] 2c: Self-Critique (both)
- [ ] 2d: Cross-Critique (in progress)
- [ ] 2e: Red Team
- [ ] 2f: Synthesis
- [ ] 2g: Approval

## Blockers
None

## Notes
Claude's RFC favors approach A, GPT's favors approach B. 
Expecting interesting cross-critique.

## Next Action
Run GPT cross-critique prompt with Claude's RFC as input
```

This file is updated after each step. When you return to a task after interruption, read this first.

---

## 2.4 Starting a New Task

```bash
# Create workspace from template
cp -r .task/_template .task/add-caching-layer

# Update status
echo "# Task: add-caching-layer" > .task/add-caching-layer/00_status.md

# Write initial brief
vim .task/add-caching-layer/01_brief.md
```

Or with a script:

```bash
#!/bin/bash
# task-init.sh

TASK_NAME=$1
if [ -z "$TASK_NAME" ]; then
    echo "Usage: task-init <task-name>"
    exit 1
fi

TASK_DIR=".task/$TASK_NAME"
mkdir -p "$TASK_DIR"/{02_questions,03_design/{claude,gpt,gemini,synthesis},04_reviews,05_implementation/{claude,gpt},06_final}

cat > "$TASK_DIR/00_status.md" << EOF
# Task: $TASK_NAME

## Current Phase
Phase 1a: Orientation

## Completed
- [ ] 1a: Orientation
- [ ] 1b: Questions
...

## Blockers
None

## Next Action
Run orientation prompt for all models
EOF

echo "Created task workspace at $TASK_DIR"
```

---

# Part III: The Question Funnel

## 3.1 Why Questions Matter

Models often guess at human decisions because they're trained to be helpful. This leads to:
- Incorrect assumptions baked into designs
- Late-stage surprises ("wait, you assumed X?")
- Decision drift across the process

The Question Funnel forces unknowns to surface early, get answered once, and become locked constraints.

---

## 3.2 Question Categories

| Category | Code | Examples |
|----------|------|----------|
| Requirements | REQ | "Should the API support batch operations?" |
| Priority | PRIO | "Is latency more important than throughput here?" |
| Constraints | CONST | "Must this work with Node 16?" |
| Data | DATA | "What's the expected size of input arrays?" |
| Security | SEC | "Who can access this endpoint?" |
| Operations | OPS | "How do we deploy this? Feature flags?" |
| UX/API | UX | "Should errors be verbose or minimal?" |
| Scope | SCOPE | "Is offline support in or out?" |

---

## 3.3 Answer Types

When answering questions, use these types:

| Type | Meaning | How Models Should Interpret |
|------|---------|----------------------------|
| **Direct** | "Use PostgreSQL" | Hard constraint. Follow exactly. |
| **Preference** | "I prefer A, but if you find strong reasons for B, decide and document" | Soft constraint. Can override with justification in ADR. |
| **Delegation** | "I'm not sure, do what you think is best" | Full autonomy. Decide and document reasoning. |
| **Priority Signal** | "Security > speed, but I don't know what that implies here" | Models figure out implications, document tradeoff. |
| **Defer** | "I need to think / ask someone / not sure yet" | Blocker. Cannot proceed until resolved. |
| **Conditional** | "If X then A, if Y then B" | Branch logic. Handle both or ask which applies. |
| **Configurable** | "Make it configurable, default to X" | Implement flexibility, document default choice. |

---

## 3.4 Question Funnel Process

```
Step 1: Each model produces questions independently
        (during Orientation, before design)
        
Step 2: One model consolidates
        - Deduplicate
        - Categorize
        - Note which model(s) raised each
        - Note if models had different preliminary answers
        
Step 3: Human answers in one batch
        - Use answer types above
        - Can add notes/context
        
Step 4: Answers become constraints
        - Incorporated into locked Task Brief
        - Referenced in design phase
        - Violations require re-asking
```

---

## 3.5 Consolidated Questions Template

```markdown
# Consolidated Questions for [TASK_NAME]

## Instructions for Human
Answer each question below. Use these answer types:
- **Direct**: Hard constraint, follow exactly
- **Preference**: Soft constraint, can override with justification
- **Delegation**: Your call, document reasoning
- **Priority**: I care about X > Y, you figure out implications
- **Defer**: Blocker, need more time/info
- **Conditional**: If X then A, if Y then B
- **Configurable**: Make it flexible, default to X

---

## REQ: Requirements

### Q1: [Question text]
**Raised by**: Claude, GPT
**Context**: [Why this matters]
**Models' preliminary takes**: Claude thinks A, GPT thinks B
**Your Answer**: 
**Answer Type**: 

### Q2: ...

---

## PRIO: Priority

### Q3: [Question text]
...

---

## CONST: Constraints

### Q4: [Question text]
...

---

## SEC: Security

### Q5: [Question text]
...

---

## OPS: Operations

### Q6: [Question text]
...

---

## SCOPE: Scope

### Q7: [Question text]
...

```

---

## 3.6 Answers File Template

After answering, the file looks like:

```markdown
# Answers for [TASK_NAME]

## Q1: Should the API support batch operations?
**Answer**: Yes, but limit to 100 items per batch
**Type**: Direct
**Notes**: This matches our existing patterns in the users API

## Q2: Is latency more important than throughput?
**Answer**: Latency for reads, throughput for writes
**Type**: Conditional
**Notes**: Reads are user-facing, writes are background jobs

## Q3: Database choice?
**Answer**: I prefer PostgreSQL but open to arguments for DynamoDB
**Type**: Preference
**Notes**: We have more Postgres expertise on the team

## Q4: Error message verbosity?
**Answer**: Do what you think is best
**Type**: Delegation
**Notes**: --

## Q5: Offline support?
**Answer**: Out of scope for now
**Type**: Direct (Scope)
**Notes**: Will revisit in Phase 3

## Q6: Node version constraint?
**Answer**: Need to check with DevOps, marking as blocker
**Type**: Defer
**Notes**: Will update by EOD tomorrow
```

---

# Part IV: Session Types

## 4.1 Overview

| Session Type | Level | Frequency | Deliverable | Duration |
|--------------|-------|-----------|-------------|----------|
| **Phase Planning** | L3 | Rare | phases.md | 2-4 hours |
| **Task Planning** | L2 | Periodic | task_list.md | 1-2 hours |
| **Task Execution** | L1 | Frequent | Code | Variable |
| **Review** | Meta | Periodic | Updated plans | 30-60 min |

---

## 4.2 Session Type: Phase Planning (L3)

**When to run:**
- Project kickoff
- Major pivot or scope change
- Quarterly review
- When current phases feel wrong

**Inputs:**
- Project goals
- High-level requirements
- Constraints (timeline, resources)
- Learnings from past phases (if any)

**Process:**
1. Orient all models to project context
2. Question Funnel (strategic questions)
3. Each model proposes phase structure independently
4. Cross-critique + Murder Board
5. Synthesis into single phases.md
6. Red Team: "How will this phasing fail?"
7. Human approval
8. Write to `.project/phases.md`

**Outputs:**
- `phases.md` with:
  - Phase names and objectives
  - Success criteria per phase
  - Dependencies between phases
  - Key risks per phase
  - Decision gates between phases

**Governance:**
- Review quarterly or when pain accumulates
- Major changes require mini Phase Planning session
- History kept in `.project/phases_history/`

---

## 4.3 Session Type: Task Planning (L2)

**When to run:**
- Start of a new phase
- When current task list is exhausted
- Weekly/biweekly refresh
- When significant blocker or discovery

**Inputs:**
- Current phase definition (from phases.md)
- What's been completed
- Current codebase state
- New requirements or discoveries
- Blockers

**Process:**
1. Orient all models to phase context
2. Question Funnel (tactical questions)
3. Each model proposes task breakdown independently
4. Cross-critique
5. Synthesis into prioritized task list
6. Human approval
7. Write to `.project/current_phase/task_list.md`

**Outputs:**
- `task_list.md` with:
  - Prioritized tasks
  - Dependencies between tasks
  - Rough effort estimates (T-shirt sizes)
  - Risk flags
  - Which tasks can parallelize

**Governance:**
- Review weekly or at sprint boundaries
- Can add tasks ad-hoc (append to list)
- Major reordering requires mini Task Planning session

---

## 4.4 Session Type: Task Execution (L1)

**When to run:**
- Whenever you're ready to work on a task

**Inputs:**
- Task from task_list.md
- All project context

**Process:**
- The full "Formoso e Seguro" workflow (detailed in Part V)

**Outputs:**
- Code (committed)
- Spec (if non-trivial, in docs/specs/)
- ADRs (if non-obvious decisions)
- Updated task_list.md (mark complete)

---

## 4.5 Session Type: Review (Meta)

**When to run:**
- End of each phase
- Monthly (regardless of phase)
- After significant discovery or incident
- When things feel off

**Inputs:**
- Current phases.md
- Current task_list.md
- Completed work since last review
- Learnings, incidents, discoveries

**Process:**
1. Orient all models to current state
2. Each model independently assesses:
   - Is phases.md still valid?
   - Is task_list.md correctly prioritized?
   - What did we learn?
   - What should change?
3. Question Funnel (if strategic questions emerged)
4. Synthesis into updates
5. Human approval
6. Write updates to `.project/`
7. Brief retrospective on the process itself

**Outputs:**
- Updated phases.md (if needed)
- Updated task_list.md (if needed)
- Learnings appended to `.project/learnings.md`
- Process improvements noted

---

## 4.6 The Human Team Leader Experience

Your typical week:

```
Monday morning:
  1. Open .project/current_phase/task_list.md
  2. Pick next task
  3. Run: task-init "implement-caching"
  4. Run Phase 1 (Orientation + Questions)
  5. Answer consolidated questions (10-15 min)
  6. Let models run Phase 2-3
  7. Review final output
  8. Approve or request fixes
  9. Mark task complete
  10. Pick next task or stop

Friday afternoon (every 1-2 weeks):
  1. Run Review session
  2. Answer strategic questions (10 min)
  3. Approve updated plans
  4. Note process improvements

At phase boundary:
  1. Run Task Planning session
  2. Answer tactical questions
  3. Approve new task list

Rarely (quarterly or at major milestones):
  1. Run Phase Planning session
  2. Answer strategic questions
  3. Approve phase structure
```

**Key point**: You are never interrupted mid-flow. Questions come in batches. Approvals come at gates. Your cognitive load is minimized.

---

# Part V: Task Execution Workflow (L1)

This is the core workflow. The most detailed section.

---

## 5.1 Complexity Gate

Before starting, choose a track:

| Task Complexity | Track | What's Included |
|-----------------|-------|-----------------|
| **Trivial** | Express | Single model, one-paragraph rationale, implement, test |
| **Standard** | Full (2 models) | All phases, default rigor |
| **High-Risk** | Full+ (3 models) | All phases, extra divergence and red-teaming |

**Decision criteria:**

Use **Express** if ALL of:
- < 50 lines changed
- No architectural impact
- Obvious fix (typo, config, simple bug)
- Low risk if wrong

Use **Full+ (3 models)** if ANY of:
- Touches core logic / state machine
- Security-sensitive
- Breaking changes possible
- Concurrency / distributed systems
- You're uncertain about the right approach
- 2 models keep agreeing suspiciously

Otherwise use **Full (2 models)**.

---

## 5.2 Phase Overview

```
PHASE 1: UNDERSTAND
  1a. Orientation (all models)
  1b. Questions (all models, independent)
  1c. Questions consolidated (one model)
  1d. Human answers
  1e. Task Brief locked

PHASE 2: DESIGN (Double-Blind → Converge)
  2a. Idea Harvest (all models, uncontaminated)
  2b. RFC Proposal (all models, uncontaminated)
  2c. Self-Critique (all models, still uncontaminated)
  ══════════════ CONTAMINATION BARRIER ══════════════
  2d. Cross-Critique / Murder Board (all models)
  2e. Red Team / Pre-mortem (one model, preferably fresh)
  2f. Synthesis → Frozen Spec + ADRs (Claude)
  2g. Spec Approval gate (GPT or Gemini)

PHASE 3: IMPLEMENT (Double-Blind → Converge → Harden)
  3a. Implementation in-memory (Claude + GPT, uncontaminated)
  ══════════════ CONTAMINATION BARRIER ══════════════
  3b. Cross-Review implementations (both)
  3c. Synthesis decision (human + Claude)
  3d. Write to disk (Claude Code)
  3e. Final Review of actual code (GPT)
  3f. Hardening patchset (Claude, if issues)
  3g. Confirm (GPT)

PHASE 4: CLOSE-OUT
  4a. Final audit checklist
  4b. ADR completeness
  4c. Documentation updates
  4d. Footguns section
  4e. Archive working memory

PHASE 5: RETROSPECTIVE (optional but recommended)
  5a. What did we learn?
```

---

## 5.3 The Prompt Header Template

Every prompt uses this header for contamination control:

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: [GENERATE | REVIEW]
PHASE: [1a | 2b | 3d | etc.]
MODEL: [claude | gpt | gemini]
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/01_brief.md
  - .task/[task]/02_questions/answers.md
  - [other allowed files]
FORBIDDEN:
  - .task/[task]/03_design/gpt/*  (example: other model's work)
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/claude/rfc.md
═══════════════════════════════════════════════════════════════════
```

This makes explicit:
- What the model should read
- What the model must NOT read
- Where to write output

---

## 5.4 Phase 1: Understand

### 1a — ORIENTATION (all models, parallel)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 1a
MODEL: [claude | gpt | gemini]
─────────────────────────────────────────────────────────────────
READ:
  - README.md
  - docs/builder_implementation.md (or equivalent)
  - [request file tree]
FORBIDDEN:
  - Nothing yet
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/[model]/orientation.md
═══════════════════════════════════════════════════════════════════

You are a Senior Principal Engineer about to work on this codebase.

1. Read the README and architecture docs.
2. Request the file tree (I'll paste `git ls-files` or `tree -L 4`).
3. After reviewing, output:

## Repo Map
[1 paragraph: key components, entry points, data flow, how the core system works]

## Files to Read Next
[8-12 file paths with one-line reasons]

## Assumptions
[Tag each as [ASSUMPTION]]

## Questions
[Tag each as [QUESTION] — these will feed into the Question Funnel]

Do NOT propose solutions yet.
Cite file paths and line numbers when referencing code.
```

### 1b — QUESTIONS (all models, parallel, independent)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 1b
MODEL: [claude | gpt | gemini]
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/01_brief.md
  - .task/[task]/03_design/[model]/orientation.md
  - Relevant repo files
FORBIDDEN:
  - Other models' orientation or questions
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/02_questions/[model]_questions.md
═══════════════════════════════════════════════════════════════════

Task: [PASTE TASK DESCRIPTION]

Before designing anything, identify what you need to know.

Produce questions organized by category:

## REQ: Requirements
[Questions about what we're building]

## PRIO: Priority
[Questions about tradeoffs]

## CONST: Constraints
[Questions about technical/environmental constraints]

## DATA: Data
[Questions about inputs, outputs, data shapes]

## SEC: Security
[Questions about security, auth, abuse]

## OPS: Operations
[Questions about deployment, monitoring, rollout]

## SCOPE: Scope
[Questions about what's in/out of scope]

For each question:
- State the question clearly
- Explain why it matters
- Note your preliminary thinking (if any)

Aim for 10-20 questions total. Don't self-censor.
Do NOT design the solution yet.
```

### 1c — QUESTIONS CONSOLIDATION (one model, typically Claude)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 1c
MODEL: claude
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/02_questions/claude_questions.md
  - .task/[task]/02_questions/gpt_questions.md
  - .task/[task]/02_questions/gemini_questions.md (if applicable)
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/02_questions/consolidated.md
═══════════════════════════════════════════════════════════════════

Consolidate all questions into a single list for the human to answer.

For each question:
1. Deduplicate (merge similar questions)
2. Clarify (make unambiguous)
3. Categorize (REQ, PRIO, CONST, DATA, SEC, OPS, SCOPE)
4. Note which model(s) raised it
5. Note if models disagreed on preliminary answer
6. Add context about why it matters

Format:

---
## [CATEGORY]: [Category Name]

### Q[N]: [Question]
**Raised by**: [Claude, GPT, Gemini]
**Context**: [Why this matters for the design]
**Models' takes**: [Any preliminary opinions, disagreements]
**Human Answer**: _______________
**Answer Type**: [Direct | Preference | Delegation | Priority | Defer | Conditional | Configurable]
**Notes**: _______________

---

Number questions sequentially (Q1, Q2, ...).
Aim for a clean, answerable list. Remove redundancy.
```

### 1d — HUMAN ANSWERS

The human fills in `consolidated.md` or creates `answers.md` with responses.

### 1e — TASK BRIEF LOCK (one model, typically Claude)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 1e
MODEL: claude
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/01_brief.md
  - .task/[task]/02_questions/consolidated.md (with human answers)
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/01_brief_locked.md
  - (optionally) docs/briefs/[task_name].md (if lasting value)
═══════════════════════════════════════════════════════════════════

Compile the locked Task Brief incorporating human answers.

## Task Brief: [TASK_NAME]

### Status: LOCKED

### Goal
[One paragraph: what success looks like]

### Success Criteria
[Observable outcomes, specific and testable]

### Non-Goals
[Explicit scope boundaries]

### Constraints
[From human answers — hard constraints]

### Preferences
[From human answers — soft constraints, can override with justification]

### Delegated Decisions
[From human answers — model has autonomy, must document reasoning]

### Deferred Items
[From human answers — blockers or future work]

### Key Context
[Important background from questions/answers]

---
This brief is now LOCKED. All design work must reference it.
Violating a constraint requires explicit re-approval.
```

---

## 5.5 Phase 2: Design

### 2a — IDEA HARVEST (all models, parallel, uncontaminated)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 2a
MODEL: [claude | gpt | gemini]
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/01_brief_locked.md
  - Relevant repo files
FORBIDDEN:
  - Other models' outputs
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/[model]/idea_harvest.md
═══════════════════════════════════════════════════════════════════

Before committing to a design, brainstorm freely.

## Possible Approaches
[3-5 distinct approaches, including unconventional ones]

For each approach:
- **Name**: [short label]
- **Core Idea**: [1-2 sentences]
- **Pros**: [why it might be best]
- **Cons**: [why it might fail]
- **Risk Level**: [Low / Medium / High]

## Gut Instinct
[Which approach feels right? Why?]

## Red Flags
[What could go wrong regardless of approach?]

## Edge Cases (Initial)
[Start listing edge cases — will expand in RFC]

## Wild Card
[One approach that seems too crazy but might actually work]

Don't self-censor. Capture breadth.
```

### 2b — RFC PROPOSAL (all models, parallel, uncontaminated)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 2b
MODEL: [claude | gpt | gemini]
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/01_brief_locked.md
  - .task/[task]/03_design/[model]/idea_harvest.md
  - Relevant repo files
FORBIDDEN:
  - Other models' idea harvests or RFCs
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/[model]/rfc.md
═══════════════════════════════════════════════════════════════════

Write a detailed RFC-style proposal. Use these exact sections:

# RFC: [TASK_NAME]

## 1. Summary
[2-3 sentences: the approach and why]

## 2. Current Behavior
[What happens today. File paths and line numbers.]

## 3. Requirements
[From Task Brief. Functional and non-functional.]

## 4. Proposed Design

### 4.1 Architecture Overview
[High-level structure]

### 4.2 Data Flow
[How data moves through the system]

### 4.3 Key Interfaces
[APIs, function signatures, data structures]

### 4.4 Component Interactions
[How pieces talk to each other]

## 5. Detailed Changes
[For each file to create/modify:]
- **File**: [path]
- **Change**: [what changes]
- **Reason**: [why]

## 6. Edge-Case Register
[Numbered list. For each:]
| # | Edge Case | How Handled | Test Scenario |
|---|-----------|-------------|---------------|
| E1 | [description] | [handling] | [test] |
| E2 | ... | ... | ... |

## 7. Backward Compatibility & Migration
[What breaks? Migration path? Rollback plan?]

## 8. Observability
[Logs, metrics, alerts, tracing]

## 9. Security & Abuse Cases
[Attack vectors, input validation, auth]

## 10. Test Plan
[Unit, integration, e2e. Map to edge-case register.]

## 11. Rollout Plan
[Feature flags, phased deploy, fallback]

## 12. Alternatives Considered
[Other approaches from Idea Harvest and why rejected]

## 13. Open Questions
[Unresolved issues]

## 14. ADR Candidates
[Decisions that should be recorded]
| Decision | Rationale | Alternatives Rejected |
|----------|-----------|----------------------|

---
Be opinionated. Make tradeoffs explicit. Don't hedge.
```

### 2c — SELF-CRITIQUE (all models, parallel, still uncontaminated)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE (introspective)
PHASE: 2c
MODEL: [claude | gpt | gemini]
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/03_design/[model]/rfc.md
  - .task/[task]/01_brief_locked.md
FORBIDDEN:
  - Other models' RFCs or self-critiques
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/[model]/self_critique.md
═══════════════════════════════════════════════════════════════════

Before seeing alternatives, capture your honest self-assessment.

## Weaknesses I'm Aware Of
[Where am I not fully confident?]

## Risky Edge Cases
[Which feel underspecified or scary?]

## Alternatives I Almost Chose
[What did I reject? Am I sure?]

## Where I'd Want Pushback
[What questions should a reviewer ask?]

## Confidence Ranking
[Rank sections of your RFC from most to least confident]

1. [Most confident]: ...
2. ...
3. [Least confident]: ...

## If I Had More Time
[What would I investigate further?]

---
Be honest. This captures genuine doubts before they get overwritten.
```

---

**══════════════════ CONTAMINATION BARRIER ══════════════════**

*Now share: each model sees all RFCs and self-critiques.*

---

### 2d — CROSS-CRITIQUE / MURDER BOARD (all models)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW
PHASE: 2d
MODEL: [claude | gpt | gemini]
─────────────────────────────────────────────────────────────────
READ:
  - All RFCs: .task/[task]/03_design/*/rfc.md
  - All self-critiques: .task/[task]/03_design/*/self_critique.md
  - .task/[task]/01_brief_locked.md
FORBIDDEN:
  - Nothing now
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/04_reviews/[model]_reviews_[other_model].md
═══════════════════════════════════════════════════════════════════

You have produced your own RFC. Now review [OTHER_MODEL]'s proposal.

Use this rubric:

## Rubric Assessment

| Category | Rating | Issues |
|----------|--------|--------|
| Correctness | [1-5] | [issues] |
| Completeness | [1-5] | [issues] |
| Maintainability | [1-5] | [issues] |
| Compatibility | [1-5] | [issues] |
| Security | [1-5] | [issues] |
| Performance | [1-5] | [issues] |
| Observability | [1-5] | [issues] |
| Testability | [1-5] | [issues] |

## Detailed Findings

For each issue:
- **Severity**: [BLOCKER | MAJOR | MINOR | NIT]
- **Location**: [Which RFC section]
- **Issue**: [Description]
- **Proposed Fix**: [Concrete suggestion]

## Comparative Analysis

### Where theirs is better than mine:
[Be honest]

### Where mine is better:
[Be specific]

### Key Disagreements:
[Fundamental differences in approach]

## Self-Critique Validation

### Their doubts that I share:
[From their self-critique]

### Their doubts I think are unfounded:
[And why]

### Concerns they missed:
[Things I worried about that they didn't]

## Missing Edge Cases
[From my register that aren't in theirs]

## Critique Checklist
□ Preserves existing behavior where required?
□ Any silent breaking changes?
□ Undefined behavior on bad inputs?
□ Partial failure handling?
□ Concurrency hazards?
□ Dependency assumptions?
□ Rollback safe?
□ Observability sufficient?
□ Tests cover failure modes?
□ ADRs for controversial choices?
```

**Murder Board Synthesis** (one model consolidates):

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 2d-synthesis
MODEL: gemini (or claude)
─────────────────────────────────────────────────────────────────
READ:
  - All cross-reviews: .task/[task]/04_reviews/*.md
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/04_reviews/murder_board.md
═══════════════════════════════════════════════════════════════════

Synthesize all cross-reviews into a Murder Board summary.

## Consensus Issues
[Issues all reviewers agreed on]

## Contested Points
[Issues where reviewers disagreed — note positions]

## BLOCKERs (must address)
| Issue | Raised By | Proposed Fix |
|-------|-----------|--------------|

## MAJORs (should address)
| Issue | Raised By | Proposed Fix |
|-------|-----------|--------------|

## Which RFC Should Be Base?
[Recommendation with reasoning]

## Critical Deltas to Cherry-Pick
[From other RFCs]

## Unresolved Questions
[Need human input or more investigation]
```

### 2e — RED TEAM / PRE-MORTEM (one model, preferably fresh)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW (adversarial)
PHASE: 2e
MODEL: gemini (ideally) or gpt
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/04_reviews/murder_board.md
  - All RFCs (or just the recommended base)
  - .task/[task]/01_brief_locked.md
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/synthesis/red_team.md
═══════════════════════════════════════════════════════════════════

Pre-mortem: It's 6 months from now. This feature caused a production incident. What went wrong?

## Failure Scenarios
[List 10+ realistic ways this could fail]

For each:
| # | Scenario | Likelihood | Impact | Current Mitigation | Gap? |
|---|----------|------------|--------|-------------------|------|
| F1 | [description] | [H/M/L] | [H/M/L] | [how design handles] | [yes/no] |

## Adversarial Inputs
[What would a malicious user do?]

## Dependency Failures
[What if DB/API/library fails, slows, or changes?]

## Concurrency & Timing
[Race conditions, retries, parallel requests]

## Partial Failures
[Step 2 of 5 fails. State consistent? Recovery?]

## Scaling Cliffs
[What happens at 10x, 100x load?]

## Unknown Unknowns
[What aren't we thinking about?]
[How would we detect problems early?]

## Recommendations
[Specific changes to address top risks]

---
Be pessimistic. Assume Murphy's Law.
```

### 2f — SYNTHESIS (Claude)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 2f
MODEL: claude
─────────────────────────────────────────────────────────────────
READ:
  - All RFCs
  - All self-critiques
  - .task/[task]/04_reviews/murder_board.md
  - .task/[task]/03_design/synthesis/red_team.md
  - .task/[task]/01_brief_locked.md
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/synthesis/spec_v1.md
  - .task/[task]/03_design/synthesis/adr_drafts.md
═══════════════════════════════════════════════════════════════════

Synthesize everything into the Frozen Spec.

You have:
- All RFCs + self-critiques
- Murder Board (consolidated critique)
- Red Team findings

## Synthesis Decisions

### Adopted from [model] RFC:
[List with reasoning]

### Rejected from critiques:
[List with reasoning]

### BLOCKER resolutions:
[How each BLOCKER is addressed]

### Risk mitigations:
[How top Red Team risks are addressed]

---

# Frozen Spec: [TASK_NAME]

[Same 14 sections as RFC, but now authoritative and complete]

Include additional sections:

## Implementation Checklist
[Ordered steps]
- [ ] Step 1: ... [STOP-AND-VERIFY]
- [ ] Step 2: ...
- [ ] Step 3: ... [STOP-AND-VERIFY]
- [ ] Step 4: ...

## Test Matrix
| Edge Case | Test Type | Test Scenario | Priority |
|-----------|-----------|---------------|----------|
| E1 | Unit | [description] | P0 |

## Decision Record
| Decision | Chosen | Rejected | Reason |
|----------|--------|----------|--------|

---

# ADR Drafts

## ADR-001: [Decision Title]
[Use ADR template]

## ADR-002: [Decision Title]
[Use ADR template]
```

### 2g — SPEC APPROVAL (GPT or Gemini)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW
PHASE: 2g
MODEL: gpt (or gemini)
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/03_design/synthesis/spec_v1.md
  - .task/[task]/03_design/synthesis/adr_drafts.md
  - .task/[task]/04_reviews/murder_board.md
  - .task/[task]/03_design/synthesis/red_team.md
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/03_design/synthesis/approval.md
═══════════════════════════════════════════════════════════════════

Final gate before implementation.

## BLOCKER Resolution Check
| BLOCKER | Status | How Resolved |
|---------|--------|--------------|
| [from murder board] | [✓/✗] | [how] |

## Red Team Risk Check
| Top Risk | Status | How Mitigated |
|----------|--------|---------------|
| [from red team] | [✓/✗] | [how] |

## Internal Consistency Check
[Any contradictions within the spec?]

## Lost Edge Cases
[Anything from earlier phases that got dropped?]

## Task Brief Compliance
[Does spec satisfy all locked constraints?]

## Implementation Checklist Review
[Is it complete and correctly ordered?]

## Verdict

**[ ] APPROVED** — Ready for implementation

**[ ] NEEDS REVISION** — Issues below must be addressed:

| Issue | Severity | Required Fix |
|-------|----------|--------------|

---

If APPROVED, proceed to Phase 3.
If NEEDS REVISION, return to 2f with issues.
```

---

## 5.6 Phase 3: Implement

### 3a — IMPLEMENT IN-MEMORY (Claude + GPT, parallel, uncontaminated)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 3a
MODEL: [claude | gpt]
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/03_design/synthesis/spec_final.md
  - .task/[task]/03_design/synthesis/adr_drafts.md
  - Relevant repo files
FORBIDDEN:
  - Other model's implementation
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/05_implementation/[model]/code.md
  - .task/[task]/05_implementation/[model]/difficulties.md
═══════════════════════════════════════════════════════════════════

Implement the Frozen Spec.

Generate complete code in your response. Do NOT write to disk yet.

## Implementation

### File: [path]
```[language]
[complete file content]
```

### File: [path]
```[language]
[complete file content]
```

[Continue for all files]

---

## Tests

### File: [test path]
```[language]
[complete test content]
```

---

## Difficulties & Discoveries

### Harder Than Expected
[What surprised you?]

### Edge Cases Discovered
[New ones not in spec]

### Judgment Calls
[Decisions you made not covered by spec]

### Confidence Ranking
[Parts you're most/least confident about]

### Spec Delta Needed?
[If YES, describe what needs to change and why]
[If NO, say "Spec is accurate as written"]

### Codebase Surprises
[Anything unexpected in existing code?]
```

---

**══════════════════ CONTAMINATION BARRIER ══════════════════**

*Now share: each model sees the other's implementation.*

---

### 3b — CROSS-REVIEW IMPLEMENTATIONS (both)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW
PHASE: 3b
MODEL: [claude | gpt]
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/05_implementation/[own]/code.md
  - .task/[task]/05_implementation/[own]/difficulties.md
  - .task/[task]/05_implementation/[other]/code.md
  - .task/[task]/05_implementation/[other]/difficulties.md
  - .task/[task]/03_design/synthesis/spec_final.md
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/04_reviews/[model]_reviews_[other]_impl.md
═══════════════════════════════════════════════════════════════════

You implemented this yourself. Now review the alternative.

## Approach Differences
[How do implementations differ?]

### Where theirs is better:
[Be honest and specific]

### Where mine is better:
[Be specific]

## Shared Difficulties
[Did they handle hard parts differently? Better?]

## Edge Cases
[Cases they caught that I missed?]
[Cases I caught that they missed?]

## Spec Compliance
[Any deviations from spec?]
[Any Spec Deltas they identified?]

## Code Quality Assessment

| Aspect | Their Rating | Issue/Praise |
|--------|--------------|--------------|
| Correctness | [1-5] | |
| Readability | [1-5] | |
| Error Handling | [1-5] | |
| Test Coverage | [1-5] | |
| Spec Compliance | [1-5] | |

## Issues Found

| Severity | File:Line | Issue | Suggested Fix |
|----------|-----------|-------|---------------|
| BLOCKER | | | |
| MAJOR | | | |
| MINOR | | | |
| NIT | | | |

## The Stuff That Gets Missed Checklist
□ Error handling complete?
□ Resources cleaned up?
□ Boundary conditions handled?
□ Null/undefined handled?
□ Concurrency safe (if applicable)?
□ Rollback safe?
□ Tests cover failure modes?
```

### 3c — SYNTHESIS DECISION (human + Claude)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE
PHASE: 3c
MODEL: claude
─────────────────────────────────────────────────────────────────
READ:
  - Both implementations
  - Both difficulties
  - Both cross-reviews
  - Spec
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/05_implementation/synthesis_decision.md
  - .task/[task]/05_implementation/spec_amendments.md (if needed)
═══════════════════════════════════════════════════════════════════

Decide on the final implementation.

## Base Implementation
[Which one? Why?]

## Cherry-Picks from Other
[Specific elements to incorporate]

## Spec Amendments Needed?
[If either implementation revealed spec errors]

**If YES:**
- What changed
- Why it had to change
- Impact on design
- New tests required

**Request approval before proceeding.**

## Final Implementation Plan
[What will be written to disk]

## ADR Updates
[New decisions or warnings from implementation]
```

**If Spec amendments needed, quick approval round:**

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW (quick)
PHASE: 3c-approval
MODEL: gpt
─────────────────────────────────────────────────────────────────

Review the proposed Spec amendment:

[Amendment details]

Is this valid? Does it introduce new risks?

[ ] APPROVED
[ ] CONCERNS: [list]
```

### 3d — WRITE TO DISK (Claude Code)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE (execute)
PHASE: 3d
MODEL: claude (Claude Code)
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/05_implementation/synthesis_decision.md
  - .task/[task]/03_design/synthesis/spec_final.md (or amended)
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - Actual source files in repo
  - Test files
═══════════════════════════════════════════════════════════════════

Write the final implementation to disk.

## Rules

1. **Follow the synthesis decision exactly.**
2. **Follow the Implementation Checklist.** Pause at STOP-AND-VERIFY points.
3. **Stop-the-Line Rule**: If you discover the Spec is wrong:
   - STOP
   - Describe the issue
   - Propose amendment
   - Wait for approval
   - Do NOT silently deviate
4. **Run tests after each significant change.** Report results.
5. **Run lint/typecheck.** Report results.

## Output

After writing to disk, report:

### Files Changed
[List with brief description of changes]

### Test Results
```
[paste test output]
```

### Lint/Typecheck Results
```
[paste output]
```

### Deviations from Plan
[Should be none. If any, explain why.]

### New Discoveries
[Anything found during actual implementation]
```

### 3e — FINAL REVIEW OF ACTUAL CODE (GPT)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW (final)
PHASE: 3e
MODEL: gpt
─────────────────────────────────────────────────────────────────
READ:
  - Actual diffs / changed files
  - Test results
  - Lint/typecheck results
  - .task/[task]/03_design/synthesis/spec_final.md
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/06_final/final_review.md
═══════════════════════════════════════════════════════════════════

The implementation is on disk. This is the FINAL review.

Review the ACTUAL code, not the theoretical version.

## Spec Compliance
[Does the real code match the spec?]

## Test Verification
[Do tests pass? Do they cover edge cases?]

## Integration Check
[Does it integrate cleanly with existing code?]

## Code Quality

| Aspect | Rating | Notes |
|--------|--------|-------|
| Correctness | [1-5] | |
| Readability | [1-5] | |
| Error Handling | [1-5] | |
| Resource Cleanup | [1-5] | |
| Naming | [1-5] | |
| Comments | [1-5] | |

## Issues Found

| Severity | File:Line | Issue | Required Fix |
|----------|-----------|-------|--------------|
| BLOCKER | | | |
| MAJOR | | | |
| MINOR | | | |
| NIT | | | |

## The Stuff That Always Gets Missed

□ Error handling complete?
□ Resources cleaned up?
□ Boundary conditions?
□ Null/undefined?
□ Concurrency safe?
□ Rollback safe?
□ Logs adequate for debugging?
□ Metrics in place?
□ Feature flags configured?

## Verdict

**[ ] APPROVED** — Ready for close-out

**[ ] NEEDS HARDENING** — Fix issues below first

---

This is the last code gate. What did we miss?
```

### 3f — HARDENING PATCHSET (Claude, if needed)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE (fix)
PHASE: 3f
MODEL: claude
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/06_final/final_review.md
  - Current source files
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - Fixed source files
  - .task/[task]/06_final/hardening_notes.md
═══════════════════════════════════════════════════════════════════

Final review found issues. Address them.

For each issue:
1. **Fix it**, OR
2. **Explain why it's not actually an issue**, OR
3. **Document as accepted risk in ADR**

## Fixes Applied

| Issue | Resolution | File:Line |
|-------|------------|-----------|
| [from review] | [fixed/explained/accepted] | |

## Test Results (after fixes)
```
[output]
```

## ADR Updates
[New warnings or accepted risks]

## Remaining Items
[Anything deferred to future work]
```

### 3g — CONFIRM (GPT, quick pass)

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW (quick)
PHASE: 3g
MODEL: gpt
─────────────────────────────────────────────────────────────────
READ:
  - .task/[task]/06_final/hardening_notes.md
  - Diffs from fixes
  - Test results
FORBIDDEN:
  - Nothing
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/06_final/confirmation.md
═══════════════════════════════════════════════════════════════════

Quick confirmation after hardening.

## BLOCKER Resolution
[All resolved?]

## MAJOR Resolution  
[All resolved or documented as accepted risk?]

## Tests
[Passing?]

## Verdict

**[ ] APPROVED** — Proceed to close-out

**[ ] STILL HAS ISSUES** — [list]
```

---

## 5.7 Phase 4: Close-Out

### 4a — FINAL AUDIT

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: REVIEW
PHASE: 4a
MODEL: claude (or gpt)
─────────────────────────────────────────────────────────────────

## "Nothing Bites Later" Checklist

□ No silent breaking changes
□ Backward compatibility maintained
□ Rollback verified safe (or documented as risky)
□ No hardcoded secrets or environment-specific config
□ No unresolved TODO/FIXME (or tracked as issues)
□ No flaky tests introduced
□ Dependencies pinned appropriately
□ Security review complete (if applicable)
□ Feature flags configured (if applicable)
□ Monitoring/alerts in place (if applicable)
□ Documentation updated

## Items for Follow-Up
[Future work, tech debt, improvements — create issues]
```

### 4b — ADR FINALIZATION

```
Review ADR drafts. For each:
- Is it still accurate after implementation?
- Does it capture implementation learnings?
- Are warnings/footguns complete?

Finalize and write to docs/adr/
```

### 4c — DOCUMENTATION

```
Update as needed:
- README (if public interface changed)
- API docs (if applicable)
- Inline comments (verify adequate)
```

### 4d — FOOTGUNS

```
═══════════════════════════════════════════════════════════════════

Create or update docs/footguns/[task_name].md:

## Footguns for [TASK_NAME]

### Things to be careful about:
[Non-obvious gotchas]

### Assumptions that might break:
[Under what conditions]

### Edge cases handled non-obviously:
[Where to look, why it's that way]

### "If you're changing X, remember to also change Y":
[Coupling that's not obvious from code]
```

### 4e — ARCHIVE WORKING MEMORY

```bash
# Option 1: Delete (if confident)
rm -rf .task/[task_name]

# Option 2: Archive (if want to keep for reference)
mv .task/[task_name] .task/_archive/[task_name]_$(date +%Y%m%d)

# Option 3: Keep temporarily, delete on next task-init
```

Update `.project/current_phase/task_list.md` — mark task complete.

---

## 5.8 Phase 5: Retrospective

```
═══════════════════════════════════════════════════════════════════
TASK: [task_name]
MODE: GENERATE (reflective)
PHASE: 5a
MODEL: claude (or any)
─────────────────────────────────────────────────────────────────
WRITE:
  - .task/[task]/06_final/retro.md (before archive)
  - Append to .project/learnings.md
═══════════════════════════════════════════════════════════════════

Brief retrospective on this task.

## What Went Well
[Smooth parts of the process]

## What Was Painful
[Friction, wasted time]

## What We Learned
[About the codebase, the domain, the tools]

## Spec Accuracy
[How much did the Frozen Spec need amendment?]
[What caused the inaccuracy?]

## Prediction Calibration
[Which self-critiques were validated?]
[Which were paranoid?]
[What did we miss that got caught later?]

## Process Improvements
[What would we do differently next time?]
```

---

# Part VI: Templates

## 6.1 ADR Template

```markdown
# ADR-[NUMBER]: [Title]

## Status
[Proposed | Accepted | Deprecated | Superseded by ADR-XXX]

## Date
[YYYY-MM-DD]

## Context
[What situation motivated this decision?]
[What problem are we solving?]

## Decision
[What are we doing?]
[Be specific and concrete.]

## Alternatives Considered

### Alternative A: [Name]
[Description]
**Rejected because**: [reason]

### Alternative B: [Name]
[Description]
**Rejected because**: [reason]

## Consequences

### Positive
- [benefit 1]
- [benefit 2]

### Negative
- [cost/tradeoff 1]
- [cost/tradeoff 2]

## Warnings / Footguns
[What could go wrong?]
[What should future developers watch out for?]
[Non-obvious gotchas]

## Conditions to Revisit
[When should we reconsider this decision?]
[What signals would indicate it's wrong?]

## Related
- [Link to related ADRs]
- [Link to related specs]
- [Link to relevant code]
```

---

## 6.2 Task Brief Template

```markdown
# Task Brief: [TASK_NAME]

## Status: [DRAFT | LOCKED]

## Goal
[One paragraph: what does success look like?]

## Success Criteria
- [ ] [Observable outcome 1]
- [ ] [Observable outcome 2]
- [ ] [Observable outcome 3]

## Non-Goals
- [Explicit scope boundary 1]
- [Explicit scope boundary 2]

## Constraints (Hard)
[Must follow exactly]
- [Constraint 1]
- [Constraint 2]

## Preferences (Soft)
[Can override with justification]
- [Preference 1]
- [Preference 2]

## Delegated Decisions
[Model has autonomy, must document reasoning]
- [Decision area 1]
- [Decision area 2]

## Deferred Items
[Out of scope for now, revisit later]
- [Item 1]
- [Item 2]

## Key Context
[Important background]

## Open Questions
[Unresolved — mark as blockers if critical]
```

---

## 6.3 Frozen Spec Template

```markdown
# Frozen Spec: [TASK_NAME]

## Status: [DRAFT | APPROVED | AMENDED]

## 1. Summary
[2-3 sentences]

## 2. Current Behavior
[File paths, line numbers]

## 3. Requirements
[From Task Brief]

## 4. Design
### 4.1 Architecture
### 4.2 Data Flow
### 4.3 Interfaces
### 4.4 Interactions

## 5. Changes
| File | Change | Reason |
|------|--------|--------|

## 6. Edge Cases
| # | Case | Handling | Test |
|---|------|----------|------|

## 7. Compatibility & Migration

## 8. Observability

## 9. Security

## 10. Tests
| Edge Case | Type | Scenario | Priority |
|-----------|------|----------|----------|

## 11. Rollout

## 12. Alternatives Rejected
| Alternative | Why Rejected |
|-------------|--------------|

## 13. Open Questions

## 14. Implementation Checklist
- [ ] Step 1 [STOP-AND-VERIFY]
- [ ] Step 2
- [ ] Step 3 [STOP-AND-VERIFY]
- [ ] Step 4

## 15. Decision Record
| Decision | Chosen | Rejected | Reason |
|----------|--------|----------|--------|

## Amendments
[If spec was updated during implementation]
| Date | Change | Reason | Approved By |
|------|--------|--------|-------------|
```

---

## 6.4 Critique Checklist

Paste into every REVIEW prompt:

```markdown
## Standard Critique Checklist

□ Preserves existing behavior where required?
□ Any silent breaking changes?
□ Undefined behavior on bad inputs?
□ Partial failure handling? Retries? Timeouts?
□ Concurrency hazards?
□ Dependency/version assumptions?
□ Rollback safe?
□ Logs/metrics sufficient for diagnosis?
□ Tests cover failure modes?
□ ADRs written for controversial choices?
□ Error messages helpful for debugging?
□ Resource cleanup (files, connections, memory)?
□ Boundary conditions (empty, max, negative)?
□ Null/undefined handling?
```

---

## 6.5 Status File Template

```markdown
# Task: [TASK_NAME]

## Track
[Express | Full | Full+]

## Current Phase
[Phase X.Y: Name]

## Current Step
[What's happening / what's needed]

## Completed
- [x] 1a: Orientation
- [x] 1b: Questions
- [ ] 1c: Consolidation
- [ ] ...

## Blockers
[None | List blockers]

## Key Decisions Made
[Running list of important decisions]

## Next Action
[Specific next step]

## Notes
[Freeform observations]
```

---

# Part VII: Quick Reference

## 7.1 Session Type Cheat Sheet

| I want to... | Session Type | Command |
|--------------|--------------|---------|
| Define project phases | Phase Planning (L3) | `session-phase-plan` |
| Break phase into tasks | Task Planning (L2) | `session-task-plan` |
| Do a specific task | Task Execution (L1) | `task-init [name]` |
| Check if plans are still valid | Review | `session-review` |

---

## 7.2 Complexity Gate Cheat Sheet

| If task is... | Use Track | Models |
|---------------|-----------|--------|
| Trivial (<50 lines, obvious) | Express | 1 |
| Standard feature/refactor | Full | 2 |
| High-risk / uncertain | Full+ | 3 |

---

## 7.3 Phase Flow Cheat Sheet

```
PHASE 1: UNDERSTAND
  1a Orientation → 1b Questions → 1c Consolidate → 1d Answers → 1e Lock Brief

PHASE 2: DESIGN  
  2a Idea Harvest → 2b RFC → 2c Self-Critique
  ═══ BARRIER ═══
  2d Cross-Critique → 2e Red Team → 2f Synthesis → 2g Approval

PHASE 3: IMPLEMENT
  3a In-Memory Code
  ═══ BARRIER ═══  
  3b Cross-Review → 3c Synthesis → 3d Disk → 3e Final Review → 3f Harden → 3g Confirm

PHASE 4: CLOSE-OUT
  4a Audit → 4b ADRs → 4c Docs → 4d Footguns → 4e Archive

PHASE 5: RETRO
  5a Learnings
```

---

## 7.4 Answer Type Cheat Sheet

| Type | Signal to Model |
|------|-----------------|
| **Direct** | Hard constraint. Follow exactly. |
| **Preference** | Soft. Can override with justification. |
| **Delegation** | Your call. Document reasoning. |
| **Priority** | X > Y. You figure out implications. |
| **Defer** | Blocker. Wait for answer. |
| **Conditional** | If X then A, if Y then B. |
| **Configurable** | Make flexible, default to X. |

---

## 7.5 Model Role Cheat Sheet

| Model | Best At | Use For |
|-------|---------|---------|
| **Claude** | Synthesis, long context, nuance | Architect, Synthesizer, Implementation |
| **GPT** | Finding bugs, security, edge cases | Red Team, Auditor, Final Review |
| **Gemini** | Lateral thinking, huge context | Tie-breaker, Fresh Red Team, Divergence |

---

## 7.6 Folder Cheat Sheet

```
.task/[name]/           ← Ephemeral (gitignored)
  00_status.md          ← Current state
  01_brief*.md          ← Task definition
  02_questions/         ← Question funnel
  03_design/[model]/    ← RFCs, per model
  04_reviews/           ← Cross-critiques
  05_implementation/    ← In-memory code
  06_final/             ← Final review, retro

.project/               ← Persists across tasks
  phases.md             ← L3: Phase definitions
  current_phase/        ← L2: Task list

docs/                   ← Permanent (committed)
  adr/                  ← Decisions
  specs/                ← Frozen specs (condensed)
  footguns/             ← Traps for future devs
```

---

## 7.7 Governance Cadence Cheat Sheet

| Artifact | Review Frequency |
|----------|------------------|
| Task List | Weekly / every 3-5 tasks |
| Phase Plan | Monthly / at phase boundaries |
| Project Goals | Quarterly / when context shifts |

---

## 7.8 Visual: Two Contamination Barriers

```
DESIGN                          IMPLEMENTATION

Claude:                         Claude:
 • Idea Harvest                  • In-memory code
 • RFC                           • Difficulties
 • Self-critique                 

GPT:                            GPT:
 • Idea Harvest                  • In-memory code
 • RFC                           • Difficulties
 • Self-critique                 

═══════ BARRIER ═══════         ═══════ BARRIER ═══════

Cross-critique                  Cross-review
Red team                        Synthesis
Synthesis                       
                                WRITE TO DISK
FROZEN SPEC                     (real artifact)
(approved)                      
                                FINAL REVIEW
                                (catches "one more thing")
                                
                                HARDENING
                                Fix + Confirm
```

---

# Part VIII: Origin & Philosophy

## The Name: "Formoso e Seguro"

From the Portuguese proverb: *"Mais vale ir feio e seguro que formoso e arriscado"* — "Better to go ugly and safe than beautiful and risky."

But this workflow rejects the tradeoff. The goal is **formoso e seguro** — beautiful AND safe. Take the time to be both.

## Why This Exists

When working solo with AI models:
- A single model has blind spots
- No one challenges assumptions
- Decisions drift without documentation
- "It works" becomes the only quality bar

This workflow provides:
- Adversarial collaboration (models challenge each other)
- Structured decision gates (human stays in control)
- Documentation as a byproduct (not an afterthought)
- Multi-pass verification (catches different issue classes)

## The Process IS Your Team

When you work alone, discipline replaces colleagues. This workflow encodes the discipline of a good engineering team:
- Architects who think before coding
- Reviewers who challenge designs
- Red teams who find failure modes
- Tech leads who document decisions
- QA who verify reality matches intent

The models play these roles. The process ensures they do it well.

---

*"Be verbose while working, be terse when done."*
