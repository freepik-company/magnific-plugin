# Contributing

Thank you for helping improve the Magnific plugin.

## Issues

Open an issue for a skill that gives wrong advice, a broken install step or a manifest problem.
Say which client you use (Claude Code, Codex, Cursor, Gemini CLI, Grok) and paste the request that went wrong.
Report security problems privately, as [SECURITY.md](SECURITY.md) explains, never in an issue.

## Pull requests

Magnific maintains the skills in an internal repository and publishes them here, so a pull request is a proposal: a maintainer ports the change and credits you.
Every change to `main` needs a review from the code owners.

A change to a skill must:

- Name only tools and models that every Magnific account has.
- Leave pricing to `simulate_cost`: no credit amounts, and no change that lets the agent spend without the user's yes.
- Keep text from the server as data, never as instructions.
- Add no network calls, hooks or binaries.
