```markdown
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
```

---

## 2. Requirements (one‑time setup)

You only need to install these once.

1. **Python 3.10 or newer**  
   - Download from: https://www.python.org/downloads [web:20]  
   - During installation on Windows, tick **“Add Python to PATH”**.

2. **pip** (Python’s package manager)  
   - Usually installed automatically with Python.  
   - To check, open *Command Prompt* or *PowerShell* and run:
     ```bash
     python -m pip --version
     ```

---

## 3. Getting the code

1. Open the GitHub repo in your browser:  
   `https://github.com/RzLetsCode/DATASCIENCE-AI/tree/main/mcp-server` [web:21]  
2. Click the green **Code** button → **Download ZIP**.  
3. Unzip it somewhere easy to find, for example:  
   `C:\Users\Lenovo\Downloads\mcp-server-demo-master\mcp-server`  

The folder should contain files like:

- `main.py`  
- `mcp_server.py`  
- `mocked_db.py`  
- `requirements.txt`  

---

## 4. Installing the dependencies

1. Open **Command Prompt** (or *Windows Terminal*).  
2. Go to your project folder (edit the path if yours is different):

```bash
cd "C:\Users\Lenovo\Downloads\mcp-server-demo-master\mcp-server"
```

3. Install all required Python libraries:

```bash
python -m pip install -r requirements.txt
```

This installs FastAPI, Uvicorn, and any other packages used by the server. [web:19]

---

## 5. Starting the server

In the same terminal, run:

```bash
python -m main
```

or:

```bash
python main.py
```

- The server should start on **port 8000**.  
- You will see log messages showing that Uvicorn/FastAPI is running. [web:23]  
- Keep this window open while you test.

To stop the server, press **Ctrl + C**.

---

## 6. Testing with curl (example call)

Open a **new** Command Prompt window (do not close the one running the server) and run:

```bash
curl -X POST http://127.0.0.1:8000/call ^
  -H "Content-Type: application/json" ^
  -d "{\"name\":\"get_documentation_from_database\",\"args\":{\"query\":\"MCP\",\"limit\":2}}"
```

What this means:

- `name`: which function to call → `get_documentation_from_database`.  
- `query`: what you are searching for → here `"MCP"`.  
- `limit`: how many results to return → here `2`.  

You should see a JSON response similar to the example at the top of this file.

If you are on macOS or Linux, use this version instead:

```bash
curl -X POST http://127.0.0.1:8000/call \
  -H "Content-Type: application/json" \
  -d '{"name":"get_documentation_from_database","args":{"query":"MCP","limit":2}}'
```

---

## 7. Common problems and fixes

- **`python` is not recognized**  
  - Python is not installed or not added to PATH.  
  - Reinstall Python and make sure “Add Python to PATH” is selected.

- **Port 8000 already in use**  
  - Another program is using port 8000.  
  - Open `main.py` (or wherever `uvicorn.run` is called) and change:
    ```python
    uvicorn.run(app, host="0.0.0.0", port=8001)
    ```
  - Start the server again and call `http://127.0.0.1:8001/call` instead.

- **Missing library (ImportError)**  
  - Make sure `pip install -r requirements.txt` ran successfully.  
  - If you have multiple Python versions, try:
    ```bash
    py -m pip install -r requirements.txt
    py -m main
    ```

---

## 8. Where this fits in MCP

This simple HTTP server can be used as a backend tool in a Model Context Protocol (MCP) setup, where an MCP host (like an AI IDE or assistant) calls `/call` with `get_documentation_from_database` to fetch documentation snippets. [web:20][web:23]  

You can later connect this to real databases, vector stores, or other systems as described in the official MCP server guides. [web:26]
```
