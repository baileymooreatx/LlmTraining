# Model Context Protocol (MCP)

The Model Context Protocol (MCP) is an open standard introduced by 
[Anthropic](https://www.anthropic.com/) in November 2024 that standardizes how 
AI applications connect to external tools, data sources, and services. Think of 
it as a **USB-C port for AI**, one universal connector instead of custom 
integrations for every pairing.

## Architecture

MCP uses a three-role client-server model:

- **Host** — the AI application (e.g., Claude Desktop, Cursor) that orchestrates
  the workflow and holds the LLM.
- **Client** — a connector the host creates for each server it needs; handles
  protocol mechanics like discovery and dispatch.
- **Server** — exposes capabilities to the client: tools, resources, and
  prompts. Servers can run locally or remotely.

All communication between client and server uses **JSON-RPC 2.0** messages,
transported over **stdio** (local, same-machine) or **Streamable HTTP** (remote,
network).

## What Servers Expose

| **Primitive**  | **Control**             | **Purpose**                                                                |
|:---------------|:------------------------|:---------------------------------------------------------------------------|
| Tools          | Model-controlled        | Callable functions (e.g., API calls, file writes, DB queries)              |
| Resources      | Application-controlled  | Readable data injected as context (files, records, documents)              |
| Prompts        | User-controlled         | Reusable templates or instructions (slash commands, predefined workflows)  |

## How It Works (Typical Flow)

1. **Discovery** — the client asks the server what tools/resources/prompts it
   offers; the server returns names, descriptions, and input schemas.
2. **Invocation** — the LLM selects a tool and the host sends a structured
   JSON-RPC request to the server.
3. **Execution** — the server performs the action and returns a structured
   result.
4. **Response** — the host feeds the result back into the LLM conversation.

## Why It Matters

Before MCP, every AI app needed a custom connector for every data source, the
so-called **N×M integration problem**. MCP reduces this to **N+M**: each server
implements the protocol once, each client implements it once, and they
interoperate automatically. The protocol has been adopted by 
[OpenAI](https://openai.com/api/), [Google DeepMind](https://deepmind.google/), 
[Microsoft](https://www.microsoft.com/en-us/copilot/blog/copilot-studio/model-context-protocol-mcp-is-now-generally-available-in-microsoft-copilot-studio/), 
and thousands of enterprise teams. Since December 2025, it is governed by the 
[Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) 
under the Linux Foundation.

A key distinction from traditional APIs: MCP supports **dynamic capability
discovery at runtime**. A client can query a server to learn what's 
available, rather than relying on hardcoded endpoints.
