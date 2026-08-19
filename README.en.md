<p align="right"><a href="README.md">한국어</a> | <b>English</b></p>
<p align="center">
  <img src="docs/images/logo.png" width="120" alt="Senvas"/>
</p>
<h1 align="center">Senvas HMI</h1>
<p align="center">AI-assisted <b>industrial HMI</b> development platform — from design to touch-panel deployment</p>
<p align="center">
  <a href="https://github.com/going-kr/Release.Senvas/releases/latest"><b>⬇ Download Senvas-win-Setup.exe</b></a>
  &nbsp;·&nbsp; Windows 10/11 x64 &nbsp;·&nbsp; Korean UI
</p>

---

This repository distributes **installers and auto-update artifacts only**. The source code is maintained privately.

> ℹ️ The **"Source code (zip / tar.gz)"** links at the bottom of each release are added automatically by GitHub and contain a **snapshot of this distribution repository (README, images)** — not the Senvas source code.

## What is Senvas

**Senvas** is [Going](https://github.com/going-kr)'s industrial HMI development tool. You draw monitoring/control screens in the **`.gudx` visual designer**, fill in communication, logic and data in **C#**, and deploy the result to a **Senvas Touch panel** (linux-arm64) with one click. Its defining feature is the **AI workflow**: Senvas embeds an MCP server that AI coding tools such as Claude Code and Codex CLI attach to, so the AI can build an entire project — requirements interview, screen design, plan, implementation and verification — and afterwards keep acting as an assistant for change requests.

<p align="center"><img src="docs/images/ui.png" width="900" alt="UI designer"/></p>

## Workflow

1. **New project** — pick a screen size (e.g. 1280×800) and theme; Senvas creates the `.senvas` project plus the AI context files (`CLAUDE.md` / `AGENTS.md`).
2. **AI creation** — on the AI page open the *Claude console* or *Codex console* and describe the HMI you want in a paragraph. The AI proceeds **Interview → Design → Plan → Implementation → Verification**, and always stops at Design and Plan for your approval.
3. **Edit** — polish screens on the UI page and C# on the Code page. The AI remains available as an assistant for modifications.
4. **Build / Run** — code generation (MakeCode) → `dotnet build` → run on Windows right away to check.
5. **Deploy** — discover Senvas Touch panels on the same network and upload, install and start in one step.

## Features

### AI workflow (MCP)
Senvas hosts a built-in **MCP server** (`http://localhost:5283`) and auto-registers it with installed **Claude Code · Codex CLI**. Through tool calls the AI validates `.gudx`, generates code (MakeCode), adds image/font resources, sets up persistence, and registers progress and artifacts. It works from the **~300 bundled knowledge documents** (controls, communication, data, design, code patterns) and workflow documents, so results follow Senvas conventions. During verification it renders pages to PNG and **inspects them visually** to find and fix layout defects.

The **AI page** shows the current mode, phase and status, the interview checklist, and artifacts such as the design document and implementation plan. There is no chat window inside Senvas — you talk to the AI CLI you already use; Senvas shows progress and results.

<p align="center"><img src="docs/images/ai.png" width="900" alt="AI progress"/></p>
<p align="center"><img src="docs/images/mcp-clients.png" width="445" alt="AI client auto-registration"/></p>

### UI designer — `.gudx` screens
Drop containers, controls, image-canvas and flow-system parts from the toolbox onto the canvas and tune them in the property panel. Navigate Master · Pages · Windows in the explorer, manage colors in the theme editor and images/fonts in the resource panel. **FlowSystem** parts (tanks, pumps, valves, pipes) connect with the pipe tool for P&ID-style process screens, and the HMI essentials — gauges, meters, trend/time graphs, bar/circle graphs, lamps, switches, data grids — are built in.

### Code — generated vs. user code, kept apart
Every build regenerates `*.Designer.cs` · `design.json` · `GlobalUsings.cs` from `.gudx` (MakeCode); communication, data and logic go into the user partials of `DeviceManager` · `DataManager` · `LogicManager`. Generated files are overwritten, user code is never touched. Edit in the Code page or use **Open in IDE** for an external IDE. Generated projects are .NET 8 OpenTK/SkiaSharp apps (Going.UI) that run unchanged on Windows and linux-arm64.

<p align="center"><img src="docs/images/code.png" width="900" alt="Code editor"/></p>

### Communication
Modbus RTU/TCP (master and slave), MQTT, LS Electric CNet and Mitsubishi MC protocol libraries ship with usage-pattern documents, so the AI or you can write them straight into `DeviceManager`. The Senvas Touch serial-port assignment (1× RS-232, 3× RS-485) is part of the knowledge base and is applied during the interview.

### Persistence (optional)
If history or settings must be stored, the MCP tool wires **EF Core + SQLite** into the project (adds packages, pins a local `dotnet-ef` tool) and generates migrations, including for multiple `DbContext`s.

### Build · Run · Deploy
- **Build**: save → MakeCode → `dotnet build`, with the log streamed live.
- **Run**: launch the build output on Windows to check the screens.
- **Deploy**: `dotnet publish` (linux-arm64) → zip → upload, install and start on the discovered **Senvas Touch panel**. Panels are found via mDNS; a device token is entered once on first connection. The Deploy page can also open the panel's web UI and start/stop the app on the device.

<p align="center"><img src="docs/images/run.png" width="700" alt="HMI app running on Windows"/></p>

## Components

| Component | Runs on | Role |
|---|---|---|
| **Senvas editor** (distributed here) | Windows PC | Projects, UI designer, code editor, AI workflow (MCP), build, run, deploy |
| **Senvas Touch panel** | On site (linux-arm64 panel) | Runs the deployed HMI app; discovered via mDNS on the editor's network |

Panel-side software is not distributed from this repository. You can build and run on Windows without a panel.

## Requirements

| Item | Requirement |
|---|---|
| OS | Windows 10/11 x64 (the editor is self-contained — no separate .NET runtime install) |
| **.NET 8 SDK** | Build · Run · Deploy call `dotnet`, and generated projects target `net8.0`, so **install it yourself** → [Download .NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0). The `dotnet` path can be changed under **Settings › Build** |
| AI CLI | **Claude Code** and/or **Codex CLI** (for the AI workflow). When installed, Senvas auto-registers its MCP server at startup |
| Node.js + qmd | Knowledge search engine used by the AI. First-run onboarding installs Node.js LTS via `winget` and `qmd` via `npm`, then deploys the search index (skippable) |
| Deploy target | Senvas Touch panel on the same network (mDNS) + device token |

## Install (Windows)

1. Download **`Senvas-win-Setup.exe`** from the [latest release](https://github.com/going-kr/Release.Senvas/releases/latest) and run it.
2. The installer is signed with a self-signed certificate, so Windows SmartScreen may warn → **"More info" → "Run anyway"**.
3. Installs per-user under `%LocalAppData%\Senvas`; no administrator rights needed. The required .NET runtime is bundled.
4. On first launch, onboarding offers to install Node.js + qmd (knowledge search). You can skip it, but it is recommended for AI workflow quality.
5. If the **.NET 8 SDK** is missing, install the x64 SDK from [here](https://dotnet.microsoft.com/download/dotnet/8.0). Without it **Build fails** (editing/saving still works). Check with `dotnet --list-sdks`.
6. With Claude Code or Codex CLI installed, **Settings › AI clients** shows them as "registered".

## Updating

The installed app silently checks for a new version at startup, shows what's new and applies it. You can check any time via **Help › Check for updates** (the app restarts when updating, so save your work). No need to come back to this page.

## Release files

| File | Purpose |
|---|---|
| `Senvas-win-Setup.exe` | **Download this for a first install** |
| `Senvas-x.y.z-full.nupkg`, `*-delta.nupkg` | Auto-update packages — not for manual download |
| `RELEASES`, `releases.win.json`, `assets.win.json` | Auto-update metadata |
| (auto) `Source code (zip)`, `Source code (tar.gz)` | GitHub's automatic **snapshot of this distribution repository**. Not the Senvas source code; no need to download |

> `v1.1.x` tags are releases of the previous-generation product **SenvasHMI**. For a new install, use the latest `v2.x`. The two products install side by side and do not update each other.

## Support

Please file bugs and requests in this repository's [Issues](https://github.com/going-kr/Release.Senvas/issues).

---

© Going. All rights reserved. Binaries are provided as-is. See [NOTICE.md](NOTICE.md).
