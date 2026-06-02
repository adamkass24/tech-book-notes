# Using CSS custom properties (variables) - CSS | MDN
Source: https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascading_variables/Using_custom_properties
Captured: 2026-05-30 | Action: reference-only

## Summary
CSS custom properties (variables) allow reusable values defined with -- prefixes, scoped by selectors and referenced via var(). The @property at-rule enables type validation, initial values, and inheritance control, improving maintainability and reducing repetition in stylesheets.

## Key Points
- Custom properties declared with -- are case-sensitive and scoped to their selector (e.g., :root for global access).
- @property at-rule defines syntax, inheritance behavior, and initial values for type-safe variables.
- var() supports fallback values (e.g., var(--color, #000)) and handles invalid values via initial-value or defaults.
- Variables resolve at use time, not storage, differing from programming variables.
