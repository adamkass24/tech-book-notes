# First Steps - FastAPI
Source: https://fastapi.tiangolo.com/tutorial/first-steps/
Captured: 2026-05-30 | Action: reference-only

## Summary
The FastAPI 'first steps' tutorial demonstrates creating a minimal API with a single endpoint at / returning 'Hello World', running a development server, and accessing interactive OpenAPI documentation at /docs and /redoc. It emphasizes the use of path operation decorators like @app.get('/') and automatic schema generation via OpenAPI.

## Key Points
- Minimal FastAPI app requires importing FastAPI, creating an instance, and defining a path operation with @app.get('/')
- Interactive API docs at /docs (Swagger UI) and /redoc (ReDoc) are auto-generated using OpenAPI schema
- OpenAPI schema powers documentation and enables automatic client code generation for API consumers
