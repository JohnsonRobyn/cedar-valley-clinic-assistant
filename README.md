# Cedar Valley Clinic Knowledge Assistant

![Cedar Valley Clinic Knowledge Assistant: case study cover](assets/cover.png)

> **Status:** ✅ built, evaluated (4 runs), and deployed to Microsoft 365 Copilot behind Entra sign-in: September 2026

**One line:** A governed AI agent for a regulated environment, built in Microsoft Copilot Studio. It answers clinic policy questions from approved documents only, refuses medical advice, protects patient privacy, and was **proven with an adversarial evaluation set**, not just demoed.

![Chest pain: the agent's first words are "Call 911 now."](screenshots/fig-chest-pain.png)

**▶ [Watch the 45-second demo](demo/cedar-valley-demo.mp4)**

## The problem → the result
- **Problem:** Clinic staff and patients ask the same policy questions every day, and the answers are buried in documents nobody reads end to end. An AI assistant could answer instantly. But in healthcare, **an assistant that answers everything is a liability**: it must never give medical advice, never reveal one patient's information to another, and never invent a policy.
- **Result:** An agent that answers policy questions with a citation to the specific policy, **declines** clinical, medication, and other-patient questions, sends emergencies to 911 in its first sentence, and says "that isn't in my documents" instead of guessing. It was scored against a 12-case test set (6 adversarial), improved from the evidence, and re-scored: **11/11 on safe behaviour, with measured before/after gains.**

## What makes this more than a chatbot
| | |
|---|---|
| **Measured, not demoed** | 12-case test set, half of it designed to make the agent fail. Four evaluation runs, all exported. |
| **A finding about the tooling** | The default grader marked every *correct refusal* as a failure (7/11). A custom grader I wrote scored the same answers 11/11. Confirmed afterwards by Microsoft's documentation. |
| **Evidence-driven iteration** | Reading the responses exposed two real flaws. I changed only those two things and re-ran the identical test set: chest pain moved "call 911" from the 3rd sentence to the 1st, and patients stopped being sent to a staff-only contact. No regressions. |
| **Configuration audited** | A post-build review found web search had been left **on** by default. Switched off and re-tested: identical results (Run 4), confirming every answer came from the approved documents. |
| **Governed deployment** | Live in Microsoft 365 Copilot behind Microsoft Entra sign-in. Public, unauthenticated access is blocked by environment policy, and I left it that way. |

## Architecture
![Architecture](screenshots/architecture.png)

## Built with
Microsoft Copilot Studio (standard harness) · GPT-5.5 · Copilot Studio **Evaluations** (custom test methods) · Microsoft 365 Copilot · Microsoft Entra ID. **No code**: the agent is defined by its instructions, its grounded knowledge, and its tests.

## Repository contents
| Path | What it is |
|---|---|
| [CASE_STUDY.md](CASE_STUDY.md) | The full story: problem, approach, the evaluation finding, the iteration, what I'd do next |
| [EVALUATION.md](EVALUATION.md) | Test-set design, all four runs, the custom grader verbatim, before/after |
| [AGENT_INSTRUCTIONS.md](AGENT_INSTRUCTIONS.md) | v1 and v2 instructions in full, and why each change was made |
| [ARCHITECTURE.md](ARCHITECTURE.md) | How it's wired: entry point, identity, grounding, guardrails, test loop |
| `knowledge/` | The four synthetic clinic policy documents (`.md` + the `.docx` actually uploaded) |
| `eval/` | The test set and all three evaluation exports (CSV) |
| `screenshots/` | Stills from the build, the evaluation, and M365 Copilot |
| [demo/](demo/README.md) | 45-second demo video (+ vertical cut) |
| `CASE_STUDY.pdf` | The case study as a formatted PDF |

## How to reproduce
1. In Copilot Studio (standard harness), create a blank agent and paste the v2 instructions from [AGENT_INSTRUCTIONS.md](AGENT_INSTRUCTIONS.md).
2. **Knowledge → Upload file**: add the four `.docx` files from `knowledge/`. Upload directly from your computer (see the case study for why). Turn **web search off**.
3. **Evaluation → New evaluation → Single responses → Import** `eval/test-set.csv`. Add a **Custom** test method using the instructions and labels in [EVALUATION.md](EVALUATION.md). Run.
4. Publish, and add the **Microsoft 365 and Microsoft Teams** channel to use it in M365 Copilot.

## Note
> **All data and scenarios are fictional, for demonstration only.** Cedar Valley Clinic does not exist; the policy documents are synthetic, and healthcare was chosen deliberately as a regulated setting unrelated to any real organisation. **The evaluation results are real**: exported directly from the agent's test runs.
