# MCP Client

An asynchronous CLI client for experimenting with the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro) using OpenAI's Responses API. The client connects to any MCP server exposed over stdio, lists the tools it offers, and brokers an interactive chat session where models can call those tools on demand.

## Requirements
- Python 3.12 or newer
- An MCP-compliant server executable (`.py` or `.js`) that can run over stdio
- An OpenAI API key with access to the `gpt-4.1-mini` model (set in your environment)

## Installation
This project uses `pyproject.toml` and ships with an `uv.lock` file. Pick the workflow that matches your toolchain:

### Using uv (recommended)
```bash
uv sync
```
This resolves dependencies and creates a virtual environment in `.venv/`.

### Using pip
```bash
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

## Configuration
Environment variables are loaded automatically from a local `.env` file. At minimum, set:

```bash
OPENAI_API_KEY=your_openai_key
```

Add any other variables your target MCP server requires.

## Running the client
Launch the client by pointing it at a server script:

```bash
python client.py path/to/server_script.py
```

- The script extension determines whether the client spawns it with `python` or `node`.
- On startup the client lists the tools the server exposes.
- Type questions or instructions to drive the conversation; enter `quit` to exit.

Behind the scenes the client passes your prompt, plus the server's tool definitions, to OpenAI's Responses API. When the model asks to call a tool, the client executes it through the connected MCP session and feeds the result back into the conversation.

## Troubleshooting
- **Server does not start**: confirm the path is correct and that the script is executable with the selected runtime.
- **Authentication errors**: re-check `OPENAI_API_KEY` in your environment or `.env` file.
- **No tools listed**: ensure the server implements the MCP `list_tools` handler.