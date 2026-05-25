# How Claude Code works in large codebases: Best practices and where to start

- **Source URL**: <https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start>
- **Publisher**: Anthropic — Enterprise AI / Claude Code
- **Original publish date**: May 14, 2026
- **Snapshot fetched**: 2026-05-24
- **Snapshot purpose**: frozen source material for the narrated walkthrough at `docs/walkthroughs/2026-05-24-claude-code-large-codebases/`. If the live URL is updated, this snapshot may diverge — regenerate the walkthrough only after refreshing this file.

---

## Overview

Claude Code operates across diverse production environments including multi-million-line monorepos, legacy systems, distributed architectures, and organizations with thousands of developers. The article presents observed patterns for successful adoption at scale.

## How Claude Code navigates large codebases

Claude Code uses **agentic search** — traversing file systems, reading files, using grep, and following references locally on developers' machines. Unlike RAG-powered tools that embed entire codebases and risk returning outdated information, this approach avoids centralized index maintenance issues. However, it works best when Claude has sufficient starting context to know where to look.

## The harness matters as much as the model

Seven extension points define Claude Code's capabilities:

**CLAUDE.md files** load automatically at session start. Root files provide the big picture; subdirectory files cover local conventions. They should remain focused to prevent performance degradation.

**Hooks** run at key moments, automating consistent behavior and capturing session learnings. Stop hooks can propose CLAUDE.md updates while context is fresh; start hooks load team-specific context dynamically.

**Skills** package instructions for specific task types, loading on-demand. They prevent expertise from competing for context space and can be scoped to specific code paths.

**Plugins** bundle skills, hooks, and MCP configurations into installable packages, distributing working setups across organizations through managed marketplaces.

**Language Server Protocol (LSP) integrations** provide symbol-level precision, allowing Claude to follow function calls to definitions and trace references accurately across files.

**MCP servers** connect Claude to internal tools, data sources, and APIs, with sophisticated teams exposing structured search as callable tools.

**Subagents** split exploration from editing through isolated Claude instances with separate context windows, allowing read-only mapping followed by editing with full picture visibility.

---

## Configuration table

| Component | What it is | When it loads | Best for | Common confusion |
|-----------|-----------|---------------|----------|-----------------|
| CLAUDE.md | Context file Claude reads automatically | Every session | Project-specific conventions, codebase knowledge | Using it for reusable expertise that belongs in a skill |
| Hooks | Scripts that run at key moments | Triggered by events | Automating consistent behavior, capturing session learnings | Using prompts for things that should run automatically |
| Skills | Packaged instructions for specific task types | On demand, when relevant | Reusable expertise across sessions and projects | Loading everything into CLAUDE.md instead |
| Plugins | Bundled skills, hooks, MCP configs | Always available once configured | Distributing a working setup across the org | Letting good setups stay tribal |
| LSP | Real-time code intelligence via language-specific servers | Always available once configured | Symbol-level navigation and automatic error detection in typed languages | Assuming that it's automatic |
| MCP servers | Connections to external tools and data | Always available once configured | Giving Claude access to internal tools it can't otherwise reach | Building MCP connections before the basics are working |
| Subagents | Separate Claude instances for specific tasks | When invoked | Splitting exploration from editing, parallel work | Running exploration and editing in the same session |

---

## Three configuration patterns from successful deployments

### Pattern 1 — Making the codebase navigable at scale

- **Keep CLAUDE.md files lean and layered.** Load them additively: root file for big picture, subdirectory files for local conventions. Root files should contain only pointers and critical gotchas.
- **Initialize in subdirectories, not at repo root.** Scope Claude to relevant codebase areas. It automatically walks up the directory tree and loads all CLAUDE.md files, preserving root-level context.
- **Scope test and lint commands per subdirectory.** Running full suites when Claude changes one service causes timeouts and wastes context. Subdirectory CLAUDE.md files should specify applicable commands.
- **Use `.claudeignore` files to exclude generated files, build artifacts, and third-party code.** Version-controlled `permissions.deny` rules in `.claude/settings.json` ensure consistent noise reduction across teams.
- **Build codebase maps for unconventional structures.** Lightweight markdown files listing top-level folders with descriptions provide tables of contents. Layer them for hundreds of folders: root file for highest-level structure, subdirectory files loading on-demand.
- **Run LSP servers for symbol-based searching.** Grep for common function names returns thousands of matches; LSP filters before Claude reads anything, reducing context burn.

### Pattern 2 — Actively maintain CLAUDE.md files as model intelligence evolves

Instructions written for current models can constrain future ones. Rules breaking refactors into single-file changes may prevent newer models from coordinated cross-file edits they handle well.

Skills and hooks compensating for specific model limitations become overhead when those limitations vanish. Teams should conduct meaningful configuration reviews **every three to six months, or after major model releases when performance plateaus**.

### Pattern 3 — Assigning ownership for Claude Code management and adoption

Technical configuration alone doesn't drive adoption. Successful rollouts invested in organizational layers too.

The fastest spreads had dedicated infrastructure investment before broad access. Small teams wired tools so Claude fit workflows from day one. At one company, a couple of engineers built plugins and MCPs available on day one. At another, an entire team managing AI coding tools had infrastructure ready pre-rollout.

These teams typically sit under developer experience or developer productivity functions. An emerging role is **"agent manager"** — a hybrid PM/engineer function managing the Claude Code ecosystem. Minimum viable: a DRI (Directly Responsible Individual) with ownership over configuration, authority on settings and permissions, and responsibility for keeping conventions current.

Bottom-up adoption generates enthusiasm but fragments without centralization. You need individuals or teams assembling and evangelizing the right conventions — standardized CLAUDE.md hierarchies, curated skills and plugins. Without this, knowledge stays tribal and adoption plateaus.

In large, regulated organizations, governance questions arise early: who controls available skills/plugins, how to prevent thousands rebuilding the same thing, how to ensure AI-generated code goes through standard review. Start with defined approved skills, required code review processes, and limited initial access, expanding with confidence.

The smoothest deployments establish cross-functional working groups early: engineering, information security, and governance representatives defining requirements together and building rollout roadmaps.

---

## Applying these patterns to your organization

Claude Code targets conventional environments where engineers are primary contributors, repos use Git, and code follows standard directory structures. Most large codebases fit this mold. Non-traditional setups (game engines with binary assets, unconventional version control, non-engineers contributing) require additional configuration work.

**Acknowledgements**: Alon Krifcher, Charmaine Lee, Chris Concannon, Harsh Patel, Henrique Savelli, Jason Schwartz, Jonah Dueck and Kirby Kohlmorgen (Anthropic Applied AI team), and Amit Navindgi (Zoox).
