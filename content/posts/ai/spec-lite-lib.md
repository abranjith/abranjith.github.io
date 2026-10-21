+++
date = '2026-10-20T20:50:12-05:00'
draft = false
title = 'Spec-Driven Development with spec-lite'
tags = ["ai", "sdd"]
+++

[spec-lite](https://github.com/abranjith/spec-lite) is my take on a library for spec-driven development ([SDD](https://github.github.com/spec-kit/concepts/sdd.html)). I built it with a simple goal: make structured AI development lightweight, easy to use, and portable across coding agents. I usually have subscriptions to multiple AI coding tools, including GitHub Copilot, Codex, Claude, and Gemini. I wanted to carry my work from one tool to another without having to explain the project all over again.

It draws inspiration from existing SDD libraries such as [spec-kit](https://github.github.com/spec-kit/), with workflows shaped around the different ways developers work: starting a project, adding a feature, fixing a bug, reviewing code, or updating documentation.

The idea is straightforward. Capture what you want to build, turn it into a plan and verifiable tasks, and keep those decisions alongside the code. That gives the next agent, or the next developer, something concrete to pick up.

Hooks take that workflow further: they let you connect spec-lite to your own tools, services, and automation, making it a foundation you can keep extending as your development process evolves.

## Features

### Switch coding agents and keep your momentum

This is the feature that motivated the project. Start planning in Claude Code, continue implementation in Codex, and review the result in Copilot. Plans, specifications, and reviews live in your repository, so each agent can read the same project state.

spec-lite provides [native integrations](https://github.com/abranjith/spec-lite/blob/main/docs/architecture.md#provider-adapters) for Codex, Claude Code, GitHub Copilot, and Pi, generating the agent and skill formats each tool expects. For other tools, you can use generic Markdown prompts or export a portable prompt bundle. The continuity comes from the artifacts you save with the project.

### Hooks: connect spec-lite to your entire development workflow

[Hooks](https://github.com/abranjith/spec-lite/blob/main/docs/features/hooks.md) are among spec-lite's most powerful concepts. They open up integration possibilities wherever a command, script, or HTTP request can take you. Your team's tools and processes can become part of the development workflow, with automation triggered when it matters most.

The model is simple: a role announces an event, such as `implement.post` or `review.verdict`, and subscribed hooks respond. Depending on the event, they receive context such as the feature, changed files, review verdict, and summary. That context lets an integration act on the work that just happened.

#### Build the integrations your team needs

For example, you could configure hooks to:

- Run your team's test suite, formatter, or custom policy checks after implementation.
- Send a review summary to Slack or Teams through a webhook or adapter.
- Update an issue tracker through its API when a feature reaches a milestone.
- Trigger a CI workflow or publish an artifact through a script or service API.
- Hand off to another skill or agent, or provide a custom prompt for the next step.

These are examples of what you can build with hooks. You supply the commands, endpoints, or handoff instructions that fit your environment. The integration surface stays open as your toolchain grows.

#### Useful built-ins from day one

spec-lite ships with [some built-in hooks](https://github.com/abranjith/spec-lite/blob/main/docs/features/hooks.md#built-in-hooks).

The optional [Git workflow hooks](https://github.com/abranjith/spec-lite/blob/main/docs/features/hooks-git-workflow.md) show how much a few integrations can accomplish together. They can give a piece of work its own worktree and branch, commit progress as tasks finish, push the completed work, and open or reuse its pull request.

Pull request support includes GitHub and Azure DevOps, plus a custom adapter option for other providers. You can build on that flow with your own checks and notifications.

#### Automate the checks and keep the decisions

[Gates and checkpoints](https://github.com/abranjith/spec-lite/blob/main/docs/features/hooks-gates-checkpoints.md) let hooks shape what happens next. A gate can block progress on a failed check and send the agent back to fix the problem within a bounded number of attempts. A checkpoint pauses for your answer in chat or through a configured GitHub channel. Rules can select the next step automatically when conditions are met, with limits on automatic handoffs.

For example, you could require tests to pass, hand the result to Review, and ask for a decision before the pull request is opened. Pending decisions survive the session locally; an active agent carries out the next step when you return.

The [CLI](https://github.com/abranjith/spec-lite/blob/main/docs/usage.md#hook) provides validation and dry-run previews so you can inspect the wiring before it runs.

Hooks are a powerful way to connect spec-lite to your existing development workflow. They let you automate checks, enforce conventions, build evals, and much more.

### Use the workflow your task needs

A new application and a small bug fix deserve different amounts of ceremony. spec-lite's [agents and skills](https://github.com/abranjith/spec-lite/blob/main/docs/features/agents-and-skills.md) let you choose the right starting point.

The Brainstormer helps refine an idea, the Planner develops a technical blueprint for work spanning multiple features, and the Feature Planner scopes a single enhancement. The Architect handles system design. For a well-scoped autonomous build, there is also a YOLO agent that can take you from idea to implementation (if you feel like living dangerously).

Focused skills cover implementation, debugging, testing, data modeling, DevOps, and more. Use a full workflow for a new project, or go straight to Fix and Review for fixes and enhancements to an existing project.

### Keep features traceable from plan to review

Large requests become easier to reason about when you can follow each piece through delivery. Features have [stable identifiers](https://github.com/abranjith/spec-lite/blob/main/docs/architecture.md#project-state), such as `FEAT-001`, that carry through plans, specifications, implementation, tests, and reviews.

Feature specifications break work into small, verifiable slices, giving implementation and testing a shared reference.

### Give your project a memory

Repeating the same conventions in every conversation gets old quickly. [Shared memory](https://github.com/abranjith/spec-lite/blob/main/docs/features/memory.md) keeps standing instructions in `.spec-lite/memory.md`, where every role can read them.

That might include your API error format, testing conventions, architecture rules, or the requirement to store timestamps in UTC. Memory can be bootstrapped from evidence in the repository, and durable conventions can be captured automatically as work progresses, so you don't have to remember to record them yourself.

Commit that memory with your plans and specifications, and the next person or agent gets the context too.

### Challenge the plan, then review the implementation

The [Plan Critic](https://github.com/abranjith/spec-lite/blob/main/docs/features/agents-and-skills.md) pressure-tests feasibility, risk, quality, and adaptability before implementation begins. It gives you a chance to catch an expensive assumption while changing it is still cheap.

Once code exists, [Review](https://github.com/abranjith/spec-lite/blob/main/docs/features/review.md) brings correctness, security, performance, testing, and dependency vulnerability checks into one report. Dependency checks use live GitHub Advisories by default and include remediation guidance.

You can review selected files, a feature, completed features in a plan, or dependencies alone. Feature reviews use the captured changeset to establish their scope. Findings include severity, evidence, and concrete fixes; unavailable checks are recorded as limitations. The report can then feed into Fix or Implement for follow-up work.

spec-lite also maintains a TODO list of open items, so you can track and prioritize them. These items are captured during implementation or review when they are identified as outside the scope of the current work.

### Keep documentation moving with the code

I wanted [documentation](https://github.com/abranjith/spec-lite/blob/main/docs/features/documentation.md) to be part of delivery. You choose the output directory and level of detail: technical documentation covers the README and architecture, while the full set adds quickstart, usage, and individual feature guides.

You can generate the configured set, refresh one document, or update only the parts affected by a change. There is also an option for Implement and Fix to request documentation updates as development happens.

The writers work from verified code and preserve clearly human-authored content. The aim is documentation that explains the software you can actually run.

### Start with conventions for your stack

spec-lite includes [baselines for common technology stacks](https://github.com/abranjith/spec-lite/blob/main/docs/features/agents-and-skills.md#stack-baselines), including TypeScript, Python, Java, .NET, Go, and Rust. These give planning and implementation sensible ecosystem defaults, such as idiomatic patterns for the chosen language.

You can also keep [project-specific helper scripts](https://github.com/abranjith/spec-lite/blob/main/docs/features/memory.md#project-tools) for your project's needs. Think of Bash scripts, which agents are good at creating and using.

### Install once and carry the workflow with you

Reusable agents and skills can be installed globally, while each repository keeps its own settings and working state. That makes it easy to bring the same workflow to another project.

The [update command](https://github.com/abranjith/spec-lite/blob/main/docs/usage.md#update) refreshes configured providers, migrates older configuration and feature layouts, and preserves Project Context edits by default. Obsolete generated files are removed with confirmation.

For a chat tool or an unsupported coding environment, [export](https://github.com/abranjith/spec-lite/blob/main/docs/usage.md#export) combines selected roles and references into a self-contained Markdown prompt. You can take the workflow beyond the native integrations.

## Getting started

The [quickstart](https://github.com/abranjith/spec-lite/blob/main/docs/quickstart.md) requires Node.js 22 or newer. Install the CLI and the roles for the coding tools you use:

```bash
npm install -g @abranjith/spec-lite
spec-lite install --global --ai codex,claude-code,copilot,pi
```

Then initialize your project:

```bash
cd your-project
spec-lite init
```

Setup records your providers, project profile, and documentation preferences. From there, select a role in your coding tool and give it a task. For example, ask the Feature Planner to scope an enhancement, use Implement to build it, and use Review to assess the result. Commit the resulting plans, specifications, and project context alongside the code so the next session can pick up where you left off.

What excites me most about spec-lite is being able to choose the tool that suits the work while keeping the project moving. The specifications, decisions, and progress stay with the code, and hooks connect that work to the rest of your development process. You can start small and build a workflow that grows with your project.

For complete documentation, visit the [spec-lite repository](https://github.com/abranjith/spec-lite). It is an open-source project, and contributions are welcome. If you try it, I would love to hear which workflows help you and where it could improve. You can report issues, suggest features, or submit pull requests on GitHub.
