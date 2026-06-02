# Prompt caching - Claude API Docs
Source: https://platform.claude.com/docs/en/build-with-claude/prompt-caching
Captured: 2026-05-30 | Action: reference-only

## Summary
Prompt caching in Claude API reduces processing time and costs by reusing cached prompt prefixes, with automatic caching for multi-turn conversations and explicit breakpoints for fine-grained control. It introduces new pricing tiers (5m/1h TTL) and requires careful prompt structuring to avoid cache invalidation.

## Key Points
- Automatic caching uses a top-level `cache_control` to cache growing conversation history; explicit caching applies to specific content blocks.
- Cache writes cost 1.25x (5m TTL) or 2x (1h TTL) base input tokens; reads cost 0.1x base.
- Cache invalidation occurs for changes to tools, system messages, or message content; minimum 1k-4k tokens required per model.
- Cache performance tracked via `cache_read_input_tokens` and `cache_creation_input_tokens` in API responses.
