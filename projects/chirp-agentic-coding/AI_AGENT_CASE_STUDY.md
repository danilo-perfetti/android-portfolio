# AI Agent Case Study

## Context

This experiment was created to evaluate AI-assisted software development in a realistic Kotlin Multiplatform codebase.

Instead of asking an AI to generate an isolated sample app, the agent had to work inside an existing multi-module project with established architecture, reusable design-system components, networking abstractions and platform-specific integration.

The selected task was the Register feature.

The experiment intentionally started from a project state in which that feature had not yet been implemented.

## Experimental Design

The central constraint was **reference isolation**.

Claude Code was allowed to inspect:

- the isolated project snapshot;
- existing architectural patterns;
- explicit Register functional requirements;
- authorized Figma design context;
- human feedback and review comments.

It was not allowed to inspect the hidden reference implementation during the implementation phase.

Only after the AI implementation had been completed, reviewed, validated and committed was the reference implementation inspected.

This made the final comparison useful: similarities could represent independently reached engineering decisions instead of code reproduction.

## Agent Workflow

### 1. Read-only analysis

Claude first inspected the codebase without modifying it.

The goal was to understand module boundaries, state-management conventions, dependency injection, networking abstractions, design-system reuse, resource handling and project-specific Compose patterns.

### 2. Plan Mode

Before implementation, Claude produced a complete plan.

The human developer reviewed and refined the plan before any substantial code change was accepted.

### 3. Implementation

Claude implemented the Register feature across the required modules, including domain validation, presentation state and actions, ViewModel integration, Koin wiring, Ktor registration networking, HTTP conflict handling, error mapping, Compose UI integration, shared resources, loading and validation behavior.

### 4. Human review

The first implementation was reviewed manually with particular attention to architecture, reuse of existing abstractions, state and event ownership, networking, error mapping, security, Compose design-system reuse and unnecessary structural changes.

### 5. Independent Gemini review

Gemini was used as an independent reviewer, not as a co-author of the first implementation.

Its findings were not applied automatically. Every finding was evaluated by the human developer against the actual project requirements and experiment scope.

This produced an important lesson: an AI reviewer can identify valuable issues, but it can also promote generic best practices into false positives when it lacks enough context.

### 6. Corrective pass

Claude received only the findings approved by the human developer and applied the agreed changes.

### 7. Delayed reference comparison

After the AI implementation was finalized, it was compared with the hidden reference implementation.

Several major decisions converged independently, including:

- Register state/ViewModel structure;
- TextFieldState-based inputs;
- real-time validation;
- DataError / UiText presentation flow;
- Koin dependency injection;
- HTTP 409 conflict handling;
- real backend registration;
- Compose Multiplatform resource usage.

The comparison also highlighted meaningful differences. The reference implementation was stronger in some local implementation choices, while the experiment introduced a stronger production-safe HTTP logging policy for sensitive authentication traffic.

## Figma Remote MCP

The experiment used Figma Remote MCP to give Claude structured access to selected Register design nodes.

```text
Claude Code
    ↓
Model Context Protocol (MCP)
    ↓
Figma Remote MCP
    ↓
authorized design nodes
```

This was preferable to treating screenshots as the only design input because the agent could request structured design context.

Access was deliberately limited to the relevant design area. The original design file was not modified; when MCP operations required edit-level access, an editable duplicate was used for the experiment.

## Human-in-the-Loop Decisions

Human intervention was intentionally part of the design, not a fallback.

Examples included defining the isolation boundary, approving architectural decisions, controlling reference access, correcting weak assumptions in plans, refining the Figma workflow, filtering Gemini findings, choosing the iOS dependency-injection bootstrap location and hardening HTTP logging around sensitive authentication traffic.

## Security Hardening

A notable review outcome was the decision to prevent sensitive authentication data from being exposed through HTTP logging.

Rather than weakening logging globally, the solution targeted sensitive authentication traffic while preserving useful logging elsewhere.

## Efficiency

The AI agent reduced the amount of manual implementation required across multi-module production code, Koin wiring, cross-platform bootstrap, Ktor integration, Compose screen/state implementation, resources and error mapping.

Some of those savings were offset by the deliberately rigorous experiment process: repeated approvals, independent review, documentation, delayed comparison and runtime investigation.

## Main Lessons

1. **Context matters more than raw autonomy.** The agent performed better when architecture, constraints and expected reuse were explicit.
2. **Human review remains essential.** Both implementation and review agents produced suggestions requiring developer judgment.
3. **Reference isolation made the final comparison meaningful.**
4. **Existing abstractions should be part of the review checklist.**
5. **Testing is valuable as a separate engineering phase.**
6. **MCP can make design collaboration more structured when access is constrained.**

## Result

The experiment objective was achieved.

Claude Code proved useful as a development collaborator for a non-trivial Kotlin Multiplatform feature, but the best outcome depended on a structured engineering process rather than unrestricted autonomous execution.
