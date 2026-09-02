# MCP Chat

MCP Chat is a command-line interface application that enables interactive chat capabilities with AI models through the Anthropic API. The application supports document retrieval, command-based prompts, and extensible tool integrations via the MCP (Model Control Protocol) architecture.

## Prerequisites

- Python 3.10+ (as declared in `pyproject.toml`)
- Anthropic API Key

## Setup

### Step 1: Configure the environment variables

1. Create or edit the `.env` file in the project root and verify that the following variables are set correctly:

```
ANTHROPIC_API_KEY=""    # Enter your Anthropic API secret key
CLAUDE_MODEL=""         # Model id, e.g. claude-sonnet-4-5
ANTHROPIC_WORKSPACE_ID="" # Only for identity-linked keys; see below
USE_UV=1                # 1 to launch MCP servers via uv, 0 to use python
```

If requests fail with `anthropic-workspace-id is required when authenticating
with an identity-linked API key`, set `ANTHROPIC_WORKSPACE_ID` to the workspace
the key belongs to (Anthropic Console -> Settings -> Workspaces; the id looks
like `wrkspc_...`). Leave it blank for ordinary API keys.

### Step 2: Install dependencies

#### Option 1: Setup with uv (Recommended)

[uv](https://github.com/astral-sh/uv) is a fast Python package installer and resolver.

1. Install uv, if not already installed:

```bash
pip install uv
```

2. Create and activate a virtual environment:

```bash
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install dependencies:

```bash
uv pip install -e .
```

4. Run the project

```bash
uv run main.py
```

#### Option 2: Setup without uv

1. Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

2. Install dependencies:

```bash
pip install anthropic python-dotenv prompt-toolkit "mcp[cli]==1.8.0"
```

3. Run the project

```bash
python main.py
```

## Usage

### Basic Interaction

Simply type your message and press Enter to chat with the model.

### Document Retrieval

Use the @ symbol followed by a document ID to include document content in your query:

```
> Tell me about @deposition.md
```

### Commands

Use the / prefix to execute commands defined in the MCP server:

```
> /summarize deposition.md
> /format report.pdf
```

Commands will auto-complete when you press Tab.

## Development

### Adding New Documents

Edit the `mcp_server.py` file to add new documents to the `docs` dictionary.

### MCP Features

The server exposes two tools (`read_doc_contents`, `edit_document`), two
resources (`docs://documents`, `docs://documents/{doc_id}`), and two prompts
(`/format`, `/summarize`). `mcp_client.py` wraps all of these.

### Linting and Typing Check

There are no lint or type checks implemented.
