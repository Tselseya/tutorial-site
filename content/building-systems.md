---
title: "How I Build My Systems"
date: 2026-05-10
description: "A step-by-step manual for building a Claude-assisted workflow, writing case studies that actually impress technical reviewers, and deploying a portfolio site from scratch or from Notion."
tags: [claude, projects, portfolio, case-study, github, netlify]
---

# How I Build My Systems

A step-by-step manual for building a Claude-assisted workflow, writing case studies that actually impress technical reviewers, and deploying a portfolio site from scratch or from Notion.

**Tags:** Beginner-Friendly · Claude Projects · Portfolio · GitHub · Netlify · Google Search Console

---

## Table of Contents

1. [Setting Up Claude Projects for Your Work](#part1)
   - [Creating the project](#project-create)
   - [Writing your instructions](#project-instructions)
   - [What files to upload](#project-files)
2. [Writing Your Case Study (Before Building the Portfolio)](#casestudy-why)
   - [Gathering your materials](#casestudy-gather)
   - [Updating LinkedIn & Resume](#casestudy-update)
   - [Writing the case study with Claude](#casestudy-write)
3. [Building Your Portfolio Site](#portfolio-overview)
   - [Path A: Migrating from Notion to HTML](#portfolio-migrate)
   - [Path B: Starting from scratch](#portfolio-scratch)
   - [GitHub repository setup](#portfolio-github)
   - [Deploying to Netlify](#portfolio-netlify)
4. [Optional: Google Search Console Indexing](#seo-google)

---

## Part 01 — Setting Up Claude Projects

Claude Projects give you a persistent workspace where Claude remembers your context, your files, and your instructions across every conversation. Instead of copy-pasting your background every time you open a new chat, a Project holds everything Claude needs to know about you and your work. Think of it as giving Claude a permanent desk drawer for your stuff.

> **Tip:** Why this matters for your workflow
> You can have one Project for your portfolio work, another for each client, and another for your personal systems building. Each one stays separate and focused.

### Step 1 — Creating Your Project

1. **Open Claude.ai and go to the sidebar**
   On the left side of the screen, look for the **Projects** section. If you don't see it, scroll down in the sidebar. You'll see a small folder icon or the word "Projects."

2. **Click "New Project" or the + icon next to Projects**
   A setup screen will appear. You'll be asked to name your project and optionally write a short description.

3. **Name your project clearly**
   Use a name you'll instantly recognize. Examples:
   - `Portfolio Builder`
   - `Scroll & Tell — Automation`
   - `Case Study Writing`
   - `[Client Name] — VA Work`

4. **Click "Create Project"**
   Your project now exists. You'll land inside it, where you'll see two main areas: **Instructions** (top) and **Files** (below). We'll fill both of these in the next steps.

### Step 2 — Writing Your Project Instructions

This is the most important step. The instructions you write here are what Claude reads at the start of every single conversation inside this project. They tell Claude who you are, what you're working on, how you want it to respond, and what rules to follow.

> **Warning:** Common mistake
> Most people write vague instructions like "help me with my portfolio." That won't work well. Claude needs specific context to give you specific, useful help.

Below is the exact instruction template structure I use. Copy and adapt it for your own project.

```markdown
## Who I Am
[Your name] — [Your current role / title]
[Location, work setup: remote, freelance, etc.]
[1–2 sentences on what kind of work you do]

## What This Project Is For
[Describe the specific purpose of this project.
Be specific. "This project is for writing my case study
on the Instagram automation system I built for Scroll & Tell,
then using it to build my portfolio page."]

## What I Want Claude To Do
- [Task 1 — e.g., "Help me write case studies from my workflow files"]
- [Task 2 — e.g., "Review and improve my resume bullet points"]
- [Task 3 — e.g., "Help me write HTML/CSS for my portfolio pages"]

## How I Want Claude To Respond
- [Tone: e.g., "Direct, professional, no filler phrases"]
- [Format: e.g., "Use headers and bullet points for structure"]
- [Depth: e.g., "Go deep on technical accuracy, don't oversimplify"]
- [Honesty: e.g., "If something I made has a weakness, say it"]

## My Skill Level
[Be honest so Claude calibrates correctly.
E.g., "I know n8n and API integrations well.
I'm comfortable with HTML/CSS but not advanced JavaScript.
I'm new to GitHub and Netlify."]

## Important Rules
- [Rule 1 — e.g., "Never exaggerate my results or experience"]
- [Rule 2 — e.g., "Always base case study content on the actual files I upload"]
- [Rule 3 — e.g., "If I ask for code, write it so I can copy and use it directly"]

## My Files (what's uploaded)
- [File 1 name and what it is]
- [File 2 name and what it is]
- [Update this section when you add new files]
```

> **Tip:** My actual example — Portfolio & Case Study Project
> Here's how I filled this out for my own portfolio project:

```markdown
## Who I Am
Chelsea Andrea Dominguez — AI Workflow Designer & Automation Builder
Philippines, fully remote.
I build n8n automation workflows and AI-powered systems,
mostly for social media management and content operations.

## What This Project Is For
This project is for building my professional portfolio.
Specifically: writing case studies from my actual workflow files,
updating my resume and LinkedIn to match, and eventually
generating the HTML/CSS code for my portfolio website.

## What I Want Claude To Do
- Analyze my n8n workflow JSON files and help me understand them deeply
- Write technical case studies that are honest about both strengths and weaknesses
- Generate portfolio copy, resume bullets, and LinkedIn descriptions
- Help me write or improve HTML/CSS for my portfolio site

## How I Want Claude To Respond
- Be honest. If a system has limitations, say so clearly.
- Use professional but human language — not robotic, not overhyped
- When writing case studies, base everything on the uploaded files
- Structure longer outputs with headers so they're easy to read

## My Skill Level
Intermediate in n8n, automation, and AI tools.
Comfortable with HTML/CSS basics.
New to GitHub and Netlify — explain those steps clearly.

## Rules
- Never claim results I haven't actually achieved
- Always distinguish between rule-based logic and AI logic in my systems
- If asked for code, make it copy-paste ready

## Files Uploaded
- Resume (DOCX) — my current resume as of 2026
- Instagram Automation Workflow (JSON) — n8n workflow for Scroll & Tell
- Notion Portfolio PDF — my old Notion-based portfolio for reference
```

> **Note:** For your own topic (not a portfolio)
> The structure above works for any project. Replace the portfolio sections with whatever your project is. Consulting clients? Systems documentation? Content creation? The same framework applies — just change the "What This Project Is For" and "What I Want Claude To Do" sections.

### Step 3 — What to Upload in the Files Section

Files you upload to a Project become part of Claude's knowledge for every conversation inside it. Claude can read, analyze, and reference them without you needing to paste the content each time.

Think carefully about what Claude actually needs to help you. Here's the framework I use:

| File Type | What to upload | Why |
|-----------|----------------|-----|
| **Your Resume** | Your most current resume (DOCX or PDF) | Claude uses your actual experience when generating copy, bullets, or case study positioning |
| **Workflow Files** | n8n JSON exports, automation diagrams, or screenshots | Claude can read the JSON and understand exactly what the system does — no guessing |
| **Reference Portfolio** | Your old Notion export, previous portfolio PDF, or website screenshot | Gives Claude your existing structure and design preferences to build from |
| **SOPs / Docs** | Any process docs, SOPs, or notes about your systems | Useful for technical deep-dives or explaining how you work to Claude |
| **Project-Specific Ref** | Client briefs, project goals, or context documents | Helps Claude understand constraints and requirements for the specific project |

> **Info:** File format notes
> Claude reads DOCX, PDF, JSON, TXT, and most plain text formats. For n8n workflows, export as JSON — Claude can read the nodes and connections directly. For images or diagrams, PDF or PNG works best.

### Step 4 — Uploading Your Files

1. **Inside your project, find the Files section**
   It's usually below the Instructions box. Look for "Add files" or a paperclip/upload icon.

2. **Click "Add files" and select your files**
   You can upload multiple files. Wait for each one to finish processing before starting a conversation.

3. **Update your instructions when files change**
   Go back to your Instructions text and update the "Files Uploaded" section at the bottom. This keeps Claude oriented on what's currently available.

> **Tip:** Now you're ready to use the project
> Start a new conversation inside the project. Claude will have read your instructions and files before you type your first message. You can jump straight to: *"Analyze my n8n workflow and identify the three most important things to highlight in my case study."* No preamble needed.

---

## Part 02 — Writing the Case Study First

Most people build their portfolio first, then try to write the case study inside it. That's backwards. The case study is the source of truth. Your portfolio is just a container for it.

Write the case study first, and three things get easier: your portfolio page writes itself, your resume bullets become obvious, and your LinkedIn summary has something real to say.

**Progress:** ✅ Project Setup → 📄 Gather Materials → 🔄 Update LinkedIn & CV → ✍️ Write Case Study → 🌐 Build Portfolio

### Step 1 — Gather Your Raw Materials

Before writing anything, collect everything that proves the work happened. Claude can only write a credible case study if it has real evidence to work from.

- [x] Your actual workflow file — n8n JSON, flowchart, or screenshot of the full workflow
- [x] Any metrics you have — time saved, response rate, volume of messages handled
- [x] The problem you were solving — what existed before you built this? What broke?
- [x] The decisions you made — what did you try first? What did you change and why?
- [ ] Any feedback you received — from a client, collaborator, or yourself after testing
- [ ] Screenshots or recordings of the system working (even locally is fine)

> **Warning:** Don't have all of this?
> That's normal. Write down what you do know in a plain text file. Things like "I built this because the client was manually responding to every DM" count as real context. Upload that file to your Claude Project and work from there.

### Step 2 — Update LinkedIn and Your Resume Before You Write

This step catches most people off guard, but it's worth doing now. Your case study will generate new language for your experience. If you update LinkedIn and your resume after writing the case study, you get consistency across all three.

Here's the exact sequence that worked for me:

1. **Check your current LinkedIn "Experience" section**
   Does it mention the project you're writing a case study for? If yes, look at how you described it. If the language is vague ("helped with automation tasks"), that needs to change. If it's missing entirely, you'll add it after the case study is done.

2. **In your Claude Project, ask for LinkedIn-ready bullet points**
   Prompt example you can copy:
   ```
   Based on my resume and workflow file, write 4–5 LinkedIn
   experience bullet points for my role at [Company Name].

   Rules:
   - Start each bullet with a strong action verb
   - Include at least one specific metric or outcome per bullet
   - Be technically accurate — don't overstate what the system does
   - Keep each bullet to 1–2 lines maximum
   ```

3. **Update your resume with the same language**
   Consistency matters. A recruiter who sees your LinkedIn then opens your resume should read the same story told in the same way. Copy the finalized bullets into your resume doc in the same Claude Project.

4. **Update your LinkedIn "About" section if needed**
   Your About section should mention the type of work you've done without going into project specifics. Ask Claude: *"Write a 3-sentence LinkedIn About section for me based on my resume and the kind of work I do."*

> **Note:** Why update these before the case study is final?
> Because writing the case study forces you to understand your own work more clearly. You'll notice what actually mattered and what was just noise. Update your professional profiles with that clarity, not with your pre-case-study vagueness.

### Step 3 — Writing the Case Study With Claude

Now you're ready. Here's the exact process I use inside my Claude Project to generate a complete, honest case study from a real workflow.

1. **Start a new conversation inside your Project**
   Don't use a regular Claude chat for this. Open your Project and start a new conversation there. Claude will already have your files and instructions loaded.

2. **Run the system analysis prompt first**
   Before asking Claude to write anything, have it analyze your workflow. This grounds everything that follows in what the system actually does.
   ```
   Before writing the case study, analyze my workflow file
   and answer these questions:

   1. What are the three main layers of this system?
      (input, decision, intelligence)

   2. Which parts are rule-based vs AI-driven?

   3. What is stateful (remembers context) vs stateless?

   4. What is the single most interesting design decision
      I made in this system?

   5. Where are the three most likely failure points?

   Be specific and reference actual nodes or components
   from the workflow file.
   ```

3. **Read the analysis and add your context**
   Claude's analysis will be accurate technically, but it won't know *why* you made certain decisions. Reply with additional context: "The reason I used TinyLlama for classification instead of a larger model is because this runs locally and latency matters." This becomes part of the case study's depth.

4. **Run the full case study prompt**
   ```
   Now write a complete portfolio case study based on your
   analysis and my workflow file.

   Structure it exactly like this:

   1. HOOK — one sharp sentence that captures what this
      system does and why it's different

   2. PROBLEM — the real-world inefficiency this solves

   3. SOLUTION — accurate description of what I built
      (no exaggeration)

   4. ARCHITECTURE — explain the three layers:
      input, decision, and intelligence

   5. KEY INNOVATION — focus on the AI conversation
      design, not just the automation

   6. LIMITATIONS — be specific and honest about what
      breaks, what doesn't scale, what's fragile

   7. FUTURE IMPROVEMENTS — how this evolves into a
      production-grade system

   8. IMPACT — only include metrics I can actually prove

   Tone rules:
   - Write for a technical reviewer who will check the details
   - Acknowledge tradeoffs, not just wins
   - Use precise language — no vague claims

   Also generate at the end:
   - 3 resume bullet points (results-oriented)
   - 1 LinkedIn feature description (2–3 sentences)
   - 1 "Systems Thinking" paragraph (for portfolio intro)
   ```

5. **Review, push back, and iterate**
   Read the output critically. If something sounds exaggerated, tell Claude. If a section is too vague, ask for more detail. The best case studies come from 2–3 revision rounds, not the first output. Treat Claude as a co-writer who needs your feedback, not a one-shot generator.

### Step 4 — What You'll Have at the End of Part 2

- [x] A full case study document (ready to paste into your portfolio)
- [x] Updated resume bullets for this project
- [x] A LinkedIn feature description
- [x] A "Systems Thinking" paragraph for your portfolio intro
- [x] Updated LinkedIn Experience section
- [x] Updated resume document in your project files

> **Tip:** Save everything before moving on
> Copy the final case study to a Notion page, Google Doc, or text file. You'll reference it directly when building your portfolio in Part 3.

---

## Part 03 — Building Your Portfolio Site

You have two starting points: you already have a Notion portfolio and want to convert it to a real website, or you're starting from scratch. Both paths end at the same place — a live site on a custom URL, deployed through GitHub and Netlify.

**Path A — Migrating from Notion:** You have an existing Notion portfolio. You want to move it to a proper website with a custom URL, better design, and no Notion branding. This is what I did.

**Path B — Starting from Scratch:** You have your case study and resume but no existing portfolio site. You'll build the HTML/CSS with Claude's help directly, then deploy it.

### Path A — Migrating Your Notion Portfolio to HTML

1. **Export your Notion pages**
   In Notion, go to the three-dot menu (···) on your main portfolio page → **Export** → choose **PDF** or **HTML**. PDF is easier to share with Claude; HTML gives you a structural starting point. Export everything you want to keep.

2. **Upload the export to your Claude Project**
   Upload the Notion export (PDF or HTML) to the Files section of your Project. This gives Claude your existing structure, sections, and content to work from.

3. **Ask Claude to reconstruct it as clean HTML/CSS**
   ```
   I'm migrating my Notion portfolio to a standalone HTML website.
   I've uploaded my Notion export as a PDF/file.

   Please:
   1. Read the structure and content from my Notion export
   2. Rebuild it as clean, single-file HTML with embedded CSS
   3. Keep the same sections but make the design more professional
   4. Use my case study content (also uploaded) for the project section
   5. Make it mobile-responsive

   Design preferences:
   - [Dark/Light theme? Minimal or detailed?]
   - [Any colors you prefer or want to avoid?]
   - [Reference sites you like the style of, if any]

   Output a single index.html file I can use directly.
   ```

4. **Review, request changes, iterate**
   Claude will generate the HTML. Read it in a browser (save the file, double-click to open). List what needs changing. *"Make the header bigger. The project cards need more spacing. Change the font to something cleaner."* Keep iterating until it looks right.

### Path B — Building From Scratch

1. **Decide your sections before asking Claude to code**
   A portfolio for a VA/automation specialist needs at minimum:
   - Hero section (name, title, one-line summary)
   - About / skills section
   - Projects / case studies section
   - Experience / background section
   - Contact / links section

2. **Use this prompt in your Claude Project**
   ```
   Build me a professional portfolio website as a single HTML file.

   Use all context from my uploaded resume, case study, and files.

   Sections needed:
   - Hero: name, title, short summary
   - About + Key Skills
   - Projects (feature my case study prominently)
   - Experience & Education
   - Certifications
   - Contact (email + LinkedIn + GitHub links)

   Requirements:
   - Single index.html file with CSS embedded in <style> tags
   - Mobile responsive
   - Professional but not boring
   - Load fast (no external libraries unless necessary)

   My info is in the uploaded files — pull from those directly.
   Don't invent experience I don't have.
   ```

3. **Save the output as index.html and preview it**
   Copy Claude's HTML output. Open a text editor (Notepad, VS Code, or any editor). Paste and save as `index.html`. Open the file in your browser to preview. Make a list of things to fix and iterate with Claude.

### Setting Up GitHub (Required for Netlify)

Netlify deploys your site from a GitHub repository. You don't need to know how to code to use GitHub for this. Think of it as a folder in the cloud that Netlify watches and publishes automatically.

1. **Create a GitHub account if you don't have one**
   Go to **github.com** → Sign up. Use a professional email. Your username will appear in your repo URL so keep it clean (e.g., `chelsea-dominguez` not `cjd_coolgal99`).

2. **Create a new repository**
   Click the green **New** button or go to `github.com/new`. Name it something like `portfolio` or `my-portfolio`. Set it to **Public**. Check "Add a README file." Click **Create repository**.

3. **Upload your index.html file**
   Inside your new repo, click **Add file** → **Upload files**. Drag and drop your `index.html` file. Scroll down, write a commit message like *"Initial portfolio upload"*, and click **Commit changes**.

4. **Confirm the file is there**
   You should now see `index.html` listed in your repository. That's all GitHub needs. You can always update the file here later by uploading a new version — it will replace the old one.

### Deploying to Netlify

Netlify takes your GitHub repo and publishes it as a live website. Every time you push a change to GitHub, Netlify automatically updates the live site.

1. **Go to netlify.com and create an account**
   Sign up with your GitHub account — this makes the connection much easier. Click **Sign up with GitHub** and authorize Netlify.

2. **Click "Add new site" → "Import an existing project"**
   Choose **Deploy with GitHub**. You'll see a list of your repositories. Select the portfolio repo you just created.

3. **Configure the deploy settings**
   For a basic HTML site, you don't need to change anything here. The defaults work:
   - **Branch:** main
   - **Build command:** leave empty
   - **Publish directory:** leave empty (or type `/`)
   Click **Deploy site**.

4. **Wait about 30 seconds for the deployment**
   Netlify will show a spinning indicator, then a green checkmark with a URL like `random-name-123.netlify.app`. Click it — your site is live.

5. **Rename your Netlify subdomain**
   Go to **Site settings** → **Domain management** → **Options** next to the Netlify subdomain → **Edit site name**. Change it to something like `chelseaandrea-dominguez`. Your URL becomes `chelseaandrea-dominguez.netlify.app`.

> **Tip:** Optional: Custom domain (e.g., chelseaandrea.dev)
> If you want a custom domain like `yourname.com` instead of `.netlify.app`, buy one from Namecheap or Porkbun (usually $10–15/year). In Netlify → Domain management → Add custom domain. Follow the DNS instructions Netlify gives you — it takes 10–30 minutes to go live. This step is optional but adds credibility.

> **Note:** Updating your site going forward
> Whenever you update your portfolio, edit your `index.html` file locally, then go to your GitHub repo → Add file → Upload files → upload the new version → commit. Netlify detects the change and redeploys within 1 minute. No code commands needed.

---

## Part 04 — Optional: Getting Indexed in Google Search

By default, Google doesn't know your site exists. It will eventually find it through links, but that can take weeks or months. Google Search Console lets you submit your site directly so it gets indexed faster and shows up when someone searches your name.

> **Info:** What "indexed" means
> Google crawls websites and adds them to its database (the index). Only indexed pages can appear in search results. This process submits your portfolio directly to that database instead of waiting for Google to find it on its own.

### Step-by-Step: Google Search Console

1. **Go to search.google.com/search-console**
   Sign in with your Google account. You'll see a "Welcome" screen if this is your first time.

2. **Click "Add property" and choose "URL prefix"**
   Choose the **URL prefix** option (not Domain). Type your full Netlify URL: `https://yourname.netlify.app`. Click Continue.

3. **Verify ownership via HTML tag**
   Google will give you a verification method. Choose **HTML tag**. You'll get a small code snippet that looks like:
   `<meta name="google-site-verification" content="abc123..." />`
   Copy that code.

4. **Paste the tag into your index.html**
   Open your `index.html` file. Find the `<head>` section near the top. Paste the meta tag inside `<head>` and before `</head>`. Save the file, upload it to GitHub (same steps as before). Wait 1–2 minutes for Netlify to redeploy.

5. **Back in Search Console, click "Verify"**
   Google checks for the tag on your live site. It should say **Verified**. If it fails, wait another minute and try again — Netlify may still be deploying.

6. **Submit your sitemap (or just submit your URL)**
   In the Search Console left sidebar, click **URL Inspection**. Type your full URL. Click **Request Indexing**. Google will queue your page for crawling. This usually happens within 24–72 hours.

7. **Test that it worked**
   After a day or two, search Google for `site:yourname.netlify.app`. If results appear, your page is indexed. You can also search your own full name — if your portfolio appears in the first page, the whole process worked.

> **Warning:** Don't expect it to rank immediately
> Indexing and ranking are different. Being indexed means Google knows about you. Ranking means Google shows you for specific searches. Your name will likely rank well quickly — keyword-based searches take longer. That's fine for a portfolio.

> **Tip:** Speed up indexing: get one backlink
> The fastest way to get indexed is to have one other site link to yours. Add your portfolio URL to your LinkedIn profile, your GitHub profile README, or any social bio. Google finds new sites by following links from known sites. LinkedIn and GitHub are both highly trusted by Google.

---

## What You Now Have

- [x] A Claude Project set up with your files, instructions, and context — ready for any future work
- [x] A complete, honest case study written from your actual workflow files
- [x] Updated resume bullets and LinkedIn experience section
- [x] A live portfolio site deployed at a public URL
- [x] Your site submitted for Google indexing

> **Note:** This is a repeatable system
> Every time you build a new system or complete a new project, you follow the same process: document it → run it through Claude → write the case study → update LinkedIn and resume → add a new page to the portfolio → resubmit to Google. The Claude Project stays open and keeps growing with you.

---

*Chelsea Andrea Dominguez · Systems Manual · v1.0 · April 2026*
