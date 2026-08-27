# aice-lab Homebrew tap

One tap for every aice-lab command-line tool.

```bash
brew install aice-lab-bd/tap/devlog
brew install aice-lab-bd/tap/envkit
```

No `brew tap` step is needed. Homebrew expands `aice-lab-bd/tap` to the GitHub
mirror at [aice-lab-bd/homebrew-tap](https://github.com/aice-lab-bd/homebrew-tap).
It only expands short tap names against GitHub, which is why the mirror exists.

This GitLab project stays the source of truth. A push mirror copies every commit
to GitHub, so the two never drift. To tap from GitLab directly instead:

```bash
brew tap aice-lab/tap https://gitlab.com/aice-lab/homebrew-tap.git
```

| Formula | What it does | Source |
|---|---|---|
| `envkit` | Back up env files to git, with keychain-backed secrets | [aice-lab/envkit](https://gitlab.com/aice-lab/envkit) |
| `devlog` | Generate changelogs from git history, optionally with an LLM | [aice-lab/devlog](https://gitlab.com/aice-lab/devlog) |

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

The GitHub mirror is configured under Settings > Repository > Mirroring
repositories. It pushes over SSH with a GitLab-generated key, registered on the
GitHub side as a write deploy key. That key reaches the mirror repo and nothing
else. CI never writes to GitHub directly.
