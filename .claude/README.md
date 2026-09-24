# Claude Code configuration — Blue3

## Model
The model is the user's choice, made with `/model` per session; a subagent inherits the
session's model. This repository sets none of it — `.claude/settings.json` carries no
model and no model environment variable (repodocs ADR-027).

## Effort
`max` + adaptive thinking disabled.

## Critical permissions
- `.env`, Passport OAuth keys and `auth.json` are blocked
- `migrate:fresh` and `db:wipe` are blocked — protection against data loss
- Direct `mysql`/`mariadb` is blocked — Claude must generate migrations, not run SQL
