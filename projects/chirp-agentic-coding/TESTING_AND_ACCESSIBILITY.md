# Testing & Accessibility

## Post-Experiment Hardening

Automated tests were intentionally excluded from the first Register implementation so that the initial experiment could focus on production implementation and architectural behavior.

A separate post-experiment phase was then used for test-infrastructure analysis, pure unit tests, ViewModel tests, deterministic networking tests, coverage, shared Compose UI tests and accessibility/semantics review.

## Test Areas

### Domain / Pure Unit Tests

Validation logic was tested directly, including the Register input rules and expected validity behavior.

### RegisterViewModel

Tests cover state transitions and Register behavior at presentation level.

### Ktor Integration

Networking behavior was tested using Ktor `MockEngine`, providing deterministic verification without relying on the live backend.

### Coverage

JVM coverage reporting was added for the relevant tested modules. Coverage was treated as an engineering signal, not as a substitute for meaningful assertions.

### Shared Compose UI Tests

Register UI tests were moved into shared `commonUiTest` so the same behavioral contracts could be exercised across Android and iOS test targets.

The final shared UI suite contained 14 Register UI tests and passed on both platforms during validation.

## Semantics-Driven Test Finders

The original UI tests located editable fields by position.

After the accessibility work, the fields gained stable semantic identities.

The final finder uses the actual semantic contract:

```kotlin
onNode(
    hasSetTextAction() and
    hasContentDescriptionExactly(title)
)
```

This avoids index-based selectors and avoids adding production `testTag`s solely for the tests.

## Accessibility Review

### Field labels

The visible field titles were originally separate from the editable nodes.

After typing, the placeholder disappeared and the field could lose its meaningful accessible name.

The fix associated the title with the editable node. The separate visual title remains drawn but is removed from the accessibility tree to prevent duplicate announcements.

### Field errors

Errors are represented semantically on the editable field rather than only by color and visible supporting text.

### Registration error

The screen-level registration error uses a polite live region on Android so TalkBack can announce it when it appears without moving focus.

### Password toggle

The Show/Hide password affordance is exposed as a button rather than as an image. The icon itself is decorative inside the actionable control.

### Loading state

Hidden progress indicators are no longer left in the semantics tree when a button is not loading.

### Heading

The main authentication screen title is marked as a semantic heading.

## Android TalkBack Validation

The field-label solution was validated with TalkBack.

A filled Username field retained both the accessible field identity and the editable value.

The appearing registration error was also verified with polite live-region behavior.

## iOS Accessibility Assessment

Compose Multiplatform exposed the expected semantic structure on iOS, but two platform limitations were documented.

### Editable value

With the selected field-label strategy, the UIKit accessibility bridge exposes the title as the accessibility label, but the typed editable value is not exposed as a separate accessibility value in the same way as Android.

A possible iOS-only bridge was investigated but intentionally deferred.

Password clear text must never be exposed through a custom value/state semantic workaround.

### Live-region announcements

The common Compose live-region semantic maps to an iOS accessibility trait but does not automatically post a native announcement notification.

A native iOS announcement bridge was deferred until it can be validated with VoiceOver on a physical device.

## Validation Summary

The completed hardening phase validated:

- shared UI tests on Android;
- shared UI tests on iOS simulator target;
- JVM unit tests;
- iOS unit tests;
- coverage tasks;
- Android assembly;
- semantic contracts;
- Android TalkBack behavior;
- iOS accessibility-node mapping.

The remaining iOS gaps are documented platform limitations rather than untracked defects.
