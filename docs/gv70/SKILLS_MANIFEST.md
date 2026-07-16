# Codex Skills Manifest

Installed on 2026-07-14 under `/Users/nick/.codex/skills`. Third-party installs are pinned to audited commits; custom packages contain no scripts or executables.

| Skill | Source/version | Installation path | Reason | Audit result | Included files |
| --- | --- | --- | --- | --- | --- |
| `technical-strategy-council` | custom five-member revision, SHA-256 `2b7c13435c9c63156c45e96a42c78acc180b6a47262437bc9c9cc8f6b22c0fc7`, verified 2026-07-15 | `~/.codex/skills/technical-strategy-council` | Meaningful decisions and confidence-gated autonomy | Required; valid `SKILL.md`; five permanent voters, cross-review, independent milestone scoring, mandatory stops, and notification integration verified | `SKILL.md` |
| `test-driven-development` | `obra/superpowers@d884ae04` | `~/.codex/skills/test-driven-development` | Regression-first changes | Approved; no credential, push, sudo, or downloader behavior | `SKILL.md`, Markdown references |
| `systematic-debugging` | `obra/superpowers@d884ae04` | `~/.codex/skills/systematic-debugging` | Root-cause analysis | Approved; evidence-first workflow; expected `find-polluter.sh` only runs local npm tests | `SKILL.md`, Markdown references, `find-polluter.sh` |
| `git-commit` | `github/awesome-copilot@2c2461a7` | `~/.codex/skills/git-commit` | Logical Git commits | Approved; explicitly rejects secrets/destructive Git | `SKILL.md` |
| `multi-stage-dockerfile` | `github/awesome-copilot@2c2461a7` | `~/.codex/skills/multi-stage-dockerfile` | Reproducible Docker guidance | Approved; documentation only | `SKILL.md` |
| `create-specification` | `github/awesome-copilot@2c2461a7` | `~/.codex/skills/create-specification` | Specification-driven work | Approved; documentation only | `SKILL.md` |
| `acquire-codebase-knowledge` | `github/awesome-copilot@2c2461a7` | `~/.codex/skills/acquire-codebase-knowledge` | Repository discovery/search | Approved; scanner is local/read-oriented; unsupported cross-agent metadata removed after backup | `SKILL.md`, `scripts/scan.py`, references, templates |
| `code-review` | `mattpocock/skills@e9fcdf95` | `~/.codex/skills/code-review` | Review against docs and spec | Approved with note: expects issue-tracker docs for remote specs; no auto-push | `SKILL.md` |
| `gv70-safety-change` | custom, 2026-07-14 | `~/.codex/skills/gv70-safety-change` | GV70 safety guardrails | Approved; no executable content | `SKILL.md`, `agents/openai.yaml` |
| `sunnypilot-submodule-release` | custom, 2026-07-14 | `~/.codex/skills/sunnypilot-submodule-release` | Ordered submodule release | Approved; prohibits force push and large unexplained LFS uploads | `SKILL.md`, `agents/openai.yaml` |
| `comma-log-analysis` | custom, 2026-07-14 | `~/.codex/skills/comma-log-analysis` | Read-only device evidence | Approved; fixed SSH identities, no deployment | `SKILL.md`, `agents/openai.yaml` |
| `gv70-deployment-gate` | custom, 2026-07-14 | `~/.codex/skills/gv70-deployment-gate` | Deployment go/no-go | Approved; explicit staged safety gate | `SKILL.md`, `agents/openai.yaml` |

## Update and removal

Before updating, audit the new diff and copy the existing directory to `/Users/nick/.codex/skills-backup/<timestamp>/`. Update third-party skills with the official installer using an audited `--ref`; it will not overwrite, so remove/rename the old directory only after backup. Update custom skills through the skill-creator workflow and run validation. To remove a skill, back it up, remove only its named directory, and restart Codex.

## Evaluated but not installed

- `github/spec-kit`: reputable, but initialization would add project workflow files outside this task's approved documentation/instruction scope; `create-specification` covers the immediate specification category without doing so.
- `Caveman`: unrelated output-compression behavior and optional file-overwrite tooling.
- `zilliztech/memsearch`: installs hooks/dependencies, starts a background watcher, and changes Codex configuration; too broad for repository search here.
- `grill-with-docs`: current package has nonstandard frontmatter and unresolved slash-skill dependencies when installed alone.
- Mermaid candidates from `markdown-viewer/skills` and `partme-ai/full-stack-skills`: advertised `mermaid` paths were absent at audited HEADs. Project Mermaid rules are therefore in `AGENTS.md`, with no unverifiable package installed.
- Rust, Excalidraw, broad refactoring, and frontend-only skills: unrelated by request.

## Universal framework audit — 2026-07-15

- **Adapted:** the repository-root and OpenDBC `AGENTS.md` files now use the universal five-member, continuous-milestone baseline while retaining the stricter GV70 vehicle, panda, deployment, test, and submodule gates.
- **Installed/required:** `technical-strategy-council`, `test-driven-development`, `systematic-debugging`, `gv70-safety-change`, `gv70-deployment-gate`, `comma-log-analysis`, and `sunnypilot-submodule-release`.
- **Installed/use when triggered:** `git-commit`, `code-review`, `acquire-codebase-knowledge`, `create-specification`, and `multi-stage-dockerfile`.
- **Duplicate:** no duplicate skill names were found.
- **Deferred:** generic Git/GitHub, Docker, repository-search, project-memory, Mermaid, security, performance, accessibility, database, API-contract, release, and UI-regression skills. Existing repository tools or narrower installed skills cover current needs; unrelated categories remain deferred until a scored milestone justifies them.
- **Rejected:** no new capability was rejected during this audit. Previously rejected candidates and reasons remain recorded above.
- No skill, plugin, dependency, model, or persistent service was installed by this framework milestone.
