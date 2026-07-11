# Hub Dashboard
A single-file personal ops dashboard: it renders a configured GitHub repo's `DASHBOARD.md` and surfaces pending acknowledgements and open pull requests as one-tap actions.
A "Start a session" row links to [Claude Code on the web](https://claude.ai/code) for the configured repo and any repos referenced by the fleet's `apps/*.md` binding files (first GitHub URL in each file); tapping a button copies the `owner/name` slug for pasting into the repo picker.
Your fine-grained personal access token is stored only in this browser's localStorage and talks directly to the GitHub API — no server, no build step, no dependencies.
MIT-ish: use it however you like, no warranty.
