# Agent Guardrails

## Why Guardrails Were Part of the Experiment

The goal was not to test how much unrestricted code an AI could generate.

The goal was to evaluate whether an AI coding agent could operate safely and productively inside a real project while respecting architectural, security and process constraints.

## Instruction Model

The local experiment used layered instructions:

```text
global project guidance
    ↓
path-specific rules
    ↓
task-specific playbooks
```

The public portfolio does not reproduce the internal instruction files verbatim; this document summarizes the engineering principles they enforced.

## Reference Isolation

Claude could not inspect the hidden course implementation during the Register implementation.

The reference became available only after implementation, review, validation and the final AI implementation commit.

## Filesystem Scope

Normal agent file access was restricted to the isolated project workspace.

A controlled test verified that a normal read outside the workspace was rejected.

This was treated as a workflow boundary rather than as a complete operating-system sandbox, so shell access and approvals were still governed separately.

## Git Safety

The agent was not allowed to perform destructive or history-altering Git operations without explicit authorization.

The workflow emphasized inspection before modification, human-reviewed diffs, no hidden commits, no automatic push and no destructive history rewriting.

## Secret Protection

Local credentials and environment-specific configuration were kept outside version control.

Secret protection was treated as a review concern rather than relying on `.gitignore` alone.

Before any future repository publication, both the current tree and Git history must be audited.

## Human Approval

The human developer retained authority over architecture, dependency additions, scope changes, reference access, security-sensitive behavior, review findings and final acceptance.

Agent output was never considered self-validating.

## Independent Review

Gemini was deliberately used as a reviewer rather than as a second implementation agent.

Its output was filtered by the human developer so that review automation could not silently expand scope or rewrite deliberate project decisions.

## Figma MCP Access

Figma Remote MCP access was narrowed to the design nodes relevant to the task.

The MCP connection provided structured design context but did not override repository isolation or human approval rules.

## Testing Rules

The agent was not allowed to:

- disable failing tests merely to obtain a green build;
- remove validation to make tests pass;
- hide compiler errors;
- silently ignore failed tasks;
- alter unrelated production behavior solely to satisfy a test.

Failures had to be investigated and reported.

## Temporary Probe / Spike Work

Temporary diagnostic work was allowed when clearly scoped.

The principles were:

- temporary files must be clearly identifiable;
- production entry-point changes must be minimal;
- original state must be restored exactly;
- temporary artifacts must be removed unless intentionally retained;
- useful findings must be preserved in documentation;
- repeated crash/retry loops must stop in favor of log inspection.

## Public vs Internal Documentation

The full local experiment contains much more operational detail than is useful in a public portfolio.

The public version therefore documents the **guardrail model**, not the complete internal configuration.
