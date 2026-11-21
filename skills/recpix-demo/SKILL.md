---
name: recpix-demo
description: Make a narrated product tutorial or demo video of a web app with RecPix (the Mac app), for a live site or localhost. Use when the person asks for a tutorial, walkthrough, demo, how-to or launch video of their product or a feature, or of any website.
---

# Make a tutorial video with RecPix

You are the director. You decide what to show, in what order, and write every spoken line. RecPix (through the `recpix_*` MCP tools) is the camera and the hands: it clicks and types exactly what you say, speaks your lines in a natural voice, records the browser, adds zooms, captions and a presenter, and cuts out all waiting, including your thinking time.

If the `recpix_*` tools are missing, tell the person: open RecPix › Claude Code, switch it on, run the setup command it shows, then restart Claude Code. RecPix must be open while recording.

## 1. Decide what the video shows

- Use the person's words. "What I just built" means the latest change: read `git log -3` and `git diff HEAD~1 --stat`, then the changed files, and show that feature from a customer's point of view.
- For a whole product with no specific flow: show the 1 main job the product does, start to finish. Not a menu tour.
- Aim for 30–90 seconds: 4–10 actions. One video, one outcome ("send your first invoice"), not everything.
- For someone else's product (outreach demos), use only what is public: the homepage, pricing, docs, a public demo or sandbox. Never sign up with real details or submit real forms on their site.

## 2. Find where it runs

- Production URL if the person gave one or it's in the README / package.json `homepage`.
- Otherwise localhost: check what's already listening (`lsof -iTCP -sTCP:LISTEN -n -P | grep -E ':(3000|3001|4000|5000|5173|8000|8080)'`). If nothing is running, start the dev server in the background with the project's own script, and wait until the URL answers.
- Logins: look for seed / fixture / .env.example test users. **Ask the person before using any credential**, then pass it in `private_values` and type it as `{{name}}`. Never put a password in a `say` line.
- If the flow needs a signed-in account that only the person has, ask them to sign in once inside RecPix's browser first (RecPix keeps the session).

## 3. Write the plan, then record it

1. `recpix_status` once to see voices. Pick a warm voice (default is fine).
2. `recpix_start` with the URL and a title like "How to create an invoice in Acme". Use `aspect: "tall"` when they want Shorts / Reels / X.
3. `recpix_look` (screenshot on). Match your plan to the real control numbers.
4. `recpix_say` a one-sentence intro: what the viewer will be able to do, e.g. "Let's create and send your first invoice in Acme."
5. `recpix_run` with the whole flow. Each step has a `say` line:
   - one short sentence, present tense, what and why: "Click New invoice to start a fresh one."
   - use the product's real names for buttons and pages
   - type realistic sample data ("Globex Corporation", "jane@globex.com"), never lorem ipsum
   - use `enter: true` for search boxes
   - after an important result appears, add `{"action":"wait","seconds":1.5}` so viewers see it
6. If a step fails: `recpix_look`, fix it (open the menu that hides the control, fill the required field it complains about, scroll), then continue with `recpix_run` for the rest. Don't start over.
7. `recpix_finish` with a short outro: "And that's how you send your first invoice in Acme."

## 4. Hand it over

Give the person the MP4 path and a 2–3 line summary of what the video shows. Offer next steps:
- a 9:16 version for social (record again with `aspect: "tall"`)
- a written help-centre article with the same steps
- the same flow for another feature

## Good narration

- Friendly product expert, talking to a new customer. Not a robot reading clicks.
- Explain the benefit once, at the start or at the key moment: "Recurring invoices go out automatically every month."
- Don't narrate waiting, scrolling or obvious moves. Never say "I", "the AI" or "the agent".
- Under about 15 words per line.
