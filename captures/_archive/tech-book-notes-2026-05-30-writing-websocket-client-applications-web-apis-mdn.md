# Writing WebSocket client applications - Web APIs | MDN
Source: https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications
Captured: 2026-05-30 | Action: reference-only

## Summary
The WebSocket API enables bidirectional communication with a server using a persistent connection. Key steps include creating a secure WebSocket instance (wss), handling events like 'open', 'message', and 'close', and managing connection lifecycle to ensure compatibility with browser back/forward cache (bfcache).

## Key Points
- Use wss (secure) for production instead of ws (insecure); browsers enforce secure connections for HTTPS pages.
- Critical events: 'open' (start sending), 'message' (receive data), 'error' (handle failures), 'close' (cleanup).
- Close connections on 'pagehide' and reconnect on 'pageshow' to support bfcache without data loss.
- Send text (JSON) or binary (ArrayBuffer/Blob); track buffer status via `bufferedAmount`.
