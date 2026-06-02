# <details>: The Details disclosure element - HTML | MDN
Source: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/details
Captured: 2026-05-30 | Action: reference-only

## Summary
The <details> element creates a collapsible section with a header defined by <summary>, defaulting to closed. It can be opened via the open attribute, user interaction, or programmatically, and supports CSS styling for open/closed states with the toggle event for state changes.

## Key Points
- Default closed state; open via presence of open attribute or user interaction
- <summary> defines header text and is styled as list-item by default
- name attribute groups elements for accordion behavior (only one open at a time)
- CSS :open pseudo-class or [open] attribute selector styles open state
- toggle event tracks state changes between open/closed
