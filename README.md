# MundaForge

> An AI coding agent that runs entirely on your own infrastructure — no cloud dependency, no data exposure.

![Status](https://img.shields.io/badge/status-beta-yellow)

## Overview

MundaForge is a terminal-based AI coding assistant built for developers and teams who cannot or will not send source code to third-party APIs. It orchestrates one or more AI models — local or hosted — to autonomously read, write, and reason about code, while keeping every token inside the operator's own environment. It was designed from the ground up to run on heterogeneous hardware, from a powerful workstation down to a Raspberry Pi acting as a thin client.

## Key Capabilities

- **Fully private by default** — all model inference runs on the operator's own hardware; no source code, prompts, or outputs leave the network unless explicitly configured otherwise
- **Multi-model parallel execution** — launch several AI agents simultaneously, each with a different model and a different task, and watch them work side by side in a split-pane interface
- **Flexible AI provider support** — switch between locally hosted models and major cloud providers (OpenAI, OpenRouter, DeepSeek, and others) per session, with no code changes
- **Persistent, context-aware sessions** — each project directory retains its own conversation history across restarts; the agent picks up exactly where it left off
- **Automatic context compression** — large tool outputs (file reads, test logs, search results) are compressed before reaching the model, reducing token consumption by 60–95% and enabling much longer working sessions
- **Configurable safety controls** — destructive operations (file writes, shell commands, git commits) require explicit confirmation by default; a permissive mode is available for trusted automation contexts
- **Extensible skill system** — reusable, Markdown-defined instruction sets can be scoped per user or per project, allowing teams to encode domain knowledge the agent applies automatically

## Tech Highlights

| Layer | Technology |
|-------|------------|
| Language | Python |
| Interface | Terminal UI (interactive, keyboard-driven) |
| AI runtime | Local models via Ollama; cloud via OpenAI-compatible APIs |
| Context management | Automatic compression with ML-based summarisation |
| Shell integration | Persistent shell sessions (state survives between agent turns) |
| Hardware targets | x86_64 workstations and ARM64 single-board computers |
| Test coverage | 166 automated tests across core agent logic, tools, and UI |

## Screenshots

> *Screenshots available on request.*

## Status & Availability

MundaForge is in active beta. Core capabilities — autonomous file editing, shell execution, git operations, multi-model swarm mode, session continuity, and context compression — are fully implemented and covered by automated tests. The project is in daily use as a private development tool and is being refined toward a stable v1.0 release. Licensing for commercial deployment or white-label adaptation is available on request.

## Interested?

This is a proprietary project by **Munda Plus d.o.o.**
The full codebase is available for review upon request.

📧 marko@munda.si
🌐 [munda.si](https://www.munda.si)
