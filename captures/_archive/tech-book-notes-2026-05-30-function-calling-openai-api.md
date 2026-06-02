# Function calling | OpenAI API
Source: https://developers.openai.com/api/docs/guides/function-calling
Captured: 2026-05-30 | Action: reference-only

## Summary
Function calling allows OpenAI models to interact with external systems via predefined tools (defined by JSON schemas), following a multi-step flow: model requests tool, application executes, returns output, and model generates final response. Supports tool search for deferred loading of infrequently used tools (GPT-5.4+).

## Key Points
- Tools are defined using JSON schemas (functions) or free-text custom tools; namespaces organize related tools (e.g., crm, billing).
- Flow: API request with tools → model tool call → application executes tool → tool output → final model response.
- Best practices: clear descriptions, strict mode, avoid token bloat, use tool search for large toolsets.
