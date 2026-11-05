# Architecture

## Scope

The experiment implemented the Register feature inside an existing Kotlin Multiplatform / Compose Multiplatform project while preserving the codebase's established module boundaries and architectural style.

## Main Structure

The approved design kept reusable authentication infrastructure in shared core modules and Register-specific logic in the auth feature.

```text
core/domain
└── AuthService

core/data
├── KtorAuthService
└── RegisterRequest

feature/auth/domain
└── UserDataValidator

feature/auth/presentation
├── RegisterState
├── RegisterAction
├── RegisterViewModel
├── RegisterScreen / RegisterRoot
├── Register-specific error mapping
└── Koin module
```

No feature-specific repository abstraction was introduced simply for architectural symmetry.

The Register operation had a single remote source, no success payload requiring mapping, no cache and no local persistence.

## Presentation

The presentation layer followed the project's existing unidirectional state approach.

Key principles:

- persistent screen information belongs in state;
- business logic stays outside Composables;
- actions handled by the ViewModel are modeled explicitly;
- UI-only callbacks remain UI/root responsibilities when appropriate;
- existing project conventions are preferred over introducing a new pattern.

The Register form uses `TextFieldState`-based inputs and real-time validation.

## Validation

Validation is performed as the user edits the fields.

The implementation separates domain validity, presentation error identity and user-facing resource resolution.

## Networking

Registration uses the shared Ktor-based authentication infrastructure.

Important behaviors include:

- real backend request;
- no local persistence on successful registration;
- HTTP 409 mapped to a conflict error;
- registration-specific conflict presentation;
- reuse of shared result/error abstractions where appropriate.

## Dependency Injection

Koin is used for dependency injection.

The experiment also included cross-platform bootstrap work so that the same shared feature could run correctly from Android and iOS hosts.

## Compose Multiplatform

The UI reuses the existing Chirp design system rather than introducing feature-local replacements.

The implementation follows existing project patterns for theming, text fields, buttons, responsive layout, Compose Multiplatform resources, loading state and validation feedback.

## Design Integration

Figma Remote MCP was used to retrieve structured context for the relevant Register nodes.

The agent was not given unrestricted design-file exploration. This kept the design workflow consistent with the experiment's broader principle of least-necessary context.

## Reference Comparison

After the AI implementation was frozen, it was compared with the hidden reference implementation.

Independent convergence was found in several architectural choices, including ViewModel/state organization, real-time validation, Koin, error presentation flow, real backend registration and Compose Multiplatform resources.

Differences were then reviewed as engineering trade-offs rather than treated as automatic errors.
