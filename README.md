# 💡 Product Idea Evaluator

A fully client-side tool that scores a product idea against four classic product management criteria — powered by Google Gemini, with no backend at all.

## What it does

1. You answer four quick questions about a product idea (Market Size, Competitive Intensity, Feasibility, Differentiation) and optionally describe the idea in a sentence
2. The page sends your answers directly to the Gemini API, asking it to act as a product management advisor
3. Gemini returns a viability score (0-100), a short verdict, honest feedback, and a note on each criterion
4. The result is displayed instantly with a color-coded score (red / yellow / green)

## Why

Most "idea scoring" tools are either static checklists or require signing up for a backend service. This project explores a different pattern: a single static HTML page, with **no server, no database, and no backend of any kind**, that still gets a real AI-generated evaluation by calling an LLM API directly from the browser.

## Bring your own API key — and why

This tool asks you to paste your own free Gemini API key rather than shipping with one baked in. This isn't a limitation — it's a deliberate security choice:

- This page is 100% static, served by GitHub Pages. There is no server-side code anywhere to keep a secret hidden.
- If an API key were hardcoded into the page's JavaScript, anyone who visited the page could open their browser's developer tools and read it straight out of the source — effectively a public, free-for-all API key.
- Instead, each visitor supplies their own free key, which is used only to call the Gemini API directly from their own browser. It is never sent to any other server, and it is never persisted anywhere unless the visitor explicitly checks "Remember this key in this browser," in which case it's saved only in that browser's local storage — not visible to anyone else, not synced anywhere.

Get a free key (no credit card required) at [Google AI Studio](https://aistudio.google.com/apikey).

## How it works

- **Language:** HTML, CSS, and vanilla JavaScript — no framework, no build step
- **AI:** [Google Gemini API](https://ai.google.dev/) (`gemini-3.5-flash-lite`), called directly from the browser via `fetch`
- **Hosting:** [GitHub Pages](https://pages.github.com/) — a free static site served directly from this repository
- **No server, no database, no GitHub Actions, no secrets required**

## Architecture

```
index.html   → The entire application: markup, styling, and the Gemini API call
```

## Setup

If you want to run your own copy:

1. Fork or clone this repository
2. Enable GitHub Pages: **Settings → Pages → Source: Deploy from a branch → Branch: main, / (root)**
3. Open the published page, paste in a free Gemini API key, and start evaluating ideas

No secrets, environment variables, or build steps needed — it's a single static file.

## Customization

- **Change the criteria:** edit the four `<div class="question">` blocks in the HTML, and update the `LABELS` object and the prompt inside `evaluateWithGemini()` in the `<script>` section to match
- **Change the model:** update the model name in the `url` constant inside `evaluateWithGemini()`. Gemini model availability changes over time — if you hit a 404, check which models your key supports by visiting `https://generativelanguage.googleapis.com/v1beta/models?key=YOUR_KEY` in a browser
- **Change the scoring prompt:** edit the `prompt` template string inside `evaluateWithGemini()` to change tone, criteria weighting, or the level of detail in the feedback

## Notes

- All evaluation logic lives entirely in the browser — there's nothing to deploy, scale, or maintain server-side.
- Errors (invalid key, rate limits, network issues) are caught and shown inline rather than failing silently.
- Gemini's free tier has rate limits (requests per minute/day); for casual personal use, this is rarely an issue.
