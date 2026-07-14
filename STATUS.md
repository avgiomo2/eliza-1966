# eliza-1966 — Status

**Live demo:** https://avgiomo2.github.io/eliza-1966/
**Repo:** https://github.com/avgiomo2/eliza-1966 (public)
**Type:** static single-page web app (HTML/CSS/JS), no backend, GitHub Pages deploy via Actions

## Checklist

- [x] GitHub Pages enabled (was never configured — root cause of the 404)
- [x] Deploy workflow (`.github/workflows/pages.yml`) passing
- [x] Core chat engine working — verified live in-browser, classic ELIZA-style reflection responses firing correctly
- [x] Restored real engine code (a prior commit had overwritten `index.html`'s `<script>` with placeholder/stub text — "[Full script content ... copied here]" — instead of the actual JS, which is why SEND did nothing)
- [ ] ElevenLabs TTS untested (needs a user-supplied API key by design — not a bug, just unverified)
- [ ] Non-English languages (DE/FR/IT/ES) not manually spot-checked yet
- [ ] Mic/STT (Web Speech API) not manually spot-checked yet

## KPIs / demo readiness

| Metric | Status |
|---|---|
| Live URL resolves (not 404) | ✅ |
| Deploy pipeline green | ✅ |
| Core interaction (type → response) | ✅ verified |
| Multilingual UI | ⚠️ unverified beyond EN |
| Voice I/O | ⚠️ unverified |

## What was actually broken (root cause)

Two independent bugs stacked:
1. GitHub Pages was never enabled in repo settings, so the Actions deploy workflow failed on every push (`Get Pages site failed... Please verify that the repository has Pages enabled`).
2. Once Pages was enabled, the live page still didn't work — the most recent commit ("Update index.html with the complete local version...") had actually *replaced* the working ELIZA engine script with placeholder/meta-commentary text instead of real code, apparently from a prior agent session that didn't actually paste the file contents.

## Fix applied

1. Enabled GitHub Pages (`build_type: workflow`) via API.
2. Re-ran the deploy workflow — went green.
3. Restored `index.html` from the last known-good commit (`fea32a35`, pre-breakage) via direct API commit to `main`.
4. Re-deployed and manually verified the chat flow in-browser.

## Not done this pass

- No local clone kept (worked entirely via GitHub API + gh CLI, per instruction — repo stays open for others to clone/edit independently).
- Didn't touch HERMES.md / hermes.config.json added in the breaking commit — left in place, unrelated to the bug.
