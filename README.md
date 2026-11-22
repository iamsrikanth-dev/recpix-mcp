# RecPix MCP server

**Let Claude Code make narrated tutorial videos of your web app.**

Type `/recpix-demo the feature I just built` in Claude Code. Claude Code reads your project, plans the flow and writes the voiceover; [RecPix](https://recpix.app) clicks through your live site or localhost with the real mouse and keyboard, narrates every step with a natural on-device voice, adds captions, zooms and a presenter, cuts out every wait (including Claude's thinking time), and exports an MP4.

Watch the [84-second demo](https://recpix.app/demo/), or see [real examples with every prompt and line](https://recpix.app/examples/).

The MCP server is built into the **RecPix Mac app** (macOS 15+, Apple silicon recommended). It runs on `127.0.0.1` only and needs a per-Mac secret token. This repository holds the setup guide, the tool reference and the `/recpix-demo` skill for Claude Code.

## Set up

1. [Download RecPix](https://recpix.app/downloads/RecPix.dmg) (your first 3 videos are free, with a “Made with RecPix” mark; a $29 one-time licence gives unlimited videos with no mark).
2. Open **RecPix › Claude Code** and switch it on.
3. Copy the setup command it shows and run it in Terminal. It looks like:
   ```bash
   claude mcp add --scope user --transport http recpix http://127.0.0.1:7457/mcp --header "Authorization: Bearer <your token>"
   ```
4. Press **Install skill** (or copy [`skills/recpix-demo`](skills/recpix-demo) into `~/.claude/skills/`), then restart Claude Code.
5. Keep RecPix open, and in Claude Code:
   ```
   /recpix-demo how to invite a teammate, on http://localhost:3000
   ```

No AI model download is needed with Claude Code: it does the thinking with your own Claude plan. The only download is the natural voice (about 330 MB, once).

## Tools

| Tool | What it does |
| --- | --- |
| `recpix_status` | Version, whether exports are watermarked, available voices, presenters and styles |
| `recpix_start` | Open a URL (production or localhost) in RecPix's browser and start recording. Options: title, voice, presenter, aspect (`wide`, `tall` with a phone-sized browser, `square`, `classic`), captions, background, private values (typed as dots, never shown or saved) |
| `recpix_look` | Screenshot plus numbered controls with role, label, value and "required", form state, messages on screen, and controls hidden in closed menus or tabs |
| `recpix_act` | One action: `click`, `type`, `choose`, `press`, `scroll`, `go_to`, `wait`, `say`, each with its spoken line. Targets are control numbers from the last look, or words like "the Save button" |
| `recpix_run` | A list of actions in order; stops at the first failure and returns the page so Claude Code can fix it and carry on |
| `recpix_say` | A narration line with no action (intro, explanation) |
| `recpix_finish` | Stop, add narration, captions, zooms and presenter, export the MP4, and return its path. The project stays editable in RecPix |
| `recpix_cancel` | Stop and discard the take |

## Safety

- Listens on `127.0.0.1` only, rejects requests without the token, and rejects browser requests from other origins.
- Refuses delete, pay, send and sign-out controls unless the session was started with `allow_risky`.
- Private values are never returned in `recpix_look` and never saved.
- A take that receives no command for 10 minutes (or runs 30 minutes) is stopped and discarded.

## Links

- Website: https://recpix.app
- How it works (every feature, with a video): https://recpix.app/how-it-works/
- Claude Code setup and requirements: https://recpix.app/features/claude-code/

RecPix isn't affiliated with Anthropic. Claude and Claude Code are trademarks of Anthropic.

The skill and documentation in this repository are MIT licensed. The RecPix app is commercial software.
