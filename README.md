# TestForge v1.0.1 — Installation Guide

## What is TestForge?

An interactive installer that sets up a **complete, production-ready test automation 
framework** in one run — virtual environments, packages, folder structure, sample 
tests, CI/CD config, README, and uninstaller.

---

## Binaries in this ZIP

| File | Platform |
|------|----------|
| `TestForge.exe` | Windows x64 (Intel/AMD — most PCs) |
| `TestForge-windows-arm64.exe` | Windows ARM64 (Surface Pro X, Copilot+ PCs) |
| `TestForge-mac-arm64` | macOS Apple Silicon (M1/M2/M3/M4) |
| `TestForge-mac-intel` | macOS Intel |
| `TestForge-linux` | Linux x64 |

---

## Windows Quick Start

1. Extract this ZIP somewhere (e.g. `C:\Tools\TestForge\`)
2. Double-click `TestForge.exe`  
   *(or open Terminal/cmd → `.\TestForge.exe`)*
3. Follow the prompts

> **SmartScreen warning?** Click **More info → Run anyway**.  
> The source code is included in `src/` — nothing hidden.

---

## macOS Quick Start

```bash
# Apple Silicon (M1/M2/M3/M4)
chmod +x TestForge-mac-arm64 && ./TestForge-mac-arm64

# Intel Mac
chmod +x TestForge-mac-intel && ./TestForge-mac-intel
```

> Gatekeeper quarantine? Run first:  
> `xattr -d com.apple.quarantine TestForge-mac-arm64`

---

## Linux Quick Start

```bash
chmod +x TestForge-linux && ./TestForge-linux
```

---

## What the wizard asks

1. **Project name** — e.g. `my-api-tests`
2. **Install location** — e.g. `C:\Projects` or `/home/user/projects`
3. **What to automate** — REST API / UI Browser / Both
4. **Language** — Python or Go (API), Python or JavaScript (UI)
5. **Framework** — requests/httpx/resty, Selenium/Playwright/Cypress
6. **Confirm** — review table before anything is installed

---

## What gets created

```
my-project/
├── tests/               ← working sample tests (API or UI)
├── pages/               ← Page Object Model (UI only)
├── fixtures/            ← JSON test data
├── config/              ← settings.py with .env support
├── utils/               ← APIHelper, screenshot helpers
├── reports/             ← HTML reports generated here
├── pytest.ini           ← CRITICAL: sets pythonpath=. so imports work
├── conftest.py          ← pytest fixtures + sys.path fallback
├── .venv/               ← isolated Python venv (auto-created)
├── requirements.txt
├── Makefile
├── .github/workflows/test.yml
├── .gitignore
├── README.md            ← generated from your choices
├── uninstall.bat        ← Windows uninstaller
└── uninstall.sh         ← macOS/Linux uninstaller
```

For "Both" mode: `api-tests/` and `ui-tests/` subfolders with the above in each.

---

## Running tests

```cmd
cd my-project

:: Python projects — ALWAYS use pytest, not python directly:
.venv\Scripts\pytest tests\ -v

:: With HTML report:
.venv\Scripts\pytest tests\ -v --html=reports\report.html

:: Or via Makefile (if Make is installed):
make test
```

> **Common mistake:** Running `python tests\test_ui.py` directly causes  
> `ModuleNotFoundError: No module named 'pages'`  
> Use `pytest tests\test_ui.py -v` instead — pytest sets up the path.

---

## Uninstalling

**Windows:**
```
Double-click  uninstall.bat  inside the project folder
```

**macOS / Linux:**
```bash
cd my-project
./uninstall.sh
```

Both uninstallers will:
- Ask for confirmation before doing anything
- Remove `.venv` / `node_modules`
- Clear `__pycache__` and `.pytest_cache`
- Delete the entire project folder

---

## Prerequisites (auto-checked by TestForge)

| Need | Install from | Note |
|------|-------------|------|
| Python 3.8+ | https://python.org/downloads/ | **Tick "Add Python to PATH"** during install |
| Node.js 18+ LTS | https://nodejs.org/en/download | Only for Cypress or Playwright JS |
| Go 1.20+ | https://go.dev/dl/ | Only for Go API tests |

TestForge checks and shows links for anything missing. It also prompts you to 
fix prerequisites and re-checks before proceeding.

---

## Troubleshooting

**"Python not found" even though I installed it**  
→ Restart your terminal after installing. The PATH update needs a fresh session.

**ModuleNotFoundError: No module named 'pages'**  
→ You're running `python tests\test_ui.py` directly.  
→ Use `pytest tests\test_ui.py -v` instead. Or activate venv first then use pytest.

**pip install fails with "access denied"**  
→ TestForge creates an isolated `.venv` — no admin needed. Try running as admin 
   if your company IT has locked down user folders.

**Playwright browser download is slow**  
→ ~130MB one-time download. Only happens on first setup.

**"make" not found on Windows**  
→ Install from https://gnuwin32.sourceforge.net/packages/make.htm  
→ Or run the pytest/npx commands directly from the project folder.

---

## Building from source

```bash
cd src/
go build -ldflags="-s -w" -o TestForge.exe ./main.go        # Windows
go build -ldflags="-s -w" -o TestForge     ./main.go        # macOS/Linux
```

No external Go dependencies — pure standard library.
