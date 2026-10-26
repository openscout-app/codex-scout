# Scout for Codex

This repository is the Codex plugin for Scout. It is public. The plugin is
named `scout`. It starts the Scout MCP server in Codex and adds a coordination
skill.

## Where work goes

- The plugin runs `scout mcp`. It carries no runtime code and no tool
  implementations.
- Behavior that every harness needs goes to the core repository,
  `openscout-app/scout`. Land it there first, then use it here.
- Behavior specific to Codex goes here: the manifests, the MCP launcher, and
  the skill.

| Path | Holds |
| --- | --- |
| `.agents/plugins/marketplace.json` | The `openscout` marketplace. It installs `scout` by default |
| `plugins/scout/.codex-plugin/plugin.json` | The plugin manifest and its interface copy |
| `plugins/scout/.mcp.json` | Starts `scripts/run-scout-mcp.sh` |
| `plugins/scout/scripts/run-scout-mcp.sh` | Finds `scout` and sets the setup root |
| `plugins/scout/skills/scout-broker/SKILL.md` | The coordination skill |
| `docs/index.html` | The GitHub Pages site |

## Rules

- The skill names only tools that `scout mcp` serves. When the core renames or
  removes a tool, update the skill.
- The launcher sets `OPENSCOUT_SETUP_CWD` from the Codex workspace variables,
  then `PWD`, then `HOME`. Keep `HOME` as the last fallback.
- The launcher tries `OPENSCOUT_MCP_BIN`, then `scout`, then
  `bunx @openscout/scout`. When none works, it must print the setup commands.
- Scout is for high-trust local developer use. Do not claim enterprise,
  compliance, or multi-tenant readiness.
- This repository is public. Do not commit local paths, host names,
  credentials, or private source.

## Checks

There is no test suite. Before you open a pull request:

- `bash -n plugins/scout/scripts/run-scout-mcp.sh`
- `python3 -m json.tool` on each JSON manifest that you change.
- Add your checkout as a marketplace in Codex, start a new session, and
  confirm that the Scout tools load.

## Commits and pull requests

- Commit subjects use gitmoji: `📝 Clarify the skill's ask routing`.
- Do not add AI co-author trailers or "Generated with" lines.
- Push a branch to `openscout-app/codex-scout` and open a pull request into
  `main`.

<!-- scout:begin -->
## Scout

- Work for another agent, or on another machine, goes through Scout: `scout ask` (MCP `ask`).
- Another machine: `scout ask --on <host> --project <path on that host> --harness <claude|codex>`; `scout hosts` lists machines and their projects. Don't ssh in to launch agents.
- Syntax: `scout ask --help`, `scout help --detail`. Added by `scout setup agents-md`; `scout setup agents-md --remove` takes it out.
<!-- scout:end -->
