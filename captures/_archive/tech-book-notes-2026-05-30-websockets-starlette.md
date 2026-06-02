# Websockets - Starlette
Source: https://starlette.dev/websockets/
Captured: 2026-05-30 | Action: reference-only

## Summary
Starlette provides a WebSocket class for bidirectional communication, enabling connection handling (accept/close), data exchange (text/bytes/JSON), and access to request details like URL components, headers, and query parameters.

## Key Points
- WebSocket class with methods for connection management (accept, close) and data transfer (send_text, receive_text, send_json).
- Access to URL components (websocket.url.path), headers (case-insensitive multi-dict), and query parameters via immutable interfaces.
- Subprotocol validation support and HTTPException for authorization; JSON messages default to text frames (binary mode available).
