# Academic Publishability Assessment: "Formoso e Seguro" Workflow

## A Reference Document for Publication Decision-Making

---

# Executive Summary

**The Question:** Is the "Formoso e Seguro" multi-model workflow for LLM-assisted software development publishable in an academic setting?

**The Answer:** Yes, with significant work. The core concepts are timely and potentially valuable. However, the current state is an *engineering contribution* (a practitioner methodology), not yet a *scientific contribution* (tested claims with evidence).

**The Path:** Workshop/position paper first (3-6 months), then fuller empirical paper (12+ months) if evaluation yields interesting results.

**The Risk:** Reviewers may dismiss it as "common sense + good engineering practice, repackaged with LLMs" unless you (a) position against prior art, (b) articulate LLM-specific failure modes, and (c) provide evidence.

---

# Part I: What Is the Contribution?

## 1.1 The Core Ideas

The "Formoso e Seguro" workflow proposes:

1. **Contamination Barriers**: Preventing AI models from seeing each other's work until they've independently produced their own solutions. This preserves creative breadth and makes critiques grounded in experience rather than reaction.

2. **Multi-Pass Verification**: Multiple review phases catch different classes of issues (design-level, integration, reality-vs-intent gaps).

3. **Structured Human Gates**: Explicit approval points with typed decisions (Direct, Preference, Delegation, Defer, etc.) that minimize human cognitive load while preserving oversight.

4. **Adversarial Collaboration**: Models critique each other rather than just cooperate on task decomposition.

5. **Reproducibility Infrastructure**: Step manifests, context bundles, and artifact trails enable debugging and replay.

## 1.2 What Type of Contribution Is This?

| Type | Description | Current Status |
|------|-------------|----------------|
| **Engineering Contribution** | "A protocol for safer LLM-assisted development" | ✅ Strong |
| **Scientific Contribution** | "Evidence that X improves Y under conditions Z" | ❌ Missing |
| **Tool Contribution** | "A reference implementation with demonstrated utility" | ❌ Not built yet |
| **Theoretical Contribution** | "A formal model of multi-agent collaboration" | ⚠️ Partial (needs formalization) |

**Key Insight:** You have an engineering contribution. To make it academic, you need either:
- Formalization + evidence (scientific contribution), OR
- A tool + adoption/utility demonstration (tool contribution)

## 1.3 The "One Sentence" Paper Thesis

Several candidate framings:

> **Version A (Empirical):** "We propose and evaluate a contamination-controlled, multi-model workflow for LLM-assisted software development, showing it improves defect discovery and reduces spec drift at measurable cost overhead."

> **Version B (Position):** "We argue that adversarial multi-agent workflows with explicit information barriers address LLM-specific failure modes (hallucination, anchoring, silent drift) that cooperative approaches cannot."

> **Version C (Tool):** "We present [tool name], an orchestrator for multi-model software development with contamination barriers, and demonstrate its utility across N real-world tasks."

**You must pick one spine.** The contribution must be clear and singular.

---

# Part II: The Academic Traps (Read This First)

Before investing months of work, understand the common failure modes for this type of contribution:

## 2.0.1 The Methodology Trap

**The Problem:** Methodology papers are rarely accepted at top venues without massive-scale data.

> "We deployed this at Google with 500 engineers" → Accepted
> "I used this solo and it worked for me" → Rejected for "lack of generalizability"

**Mitigation:** Don't try to publish a methodology paper at a main conference track. Start with workshops, position papers, or experience reports where the bar is different.

## 2.0.2 The Tool Trap

**The Problem:** Building a tool feels like progress, but tool papers require either:
- Significant technical novelty in implementation (not just "workflow encoded as CLI"), OR
- Demonstrated adoption/utility (real users, not just you)

**Mitigation:** A minimal tool is fine for supporting a position paper or experience report. Don't over-invest in tooling before validating the concepts are publishable.

## 2.0.3 The Evaluation Gap

**The Problem:** "Evaluation" is listed as step 3 or 4 in most advice. In academia, evaluation is effectively Step 0. If you cannot measure improvement, you don't have a paper — you have a blog post.

**Mitigation:** Decide upfront: Are you writing a position paper (no evaluation required) or an empirical paper (evaluation is everything)? Don't land in the middle.

## 2.0.4 The Generalizability Trap

**The Problem:** N=1 (solo developer) doesn't generalize. Reviewers will ask: "Does this work for teams? Different domains? Different skill levels?"

**Mitigation:** 
- Acknowledge limitation explicitly
- Frame as "initial operationalization" not "proven methodology"
- Propose how it could be validated at scale
- Emphasize replicable artifacts (someone else could run the same protocol)

---

# Part III: Prior Art and Positioning

## 2.1 The Novelty Challenge

**The Problem:** "Contamination barriers" sounds novel, but it's actually a well-known concept under different names:

| Existing Concept | Field | Relationship to Your Work |
|------------------|-------|---------------------------|
| **Anchoring Bias Control** | Cognitive Psychology | Your barriers prevent models from anchoring on each other's solutions |
| **Double-Blind Review** | Scientific Publishing | Same principle: independent evaluation before convergence |
| **Delphi Method** | Expert Consensus | Structured rounds of independent input → synthesis → refinement |
| **Ensemble Methods** | Machine Learning | Multiple independent models → aggregation |
| **Self-Consistency Prompting** | LLM Research | Multiple samples → voting/selection |
| **Red Teaming** | Security/AI Safety | Adversarial evaluation of proposals |
| **Separation of Concerns** | Software Engineering | Role-based independence in review processes |

**The Risk:** If you claim novelty without positioning against these, reviewers will pounce.

**The Correct Framing:** Not "we invented contamination barriers" but rather:

> "We provide a concrete operationalization of independence and adversarial review principles for LLM-assisted software engineering, with explicit visibility constraints, artifact specifications, and human gate definitions."

## 2.2 What IS New (The Defensible Claims)

What you can legitimately claim:

1. **LLM-Specific Application**: These principles haven't been systematically applied to multi-LLM software development workflows with this level of specification.

2. **Operationalization**: You've defined exactly *when* barriers apply, *what* artifacts are allowed/forbidden at each step, and *how* gates work.

3. **Integration**: You've combined multiple concepts (independence, adversarial review, multi-pass verification, reproducibility) into a coherent workflow.

4. **Practitioner Focus**: Most LLM multi-agent work is about autonomous agents. Yours is explicitly human-in-the-loop.

## 2.3 Required Literature Review Topics

Before submitting anything, you need to review and cite:

### Multi-Agent LLM Systems
- AutoGen (Microsoft)
- MetaGPT
- ChatDev
- CrewAI
- CAMEL (Communicative Agents)

### LLM-Assisted Software Engineering
- GitHub Copilot studies
- ChatGPT for code generation evaluations
- LLM hallucination in code
- Prompt engineering for software development

### Software Process Research
- Code review effectiveness studies
- Pair programming research
- Inspection methods (Fagan, etc.)

### Cognitive Bias in Software Engineering
- Anchoring effects in estimation
- Confirmation bias in debugging
- Group dynamics in design review

### AI Safety and Red Teaming
- Red teaming LLMs
- Adversarial evaluation methods

---

# Part IV: The Killer Objections (And How to Answer Them)

## 3.1 Objection: "This is just common sense"

**The Attack:** "Isn't this just good engineering practice? Independent review, multiple passes, documentation... we've always done this."

**The Defense:** LLMs introduce *new* failure modes that require *specific* mitigations:

| LLM-Specific Failure Mode | Why It's Different | How the Workflow Addresses It |
|---------------------------|-------------------|------------------------------|
| **Hallucination** | LLMs confidently produce non-existent APIs, wrong facts | Multi-model cross-check, verification reads |
| **Anchoring Amplification** | LLMs may be MORE susceptible to anchoring than humans | Contamination barriers before convergence |
| **Non-Determinism** | Same prompt → different outputs | Step manifests, reproducibility infrastructure |
| **Prompt Injection** | Repo content can hijack model behavior | Context safety policy, guard lines |
| **Context Window Limits** | "Forgetting" mid-task | Context packs, chunked information flow |
| **Silent Drift** | Spec → code divergence without warning | Stop-the-line rule, spec amendment gates |
| **Sycophancy** | Models agree with each other to be "helpful" | Adversarial roles, forced disagreement phases |

**The Claim:** "Traditional code review assumes human failure modes. LLMs fail differently. Our protocol addresses LLM-specific failure modes systematically."

## 3.2 Objection: "Where's the evidence?"

**The Attack:** "You say this is better. Compared to what? Measured how?"

**The Defense Options:**

1. **Position Paper Path:** "We're not claiming it's proven better. We're proposing a framework and hypotheses for future testing."

2. **Empirical Paper Path:** "We evaluated on N tasks and found [specific results]."

3. **Experience Report Path:** "We used this for M months and observed [specific outcomes]."

You must choose one. Position papers don't need evidence. Empirical papers do.

## 3.3 Objection: "N=1 doesn't generalize"

**The Attack:** "You're one developer. How do we know this works for others?"

**The Defense:**
- Acknowledge the limitation explicitly
- Frame as "initial operationalization" not "proven methodology"
- Propose how it could be validated at scale
- Point to the replicable artifacts (someone else could run the same protocol)

## 3.4 Objection: "This is too expensive/slow"

**The Attack:** "Running 3 models, multiple passes, all this documentation... isn't this overkill?"

**The Defense:**
- Acknowledge the cost explicitly (this is a feature, not a bug — you're trading cost for quality)
- Provide the cost/benefit framing: "We traded X% higher token cost for Y% reduction in defects"
- Emphasize the "complexity gate" — trivial tasks use Express track
- Compare to the cost of fixing bugs in production

---

# Part V: Evaluation Design (The Hard Part)

## 4.1 Why Evaluation Matters

Without evaluation, you have a blog post, not a paper. Academia requires:
- Claims that are falsifiable
- Evidence that the claims hold (or don't)
- Baselines for comparison

## 4.2 What to Measure

| Metric Category | Specific Metrics | How to Operationalize |
|-----------------|------------------|----------------------|
| **Defect Discovery** | Bugs found pre-merge, bugs found post-merge, severity distribution | Count issues by phase, categorize by severity |
| **Spec Accuracy** | Amendments required during implementation, divergence incidents | Count spec changes, categorize causes |
| **Code Quality** | Static analysis findings, complexity metrics, test coverage | Run tools (ESLint, SonarQube, etc.) |
| **Correctness** | Tests passing, edge cases covered, acceptance criteria met | Map AC → tests → verification |
| **Cost** | Tokens used, API costs, wall-clock time | Log all API calls |
| **Human Effort** | Time spent, cognitive load (self-report) | Time tracking, surveys |
| **Reproducibility** | Can another person/run get similar results? | Replication attempts |

## 4.3 Baselines to Compare Against

| Baseline | Description | Why Include It |
|----------|-------------|----------------|
| **B1: Single Model** | Just Claude, standard prompting | The "naive" approach |
| **B2: Single Model + Self-Review** | Claude generates, then reviews own work | Common current practice |
| **B3: Dual Model, No Barriers** | Claude + GPT, but they see each other's work immediately | Tests the barrier hypothesis |
| **B4: Dual Model, With Barriers** | Your workflow, 2-model version | The experimental condition |
| **B5: Dual Model + Red Team (3rd)** | Full workflow with Gemini as fresh red team | Maximum rigor condition |
| **B6: Human Only** | No LLM assistance | Traditional baseline |

## 4.4 Task Corpus Design

**Requirements:**
- 15-30 tasks minimum for statistical validity
- Varied complexity (trivial, standard, high-risk)
- Varied type (bug fix, feature, refactor, security-sensitive)
- Clear success criteria per task
- Reproducible starting state (specific commit, test suite)

**Avoiding Cherry-Picking:**
- Pre-register task selection criteria
- Use existing benchmark tasks if available
- Have someone else select tasks (blind selection)

## 4.5 Handling Non-Determinism

LLMs are non-deterministic. Same prompt → different outputs.

**Strategies:**
- Run each condition multiple times (3-5 runs per task)
- Report variance, not just means
- Use temperature=0 where possible (not always available)
- Log exact prompts, responses, and random seeds
- Focus on *reproducible process* not *identical outputs*

## 4.6 Testable Hypotheses

| ID | Hypothesis | How to Test |
|----|------------|-------------|
| **H1** | Double-blind generation before critique increases issue discovery vs. single-model + review | Compare B4 vs B2 on defect counts |
| **H2** | Contamination barriers increase design diversity before convergence | Measure RFC similarity scores B3 vs B4 |
| **H3** | Final "on-disk" review catches different defect class than spec review | Categorize defects by phase discovered |
| **H4** | Multi-pass verification catches more issues than single-pass | Compare defect yield by pass number |
| **H5** | The workflow costs more but produces fewer post-merge defects | Cost vs. defect rate comparison |
| **H6** | Step manifests enable successful replay/debugging | Attempt replays, measure success rate |

## 4.7 Ablation Studies (What Would Make This Publishable)

**What is an ablation study?** Systematically removing components to measure their individual contribution. This is the GOLD STANDARD for proving a workflow's value academically.

**Why ablation matters here:** Your workflow has multiple components. Reviewers will ask: "Which parts actually help? Is the full complexity necessary?" Ablation answers this definitively.

### Ablation Study Design for Formoso e Seguro

| Component to Ablate | Full Workflow | Ablated Version | What It Tests |
|---------------------|---------------|-----------------|---------------|
| **Contamination Barriers** | Models work independently until cross-critique | Models see each other's work from the start | Does independence actually improve outcomes? |
| **Multi-Pass Verification** | Spec review + in-memory review + on-disk review + hardening | Single review pass only | Does each pass catch unique issues? |
| **Adversarial Roles** | GPT explicitly assigned "red team" role | Both models have same "helpful" role | Does adversarial framing improve critique quality? |
| **Human Gates** | Explicit approval required at key points | Automated continuation without human approval | Do gates catch issues that would otherwise slip through? |
| **Question Funnel** | Structured question collection before design | Direct to design without question phase | Does early question collection reduce spec drift? |
| **Step Manifests** | Full logging of inputs/outputs/params | No logging | Do manifests enable better debugging/replay? |
| **Third Model (Red Team)** | Fresh model for final red team | Same models throughout | Does a fresh perspective add value? |

### Ablation Results That Would Be Compelling

**If you can show:**

1. **Each component contributes distinct value:**
   > "Removing contamination barriers reduced design diversity by 40%. Removing multi-pass verification missed 25% of defects that on-disk review caught. Removing adversarial roles reduced critique severity by 30%."

2. **Components have different cost/benefit profiles:**
   > "Contamination barriers cost 2x tokens but found 3x more design issues. Third model red team cost +50% but only found 10% more issues—may not be worth it for standard tasks."

3. **There's a minimal effective configuration:**
   > "The full workflow costs $X per task. A 'lite' version with just [components A, B, C] achieves 80% of the benefit at 40% of the cost."

**This is what transforms a methodology paper into a scientific contribution.** You're not just saying "here's a workflow" — you're saying "here's evidence that each component provides measurable value."

### Practical Ablation Approach

**Phase 1: Binary Ablations** (each component on/off)
- Run 5-10 tasks with full workflow
- Run same tasks with each component removed
- Compare defect counts, cost, spec drift

**Phase 2: Component Combinations** (if Phase 1 shows signal)
- Test minimal configurations
- Find the "Pareto frontier" of cost vs. quality

**Phase 3: Per-Task-Type Analysis** (if resources allow)
- Do certain components matter more for security-sensitive tasks?
- Are barriers more important for design tasks than bug fixes?

### The Key Ablation Claim

If your paper can credibly state:

> "We performed ablation studies removing each workflow component. We found that (a) contamination barriers and (b) multi-pass verification each contribute statistically significant defect discovery improvements, with (c) the full workflow achieving X% better outcomes than single-model baseline at Y% higher cost."

**That is a publishable empirical contribution at a major SE venue.**

## 4.8 The Unit of Analysis Problem

**Critical Decision:** What exactly are you measuring? Different units require different experimental designs:

| Unit of Analysis | What You're Asking | Design Implications |
|------------------|-------------------|---------------------|
| **Tasks** | "Did this task succeed?" | Need many tasks (15-30), clear success criteria |
| **Defects** | "Did we catch this bug?" | Need defect classification, severity ratings |
| **Decisions** | "Did we make the right architectural choice?" | Need expert evaluation, long-term followup |
| **Cost** | "Did we save time/money?" | Need careful time tracking, cost accounting |
| **Process Compliance** | "Did people follow the workflow?" | Need observation, self-report data |

**Recommendation:** Start with defects as primary unit (most objective, easiest to count) and cost as secondary (most practical concern).

## 4.9 What If It Doesn't Work? (Negative Results)

**The Scenario:** Your evaluation shows the workflow doesn't help, or costs more than it saves.

**This is still publishable** if framed correctly:

> "We tested the hypothesis that contamination barriers improve defect discovery in LLM-assisted development. Contrary to our expectations, we found [X]. We analyze why this approach may not provide benefits under conditions [Y] and identify circumstances where it might still be valuable."

Negative results are valuable because they:
- Save others from pursuing dead ends
- Identify boundary conditions for when techniques work/don't
- Generate hypotheses for why

**Key:** Be honest, analyze why, propose what would need to change.

---

# Part VI: Threats to Validity

Any empirical study must address threats. Prepare answers for these:

## 5.1 Internal Validity

| Threat | Description | Mitigation |
|--------|-------------|------------|
| **Learning Effects** | You get better at tasks over time, confounding results | Randomize task order, counterbalance conditions |
| **Selection Bias** | Cherry-picked tasks that favor your approach | Pre-register task selection criteria, use existing benchmarks |
| **Experimenter Bias** | You want it to work, so you unconsciously favor it | Blind evaluation where possible, pre-register hypotheses |
| **Confounding Variables** | Task difficulty, time of day, model version changes | Control for these, document model versions |

## 5.2 External Validity

| Threat | Description | Mitigation |
|--------|-------------|------------|
| **Generalizability** | N=1 developer, specific domain | Acknowledge limitation, propose replication |
| **Task Representativeness** | Are these tasks typical? | Use diverse task types, document selection |
| **Tool Specificity** | Results may depend on specific LLM models | Test with multiple models if possible |
| **Expertise Level** | Results may differ for novices vs. experts | Document your expertise, discuss implications |

## 5.3 Construct Validity

| Threat | Description | Mitigation |
|--------|-------------|------------|
| **Defect Definition** | What counts as a "defect"? | Use established defect taxonomies |
| **Quality Metrics** | Are your metrics actually measuring quality? | Use multiple metrics, triangulate |
| **Cost Measurement** | Token count ≠ actual cost/effort | Include time, cognitive load, not just tokens |

## 5.4 The Creativity/Convergence Tradeoff (A Real Research Question)

**Important:** Contamination barriers have a potential downside that must be acknowledged.

**The Tradeoff:**
- **Pro:** Independence increases exploration, prevents anchoring
- **Con:** Independence increases duplicated effort, may miss "shared corrections" where early convergence would help

**This is a legitimate research question**, not just a threat. Your evaluation could investigate:
- When do barriers help vs. hurt?
- Are there task types where early convergence is better?
- What's the cost/benefit curve?

**Do not ignore this.** Reviewers will ask. Either investigate it or acknowledge it as a limitation and future work.

---

# Part VII: Publication Venues

## 6.1 Venue Types (Not Specific Names)

Specific workshops and conferences change yearly. Focus on *types*:

| Venue Type | Bar | Fit | Notes |
|------------|-----|-----|-------|
| **LLM/AI + SE Workshops** | Lower | High | Best entry point. Examples: workshops at ICSE, FSE, NeurIPS on LLM4Code, AI4SE, etc. |
| **SE Conference Industry/Practice Track** | Medium | High | ICSE SEIP, FSE Industry, etc. Want real-world application. |
| **SE Conference Main Track** | Very High | Medium | Need strong empirical results or significant tool adoption |
| **HCI/CSCW** | High | Medium | If framing around human-AI collaboration |
| **Practitioner Journals** | Medium | High | IEEE Software, ACM Queue. More forgiving of methodology papers. |
| **ArXiv Pre-print** | None | High | Not peer-reviewed but establishes priority, gets feedback |

## 6.2 Paper Types

| Paper Type | Length | Evidence Required | Venues |
|------------|--------|-------------------|--------|
| **Position Paper** | 2-4 pages | Argument, not evidence | Workshops |
| **Short Paper** | 4-6 pages | Preliminary results OK | Workshops, some conferences |
| **Tool/Demo Paper** | 2-4 pages | Working demo | Workshops, demo tracks |
| **Experience Report** | 6-10 pages | Systematic observation | Journals, SEIP tracks |
| **Full Research Paper** | 10-12 pages | Rigorous evaluation | Main conference tracks |

## 6.3 Formalization Spectrum (What Level Is Required?)

**Common misconception:** "I need mathematical proofs to publish."

**Reality:** Different venues expect different formalization levels.

| Venue Type | Formalization Expected | What That Looks Like |
|------------|----------------------|---------------------|
| **Workshop/Position** | Conceptual clarity | State machine diagram, clear definitions, visibility constraints in prose |
| **Tool/Demo** | Operational specification | Config file formats, CLI interface, state transitions |
| **Experience Report** | Systematic description | Protocol steps, decision points, artifact types |
| **SE Conference** | Semi-formal | State machine + operational definitions + evaluation |
| **Formal Methods Venue** | Full formalization | Mathematical definitions, theorems, proofs |

**For your work:** SE venues (ICSE, FSE, ASE) typically expect the "semi-formal" level: clear state machine, explicit visibility constraints, operational definitions of terms. You do NOT need to prove theorems like "Convergence(Critique(A,B)) > A alone" — that's overkill for SE venues.

**Minimum viable formalization:**
1. State machine with named states and transitions
2. Artifact definitions (what each file contains)
3. Visibility constraints (what can be read at each step)
4. Gate definitions (what approval means, exit criteria)

## 6.4 Recommended Path

```
Phase 1 (3-6 months):
├── Formalize the concepts (state machine, visibility constraints)
├── Write position paper (4 pages)
├── Post to ArXiv
├── Submit to workshop (LLM4Code, AI4SE, or similar)
└── Build minimal CLI tool

Phase 2 (6-12 months):
├── Use tool on real tasks, collect data
├── Run small-scale evaluation (15-20 tasks)
├── Write experience report or empirical short paper
└── Submit to SEIP track or practitioner journal

Phase 3 (12+ months, if results warrant):
├── Expand evaluation
├── Write full research paper
└── Target main conference track
```

---

# Part VIII: Risks and Mitigations

## 6.1 Technical Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Evaluation shows workflow doesn't help | Medium | Frame as valuable negative result; identify conditions where it does/doesn't help |
| Workflow too expensive for benefits | Medium | Document cost explicitly; identify cost-effective subset |
| Tool too complex to build | Low | Start minimal; CLI before GUI |
| LLM APIs change, breaking workflow | Medium | Abstract provider interface; document API assumptions |

## 6.2 Academic Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Reviewer says "just common sense" | High | Position against prior art; articulate LLM-specific failures |
| Reviewer says "not generalizable" | High | Acknowledge limitation; propose replication path |
| Reviewer says "no evaluation" | High (for full paper) | Do position paper first; or run actual evaluation |
| Similar work published first | Medium | Move quickly; establish priority via ArXiv |
| Venues reject as "not novel enough" | Medium | Workshop first; build credibility |

## 6.3 Practical Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Takes too long, lose motivation | Medium | Start with position paper (lower effort) |
| Field moves on, work becomes irrelevant | Low | The problem isn't going away; timing is good |
| Nobody cares | Low | LLM-assisted development is extremely hot right now |

---

# Part IX: What Would Make This Solid

## 7.1 The Minimum for a Position Paper

1. **Clear problem statement**: LLMs fail in specific ways that current approaches don't address
2. **Literature positioning**: Show awareness of prior art, explain what's different
3. **Formal-ish protocol description**: State machine, visibility constraints, gate definitions
4. **Argument for why this should work**: Connect mechanisms to failure modes
5. **Sketch of evaluation approach**: What you would measure, how

## 7.2 The Minimum for an Empirical Paper

Everything above, plus:

1. **Implementation**: At least a minimal tool or documented manual process
2. **Task corpus**: 15+ tasks with clear selection criteria
3. **Baselines**: At least 2-3 comparison conditions
4. **Results**: Quantitative outcomes on defined metrics
5. **Statistical analysis**: Appropriate tests, effect sizes, confidence intervals
6. **Threats to validity**: What could be wrong with your conclusions

## 7.3 The Minimum for a Tool Paper

1. **Working tool**: Installable, documented, usable by others
2. **Design rationale**: Why the tool is designed this way
3. **Demo scenarios**: Concrete examples of tool in action
4. **Initial utility evidence**: Either user studies or real usage data

---

# Part X: Key Distinctions to Keep Clear

## 8.1 Scientific vs. Engineering Contribution

| Scientific | Engineering |
|------------|-------------|
| "Does isolating LLMs reduce hallucination rates?" | "A protocol for safer code generation" |
| Requires controlled study | Requires proof of utility |
| Claims about phenomena | Claims about practice |
| Evaluated by experiments | Evaluated by adoption/utility |

You have engineering. Add evaluation to get science.

## 8.2 Reproduction vs. Replication

| Reproduction | Replication |
|--------------|-------------|
| Same researcher, same data, same result | Different researcher, same protocol, similar result |
| Hard with LLMs (non-determinism) | Achievable with LLMs |
| Focus on exact outputs | Focus on process and artifact quality |

Aim for replication, not reproduction.

## 8.3 Novelty vs. Contribution

| Novelty | Contribution |
|---------|--------------|
| "No one has done this exact thing before" | "This advances the field in a meaningful way" |
| Can be trivially satisfied | Requires demonstrated value |
| Necessary but not sufficient | What reviewers actually care about |

Position your work as a contribution (operationalization, integration, evidence) not just novelty.

---

# Part XI: Consensus Across All Critiques

Multiple independent analyses agreed on these points:

## 9.1 Universal Agreement

| Point | Consensus |
|-------|-----------|
| **Publishable?** | Yes, with work |
| **Main contribution** | Contamination barriers + structured multi-model workflow |
| **Biggest gap** | Evaluation / evidence |
| **Main risk** | "Just common sense repackaged" objection |
| **Best path** | Workshop/position paper first |
| **Critical need** | Prior art positioning |

## 9.2 Points of Disagreement

| Point | Disagreement |
|-------|--------------|
| **How much formalization?** | Ranges from "state machine is enough" to "need mathematical proofs" |
| **Tool paper viability** | Some say requires adoption, others say utility demo is enough |
| **Experiment size** | Ranges from "5 tasks" to "15-30 minimum" |
| **Venue specificity** | Some name venues, others say this is unreliable |

## 9.3 What All Critiques Missed or Underemphasized

1. **The creativity/convergence tradeoff**: Barriers reduce anchoring but also reduce beneficial early convergence. This is a real research question.

2. **Negative results possibility**: What if evaluation shows it doesn't work? That's still publishable.

3. **Ethics/IRB**: If running studies with humans, may need ethics review.

4. **Licensing and attribution**: For generated code and published artifacts.

5. **The "unit of analysis" problem**: Tasks? Defects? Decisions? Cost? These require different experimental designs.

---

# Part XII: Decision Framework

## 10.1 Should You Pursue Academic Publication?

**Pursue if:**
- You want the rigor of peer review
- You want academic credibility
- You're willing to do the formalization and evaluation work
- You have 6-18 months to invest

**Don't pursue if:**
- You just want to share the methodology
- You want fast impact
- Formal evaluation feels like overkill for your goals
- You'd rather iterate quickly based on practitioner feedback

**Alternative path:** Blog posts, open-source tool, conference talks (StrangeLoop, etc.), practitioner articles. This can have MORE impact than academic publication, just different credibility.

## 10.2 Quick Decision Checklist

| Question | If Yes | If No |
|----------|--------|-------|
| Do you care about academic credibility? | Pursue publication | Practitioner path |
| Do you have 6+ months to invest? | Pursue publication | Position paper or practitioner path |
| Are you willing to run systematic evaluation? | Full paper viable | Position paper only |
| Do you already have data from using the workflow? | Experience report | Need to collect data |
| Do you have a working tool? | Tool paper viable | Build tool or do position paper |

---

# Part XIII: Next Steps (Concrete Actions)

## 11.1 If Pursuing Academic Publication

### Immediate (This Month)
1. [ ] Decide on contribution type: position, empirical, or tool
2. [ ] Start literature review: multi-agent LLM, LLM-assisted SE, cognitive bias
3. [ ] Identify 2-3 target workshops (check current CFPs)

### Short-Term (1-3 Months)
4. [ ] Formalize the state machine and visibility constraints
5. [ ] Write 4-page position paper draft
6. [ ] Post draft to ArXiv for feedback
7. [ ] Submit to workshop

### Medium-Term (3-12 Months)
8. [ ] Build minimal CLI tool
9. [ ] Design evaluation study
10. [ ] Run evaluation on 15-20 tasks
11. [ ] Write experience report or empirical paper

## 11.2 If Pursuing Practitioner Path

### Immediate (This Month)
1. [ ] Polish the workflow document
2. [ ] Start building the CLI tool
3. [ ] Write blog post explaining the concepts

### Short-Term (1-3 Months)
4. [ ] Release tool as open source
5. [ ] Write tutorial content
6. [ ] Submit talk proposals to practitioner conferences

### Medium-Term (3-12 Months)
7. [ ] Gather user feedback
8. [ ] Iterate on tool and workflow
9. [ ] Build community around the approach

### Important Caveat: Popularity ≠ Validation

**Be honest with yourself:** "500 GitHub stars" or "trending on Hacker News" is a signal of *interest*, not *correctness*. 

- Popularity means people find the idea appealing
- It does NOT mean the workflow actually improves outcomes
- Reviewers will not accept adoption metrics as substitute for evaluation

If you later want to transition from practitioner path to academic publication, you'll still need systematic evaluation. However, adoption data CAN support an experience report: "N developers used this, here's what we observed."

---

# Part XII: What Would Make This Valuable (Summary)

## 12.1 The Transformation: From Blog Post to Paper

| Current State | What's Needed | Outcome |
|---------------|---------------|---------|
| "Here's a workflow" | + Literature positioning | Position paper |
| Position paper | + Basic evaluation (10-15 tasks, 2-3 baselines) | Experience report |
| Experience report | + Ablation studies (component-by-component testing) | Strong empirical paper |
| Strong empirical paper | + Replication by others + tool adoption | Definitive contribution |

## 12.2 The Single Most Valuable Addition: Ablation

If you do ONE thing to make this academically valuable, it's **ablation studies**.

**Why:** Ablation is the difference between "I made a workflow and it seems good" and "I can prove which components matter and by how much."

**The claim you could make:**
> "Through systematic ablation, we demonstrate that contamination barriers contribute X% defect reduction, multi-pass verification contributes Y%, and adversarial roles contribute Z%. The minimal effective configuration is [A + B] at cost [C], while the full workflow achieves [D] at cost [E]."

That's a real scientific contribution.

## 12.3 The Three Levels of "Publishable"

### Level 1: Workshop/Position Paper (3-6 months work)
**What you need:**
- Formalized state machine + visibility constraints
- Literature review showing awareness of prior art
- Clear articulation of LLM-specific failure modes
- Argument connecting mechanisms to failure modes
- Sketch of evaluation approach

**What you DON'T need:**
- Actual evaluation data
- Working tool
- Proof it works

**Suitable venues:** AI4SE workshops, LLM4Code, position paper tracks

---

### Level 2: Experience Report / Short Paper (6-12 months work)
**What you need:**
Everything from Level 1, plus:
- 10-15 tasks with documented outcomes
- At least 2 baselines (single model, dual model without barriers)
- Quantitative metrics (defects found, cost, time)
- Honest discussion of what worked and didn't
- Threats to validity

**What you DON'T need:**
- Full ablation studies
- Large-scale evaluation
- Statistical significance

**Suitable venues:** ICSE SEIP, IEEE Software, practitioner journals

---

### Level 3: Full Research Paper (12-24 months work)
**What you need:**
Everything from Levels 1-2, plus:
- 20-30 tasks with rigorous selection criteria
- Full ablation studies (component-by-component)
- Statistical analysis with effect sizes
- Multiple baselines including "no barriers" condition
- Replication data (multiple runs, variance reported)
- Clear contribution to knowledge ("we now know X that we didn't before")

**What would make it exceptional:**
- Replication by independent researchers
- Tool with real adoption
- Long-term followup (did the code hold up?)

**Suitable venues:** ICSE main track, FSE, TSE journal

## 12.4 The Key Questions Reviewers Will Ask

| Question | You Need To Answer |
|----------|-------------------|
| "What's new here?" | Operationalization for LLM-specific failure modes |
| "How is this different from [prior art]?" | Clear positioning against Delphi, double-blind, ensemble methods |
| "Does it actually work?" | Evaluation data with baselines |
| "Which parts matter?" | Ablation studies |
| "Is it worth the cost?" | Cost/benefit analysis |
| "Does it generalize?" | Discussion of limitations, replication path |
| "What if it doesn't work?" | Honest framing of negative results possibility |

## 12.5 The "Spine" Decision

Before proceeding, you must decide your primary contribution claim. Pick ONE:

| Option | Claim | Evidence Required |
|--------|-------|-------------------|
| **A: Protocol** | "A reproducible protocol for LLM-assisted development" | Formalization + utility demonstration |
| **B: Barrier Effectiveness** | "Contamination barriers improve outcomes in multi-LLM development" | Controlled experiment: barriers vs. no barriers |
| **C: Multi-Pass Value** | "Multi-pass verification catches distinct defect classes" | Per-phase defect analysis |
| **D: Human Gates** | "Structured human gates reduce spec drift" | Comparison with automated/ungated workflows |
| **E: Full Framework** | "The integrated framework outperforms alternatives" | Full ablation + baseline comparisons |

**Option E is strongest but requires most evidence.** Option A is easiest but least scientific.

## 12.6 Honest Assessment: Is It Worth Pursuing?

### Arguments FOR pursuing academic publication:
- Timing is excellent (LLM-assisted SE is very hot)
- The concepts are genuinely useful
- Formalization would improve the workflow itself
- Academic credibility opens doors

### Arguments AGAINST pursuing academic publication:
- 6-24 months of work for uncertain outcome
- Practitioner impact might be faster via blog/tool
- Evaluation is genuinely hard (non-determinism, task selection)
- You might discover it doesn't work as well as hoped

### The Middle Path:
1. Write position paper (low effort, tests the waters)
2. Post to ArXiv (establishes priority)
3. Build minimal tool (useful regardless)
4. Collect data while using it (opportunistic evaluation)
5. Decide on full paper based on what you learn

This hedges your bets while making progress on all fronts.

---

# Appendix A: Key Terms Glossary

| Term | Definition |
|------|------------|
| **Contamination Barrier** | A phase boundary where models are forbidden from seeing each other's outputs, preserving independent thinking |
| **Gate** | A human decision point with explicit approval requirements |
| **Double-Blind** | Neither model sees the other's work before producing its own |
| **Multi-Pass Verification** | Multiple review phases, each catching different defect classes |
| **Step Manifest** | Machine-readable record of inputs, outputs, and parameters for a workflow step |
| **Context Pack** | Aggregated context file to manage LLM context window limits |
| **Spec Drift** | Divergence between specification and implementation without explicit acknowledgment |
| **Stop-the-Line** | Rule requiring work to halt when spec violations are discovered |

---

# Appendix B: Sample Position Paper Abstract

> **Title:** Contamination Barriers for Multi-Model LLM-Assisted Software Development
>
> **Abstract:** Large Language Models are increasingly used for software development, but their failure modes differ from human developers: they hallucinate confidently, anchor strongly on prior context, and drift silently from specifications. We propose a structured workflow for LLM-assisted development that addresses these failure modes through *contamination barriers* (preventing models from seeing each other's solutions before independent generation), *adversarial review phases* (models critique each other's work), and *explicit human gates* (structured approval points with typed decisions). We formalize the workflow as a state machine with visibility constraints and artifact specifications. We argue that this approach—inspired by double-blind review and Delphi methods but operationalized for LLM-specific failure modes—offers a path toward higher-assurance AI-assisted software development. We outline evaluation approaches and hypotheses for future empirical validation.

---

# Appendix C: One-Page Summary for Sharing

## Is "Formoso e Seguro" Publishable?

**Yes, with work.**

**What it is:** A multi-model workflow for LLM-assisted software development featuring contamination barriers (models work independently before seeing each other's solutions), adversarial review phases, and structured human gates.

**What's potentially novel:** Not the individual concepts (independence, adversarial review, multi-pass verification) but their concrete operationalization for LLM-specific failure modes (hallucination, anchoring, silent drift).

**What's missing for publication:**
1. Literature positioning against prior art
2. Formalization of the protocol
3. Empirical evaluation

**Best path forward:**
1. Workshop/position paper first (3-6 months)
2. Build minimal tooling, collect data
3. Experience report or empirical paper (6-12 months)

**Main risk:** Reviewers say "this is just common sense." Defense: articulate LLM-specific failure modes and how the protocol addresses them systematically.

**Alternative:** Skip academia, publish as practitioner content (blog, open-source tool, conference talks). Different credibility, potentially more impact.

---

*Document compiled from synthesis of multiple independent critiques. Last updated: January 2025.*
