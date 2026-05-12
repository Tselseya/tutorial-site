# How to Use Your 2nd Brain
### A practical guide for Chelsea's Claude Code + Ollama + Obsidian setup
**Confirmed working:** May 8, 2026 · minimax-m2.5:cloud · MyBrain vault

---

## The Mental Model Before Anything Else

Your 2nd Brain is not a chatbot you talk to. It's an **agent you deploy into your vault**. The difference matters in practice:

- A chatbot answers questions and forgets everything
- Claude Code reads your actual files, creates real files, edits real content, and remembers your entire vault structure — because it's sitting inside it

Every session starts with Claude Code reading your `CLAUDE.md`. That file is why it knows what PettyPal sounds like, what the MathDesk deployment rules are, what folders it's allowed to touch. You spent real time building that document — every session benefits from it automatically.

The other mental shift: **you are not the one doing the organizing**. Your job is to have the idea and drop it in. Claude Code's job is to route it, format it, draft it, file it, and report back.

---

## Part 1 — The Walkthrough (Do These in Order Your First Few Sessions)

---

### Step 1 — Starting Any Session (Now Just One Word)

```powershell
brain
```

That's it. The `brain` alias (set up in your PowerShell profile) automatically navigates to your vault AND launches Claude Code with `minimax-m2.5:cloud`. No more typing the full path and command separately.

You'll see Claude Code boot and print your greeting:

> *"Vault loaded. What are we building today — Scroll And Tell content, MathDesk, workflows, or something else?"*

That greeting confirms CLAUDE.md was read. If you don't see it, something is wrong — check the troubleshooting section.

**If minimax-m2.5:cloud is rate-limited, use a fallback alias:**

```powershell
brain2    # glm-4.7:cloud
brain3    # nemotron-3-nano:cloud
```

> **If `brain` gives you "Not logged in · Run /login"** — this means you typed `claude` directly instead of using the `brain` alias. Always use `brain`, never plain `claude`. The plain command tries to reach Anthropic's servers directly and will always show that warning on your setup.

---

### Step 2 — Your First Scroll And Tell Task (Idea → Script)

You have a Reddit thread you want to turn into content. This is the full pipeline in one shot.

**Type this into Claude Code:**

```
new idea: [paste the Reddit URL here]
```

Claude Code will:
1. Read the thread
2. Create an IDEA file in `ScrollAndTell/Ideas/` with frontmatter filled in
3. Build an OUTLINE with the story arc mapped
4. Write a full PettyPal script draft
5. Save it to `ScrollAndTell/Scripts/` with the correct filename format
6. Report back what it made and where it lives

You do nothing except paste the URL. Then you open the file in Obsidian, read it, and edit what doesn't feel right.

**What if you have an original idea instead of a Reddit thread?**

```
new idea: coworker who keeps "borrowing" equipment and never returns it — protagonist secretly tracks everything and presents a bill at the end of the year
```

Same process — Claude Code creates the IDEA, OUTLINE, and full script autonomously. It will mark `source_url: "original"` in the frontmatter.

**Checking what's in your pipeline at any point:**

```
pipeline status
```

This scans all your ScrollAndTell subfolders and tells you: 3 scripts in DRAFT, 1 in APPROVED, 2 in Ideas, etc. Useful when you've accumulated content and need to see what still needs your review.

---

### Step 3 — Your First MathDesk Task (Building a Feature)

You want to add Camera Scan (Feature 1 from your roadmap). Here's how you kick it off.

**First, check current status:**

```
mathdesk status
```

Claude Code reads your Build-Log.md and Features/ folder and tells you exactly what's live, what's in-progress, and what's pending. This is your starting point for any MathDesk session.

**Then, to start a new feature:**

```
new feature: camera scan
```

Claude Code creates a feature brief in `MathDesk/Features/planned/` with a checklist, frontmatter, and a structured plan. It won't write code yet — just the brief.

**When you're ready to actually build:**

```
build feature: camera scan
```

Claude Code will:
1. Read the feature brief
2. Read your current `index.html` in full
3. Produce an implementation plan first (nodes to add, HTML sections to modify, JS functions needed)
4. Then produce the actual code changes as **annotated snippets**, not the full file

> **Important:** Claude Code delivers MathDesk changes as self-contained snippets with clear "insert after line X" instructions — not the full 2,000-line HTML file. This is intentional. You copy the snippet, find the insertion point in GitHub's browser editor, paste, and commit.

---

### Step 4 — When MathDesk Breaks (The Debugging Flow)

Your AI stops responding. Here's the exact sequence before panicking:

**Step 1 — Run the diagnostic:**

```
mathdesk debug: AI not responding
```

Claude Code checks your known failure patterns and tells you what's most likely wrong.

**Step 2 — 80% of the time, it's the ngrok URL.** Your Docker restarted and the webhook URL changed. Get your current ngrok URL from your ngrok dashboard, then:

```
mathdesk debug: ngrok URL changed to https://[new-url].ngrok-free.app
```

Claude Code will tell you exactly which line in `index.html` to update and give you the deployment steps.

**Step 3 — After any fix, always log it:**

```
update build log: fixed ngrok URL change after Docker restart — new URL: [url]
```

This keeps your Build-Log.md current so future sessions have the right history.

---

### Step 5 — Switching Between Projects Mid-Session

This is normal for you. You don't need to restart Claude Code to switch contexts. Just change what you're asking for:

```
# Working on Scroll And Tell, then switching to MathDesk:

script: coworker-lunch-theft

[Claude Code produces the script]

mathdesk status

[Claude Code switches context and reports MathDesk status]
```

Claude Code re-reads the relevant parts of CLAUDE.md for whichever section it needs. Your instructions file handles the context switch — you don't have to re-explain anything.

---

### Step 6 — Closing a Session (Always Do This)

Before you close Claude Code, commit what was created to git:

```powershell
# In a separate PowerShell window (not inside Claude Code):
cd "C:\Users\Hi\Documents\MyBrain"
git add .
git commit -m "session: [brief description of what you made]"
```

Examples of good commit messages:
- `"session: drafted 3 scripts, outlined 2 new ideas"`
- `"session: camera scan feature brief created, build plan ready"`
- `"session: fixed ngrok URL, updated build log"`

This is your undo button. If Claude Code ever edits something in a way you didn't want, `git checkout [filename]` restores it instantly.

---

## Part 2 — Idea-Driven Workflows (For When Inspiration Hits)

Since you work on ideas as they come rather than on a schedule, here are the exact entry points depending on what sparked the session.

---

### You Found a Reddit Thread Worth Turning Into Content

```
new idea: [URL]
```

Done. Claude Code handles the rest autonomously.

---

### You Have a Half-Formed Content Angle in Your Head

```
new idea: [describe it in one sentence, as rough as it is]
```

Example:
```
new idea: what happens when someone discovers their entire office has been secretly pranking them for months and they get the perfect revenge at the holiday party
```

Claude Code will extract the protagonist, conflict, and payoff from your description, structure it into PettyPal's voice, and save the full script draft.

---

### You Want to See What Content Is Ready to Record

```
pipeline status
```

This tells you what's sitting in APPROVED (ready to record) vs DRAFT (needs your review) vs OUTLINE (still needs a full script). Run this whenever you sit down at CapCut.

---

### You Have a MathDesk Bug Report or Idea

```
mathdesk debug: [describe the symptom]
```

or

```
new feature: [name the feature in a few words]
```

Claude Code doesn't need a formal briefing. A casual description is enough — it will structure it properly from there.

---

### You Want to Document Something You Just Built

```
doc this: [workflow name]
session log: [brief topic]
update build log: [what you built/fixed]
```

Use these after any productive work session to capture what happened before you forget. Claude Code writes the documentation from context — you just trigger it.

---

### You Want to Build a Portfolio Entry

```
portfolio: MathDesk
```

Claude Code creates a structured case study in `Portfolio/Case-studies/` covering: the problem it solves, the technical stack, what's live, and what you'd do differently. You review and edit — it gives you the first draft.

---

## Part 3 — Reference Section

---

### Full Command List

**Scroll And Tell:**

| Command | What happens |
|---|---|
| `new idea: [URL or description]` | Full pipeline → IDEA → OUTLINE → SCRIPT, all saved autonomously |
| `outline: [slug]` | Creates outline for an existing IDEA file |
| `script: [slug]` | Writes full script from existing OUTLINE |
| `pipeline status` | Counts files at each stage across all ScrollAndTell folders |

**MathDesk:**

| Command | What happens |
|---|---|
| `mathdesk status` | Reads Build-Log + Features/ and reports what's live, in-progress, pending |
| `new feature: [name]` | Creates feature brief in Features/planned/ with checklist |
| `build feature: [name]` | Reads brief + index.html, produces implementation plan + code snippets |
| `mathdesk debug: [symptom]` | Runs through known failure patterns, diagnoses, proposes fix |
| `update build log: [summary]` | Appends dated entry to Build-Log.md |
| `session log: [topic]` | Creates dated session log in MathDesk/Session-Logs/ |
| `mathdesk deploy` | Outputs the exact browser-upload steps for GitHub |

**General:**

| Command | What happens |
|---|---|
| `doc this: [workflow name]` | Creates workflow doc in Automations/n8n-workflows/ |
| `daily log: [note]` | Appends timestamped entry to today's daily note |
| `recap: [date or range]` | Summarizes daily notes from that period |
| `portfolio: [project name]` | Creates case study draft in Portfolio/Case-studies/ |

---

### Model Fallback Order (If minimax-m2.5:cloud Is Rate-Limited)

Use your aliases — all confirmed free on Ollama:

```powershell
brain     # minimax-m2.5:cloud (primary)
brain2    # glm-4.7:cloud (fallback 1)
brain3    # nemotron-3-nano:cloud (fallback 2)
```

Or launch manually for other models:

```powershell
ollama launch claude --model gemma4:cloud
```

> If any model prompts `/login` at startup, it requires a paid Ollama plan. Skip it and try the next one.

---

### When Something Goes Wrong — Quick Diagnosis

| Symptom | Most likely cause | Fix |
|---|---|---|
| AI doesn't respond in MathDesk | ngrok URL changed | Get new ngrok URL, update `N8N_WEBHOOK_URL` in index.html, redeploy |
| Claude Code answers but won't create files | On a local model | Switch to `minimax-m2.5:cloud` via `brain` |
| Claude Code can't find files | Wrong launch directory | Exit, run `brain` (alias handles navigation automatically) |
| "Not logged in · Run /login" on startup | Typed `claude` instead of `brain` | Always use `brain` alias, never plain `claude` |
| "64 skill descriptions dropped" warning | Too many skills for 1% context budget | Run: `@'{"hasCompletedOnboarding":true,"claudeCodeAttribution":false,"skillListingBudgetFraction":0.05}'@ \| Set-Content "$env:USERPROFILE\.claude\settings.json"` |
| Skill truncation still showing after fix | Settings file not saved correctly | Run `type "$env:USERPROFILE\.claude\settings.json"` — verify all 3 fields present |
| Session cuts off mid-task | Context length too low | Ollama Settings → context slider → 64k |
| Auth conflict warning on startup | `primaryApiKey` in settings.json | Re-run settings fix — two fields only: `hasCompletedOnboarding` + `claudeCodeAttribution` |
| Script sounds generic, not like PettyPal | CLAUDE.md wasn't read | Check CLAUDE.md is in vault root, relaunch with `brain` |
| Only one n8n mode works | groqBody field bug | Check all Switch branches use `$json.groqBody` |
| `brain` command not recognized | PowerShell profile not loaded | Run `. $PROFILE` in PowerShell to reload, or restart terminal |

---

### Installing New Skills — Standard Process

Skills extend what Claude Code knows how to do. They live in `.claude\skills\` — one folder per skill, each containing a `SKILL.md` file. Claude Code auto-discovers them every session. No config files to edit, ever.

**Your confirmed skills location:**
```
C:\Users\Hi\Documents\MyBrain\.claude\skills\
```

**Currently installed (as of May 8, 2026):**
- `obsidian-markdown` — writes proper Obsidian wikilinks, embeds, callouts, frontmatter
- `obsidian-bases` — creates Obsidian Bases (.base) database views
- `json-canvas` — creates visual canvas mind maps (.canvas)
- `obsidian-cli` — interacts with a live running Obsidian instance via CLI
- `defuddle` — cleans and extracts web content (useful for Reddit threads)

---

#### Installing a Skill Set from a GitHub Repo

Use this pattern whenever a skill repo is available on GitHub (like kepano's):

```powershell
# Step 1 — navigate to vault (always first)
cd "C:\Users\Hi\Documents\MyBrain"

# Step 2 — clone the repo to a temp folder
git clone https://github.com/[username]/[repo-name].git temp-skills

# Step 3 — copy the skill folders into your skills directory
Copy-Item -Recurse temp-skills\skills\* .claude\skills\

# Step 4 — delete the temp folder
Remove-Item -Recurse -Force temp-skills

# Step 5 — verify the skills landed
dir .claude\skills
```

> **Note:** Some repos may organize their skills differently. If `temp-skills\skills\*` returns nothing, check what folders exist inside `temp-skills` first with `dir temp-skills` and adjust the path in Step 3 accordingly.

---

#### Installing a Single Skill Manually

If a skill is just a single `SKILL.md` file someone shared (not a full repo):

```powershell
# Step 1 — create a folder for it inside your skills directory
New-Item -ItemType Directory -Force -Path ".claude\skills\[skill-name]"

# Step 2 — place the SKILL.md file inside that folder
# (copy it there manually via File Explorer or download it directly)

# Step 3 — verify
dir .claude\skills\[skill-name]
# Should show: SKILL.md
```

---

#### Checking What Skills Are Installed

```powershell
dir "C:\Users\Hi\Documents\MyBrain\.claude\skills"
```

Each folder listed = one installed skill. Skills take effect on the next Claude Code session start — no restart of anything else needed.

---

#### Removing a Skill

```powershell
Remove-Item -Recurse -Force ".claude\skills\[skill-name]"
```

Gone on next session. Nothing else to update.

---



### The Non-Negotiables (Things That Break If You Skip Them)

1. **Always launch with `brain`.** Never type plain `claude` — it bypasses Ollama and hits Anthropic's servers directly.
2. **VS Code terminal is fine.** Confirmed working on your hardware — use whichever terminal is convenient.
3. **Always commit after a productive session.** `git add . && git commit -m "session: ..."` — this is your undo button.
4. **Never deploy MathDesk via `git push`.** Browser upload to GitHub only — port 443 is blocked for authenticated pushes on your network. Read-only `git clone` works fine.
5. **Never use local models for file operations.** `qwen3:1.7b` is for offline quick questions only. Anything involving reading or writing files needs a cloud model.

---

### On the YouTube Tutorials — Notes and Verdicts

**Eric Tech — "Obsidian + Claude Code: The Second Brain Setup That Actually Works"**
Obsidian setup followed from this tutorial (timestamps 1:28–7:32):
- GitHub repo created and connected to vault for version control and cloud backup
- Obsidian Git plugin installed for automatic syncing without touching the terminal
- kepano/obsidian-skills installed (all five skills now live in `.claude\skills\`)

Skip his model/Claude Code installation steps — he's on Mac with a GPU and his commands conflict with your confirmed working Windows + Ollama setup. Everything from the Obsidian section onward is applicable.

**Ksk Royal — "How to USE Claude Code for FREE with Ollama"**
Skip the installation steps — already past them and his steps don't match your confirmed working setup. The section explaining *what Claude Code is capable of* is useful conceptual background. Nothing else.

**kepano/obsidian-skills (GitHub)**
Already installed — all five skills (`obsidian-markdown`, `obsidian-bases`, `json-canvas`, `obsidian-cli`, `defuddle`) are live in `.claude\skills\`. See the "Installing New Skills" section for how to add more in the future.

**Bottom line:** Your setup is more customized and more debugged than anything those tutorials cover. Use them for conceptual understanding, not instructions.

---

---

*Usage guide · Chelsea's 2nd Brain · Last updated May 8, 2026 — troubleshoots updated, VS Code correction, Obsidian setup noted*

You have two GitHub repos documenting what you've built and learned:
- `github.com/Tselseya/Building-Systems-Guide`
- `github.com/Tselseya/Self-Hosted-AI-Stack-Setup-Guide`

Combined with this usage guide and your Obsidian vault, these are the raw material for a personal tutorial website — a living, public record of what you know and how to do it. The site grows as your knowledge grows.

---

### The Architecture (Keep It MathDesk-Simple)

Same philosophy as MathDesk: **single HTML file, GitHub Pages, no framework, no build step.** You already know this deployment pattern cold. The site reads your markdown content and renders it as a clean tutorial site.

```
github.com/Tselseya/tutorial-site/     ← new repo (create this)
├── index.html                          ← the entire site
├── content/
│   ├── 2nd-brain-setup.md             ← this guide
│   ├── building-systems.md            ← from Building-Systems-Guide repo
│   └── self-hosted-ai-stack.md        ← from Self-Hosted-AI-Stack-Setup-Guide repo
└── assets/
    └── og-image.png                   ← social share card
```

The site fetches and renders your markdown files dynamically — so updating a tutorial means editing a `.md` file, not touching HTML.

---

### Phase 1 — Set It Up With Claude Code

**Step 1 — Create the repo on GitHub**

Go to `github.com/new` → name it `tutorial-site` → public → no template → create.

**Step 2 — Tell Claude Code to build it**

Open a new Claude Code session (`brain`) and paste this:

```
Build me a tutorial website. Single HTML file, GitHub Pages deployable.
It should:
- Fetch and render markdown files from a /content/ folder
- Have a sidebar with navigation between tutorials
- Match a clean dark theme similar to my MathDesk app
- Have a search bar that filters tutorials by keyword
- Show "last updated" date pulled from each markdown file's frontmatter
- Be fully mobile responsive
- Include OG meta tags for social sharing

Start by reading my MathDesk index.html for my code style reference,
then produce the full index.html.
```

Claude Code will read your MathDesk code style and build a site that feels consistent with it. It already knows your deployment constraints from CLAUDE.md.

**Step 3 — Add your content files**

Copy your existing markdown guides into `content/`. Claude Code can also do this:

```
Create content/2nd-brain-setup.md by summarizing the key steps
from the 2nd-brain-setup-guide.md and 2nd-brain-usage-guide.md files in the vault.
Format it as a clean tutorial with numbered steps and code blocks.
```

```
Create content/self-hosted-ai-stack.md as a tutorial page based on
the content from github.com/Tselseya/Self-Hosted-AI-Stack-Setup-Guide
```

---

### Phase 2 — The "Grows As You Learn" Workflow

This is what makes it a living site rather than a static document dump. Every time you figure something out — a new Claude Code trick, a new n8n pattern, a new MathDesk feature — you add a tutorial and the site updates automatically.

**The workflow:**

```
1. Learn something / build something
        ↓
2. In Claude Code: "session log: [what you just learned]"
   → Creates a session log in your vault
        ↓
3. In Claude Code: "new tutorial: [topic]"
   → Claude Code drafts a clean tutorial from your session log
   → Saves it to content/ in the tutorial site repo
        ↓
4. Commit to GitHub → GitHub Pages auto-rebuilds
        ↓
5. Tutorial is live
```

Add this command to your CLAUDE.md quick commands table:

| Command | What to do |
|---|---|
| `new tutorial: [topic]` | Draft a tutorial page in `content/` based on recent session logs or a topic I describe |
| `update tutorial: [filename]` | Find the existing tutorial file and update it with new information I provide |
| `tutorial site status` | List all files in `content/` with their last-updated dates |

---

### Phase 3 — Connecting the Three Sources

Your tutorial content will come from three places and Claude Code can pull from all of them:

**From your vault (session logs, build logs, daily notes):**
```
new tutorial: how to install Claude Code on Windows with Ollama
— pull from the session logs and setup notes in my vault
```

**From your GitHub repos (existing documentation):**
```
new tutorial: self-hosted AI stack setup
— the source material is at github.com/Tselseya/Self-Hosted-AI-Stack-Setup-Guide
fetch it and rewrite it in tutorial format with numbered steps
```

**From a live session (you just figured something out):**
```
new tutorial: fixing the skillListingBudgetFraction warning in Claude Code
— I just solved this, here's what happened: [describe it]
```

Claude Code converts all three formats into consistent, clean tutorial pages.

---

### Deployment (Same as MathDesk)

```
1. Go to github.com/Tselseya/tutorial-site
2. Click index.html or the relevant content file
3. Click pencil (edit) icon
4. Paste the new/updated content
5. Commit changes
6. Wait ~30 seconds for GitHub Pages to rebuild
7. Hard refresh your site URL to verify
```

Enable GitHub Pages: repo Settings → Pages → Branch: main → folder: / (root) → Save.

Your site will be live at: `https://tselseya.github.io/tutorial-site/`

---

### What This Becomes Over Time

Right now you have: 2nd Brain setup, Obsidian setup, MathDesk build, Self-Hosted AI Stack — that's already 4 solid tutorials that very few people have documented properly at your level of detail. In six months of building at your current pace, you'll have 20+. That portfolio of documented knowledge is more credible than any resume because it shows the actual work, not a summary of it.

The site is also a natural extension of your portfolio in `Portfolio/Case-studies/` — Claude Code can cross-reference both and generate case study pages for the website automatically.

---

*Usage guide · Chelsea's 2nd Brain · Last updated May 8, 2026 — troubleshoots updated, VS Code correction, Obsidian setup noted, tutorial website section added*
