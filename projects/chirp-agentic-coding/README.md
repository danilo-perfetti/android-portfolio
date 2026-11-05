# Chirp — Agentic Coding Experiment

A Kotlin Multiplatform / Compose Multiplatform engineering experiment focused on using an AI coding agent as a constrained development collaborator rather than as an autonomous code generator.

The experiment started from an isolated Chirp project snapshot before the Register feature was implemented. The AI agent had access to the existing codebase, explicit functional requirements, selected Figma design context and human feedback, but was intentionally prevented from inspecting the reference implementation until the AI implementation had been completed, reviewed and committed.

## Goals

The experiment evaluated whether an AI coding agent could:

- understand an existing multi-module Kotlin Multiplatform codebase;
- preserve the established architecture and conventions;
- design and implement a non-trivial Register feature;
- integrate with real networking, dependency injection and shared UI infrastructure;
- use structured Figma context through Remote MCP;
- operate under explicit security, Git and repository-isolation guardrails;
- react to human code review and an independent AI review;
- build and validate its work across Android and iOS;
- be compared afterwards with a hidden reference implementation;
- support a later engineering-hardening phase with automated tests and accessibility work.

## Workflow

The core workflow was:

1. isolate a pre-Register project baseline;
2. configure Claude Code with project-specific rules and permissions;
3. connect Claude Code to selected Figma nodes through Figma Remote MCP;
4. run a read-only codebase analysis;
5. review and approve an implementation plan;
6. let Claude implement the Register feature;
7. perform a human review;
8. run an independent Gemini review;
9. apply only human-approved corrections;
10. commit the final AI implementation;
11. compare it with the previously hidden course reference;
12. perform a retrospective;
13. add a separate post-experiment testing and accessibility phase.

The human developer remained responsible for scope, architecture approval, review, security decisions and acceptance.

## Technical Areas

- Kotlin Multiplatform
- Compose Multiplatform
- Clean Architecture / MVVM-style presentation
- Koin dependency injection
- Ktor networking
- Compose state management
- Compose Multiplatform resources
- Figma Remote MCP
- Claude Code
- Gemini independent review
- automated unit, integration and UI testing
- accessibility / semantics
- Android TalkBack validation
- iOS accessibility gap analysis

## Key Outcome

The experiment showed that an AI coding agent can be productive on a non-trivial KMP/CMP feature when it operates inside a controlled engineering process.

The strongest result came from combining:

```text
AI implementation
+ explicit project constraints
+ human review
+ independent AI review
+ delayed reference comparison
+ targeted corrective passes
```

rather than from autonomous execution alone.

## Documentation

- [AI Agent Case Study](AI_AGENT_CASE_STUDY.md)
- [Architecture](ARCHITECTURE.md)
- [Agent Guardrails](AGENT_GUARDRAILS.md)
- [Testing & Accessibility](TESTING_AND_ACCESSIBILITY.md)
- [Attribution](ATTRIBUTION.md)

## Source Code

The source repository for this experiment is intentionally kept private.

This public portfolio entry documents the engineering process, decisions and results without redistributing course source code or course-only design assets.
