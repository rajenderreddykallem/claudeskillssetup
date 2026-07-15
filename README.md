# Senior Engineer Skills

A portable, curated set of [Claude Skills](https://code.claude.com/docs/en/skills) covering the
full software engineering lifecycle — from a raw idea to a deployed, documented, tested
solution. Built for engineers who want Claude to think like a staff/principal engineer at each
stage, not just autocomplete code.

Clone this repo once, install the pack, and it works the same way in any Claude Code session —
on any machine, in any project — without re-copying files by hand.

## The 10 skills

| Skill | Lifecycle stage | Use it when you... |
|---|---|---|
| [`idea-to-spec`](skills/idea-to-spec/SKILL.md) | Idea → spec | Have a rough idea or vague ticket and need a real spec before designing anything |
| [`architecture-decision`](skills/architecture-decision/SKILL.md) | Architecture | Face a consequential technical choice and need alternatives + tradeoffs on record |
| [`design-grill`](skills/design-grill/SKILL.md) | Critical review | Want a design/spec/PR adversarially pressure-tested before you commit to it |
| [`figma-to-prototype`](skills/figma-to-prototype/SKILL.md) | Design → code | Have a Figma frame/link and need working prototype code that fits your codebase |
| [`implementation-plan`](skills/implementation-plan/SKILL.md) | Implementation | Have an approved design and need it sequenced into safe, ordered build steps |
| [`test-strategy`](skills/test-strategy/SKILL.md) | Unit testing | Need to decide what to test, what to skip, and write tests that survive refactors |
| [`root-cause`](skills/root-cause/SKILL.md) | Fixing problems | Have a bug, failing test, or incident and want a systematic fix, not a guess |
| [`write-docs`](skills/write-docs/SKILL.md) | Documenting | Need a README, runbook, API doc, or PR description written for the next reader |
| [`deploy-check`](skills/deploy-check/SKILL.md) | Deployable solutions | Are about to ship and need a rollout plan and an explicit go/no-go |
| [`feature-traceability`](skills/feature-traceability/SKILL.md) | Traceability & audit | Need to trace a feature/fix across branches, releases, and time, and find gaps |

Each skill is a self-contained `SKILL.md` — a text file with a `description` (so Claude knows
when to load it automatically) and instructions (what Claude does once loaded). Claude invokes
these automatically when your prompt matches, or you can invoke one directly, e.g. `/design-grill`.
Nothing here talks to an external service or grants any tool access beyond what your own Claude
setup already allows — these are pure prompt/instruction files, safe to read before installing.

## Install — pick whichever matches where you're using Claude

### Option A — Claude Code: one-command install (recommended)

Works in the terminal, IDE extensions, or Claude Code on the web. Installs all 10 skills as a single plugin, namespaced so it never collides with anything else in your setup.

```
/plugin marketplace add rajenderreddykallem/claudeskillssetup
/plugin install senior-engineer-toolkit@senior-engineer-skills
```

Then reload if needed (`/reload-plugins`, or restart the session). Skills appear as
`/senior-engineer-toolkit:idea-to-spec`, `/senior-engineer-toolkit:design-grill`, etc., and Claude
will also invoke them automatically based on their descriptions.

To update later, after this repo receives new commits:

```
/plugin marketplace update senior-engineer-skills
```

To remove:

```
/plugin uninstall senior-engineer-toolkit@senior-engineer-skills
```

### Option B — Claude Code: manual copy (no plugin system, full control)

Use this if you'd rather have the skills live directly in your dotfiles or a specific project,
or you want to edit them locally without the plugin cache layer.

```bash
git clone https://github.com/rajenderreddykallem/claudeskillssetup.git /tmp/senior-engineer-skills

# Personal — available in every project on this machine:
cp -r /tmp/senior-engineer-skills/skills/* ~/.claude/skills/

# OR project-only — available just in this repo, committed alongside your code:
cp -r /tmp/senior-engineer-skills/skills/* /path/to/your/project/.claude/skills/
```

That's it — no restart needed for adding new skill files to an already-running session (Claude
Code watches these directories live).

### Option C — claude.ai / Claude Desktop (web/desktop app, no CLI)

Skills work in the claude.ai web app and desktop app too, uploaded one at a time as a zip:

1. Clone or download this repo.
2. For each folder under `skills/` (e.g. `skills/design-grill/`), zip **the contents of the
   folder** so that `SKILL.md` sits at the root of the zip — not inside an extra parent folder.
3. In claude.ai: **Settings → Capabilities → Skills → Upload skill**, and select the zip.
4. Repeat for the skills you want. Enable them per-conversation or by default per the Skills UI.

See [Anthropic's skills guide](https://support.claude.com/en/articles/12512180-use-skills-in-claude)
for the exact current UI steps, since this surface changes independently of this repo.

### Option D — Claude API (programmatic)

Upload any `skills/<name>/` folder via the
[Skills API](https://docs.claude.com/en/api/skills-guide#creating-a-skill) if you're building on
the Messages API directly rather than using Claude Code or claude.ai.

## Exporting this to a new machine / team

Because everything here is plain text under version control, "exporting" is just:

```bash
git clone https://github.com/rajenderreddykallem/claudeskillssetup.git
```

...followed by whichever install option above matches the new environment. There's no export
step beyond the clone — the repo *is* the portable artifact. If you fork this to extend it with
your own org's conventions, keep the same `skills/<name>/SKILL.md` layout so Option A/B/C above
keep working unmodified.

## Customizing a skill

Each `SKILL.md` is intentionally opinionated and terse — edit it directly to match your team's
conventions (e.g. change `docs/adr/` to wherever your org keeps decision records). Keep each file
well under Anthropic's 500-line guidance; if a skill grows past that, split reference material
into a second file in the same folder and link to it from `SKILL.md`.

## Repo layout

```
.claude-plugin/
  marketplace.json     # lets `/plugin marketplace add` find this repo
  plugin.json           # plugin manifest bundling all 10 skills
skills/
  idea-to-spec/SKILL.md
  architecture-decision/SKILL.md
  design-grill/SKILL.md
  figma-to-prototype/SKILL.md
  implementation-plan/SKILL.md
  test-strategy/SKILL.md
  root-cause/SKILL.md
  write-docs/SKILL.md
  deploy-check/SKILL.md
  feature-traceability/SKILL.md
```

## A note on the two newest skills

- **`figma-to-prototype`** gets real design data either through a Figma MCP connection (e.g.
  Anthropic's official `figma@claude-plugins-official` plugin) or exported files you provide —
  it won't fabricate a design from a bare share-link URL. Install the Figma plugin separately if
  you want the live-connection path:
  ```
  /plugin marketplace add anthropics/claude-plugins-official
  /plugin install figma@claude-plugins-official
  ```
- **`feature-traceability`** works purely off local git history (branches, tags, commit search) —
  no external tracker integration required. It pre-approves read-only `git log`/`branch`/`tag`/
  `show`/`diff`/`grep` commands via `allowed-tools` so it doesn't stop for permission on every
  inspection command; it never runs anything that mutates the repo.
