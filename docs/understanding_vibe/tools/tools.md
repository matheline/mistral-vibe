# Documentation de la partie Outils

## Général (Matheline)

> Doc complète : [`general/tools.md`](./general/tools.md)

`vibe/core/tools/` est le système qui permet à l'agent d'agir concrètement : lire des fichiers, lancer des commandes, appeler des services externes. Il se divise en deux :

- **À la racine : le moteur commun** (documenté dans cette partie). C'est lui qui fait fonctionner tous les outils, quelle que soit leur origine : la classe de base `BaseTool` dont chaque outil hérite, le `ToolManager` qui découvre et enregistre tous les outils disponibles, et le système de permissions (`ALWAYS` / `NEVER` / `ASK`) qui décide si une action s'exécute seule, est interdite, ou demande une confirmation à l'utilisateur.
- **Dans les sous-dossiers : les outils eux-mêmes**, venus de trois sources, chacun documenté par un membre de l'équipe :
    - `builtins/` → [Builtins (Eve)](#builtins-eve) — les outils intégrés à Vibe
    - `connectors/` → [Connecteurs (Ibtihel)](#connecteurs-ibtihel) — les outils de services externes
    - `mcp/` → [MCP (Aroua)](#mcp-aroua) — les outils de serveurs distants (protocole MCP)

Cette partie décrit le moteur qui fait tourner tous les outils ; les trois sections suivantes présentent chaque famille d'outils.

## MCP (Aroua)

## Builtins (Eve)

Builtin tools are the actions Mistral Vibe can perform out of the box. Each one is a Python class in `vibe/core/tools/builtins/` inheriting `BaseTool` — its tool name is the snake_case form of the class name (`ReadFile` → `read_file`) and its model-facing description is loaded from a matching file in `builtins/prompts/`. A call from the model arrives as raw JSON arguments: they are first validated into a typed model (Pydantic), then the agent loop resolves the permission — `ALWAYS` runs immediately, `NEVER` refuses, `ASK` reuses an approved session rule or asks the user (file tools force `ASK` on sensitive paths such as `.env` files). Only once approved does `invoke()` execute the tool's `run()`, which streams progress events and ends with exactly one typed result or one `ToolError`, oversized output being capped and flagged as truncated. In practice this covers file, shell (stateless `bash` or session-based managed shell), web, and user-interaction tools. The per-tool details live in [builtin-tools.md](builtins/builtin-tools.md), verified against the source in the [corrections log](builtins/builtin-tools-corrections.md) and browsable in the [interactive explorer](builtins/interactive/index.html).

The builtins are not a separate system — they are the default instances of the generic machinery described in [Général (Matheline)](#général-matheline) ([`general/tools.md`](general/tools.md)). Each builtin is a `BaseTool` subclass that the `ToolManager` discovers, registers and instantiates. Its arguments are validated by Pydantic, its execution is gated by the same `PermissionStore`, and its `run()` streams events before yielding one final `ToolResult`.

## Connecteurs (Ibtihel)