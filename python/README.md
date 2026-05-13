# InsightDesk — Python backend

Local API and agents for the **InsightDesk** ENMGT 5400 prototype. The installable package name remains **`valuecell`** so imports stay stable (`import valuecell`).

## Run (development)

From `python/`:

```bash
uv run python -m valuecell.server.main
```

`python/valuecell/server/main.py` skips the interactive stdin control thread when stdin is not a TTY so Uvicorn keeps running under CI, background launch, or IDEs.

Architecture notes: [../docs/CORE_ARCHITECTURE.md](../docs/CORE_ARCHITECTURE.md)
