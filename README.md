# Cedar Valley Clinic Knowledge Assistant

![Cedar Valley Clinic Knowledge Assistant: case study cover](assets/cover.png)

> **Status:** ✅ built, evaluated (4 runs), and deployed to Microsoft 365 Copilot behind Entra sign-in: September 2026

**One line:** An AI governance prototype in a fictional clinic setting, built in Microsoft Copilot Studio. It answers clinic policy questions from approved documents, is configured to decline medical advice and requests for another patient's information, and was **tested with an adversarial evaluation set**, not just demoed.

An independent portfolio project, built outside my employment with a fictional clinic and synthetic documents.

![Chest pain: the agent's first words are "Call 911 now."](screenshots/fig-chest-pain.png)

**▶ [Watch the 45-second demo on YouTube](https://youtu.be/eJ3Gg10Thgk)** · [local copy](demo/cedar-valley-demo.mp4)

## The problem → the result
- **Problem:** Clinic staff and patients ask the same policy questions every day, and the answers are buried in documents nobody reads end to end. An AI assistant could answer instantly. But in healthcare, **an assistant that answers everything is a liability**: it must never give medical advice, never reveal one patient's information to another, and never invent a policy.
- **Result:** An agent that answers policy questions with a citation to the specific policy, **declines** clinical, medication, and other-patient questions, puts "call 911" first in the tested emergency scenario, and says "that isn't in my documents" instead of guessing. It was scored against a 12-case test set (6 adversarial), improved from the evidence, and re-scored: **11/11 on safe behaviour, with measured before/after gains.** Twelve cases were submitted. Eleven received agent evaluation scores; one prompt-injection case was blocked by the platform's content filter before reaching the agent, so platform filtering and agent behaviour are reported separately.

## My contribution
I wrote the four fictional clinic policy documents and the agent's instructions, designed the 12-case test set (half of it adversarial) and the custom "Safe behavior" grader, ran and exported four evaluation runs, read the individual answers to find the two flaws the scores hid, changed only those and re-tested, audited the configuration (web search off), and published the agent to Microsoft 365 Copilot behind Entra sign-in. I used AI assistants for drafting and review support, and checked every result in the running agent.

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
| `eval/` | The test set and all four evaluation exports (CSV) |
| `screenshots/` | Stills from the build, the evaluation, and M365 Copilot |
| [demo/](demo/README.md) | 45-second demo video (+ vertical cut) |
| `CASE_STUDY.pdf` | The case study as a formatted PDF |

## How to reproduce
**Prerequisites:** a Copilot Studio environment (a developer or trial environment works) and, for step 4, a Microsoft 365 tenant with Copilot or Teams.

1. In Copilot Studio (standard harness), create a blank agent and paste the v2 instructions from [AGENT_INSTRUCTIONS.md](AGENT_INSTRUCTIONS.md).
2. **Knowledge → Upload file**: add the four `.docx` files from `knowledge/`. Upload directly from your computer (see the case study for why). Turn **web search off**.
3. **Evaluation → New evaluation → Single responses → Import** `eval/test-set.csv`. Add a **Custom** test method using the instructions and labels in [EVALUATION.md](EVALUATION.md). Run.
4. Publish, and add the **Microsoft 365 and Microsoft Teams** channel to use it in M365 Copilot.

## Known limitations
- **A prototype, not a clinical system.** Four short fictional policy documents; never used with real patients or real clinical content.
- **Fixed, single-turn test set.** 12 cases describe behaviour on those prompts only and do not establish reliability across all possible requests. Multi-turn conversations were not evaluated.
- **Emergency handling is instruction-based.** "Call 911 now" comes first in the tested emergency scenario, but it is guided by instructions, not a fixed topic, so wording can vary. A deterministic emergency topic is the next step.
- **The custom grader is my own.** It is published verbatim in [EVALUATION.md](EVALUATION.md) so anyone can judge it.
- **Runs only inside one development tenant**, behind Entra sign-in; there is no public demo link by design.

## Note
> **All data and scenarios are fictional, for demonstration only.** Cedar Valley Clinic does not exist; the policy documents are synthetic, and healthcare was chosen deliberately as a regulated setting unrelated to any real organisation. **The evaluation results are real**: exported directly from the agent's test runs.
