# IT Agent v2.7 – What changed (2026-10-24)

Based on IT Agent v2.6. This version brings into the prompt the rules that were added to the it-quick-support skill after use on real support requests. Every new rule is marked in the prompt with `[sync: <name>]`. The prompt is still a single self-contained file: where the skill uses its own helper scripts, the prompt describes the behavior and the file format, and the agent creates and maintains the files itself.

A Hebrew version of this changelog is in [docs/CHANGELOG-he.md](docs/CHANGELOG-he.md).

## Added
- **The agent offers to do the action for you.** Every step it can perform itself is shown as the primary option, "I'll do it for you", next to the manual option. It acts only after you approve that specific action ID. A step stays manual only when there is no alternative, such as hardware, a password or a screen only you can see, and the agent says why.
- **End-to-end handling after 4 attempts.** If the fault is not resolved after 4 actions on the same item, the agent stops proposing single steps and offers one complete plan: diagnosis, fixes in order, risk, undo and verification. Any change outside the plan needs a new approval.
- **Additional faults are always offered.** If the agent notices another fault, in a screenshot, a log or a check, it says so at once and offers to open a separate ticket for it, with the options "Now", "After the current fault", "I'll handle it myself" and "Not now". It does not propose closing a ticket while there is a fault it saw but has not offered to handle. Why: in real use, the agent noticed another fault in a screenshot and did not offer to fix it.
- **Free text and screenshots, always.** Every question has a free-text box, not only buttons. The agent invites you to upload a screenshot in the chat, treats the image as evidence and records it in the activity log.
- **Voice dictation and conversation mode.** Next to every text box there is a 🎤 button that recognizes speech in the language you chose, including Hebrew (he-IL). The language is set explicitly and is not taken from the browser or Windows default. The text is never sent automatically; you review it and send it. In conversation mode the agent reads each new question aloud and listens for your answer, and if you say the name of an option, it is selected. If the browser does not support this or the microphone is blocked, the page keeps working with text.
- **Agent error log, size-capped.** The agent logs its own errors to `Documents\IT-Agent-Quick\Logs\agent-errors.jsonl`, for example a failed command, a failed publish, a failed check or a bug you reported. Each error is one line, and the message is capped at 500 characters. A repeated error only increments a counter. At 512 KB the file rotates and only two old files are kept; rotated files older than 30 days are deleted. Secrets are masked. At the start of every run the agent reads the last 20 errors so it does not repeat them.
- **Local knowledge base.** Every finished ticket adds one line to `Documents\IT-Agent-Quick\Knowledge\knowledge.jsonl`: the symptom, the checks that mattered, the cause, the fix and whether it worked. Failed fixes are recorded too. Tags are written in both Hebrew and English. The agent searches the file at the start of every ticket and before any outside lookup, such as a web search, documentation or escalation, and tries what already worked first, while still verifying the cause on the PC. The file is capped at 2 MB. The same category with the same cause increments a counter instead of adding a line. Everything stays on the PC.

## Fixed
- **Choice buttons start empty.** No button is pre-selected, not even the recommendation or the default, which only get a small tag. Only the button you clicked is filled, and "Change choice" moves the fill to the new choice. Why: previously the default stayed highlighted even after another option was chosen.
- **Auto-jump to a new request.** The page jumps to the request waiting for you on every load, not just once per request. If the jump does not succeed right away, it retries several times, with a scrolling fallback. Before every publish, a headless browser check confirms that the request actually reaches the screen. Why: the jump did not work in the Claude side panel.
- **"📋 Copy" button outside the command box.** Only the command scrolls, and the button stays visible even when the command is long. Why: with long commands the button was hidden.

## Changed from v2.6
- **Setup questions (S1–S6):** the default is no longer pre-selected; only a "default" tag is shown. Each question now has a free-text box and a 🎤 button. "Start with the defaults" works as before.
- **The main orange button** is now the item's send button, not the first option, so no option looks pre-selected.
- **A fault outside the request** no longer goes silently to Deferred. It is offered to you as a separate ticket, and goes to Deferred only if you choose "Not now".
- **The element ID of a waiting item** is now `w-<id>`, so the auto-jump always finds it.
- **The sample JS code** was updated: choice fill, repeated jump, copy button, dictation and conversation mode. The code passed `node --check` and a click test in a simulated DOM.
- **Pre-publish gate:** added checks for the choice buttons, the free-text box with 🎤, the copy-button layout and the headless-browser jump test. A failed check is also recorded in the error log.
- **Removing the agent:** the error log and the knowledge base in `Documents\IT-Agent-Quick` are kept unless you ask to delete them too.
- **Chat-only version** (ChatGPT and similar): it has no error log or knowledge base, because it does not create files. The copy-ready summary at the end includes the cause and the fix that worked.
