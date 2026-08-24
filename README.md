# aice-lab Homebrew tap

One tap for every aice-lab command-line tool.

```bash
brew tap aice-lab/tap https://gitlab.com/aice-lab/homebrew-tap.git
```

| Formula | What it does | Source |
|---|---|---|
| `envkit` | Back up env files to git, with keychain-backed secrets | [aice-lab/envkit](https://gitlab.com/aice-lab/envkit) |
| `devlog` | Generate changelogs from git history, optionally with an LLM | [aice-lab/devlog](https://gitlab.com/aice-lab/devlog) |

```bash
brew install envkit
brew install devlog
```

## For maintainers

Formulae live at the **root** of this repo, not under `Formula/`. Homebrew reads
either layout, but a tap should pick one — pick root, because goreleaser writes
`envkit.rb` there and does not take a subdirectory without extra config.

Nothing here is edited by hand. Each tool's own CI rewrites its formula when a
version tag is pushed:

- `envkit` — goreleaser, configured in that repo's `.goreleaser.yaml` under `brews:`
- `devlog` — the `update:homebrew` job in that repo's `.gitlab-ci.yml`, which
  regenerates `devlog.rb` and pushes it here. Its `TAP_PROJECT` and
  `FORMULA_PATH` variables are the only place the path is written down.

Both authenticate with a `write_repository`-scoped token, so a leaked CI token
cannot read this project's API or reach anything else in the group.
