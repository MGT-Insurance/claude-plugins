> **Generated from [MGT-Insurance/claude-plugins-main](https://github.com/MGT-Insurance/claude-plugins-main). Don't commit here.** Open pull requests in that repository. The promote workflow overwrites this repository, so a direct commit is lost.

# MGT Insurance plugin marketplace

This repository is a Claude Code marketplace (`.claude-plugin/marketplace.json`). Each plugin lives in `plugins/<name>/` with its own `.claude-plugin/plugin.json`.

## Version bump rule

Every change to a plugin's content raises the `version` in that plugin's own `plugins/<name>/.claude-plugin/plugin.json`. Use semver:

- **patch:** wording or typo fixes in the plugin's skill or README, with no behavior change.
- **minor:** new or changed skill guidance, new MCP tools the skill references, or new components (skill, command, agent).
- **major:** a renamed or removed component, or an MCP URL or sign-in change that breaks existing installs.
- **no bump:** edits only to the root `CLAUDE.md`, the root `README.md`, `.github/`, or `.claude-plugin/marketplace.json`.

Why: Claude Code uses the manifest version to detect an update. A manifest that pins a version keeps every user on the cached copy until the version string changes, however many commits you push ([versions and updates](https://code.claude.com/docs/en/plugins/loading#versions-and-updates)). The Claude directory also instructs you to raise the version with every release ([update a published plugin](https://claude.com/docs/plugins/submit#update-a-published-plugin)).

Don't put a `version` in the marketplace entry. When both files set one, `plugin.json` wins and `claude plugin validate --strict` fails on the warning.

## Checks

The `plugin-checks` workflow runs on every pull request to `main`:

1. `claude plugin validate --strict` at the repository root and in each `plugins/*/` folder.
2. For each plugin folder a pull request touches, the plugin's version must be greater than the version on `main`. A plugin folder that doesn't exist on `main` is new and passes.

Run the validation locally before you push:

```bash
claude plugin validate --strict .
claude plugin validate --strict plugins/mgt-quoting
```

## Plugin folder constraints

The directory submission requires each plugin folder to be self-contained. Keep the plugin's `README.md` and `LICENSE` inside its folder, and don't use paths that escape it. Don't add symlinks, submodules, Git LFS files, `bin/`, `package.json`, lockfiles, or `.DS_Store`.
