# PDRA — Tester Guide

Thanks for testing PDRA! This guide gets you from installer to first prompt in
about five minutes.

> **Feedback & known issues:** https://github.com/thomhoffer-arch/PDRA_public —
> file bugs there (free GitHub account is enough) and check open issues before
> reporting a duplicate.

> **Work on a COPY of your Revit model.** PDRA can create, modify and delete
> elements. Everything is undoable in the Revit session, but test on a copy anyway.

## 1. Install

1. Run `PDRA-Setup-<version>.exe` (no admin rights needed).
   - Windows SmartScreen may warn about an unknown publisher: click
     **More info → Run anyway** (one time).
2. Pick your Revit version(s) — 2025 and/or 2026 are detected automatically.
3. Keep **MCP bridge** checked (this is how Claude Desktop talks to Revit).
4. Start Revit. On first launch Revit asks about the new add-in: click
   **Always Load**.

You should now see a **PDRA** tab in the Revit ribbon.

## 2. Connect your AI (pick one)

PDRA does not include an AI subscription — you bring your own:

### Option A — Claude Desktop (recommended)
1. Install [Claude Desktop](https://claude.ai/download) and sign in with your
   own Claude account.
2. In Revit: **PDRA → Connect Claude Desktop** (one click, safe to repeat).
3. Restart Claude Desktop. You'll see PDRA's Revit tools available in the chat.
4. With a Revit project open, ask Claude something like:
   *"List the levels in my Revit model"* — it should answer with real data.

Multiple Revit windows open? Use **PDRA → Connect This Project** to point
Claude at a specific one.

### Option B — OpenAI Codex
1. In Revit: **PDRA → Connect Codex** (writes `~/.codex/config.toml`).
2. Codex sessions can now drive the same Revit tools.

### Option C — In-Revit chat panel (needs an Anthropic API key)
1. **PDRA → PDRA Chat** opens a dockable chat panel.
2. Click **API key…** and paste a key from
   [console.anthropic.com](https://console.anthropic.com) (usage bills to that key).
3. Skip this option if you only use Claude Desktop — it is not required.

## 3. Things to try

- "Create a floor plan for every level and place them on sheets"
- "Which walls don't have a fire rating set?"
- "Dimension the grids in the active view"
- Open the **Tools Browser** on the PDRA ribbon to see every available tool.

## 4. Reporting bugs

Open an issue on the public feedback tracker (no account on the PDRA code
repo needed — just a free GitHub account):
https://github.com/thomhoffer-arch/PDRA_public/issues/new/choose

No GitHub account? Email your report instead.

Always include:
- **PDRA version** — shown in the title of the PDRA Chat panel (e.g. `PDRA Chat v1.1.0`)
- **Revit version** (2025 / 2026, with update number from Help → About)
- What you asked / clicked, what you expected, what happened
- If Revit showed an error dialog, a screenshot of it

## 5. Known limitations for testers

- **Beta builds expire ~60 days after release.** An expired build shows a
  dialog at Revit startup instead of loading — just install the latest
  release and you're running again. The chat panel warns you during the
  last two weeks.
- **Tool vetting is disabled by design.** The Tools Browser shows every tool,
  but Accept/Revoke is a developer-only operation. Found a tool that should
  exist but doesn't? Use the **Propose new tool…** button in the Tools Browser
  — proposals reach the developer directly.
- **Schedule transposing needs Revit 2026** — the Revit 2025 API has no
  transpose support at all; the tool reports this rather than failing silently.

## 6. Privacy

What leaves your machine: only what you type/attach in a chat plus the data the
tools return (element names, parameters, rendered view images when verifying).
Nothing is auto-uploaded; project-standards files are read only on demand.

- **Claude Desktop / Codex route:** conversations live in YOUR account history,
  under your provider's retention rules — delete chats there like any other chat.
  PDRA cannot delete them for you.
- **In-Revit chat (API key):** API traffic is not used for model training by
  default and never appears in any account history.
- **Local:** PDRA stores the last conversation in `%APPDATA%\PDRA`. Set the
  `privacy_mode` setting to `on` to store nothing locally (and purge what's
  there), or delete the `%APPDATA%\PDRA` folder anytime.

## 7. Troubleshooting

| Symptom | Fix |
|---|---|
| No PDRA tab in Revit | Re-run the installer; confirm your Revit version was checked. |
| Claude Desktop doesn't list Revit tools | Click **Connect Claude Desktop** again, then fully quit and restart Claude Desktop. |
| Tools time out / "no instance" | Make sure Revit is running with a project open before asking Claude. |
| Chat panel says "No API key set" | Only needed for the in-Revit panel — use Claude Desktop instead, or add a key. |

## 8. Uninstall

Windows **Settings → Apps → PDRA → Uninstall**. Your settings and API key in
`%APPDATA%\PDRA` are kept; delete that folder manually for a full wipe.
