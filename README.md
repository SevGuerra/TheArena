# TheArena — MCP Agentic Playground for AI Workflows & Games

[![Releases](https://img.shields.io/github/v/release/SevGuerra/TheArena?label=Releases&color=0052CC)](https://github.com/SevGuerra/TheArena/releases)

[![ai-playground](https://img.shields.io/badge/ai--playground-blue)](https://github.com/SevGuerra/TheArena)
[![mcp](https://img.shields.io/badge/mcp-protocol-brightgreen)](https://github.com/SevGuerra/TheArena)
[![automation](https://img.shields.io/badge/automation-orange)](https://github.com/SevGuerra/TheArena)
[![games](https://img.shields.io/badge/games-purple)](https://github.com/SevGuerra/TheArena)
[![rpg-game](https://img.shields.io/badge/rpg--game-red)](https://github.com/SevGuerra/TheArena)
[![chess-game](https://img.shields.io/badge/chess--game-black)](https://github.com/SevGuerra/TheArena)
[![rubiks-cube](https://img.shields.io/badge/rubiks--cube-yellowgreen)](https://github.com/SevGuerra/TheArena)

Hero images
- RPG: ![RPG](https://upload.wikimedia.org/wikipedia/commons/3/3e/OOjs_UI_icon_game.svg)  
- Chess: ![Chess Knight](https://upload.wikimedia.org/wikipedia/commons/2/2e/Chess_knight.svg)  
- Rubik's Cube: ![Rubik](https://upload.wikimedia.org/wikipedia/commons/3/34/Rubik%27s_Cube.svg)

What TheArena is 🧩
- A unified playground for agentic AI tasks, benchmarking, and game-based experiments.
- A set of MCP (Model Context Protocol) servers. Each server implements a domain: agent flows, RPGs, chess, and Rubik’s Cube.
- A testbed for workflows that need multi-step agent coordination, environment state, and human-in-the-loop hooks.
- Built for integration with Claude and other LLMs that support the MCP model.

Why use TheArena
- Run consistent benchmarks across game-style tasks.
- Prototype agentic automation pipelines with persistent state.
- Test multi-agent interactions in controlled environments.
- Compare model behaviors inside game logic and scripted workflows.

Core components 🧱
- MCP core: message routing, session state, and context adapters.
- Agentic Flow Server: orchestrates agents, tasks, and resource guards.
- RPG Server: grid-based adventure engine with item, NPC, and quest systems.
- Chess Server: move validation, engines (UCI), and replay toolset.
- Rubik’s Cube Server: cube state, solver benchmarks, and scramble generator.
- Adapter layer: Claude adapter plus example connectors for other LLMs.

High-level architecture
- Clients talk MCP HTTP/WebSocket to servers.
- Servers store session and game state in embedded DB or Redis.
- Agent adapters convert MCP messages to provider-specific calls.
- Web UI and CLI clients let you run matches, workflows, and scenarios.

Quick demo images
- Flow diagram: ![Flow](https://upload.wikimedia.org/wikipedia/commons/8/87/Diagram-example.svg)
- Chess board preview: ![Chess Board](https://upload.wikimedia.org/wikipedia/commons/1/1b/Chessboard480.svg)

Getting started — local (Docker)
- Install Docker and Docker Compose.
- Clone the repo and change into it.
- Build and start services:
  - docker compose up --build
- The default ports:
  - Agentic Flow: 8001
  - RPG: 8002
  - Chess: 8003
  - Rubik’s Cube: 8004
- Open the Web UI at http://localhost:3000

Quickstart — manual (Linux / macOS)
1. Install Python 3.10+ and Node 18+.
2. Create a virtualenv and install server requirements:
   - python -m venv venv
   - source venv/bin/activate
   - pip install -r server/requirements.txt
3. Install web client dependencies:
   - cd web && npm ci
4. Start servers:
   - python server/flow_server.py
   - python server/rpg_server.py
   - python server/chess_server.py
   - python server/rubiks_server.py
5. Start the web client:
   - cd web && npm run dev

Releases — download and run the packaged build
- Visit the Releases page: https://github.com/SevGuerra/TheArena/releases  
- Download the release file and execute it on your host. The release contains a packaged build with prebuilt binaries, a run script, and Docker images. After download:
  - tar -xzf TheArena-<version>.tar.gz
  - cd TheArena-<version>
  - sudo ./run.sh
- If the release link does not work, check the "Releases" section on the repository page.

MCP basics (easy)
- MCP messages carry model context and action intents.
- A typical flow:
  1. Client starts a session.
  2. Server replies with context and possible actions.
  3. Client (agent) chooses an action and sends a command.
  4. Server updates state and returns the result.
- MCP keeps the model in context. It helps chain decisions across turns.

Agent flows — how to use
- Compose tasks in JSON or YAML.
- Each task declares:
  - inputs: required data fields.
  - agents: roles and policies.
  - steps: ordered actions.
- Agents can:
  - call external APIs,
  - modify world state,
  - spawn sub-tasks,
  - request human approval.

RPG Server — feature list 🎲
- Tile-based maps with custom level loaders.
- NPC AI hooks using MCP messages.
- Quest templates and rewards.
- Replay logs for deterministic testing.
- Scenario player to run reproducible benchmarks.

Chess Server — feature list ♟️
- FEN import/export and PGN export.
- Legal move validation and checkmate detection.
- Integration with local UCI engines.
- Match runner for tournaments and Elo tracking.
- Move-by-move MCP events for agent evaluation.

Rubik’s Cube Server — feature list 🔷
- 3x3 and 2x2 implementations with state encoding.
- Scramble generator and timing hooks.
- Pluggable solvers for benchmark comparison.
- Visualizer API returning cube SVG.

Claude integration
- TheArena provides a Claude adapter that translates MCP messages to Claude-compatible requests.
- The adapter supports:
  - synchronous and streaming responses,
  - role-based system prompts,
  - context window management.
- To enable Claude:
  - set CLAUDE_API_KEY in your env.
  - configure adapter in server/config.yaml
- Examples:
  - flow_server/examples/claude-playbook.yaml

Examples and scenarios
- examples/flow/auto-assistant.yaml — agent coordinates a data-gathering pipeline.
- examples/rpg/treasure-hunt.json — simple quest for two agents.
- examples/chess/engine-match.yaml — run engine vs agent.
- examples/rubiks/timing-benchmark.yaml — run timed solves for different solvers.

APIs and endpoints
- Base MCP endpoint: POST /mcp/session
- WebSocket real-time: /mcp/ws
- Health: GET /health
- Chess moves: POST /chess/{session}/move
- Rubik commands: POST /rubik/{session}/action
- All endpoints accept and return JSON with MCP envelope objects.

CLI
- bin/arena-cli — lightweight tool to:
  - spawn sessions
  - run sample flows
  - run benchmarks
- Usage:
  - bin/arena-cli start flow examples/flow/auto-assistant.yaml
  - bin/arena-cli replay chess session-12345

Development notes
- Code structure:
  - server/ — Python servers and adapters
  - web/ — React UI and dashboards
  - examples/ — sample scenarios and playbooks
  - tools/ — scripts for testing, generation, and analysis
- Use pre-commit hooks:
  - pip install pre-commit
  - pre-commit install
- Run tests:
  - pytest -q

Benchmarks and metrics
- Each server logs time per step.
- Benchmarks include:
  - average decision latency
  - action success rate
  - reproducibility score
- Use tools/bench to run automated passes and export CSV for analysis.

Security and keys
- Keep API keys in env files or key vault.
- TheArena supports OAuth and API token hooks.
- For production, use TLS and a reverse proxy.

Contributing
- Open an issue for ideas or bugs.
- Fork the repo and create a feature branch.
- Keep PRs small and focused.
- Add tests for new behavior and run linters.

License and attribution
- The repository ships permissive open-source code. See LICENSE for details.
- Third-party assets include icons and public domain images.

Credits
- Project lead: SevGuerra
- Contributors: community-driven list in CONTRIBUTORS.md
- Special thanks to open-source providers of game assets and model adapters.

Troubleshooting
- If a server fails to start, check logs in logs/ and system journal.
- If the Claude adapter fails, verify CLAUDE_API_KEY and network access.
- For release artifacts, download from the Releases page and execute the bundled run script:
  - https://github.com/SevGuerra/TheArena/releases

Reference links
- Releases page: https://github.com/SevGuerra/TheArena/releases  
- MCP primer: docs/mcp.md
- Examples index: examples/README.md

Emojis legend
- 🧩 core features
- 🎲 RPG features
- ♟️ Chess features
- 🔷 Rubik’s Cube features

Readable templates
- Use the example playbooks as templates for your own tasks.
- The JSON/YAML schema in docs/schema.md explains the structure for tasks, agents, and steps.

Contact and support
- Open an issue for bugs or feature requests.
- Use discussions for design questions and collaboration.

License: MIT