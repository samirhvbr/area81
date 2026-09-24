# Changelog

Entries in the commit-message format (`version - short description in English`),
newest first. **Each `##` heading is literally the commit subject** — the entry
is written *before* the commit, so the sentence that lands in `git log` is one
that was weighed rather than improvised at `git commit` time.

Bodies are narrative: what changed, why, and what was measured. This file is
never rewritten.

> **The record starts here.** This file was created after the repository was:
> earlier versions are in `git log` and are deliberately not back-filled, because
> reconstructing them now would produce a plausible history rather than a true
> one.


## 0.1.20 - the permission lists follow repodocs: five commands move to ask, seven rules leave deny

`rm -rf` and `curl`/`wget` piped into a shell leave `deny` and now ask for confirmation.
Reading `.env`/`.env.*`, `git push --force`/`-f`, `git reset --hard` and `git clean -fd`
leave `deny`. Key reads (`*.pem`, `*.key`, `*.p8`, `*.p12`, `*.pfx`) stay blocked. The
owner's decision on 24/09/2026, replicated from repodocs 1.17.0 (ADR-028).

## 0.1.19 - the repository stops choosing the model

`CLAUDE_CODE_SUBAGENT_MODEL` leaves `.claude/settings.json`. The model is now the user's
choice, made with `/model`, and a subagent inherits it: by default Claude Code gives a
subagent the session's model, and this variable was the only thing making it different —
with the session on Opus 5.5, subagents were measured on Opus 5, because the variable names
the `opus` alias and the alias still resolves through the organization's managed pin.

The 1 stand-by profiles (`opus-sonnet`) go too: their only job was to change the model by being
copied over `settings.json`, and they pinned models the catalog has moved past. Recover
one with `git show <this commit>^:.claude/<name>`.

`.claude/README.md` stop(s) describing a model profile.

Rule and measurement: repodocs ADR-027.

No test: configuration and documents. Checked that the file parses and that repodocs
runbook §7's check is silent here.

## 0.1.18 - the model pin leaves .claude/settings.json

`"model": "opus[1m]"` and the `ANTHROPIC_DEFAULT_OPUS_MODEL` env pin are gone.
The window suffix was a version pin in disguise — the 1M variant existed only for the
previous Opus, so every session was born on it while the catalog already offered the
newer one. The env var is worse than a pin: it redefines what `opus` means for
everything that reads it, the model picker included.

It unblocks nothing on its own: the deciding layer is the account's server-managed
settings, which outrank every local file. Rule, measurement and what to write instead
(`"model": "opus55"`, the version named): repodocs ADR-026.

No test: two JSON keys and a comment. Checked that the file still parses.

## 0.1.17 - the git hooks are regenerated from repodocs

Both hooks of the standard are rewritten from repodocs, and `tools/release.sh`
with them when it came from there. `commit-msg` checks the shape of the subject
(`X.Y.Z - description`), refuses a Conventional Commits prefix and a vague
message, **and checks that the subject's `X.Y.Z` is the version this commit
carries in `version.md`**. `pre-push` compares the local `version.md` against
the remote default branch for a repeated or a backwards version — **only when
the push actually updates that branch**, so a branch deletion, a tag and a topic
branch pass through.

The hook does **not** check the language and could not: what it measures is the
shape and the number.

Escape hatch, declared in both: `REPODOCS_NO_HOOK=1`. In a fresh clone, enable them with
`git config core.hooksPath tools/git-hooks`.
