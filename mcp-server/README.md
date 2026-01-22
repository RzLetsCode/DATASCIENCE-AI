# MCP Server – Simple Mock Documentation API

This project is a very small web server that returns fake documentation from a “mock database”.  
It is ideal for learning, demos, or testing how an MCP-style tool call might work. [web:20]

---

## 1. What this project does

- Starts a small web server on your computer (default: http://127.0.0.1:8000).  
- Exposes one main function: `get_documentation_from_database`.  
- When you send a request with a search word (for example `MCP`), it sends back matching documentation items as JSON.

Example response:

```json
{
  "name": "get_documentation_from_database",
  "result": [
    {
      "id": 3,
      "title": "MCP Server Quickstart",
      "content": "Examples for implementing an MCP server endpoint and registering functions.",
      "source": "internal/docs/mcp_quickstart.md"
    },
    {
      "id": 5,
      "title": "Advanced Integration Patterns",
      "content": "How to extend the MCP server for custom function routing and storage.",
      "source": "internal/docs/advanced.md"
    }
  ]
}


## 2. Requirements (one‑time setup with uv)

You only need to install these once.

1. **uv – Python package & project manager**  
   - uv is an extremely fast Python package and project manager written in Rust. [web:29][web:35]  
   - Install uv from the official docs: https://docs.astral.sh/uv/getting-started/installation/ [web:29]  
   - On Windows (PowerShell), for example:
     ```powershell
     irm https://astral.sh/uv/install.ps1 | iex
     ```

2. **Python (managed by uv)**  
   - uv can install and manage Python versions for you. [web:31][web:33]  
   - After installing uv, in a terminal run (example for Python 3.11):
     ```bash
     uv python install 3.11
     ```
   - uv will automatically use the right Python version for the project via its environment. [web:31][web:33]

You do **not** need to run `python -m venv` or `pip install` manually; uv will handle environments and dependencies. [web:35][web:36]

---

## 3. Getting the code

1. Open the GitHub repo in your browser:  
   `https://github.com/RzLetsCode/DATASCIENCE-AI/tree/main/mcp-server` [web:21]  
2. Click the green **Code** button → **Download ZIP** (or clone with Git).  
3. Place or extract the project somewhere easy to find, for example:  
   `C:\Users\Lenovo\Downloads\mcp-server-demo-master\mcp-server`  

The folder should contain files like:

- `main.py`  
- `mcp_server.py`  
- `mocked_db.py`  
- `pyproject.toml` or `requirements.txt` (depending on how the project was created)  

---

## 4. Setting up the project with uv

1. Open **Command Prompt**, *PowerShell*, or a terminal.  
2. Go to your project folder (adjust the path if different):

```bash
cd "C:\Users\Lenovo\Downloads\mcp-server-demo-master\mcp-server"
```

3. If the project already has a `pyproject.toml` and `uv.lock`, simply sync dependencies:

```bash
uv sync
```

- This will create a virtual environment for the project and install all needed packages in one step. [web:33][web:35]

4. If instead you only have a `requirements.txt`, you can install using the pip‑compatible interface:

```bash
uv pip install -r requirements.txt
```

- uv acts as a drop‑in replacement for `pip install` but much faster. [web:35][web:36]

---

## 5. Starting the server (using uv)

From the project directory:

```bash
uv run python -m main
```

or, if `main.py` is the entry point:

```bash
uv run python main.py
```

- `uv run` makes sure the correct environment and Python version are used automatically. [web:33][web:35]  
- The server should start on **port 8000** and show FastAPI / Uvicorn logs. [web:23]  
- Keep this window open while you test.

To stop the server, press **Ctrl + C**.

---

## 6. Testing with curl (example call)

Open a **new** Command Prompt or terminal window (do not close the one running the server) and run:

### On Windows (PowerShell / CMD)

```bash
curl -X POST http://127.0.0.1:8000/call ^
  -H "Content-Type: application/json" ^
  -d "{\"name\":\"get_documentation_from_database\",\"args\":{\"query\":\"MCP\",\"limit\":2}}"
```

### On macOS / Linux

```bash
curl -X POST http://127.0.0.1:8000/call \
  -H "Content-Type: application/json" \
  -d '{"name":"get_documentation_from_database","args":{"query":"MCP","limit":2}}'
```

What the fields mean:

- `name`: which function to call → `get_documentation_from_database`.  
- `args.query`: what you are searching for → here `"MCP"`.  
- `args.limit`: how many results to return → here `2`.  

You should see a JSON response similar to the example in section 1.

---

## 7. Common problems and fixes (uv‑specific)

- **uv command not found**  
  - Installation might have failed or terminal was opened before installation.  
  - Re-run the installer from the uv docs and then open a fresh terminal. [web:29][web:35]

- **Wrong or missing Python version**  
  - Install a compatible version via uv:
    ```bash
    uv python install 3.11
    uv python list
    ```
  - uv will automatically use the correct version when you `uv sync` and `uv run`. [web:31][web:33]

- **Dependencies not installed / ImportError**  
  - Make sure you ran:
    ```bash
    uv sync
    ```
    or, for `requirements.txt`:
    ```bash
    uv pip install -r requirements.txt
    ```
  - Then re-run the server:
    ```bash
    uv run python -m main
    ```

- **Port 8000 already in use**  
  - Another process is listening on port 8000.  
  - Edit `main.py` (where `uvicorn.run` is called) and change:
    ```python
    uvicorn.run(app, host="0.0.0.0", port=8001)
    ```
  - Then start the server again and call `http://127.0.0.1:8001/call`.

---

## 8. Where this fits in MCP

This simple HTTP server can be used as a backend tool in a Model Context Protocol (MCP) setup, where an MCP host (like an AI IDE or assistant) calls `/call` with `get_documentation_from_database` to fetch documentation snippets for the user. [web:20][web:23]  

You can later connect this to real databases, vector stores, or other systems, and still manage the project with uv for fast, reproducible environments and dependency management. [web:26][web:33][web:35]
```
