# ValueCell (ENMGT 5400 Course Demo)

**ValueCell** is a local course demo for market data, multi-agent research, and strategy workflows. This version is prepared for the **Cornell ENMGT 5400** presentation and runs on your own machine by default.

## Run

**macOS / Linux**

```bash
bash start.sh
```

- **Local Web UI:** [http://localhost:1420](http://localhost:1420)  
- **Local API:** [http://localhost:8000](http://localhost:8000)  

> These links work only after the app is running locally with `bash start.sh`.

**Windows (PowerShell):** `.\start.ps1`

Prerequisites: `bun` and `uv` (the script can install them on macOS via Homebrew). Environment files follow the layout under the system app config path (see `docs/CONFIGURATION_GUIDE.md` if you add API keys).

## Optional environment (frontend)

Copy `frontend/.env.example` to `frontend/.env` and adjust as needed. For the **classroom build**, leave the optional cloud variables empty so the app does not call external SaaS auth or analytics. Set `VITE_AUTH_LOGIN_BASE_URL` and `VITE_REMOTE_SAAS_API_BASE` only if you wire your own services.

## Configuration

Model providers, keys, and data sources: [docs/CONFIGURATION_GUIDE.md](docs/CONFIGURATION_GUIDE.md)

## License and attribution

- This tree includes code under the **Apache License 2.0**; see [LICENSE](LICENSE).  
- Third-party services (LLM APIs, exchanges, embedded charts) remain subject to their own terms.  
- The Python package directory is named `valuecell` and is kept that way for import compatibility.

## 中文说明

简版与运行说明见 [README.zh.md](README.zh.md)。
