# MundaForge

**A terminal AI coding agent that runs against your own model server.** MundaForge reads, writes and runs code in a project directory using models served by Ollama, llama.cpp or llama-swap, so source code and prompts stay on your own network.

It is a personal alternative to Claude Code: a Textual TUI plus a scriptable print mode, with Claude-Code-compatible skills, hooks and MCP servers. It is built for mixed hardware, with a workstation serving the model and a lighter machine such as a Raspberry Pi 5 running the client. Hosted OpenAI-compatible providers (OpenAI, OpenRouter, DeepSeek) can be selected per session when wanted.

<!-- screenshots -->

## Features

- **Terminal UI.** Chat view, sidebar, a four-row status bar (model, branch, mode, context usage, compression stats, tool-call tally, active goal) and a command palette.
- **Goal mode.** `/goal <description>` breaks a task into subtasks and works through them one by one. Progress is saved after each subtask, so an interrupted run can resume, and the agent can queue questions for you to answer later instead of stopping.
- **Plan mode.** A read-only mode for exploring and planning; `shift+tab` switches to editing.
- **Swarm mode.** Several agents, each with its own model and task, run in parallel in a split-pane view.
- **Undo and redo per turn.** Every file the agent writes is checkpointed, grouped by turn, so `/undo` takes back exactly what the agent changed without touching your own uncommitted work.
- **Checks after every write.** After writing a file, MundaForge runs the project's own checker for that language (ruff, tsc, go vet, shellcheck and similar) and feeds errors back so the agent fixes them in the same turn.
- **Context compression.** Tool output such as file reads, test logs and search results is compressed in-process with Headroom before it reaches the model: ML-based on x86_64, structural on ARM64.
- **Code search.** Text search plus semantic search over a local index. Python is chunked one definition at a time, and dense and keyword results are fused.
- **Sessions per directory.** Each project directory resumes its previous conversation automatically; sessions can be searched, rewound and forked.
- **Persistent shell.** Shell commands run in a tmux session per agent, which keeps state (working directory, environment, running processes) between agent turns.
- **Skills, hooks and MCP.** Markdown skills in Claude Code's format, shell hooks around tool calls and session boundaries (subject to the same safety checks), and MCP servers configured in the same JSON shape as Claude Code. Built-in skills cover git workflow, code review, Playwright/Cypress E2E testing, a sigma.js site map of a web app's routes, user documentation, and upgrading MundaForge itself.
- **Model awareness.** `mundaforge models` lists installed models with size, RAM warnings and what is currently loaded. The model picker marks which models actually emit tool calls, and per-model speed is logged from real sessions.
- **Print mode for scripts.** `-p` runs one task, prints the answer and exits with a meaningful code (done, failed, needed a human), with plain, JSON or streamed JSON output.
- **Explicit safety.** Shell commands ask for confirmation by default. A YOLO mode skips routine confirmations, but destructive commands (`rm`, `dd`, `mkfs` and similar) are always confirmed, and safety settings can't be overridden by a project's config.

## Tech stack

Python · Textual · Click · httpx · Headroom · Ollama · llama.cpp / llama-swap · OpenAI-compatible APIs · MCP · tmux · pytest

## How it works

```
  client (workstation or Raspberry Pi 5)         model server
  ┌───────────────────────────────────┐        ┌──────────────────┐
  │ TUI / print mode                  │        │ Ollama, llama.cpp│
  │   agent loop ── tool registry     │ ─────► │ or llama-swap    │
  │     filesystem, shell, git,       │  chat  └──────────────────┘
  │     search, tasks, MCP tools      │
  │   headroom.compress() on output   │
  │   checkpoints · sessions · index  │
  └───────────────────────────────────┘
```

The agent loop sends the conversation to the model, runs the tool calls it gets back, compresses each tool result and appends it to the history. Configuration is layered: package defaults, then user config, then project config (excluding safety settings), then environment variables. `/init` writes an `AGENTS.md` for the repository that later sessions load, and cross-session corrections accumulate there.

## Design principles

- **Your server, your data.** The default provider is a local model server; hosted providers are opt-in per session.
- **Measure before changing.** Retrieval, chunking and recommendation thresholds are tuned against logged sessions and recorded measurements, and the roadmap records the evidence behind each decision.
- **Errors the model can act on.** A mistyped tool argument gets a reply listing the arguments the tool accepts, not a Python traceback.
- **Destructive means confirmed.** No flag removes the confirmation for destructive commands.

## Availability

The source code is not public. MundaForge is in beta and used daily as a private development tool. Available for licensing, custom deployment or white-label adaptation. Get in touch via [munda.si](https://www.munda.si/#contact).

## License

Proprietary. © 2026 MUNDA PLUS d.o.o. All rights reserved. See [LICENSE](LICENSE).

## Author

Built by [Marko Munda](https://www.munda.si/) · [Munda Plus](https://github.com/MundaPlus)
