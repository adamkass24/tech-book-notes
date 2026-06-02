# WebSocket - Web APIs | MDN
Source: https://developer.mozilla.org/en-US/docs/Web/API/WebSocket
Captured: 2026-05-30 | Action: reference-only

## Summary
The WebSocket API enables bidirectional, real-time communication between client and server using a persistent connection. It requires the WebSocket() constructor but lacks built-in backpressure, risking memory overflow or CPU overload during high message throughput.

## Key Points
- WebSocket() constructor initializes persistent connections for real-time data exchange
- No native backpressure mechanism can lead to memory/CPU issues
- WebSocketStream offers a backpressure-aware alternative
