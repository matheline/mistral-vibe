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

## Connecteurs (Ibtihel)