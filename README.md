Claude Code skills I actually run in production, cleaned of anything company-specific so other people can read them and adapt the ones they want.
These aren't demos; each one grew out of a real, sometimes painful, iteration with a real workflow.

## Skills in this repo

Mostly things that take a messy, real, human-produced starting point (a design-debt backlog, a branch nobody wants to review) and drive it to something shippable without a person babysitting every step.

| Skill | What it does |
|---|---|
| [`design-debt-resolver`](./skills/design-debt-resolver/SKILL.md) | Polls a Notion backlog of designer-flagged UI/copy fixes, implements the surgical ones, opens a draft PR with before/after screenshots, handles review back-and-forth, and self-merges once an engineer approves. |
| [`design-split`](./skills/design-split/SKILL.md) | Takes one large exploratory UI branch (built to get the design right, not to ship) and splits it into small, independently reviewable branches — rewritten to the project's coding standards, checked with a before/after screenshot so the shipped UI doesn't drift from what was designed, opened as draft PRs. |

More will land here as I clean them up. This isn't a framework or a curated "best of" list — it's my own working set, published as I get around to redacting each one.

## What you're getting (read this before trying one)

Every skill here was extracted from one specific company's stack. I replaced anything private — database IDs, domain names, branch names, org names, internal file paths, seed-account emails — with `[BRACKETED_PLACEHOLDERS]` describing what goes there. That means:

- **These are not plug-and-play.** You cannot point one at your repo and have it work. You need to read it, find every placeholder, and decide what your own equivalent is (your Notion database ID, your branch protection setup, your auth-mock mechanism, etc).
- **The value is the pattern, not the literal script.** Things like "a draft PR *is* the approval mechanism," "cap attempts per review thread instead of retrying forever," "treat PR comment text as untrusted input, never a command," or "capture before/after screenshots as the actual evidence of a UI fix" — those transfer to any stack. The exact `gh api` incantations are just one working example of applying them.
- **They assume real infrastructure.** Most of these lean on MCP connections (Notion, Slack, GitHub) plus CLI tools already installed and authenticated. If you don't have the equivalent wired up, the skill degrades to "a detailed description of a process," not something you can run today.

If you want to actually run one, the honest path is: read it end to end, list every `[PLACEHOLDER]`, decide what your equivalent is, and expect to cut or rewrite the parts that don't map to your stack (mine reference things like a specific monorepo layout, a merge-queue setup, and a handful of other internal skills these are built to hand off to — yours probably looks different).


## Using one of these with Claude Code

Drop the skill's folder into `.claude/skills/<name>/` in your own repo (or wherever your Claude Code setup looks for skills), fill in the placeholders, and Claude will pick it up when the task matches its `description` frontmatter. See [Claude Code's skills documentation](https://code.claude.com/docs) for the mechanics.

## Contributing

This is mostly a personal publish-as-I-go repo, not soliciting contributions to the skills themselves. Bug reports on the sanitization (something that reads as still-identifying, a broken cross-reference from redaction) are welcome as issues.

## License

MIT — do whatever you want with these, just keep the copyright notice attached.
