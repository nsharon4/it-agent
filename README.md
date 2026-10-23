# IT Agent

IT Agent is a prompt that turns an AI coding agent on a Windows PC into a careful IT support technician. It checks the computer read-only, finds the real cause of a problem and what caused it, proposes a fix with the exact command, its risk and how to undo it, runs nothing without your explicit approval, verifies that the fix holds, and documents everything on a live ticket page.

The repository also includes **it-quick-support**, a Claude Code skill for fast, single-problem support ("no internet", "the printer doesn't work"). It skips setup, opens a ticket and goes straight to the fault.

## What's in the repo

| Path | Contents |
|---|---|
| `prompts/IT-Agent-prompt-v2.7.md` | The full IT Agent prompt, version 2.7 |
| `prompts/IT-Agent-prompt-v2.7.pdf` | The same prompt as a PDF |
| `CHANGELOG.md` | What changed in v2.7 (Hebrew version: `docs/CHANGELOG-he.md`) |
| `docs/IT-Agent-guide-en.pdf` | Quick start guide, English |
| `docs/IT-Agent-guide-he.pdf` | Quick start guide, Hebrew |
| `docs/CHANGELOG-he.md` | The v2.7 changelog in Hebrew |
| `skill/it-quick-support/` | The Claude Code skill: `SKILL.md`, `references/` and `scripts/` |

## Requirements

- Windows 10 or 11 (tested on Windows 11 with Windows PowerShell 5.1).
- Full mode: Claude Code (also works with Codex CLI or Gemini CLI). The agent runs the checks itself.
- Chat-only mode: ChatGPT, Grok, Gemini or Claude in the browser. The agent gives you one command at a time, you run it in PowerShell and paste back the result.

## Using the prompt in Claude Code

1. Create an empty folder, for example `C:\IT-Agent`, and open Claude Code in it.
2. Paste the contents of `prompts/IT-Agent-prompt-v2.7.md` as your first message.
3. Answer the short setup: language, a name for your agent and your IT experience level.
4. Choose what to run from the menu, or describe your problem.

In chat-only tools, paste the prompt the same way; the agent works without creating files.

## Installing the skill

Copy the `skill/it-quick-support` folder into your Claude Code skills folder:

```powershell
# Windows (PowerShell), run from the repository root
Copy-Item -Recurse .\skill\it-quick-support "$env:USERPROFILE\.claude\skills\"
```

```bash
# macOS / Linux / Git Bash
cp -r skill/it-quick-support ~/.claude/skills/
```

Start a new Claude Code session and describe the problem, for example "the printer doesn't work", or invoke `/it-quick-support`. The skill keeps its reports, tickets, error log and local knowledge base under `Documents\IT-Agent-Quick` on your PC. Nothing is sent anywhere else.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for the v2.7 changes.

## Disclaimer

This is a personal, open project, provided as is. Use it at your own risk. Review every command before you approve it, back up important data before any change, and never give the agent passwords, codes or keys.

## Author

Nir Sharon · [LinkedIn](https://www.linkedin.com/in/nir-sharon-270439169)
