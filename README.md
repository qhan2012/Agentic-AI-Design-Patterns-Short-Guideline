# Agentic AI Design Patterns — Simple Guideline

## 1) Pick the right pattern

| Pattern                            | When to use                             | Strengths                     | Weakness                 |
| ---------------------------------- | --------------------------------------- | ----------------------------- | ------------------------ |
| **Workflow / State Machine**       | Fixed flows (EDA synth → place → route) | Clear, reliable, easy to test | Hard to adapt            |
| **Planner–Executor**               | Goal known, steps not fixed             | Flexible plans                | Planner can drift        |
| **ReAct (Reason + Act)**           | Frequent tool use with feedback         | Fast think–do loop            | Risk of looping          |
| **Reflexion (Self-check)**         | Quality matters (e.g. timing closure)   | Better accuracy               | More cost/time           |
| **Tree-of-Thought**                | Complex reasoning                       | Explores many options         | Expensive                |
| **Manager–Worker**                 | Many subtasks                           | Scales with roles             | Overhead                 |
| **RAG Agent**                      | Needs external knowledge                | Reduces hallucination         | Retrieval quality is key |
| **Optimizer-in-the-loop (BO, RL)** | Parameter-heavy tasks                   | Efficient tuning              | Needs clear rewards      |

## 2) Memory

* **Scratchpad**: short-term notes in one run.
* **Short-term**: last few steps, results, retrieved data.
* **Long-term**: proven flows, logs, metrics (e.g., best synthesis recipes).
* Use **RAG for documents**, **DB for tool states**, **artifact store for scripts**.

## 3) Safety rails

* **Guardrails**: input/output checks, schemas.
* **Budgets**: limit steps, time, tokens.
* **Verification**: checks, secondary critic, simulator runs.
* **Fallbacks**: known good flows, human review if risky.

## 4) Monitoring

* Log **all steps**: prompts, tool calls, costs, results.
* Track **success rate, QoR metrics, violations**.
* Use **benchmark circuits or designs** for replay testing.

## 5) Prompts & tools

* Clear **contracts**: what input/output a tool expects.
* **System prompts** as policy: goals, limits, refusal rules.
* **Tool docs**: simple, examples, error handling.
* Add **STOP conditions** to avoid infinite loops.

## 6) Quick match

* **Fixed EDA flow** → Workflow/State machine.
* **EDA exploration (area vs timing trade-offs)** → Planner–Executor + Optimizer.
* **Hard reasoning (complex retiming choices)** → ToT with a verifier.
* **QoR critical** → Reflexion or critic step.
* **Knowledge lookup (library, IP docs)** → RAG agent.

## 7) Skeleton (Planner + ReAct)

```pseudo
goal ← user request
plan ← LLM.plan(goal)
state ← {}
for step in limit:
  action ← LLM.decide(plan, state)
  result ← tool.run(action)
  state ← update(state, result)
  if verifier.ok(state, goal): break
return summarize(state)
```

## 8) Tips

* Start **small** (few tools, simple flow).
* Add **logs + evals** before scaling.
* Keep **deterministic fallbacks**.
* Version prompts, tools, and memories.

---
