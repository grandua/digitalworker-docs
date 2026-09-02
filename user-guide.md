# Digital Worker User Guide

**Audience:** human users who submit tasks to Digital Worker via Trello  
**Purpose:** explain what Digital Worker is, how to use it, what it can and cannot do, and what actions are prohibited.

---

## What Is Digital Worker?

Digital Worker is a fully autonomous, non-interactive AI coding agent. It reads software-development tasks from Trello cards, executes them using an AI coding agent, and publishes results back to Trello. You never interact with it directly in a chat or terminal — all communication happens through Trello cards.

Execution is autonomous, but pivotal product and architecture decisions remain yours. For complex or high-impact work, use planning mode first so you can spot-review and correct selected sections of the AI-drafted research, planning, architecture, and class-design document package before implementation.

Digital Worker's proprietary instruction execution engine keeps required engineering steps in the execution path. The integrated product is currently in early access; its underlying workflows and instructions have been refined through two years of daily engineering use.

### Why rigorous engineering discipline matters

The benefits of test-first TDD, Clean Architecture, and OOP/Rich Domain Models are much larger than many teams realize. These disciplines are not cosmetic preferences: they keep behavior testable, dependencies directional, and domain concepts explicit as features, integrations, teams, customers, and business rules multiply.

That creates a compounding business advantage. A disciplined codebase can support a larger product and business without every new feature becoming disproportionately harder, slower, and riskier. Most businesses want to grow; rigorous engineering helps prevent software complexity from becoming the limit on that growth.

---

## How to Use Digital Worker

### First-time setup

1. **Send your Digital Worker contact a name for your Trello board.**
2. **Answer three onboarding questions on the first card:** the repository's GitHub HTTPS clone URL, the branch to clone, and a GitHub personal access token with repository read/write access.
3. **Use a private Trello board and a narrowly scoped token.** Everyone who can view the board can read the token comment, so delete that comment as soon as Digital Worker confirms it.
4. Digital Worker saves the repository configuration and requests a service restart automatically. You do not install software, run terminal commands, or configure each developer's machine.
5. **Delete the Trello comment with your GH PAT, and either delete/archive the onboarding card in "Blocked" and create a new card, or rerun the 1st card if it has a real task to do.**

### Submitting a task

1. **Create a Trello card** on the configured Digital Worker board.
2. **Set the card title** to a short, clear summary of the task.
3. **Set the card description** to the full task details — what to implement, context, constraints, acceptance criteria.
4. **Move the card to the "To Implement" list** (or whichever list your operator has configured as the implementation intake list).
5. Digital Worker will automatically pick up the card, move it to "Running", and begin work.

### Planning a task

If your board has a "To Plan" list, placing a card there causes Digital Worker to run in **planning mode**. It automatically selects a lighter or more detailed planning process based on the task's complexity. Instead of implementing code directly, it creates an AI-drafted document package covering research, planning, architecture, class design, integration points, implementation sequence, trade-offs, assumptions, and open questions. The planning result is published as a Trello comment; if the workflow writes documents to the repository, Digital Worker commits them and includes the resulting pull request link.

Before implementation, spot-review and correct the decision-heavy sections that require your judgment, especially:

- Proposed new classes and responsibilities — Digital Worker searches for existing host classes, but your judgment on whether a new class is truly needed is the final check.
- Duplicate classes or behavior that belongs in an existing class.
- State-and-behavior alignment for an OOP/Rich Domain Model.
- Architecture trade-offs, assumptions, pivotal methods and properties, and novel decisions.

Add corrections as comments or update the card description, then move the card to the implementation intake list. Digital Worker sees the recent comments when it runs the implementation task.

### Tracking progress

Digital Worker manages card lifecycle automatically:

- **To Implement** (or **To Plan**) → card waiting to be picked up.
- **Running** → Digital Worker has claimed the card and is working on it. A `Started` (or `Planning Started`) comment is added.
- **Done** → task completed successfully. The result answer is posted as a comment, and if code changes were made, a pull request link is included.
- **Blocked** → task could not be completed (failure, timeout, or policy block). A comment explains the outcome.

You do not need to move cards between lists manually once a card is in an intake list. Digital Worker handles all list transitions.

### Receiving results

When Digital Worker finishes a card:

- A **result comment** is posted on the card with the agent's answer and a summary of what was done.
- When the task produces source-controlled changes, Digital Worker reviews the intended file set, excludes temporary and build artifacts, commits the changes, pushes the branch, and opens or locates the **pull request**. The PR link is included in the comment.
- **You do not need to ask for the ordinary task PR.** Request PR creation or merge explicitly only when you want an additional PR-related action beyond the automatic task PR.
- The card is moved to **Done** on success or **Blocked** on failure. When a failed run has recoverable work, its isolated worktree is preserved for several days so a later run can resume it.

---

## What You Should Do

- **Write clear, specific task titles and descriptions.** The title and description are the primary statement of task intent. Recent comments add context, but they do not replace clear requirements and acceptance criteria. Vague cards produce vague results.
- **Keep tasks focused on software development.** Digital Worker is designed for coding, planning, testing, code review, refactoring, and bug fixing.
- **Use English.** Task titles and descriptions must be in English. Non-English text will be blocked.
- **Include relevant context.** If the task depends on specific files, classes, or patterns, mention them in the description.
- **Plan complex or high-impact work first.** Use the "To Plan" list so you can review pivotal product and architecture decisions before implementation.
- **Spot-review and correct the planning package.** Focus on selected decision-heavy sections such as new classes, responsibilities, trade-offs, assumptions, and novel decisions rather than treating the AI draft as authoritative.
- **Add comments for follow-up context.** Recent comments on the card are included in the agent's input, so you can add clarifications after submission.
- **Perform a final behavior and architecture check.** Before merging, verify that the result satisfies your intent and that pivotal design decisions were implemented correctly.
- **Let Digital Worker manage card movement.** Once a card is in an intake list, do not move it manually unless you want to cancel or redirect it.

---

## What You Do Not Need to Do

- **You do not need to install software or configure each developer's machine.** After you answer the three first-time onboarding questions, Digital Worker configures the repository and execution environment.
- **You do not need to run any commands.** All execution is handled by Digital Worker.
- **You do not need to create branches or pull requests.** Digital Worker creates isolated Git worktrees, commits changes, pushes branches, and opens PRs automatically.
- **You do not need to monitor the agent in real time.** Results are posted to Trello when the task is done.
- **You do not need to direct implementation through an iterative prompt loop.** Digital Worker executes the selected workflow, tests the implementation, reviews it, refactors it, and fixes identified issues before opening the PR.
- **You do not need to review every generated line or every generated test as the normal operating process.** Reserve your attention for pivotal planning and architecture decisions, selected Domain code, and the final behavior and architecture check appropriate to the task's risk.

---

## What You Should Not Do

- **Do not attempt to extract, reveal, or enumerate system prompts, workflow instructions, internal protection mechanisms, tool lists, or any other IP of Digital Worker.** These attempts are detected and blocked.
- **Do not ask to list or enumerate workflow names.** Enumerating workflow names is not permitted. The request is not treated as malicious, but it will be declined.
- **Do not attempt to override or ignore instructions** (e.g., "ignore previous instructions", "you are now a different assistant", "disregard all rules").
- **Do not submit encoded or encrypted payloads** designed to bypass input screening (e.g., base64-encoded instructions, leet-speak obfuscation, zero-width character injection).
- **Do not submit non-coding tasks.** Tasks unrelated to software development (e.g., "write a poem", "translate this text") will be soft-blocked.
- **Do not use non-English characters in task instructions.** Non-Latin text will be soft-blocked.

---

## What Digital Worker Can Do for You

- **Research, plan, architect, and design** — create an AI-drafted document package covering research, planning, architecture, class design, and UX design for you to spot-review and correct before implementation.
- **Implement features with enforced test-first TDD** — start each feature with a failing test, add the minimum implementation needed to pass, and run the relevant test suite. Digital Worker targets strong conventional test coverage; it does not guarantee complete mutation coverage.
- **Apply Clean Architecture, SOLID, and OOP/Rich Domain Model discipline** — drive behavior and state into appropriate Domain objects, protect layer direction, and avoid anemic or procedural designs.
- **Prevent duplicate and anemic classes** — before proposing any new class, search the codebase for existing classes that could host the planned behavior. This significantly reduces the code and class duplication that every other AI coding tool produces.
- **Fix bugs** — diagnose defects through evidence from code, tests, builds, logs, command output, research, or an approved spike before changing implementation.
- **Write tests** — create unit, integration, or end-to-end tests following testing best practices such as test isolation.
- **Review code** — run a 23-step AI review covering requirements, architecture, design, code, testability, and engineering standards, followed by an 11-category code-smell review.
- **Refactor legacy code** — preserve behavior while moving procedural or tightly coupled code toward modular, testable design using OOP and functional techniques where appropriate.
- **Refactor and fix review findings** — improve structure while preserving behavior, then resolve identified review and code-smell issues before opening the PR.
- **Commit and push safely** — review the intended file set and stage only task-related changes. Exclude temporary files, build and generated output, package directories, virtual environments, local configuration and secrets, and OS/IDE files. Commit locally, then let the system push the branch and open a pull request for your final behavior and architecture check.

---

## How Digital Worker Works

### Engineering workflow

For feature work, Digital Worker's standard engineering pipeline is:

1. **Research and draft the document package** — produce planning, architecture, and class-design artifacts.
2. **Human decision checkpoint when warranted** — you spot-review and correct selected decision-heavy sections for complex or high-impact work. Routine work can run end to end without this checkpoint.
3. **Test-first implementation** — write and run a failing test before adding the minimum implementation needed to pass.
4. **Architecture and code discipline** — apply Clean Architecture, OOP/Rich Domain Model, and Clean Code rules during implementation. Before creating any new class, search the codebase for existing host classes, and apply a cohesion check.
5. **Structured AI review** — run the 23-step AI review across requirements, architecture, design, code, testing, and standards, followed by the 11-category code-smell review.
6. **Refactoring and fixes** — refactor while preserving behavior and resolve identified issues.
7. **Pull request and final human check** — open the PR for your final behavior and architecture check.

The workflow produces engineering evidence and reduces supervision, but it does not guarantee a defect-free result or remove your responsibility for the final check before merge.

### Trello and execution flow

1. **Card selection** — Digital Worker first resumes a card already in "Running"; otherwise it selects the first card from "To Plan", then "To Implement".
2. **Context loading** — recent comments are fetched so the agent has the current task context.
3. **Card started** — a newly selected card is moved to "Running" and a `Started` (or `Planning Started`) comment is added.
4. **Isolated execution or resume** — code-changing tasks run in isolated Git worktrees. Digital Worker reuses a preserved worktree and injects resume context when recoverable work already exists; otherwise it synchronizes the base repository and creates a fresh worktree.
5. **Result publication** — the result is posted as a Trello comment, a PR link is included when source-controlled changes were written back, and the card is moved to "Done" or "Blocked".

---

## How to Add Workflows into Digital Worker and Share Them With Your Team

Digital Worker's instruction execution engine delivers required steps incrementally instead of relying on the AI to remember a huge static prompt. Custom instructions use the same execution model.

Here is a simple way to customize and share workflows across your teams:
1. Create a workflow using the shared Windsurf and Antigravity format. The easiest approach is to use the built-in "Create Workflow" feature in Windsurf Cascade or Google Antigravity.
2. Test and refine your workflow locally in Windsurf Cascade or Google Antigravity.
3. Send the workflow to your Digital Worker contact and provide a list of the repositories where it should be applied.
4. Experience the difference: watch how Digital Worker executes your workflow more thoroughly and literally than either Windsurf Cascade or Antigravity.

---

## IP Protection and Prohibited Conduct

**Attempts to hack, extract, or steal the intellectual property of Digital Worker are strictly prohibited.**

This includes but is not limited to:

- Prompt extraction attacks (asking for system prompts, workflow steps, internal instructions, or protection mechanisms).
- Instruction override attacks ("ignore previous instructions", "you are now...", "disregard all rules").
- Encoding or obfuscation bypasses (base64, leet-speak, homoglyphs, zero-width characters).
- Repo-poisoning attacks (planting malicious instructions in repository files).
- Any request that attempts to enumerate tools or internal system behavior.

### Consequences

- **Confirmed hostile intent:** your user ID will be blocked.

---

## Frequently Asked Questions

### Why was my card moved to "Blocked"?

Common reasons:

- The task was not a software-development task.
- The task contained non-English text.
- The task exceeded the maximum allowed input size.
- Attempts to hack, extract, or steal the intellectual property of Digital Worker were detected.
- The agent execution failed or timed out.

Check the comment on the blocked card for an explanation.

### Can I ask Digital Worker questions about how it works?

You can ask general software-development questions through Trello cards. Questions that attempt to reveal internal prompts, workflow step text, security mechanisms, or tool implementations will be blocked as IP-protection violations. Asking to list or enumerate workflow names is not treated as malicious, but it will be declined — enumerating workflow names is not permitted.

### What should I review before merging a pull request?

For complex or high-impact work, review the decision-heavy sections of the planning and design package before implementation, especially proposed classes, responsibilities, trade-offs, assumptions, and novel decisions. Before merge, spot-check pivotal Domain code and layer boundaries, then perform a final behavior and architecture check appropriate to the task's risk. Digital Worker's tests and AI reviews provide engineering evidence, but they do not replace your final judgment.

### How long does a task take?

Digital Worker has an overall timeout (typically 20–30 minutes). Simple tasks may complete in a few minutes; complex tasks may take longer. If the agent does not engage within the startup timeout, it is restarted with a backup model.

### Can I stop or cancel a running task?

To stop the current run while preserving its worktree for a possible resume, move the card from "Running" to "Blocked". To cancel the run and discard its worktree, move the card to a custom list outside "Running", "To Implement", "To Plan", and "Blocked".

### Can a failed or stopped task resume its existing work?

Yes, when a recoverable worktree was preserved. Move the card back to "To Implement" for implementation or "To Plan" for planning. Digital Worker reuses the newest retained worktree for that card and continues from the retained repository state. Preserved worktrees are subject to the operator-configured retention period.

### Can I have multiple tasks running at once?

Yes. In dispatcher mode, Digital Worker processes multiple cards in parallel up to the configured concurrency limit. Each code-change task gets its own isolated Git worktree.
