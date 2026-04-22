# Potential Bugs

## [WARNING] boris.md lists agents that do not exist in this repo
**File:** .claude/commands/boris.md (line 133)
**Detail:** Section 6 (Subagents) lists `code-simplifier.md` and `verify-app.md` as example agent files, but neither exists in `.claude/agents/`. A user following these examples would look for agents that are not shipped. This is external content from howborisusesclaudecode.com so it reflects Boris's personal setup, but the discrepancy could confuse users who expect the examples to match this repo.

## [WARNING] Opus 4.7 referenced as a real model throughout the repo
**File:** README.md (lines 5, 196-198), CLAUDE.md (line 34), .claude/claude-defaults.md (lines 5, 8, 11), .claude/commands/boris.md (lines 44, 46, 321, 332, 334, 566)
**Detail:** The repo repeatedly references "Opus 4.7" as the target model (e.g., "Built for Opus 4.7", "Planning & thinking (Opus 4.7)", adaptive thinking descriptions). As of the knowledge cutoff (May 2025), no model named "Opus 4.7" exists in the Claude model lineup. The actual models are claude-opus-4, claude-sonnet-4, etc. If 4.7 does not exist when users adopt this config, all the behavioural guidance (adaptive thinking, xhigh default, reduced delegation) may not apply and will confuse users expecting those features. This may be intentional forward-looking guidance; flagged as WARNING since the CLAUDE.md explicitly says "Opus 4.7 is the target" and "Don't rewrite for 4.6."

## [WARNING] Inventory table mismatch: /grill "Dispatches agents" column differs
**File:** .claude/README.md (line 42) vs README.md (line 46)
**Detail:** In .claude/README.md, the `/grill` command row shows "Dispatches agents" as `—` (none). In README.md, the same row also shows `—`. However, the `/grill` command body (grill.md line 20) explicitly says "For architecture, patterns, dependency choices, and naming, dispatch the `code-architect` agent separately" as a sibling suggestion. Meanwhile, `.claude/README.md` agent table (line 27) lists `code-architect` as invoked by `/grill (architecture angle)`. The agent table claims `/grill` dispatches `code-architect`, but the command table says it dispatches nothing. One of these is wrong -- either the agent table overstates or the command table understates.

## [INFO] verify.md calls itself a "skill" instead of "command"
**File:** .claude/commands/verify.md (line 15)
**Detail:** The last line says "Do not modify files from this skill" but verify.md is a command (`.claude/commands/verify.md`), not a skill. Per the repo's own conventions (CLAUDE.md, .claude/README.md), commands and skills are distinct concepts. Should say "from this command" for consistency.

## [INFO] /stack command table shows stack-navigator dispatch inconsistently
**File:** .claude/README.md (line 47) vs README.md (line 51)
**Detail:** In .claude/README.md, `/stack` dispatches `stack-navigator (no args)`. In README.md, `/stack` dispatches just `stack-navigator` without the "(no args)" qualifier. Minor phrasing inconsistency between the two inventory tables that are supposed to stay in sync.

## [INFO] boris.md references Opus 4.5 for permission routing
**File:** .claude/commands/boris.md (line 148)
**Detail:** Section 6 says "Route permission requests to Opus 4.5 via a hook". Opus 4.5 is a real model but this is the only mention of 4.5 in the entire repo, which otherwise targets 4.7. This is external content so likely intentional, but it could confuse users about which model generation to use for this pattern.

## [INFO] settings.json deny patterns incomplete for SSH key coverage
**File:** .claude/settings.json (lines 107-112)
**Detail:** The deny list covers `id_rsa` and `id_rsa.*` but does not cover other common SSH key filenames: `id_ed25519`, `id_ecdsa`, `id_dsa`, `*.pem` private keys. The README (line 65) claims SSH keys are denied for "read/edit/write" but only RSA keys are actually blocked. A user with Ed25519 keys (the modern default) would not be protected by these deny rules.

## [INFO] Deny list does not cover .env files in subdirectories
**File:** .claude/settings.json (lines 101-106)
**Detail:** The deny patterns `Read(./.env)` and `Read(./.env.*)` only match `.env` files at the repo root. Nested `.env` files (e.g., `packages/api/.env`) are not blocked. The README (line 65) says ".env reads and writes" are denied, which implies comprehensive coverage. Consider using `**/.env` and `**/.env.*` patterns for full directory-tree coverage.
