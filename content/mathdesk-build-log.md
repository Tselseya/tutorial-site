# MathDesk — Full Build Log
**Project:** MathDesk (math learning web app)
**Builder:** Chelsea Andrea Dominguez (Tselseya)
**AI Co-builder:** Claude (Anthropic)
**Session span:** Multi-session build from concept to live deployment
**Live URLs:**
- https://mathdesk.page.gd (primary — InfinityFree)
- https://tselseya.github.io/MathDesk (backup — GitHub Pages)

---

## Project Summary

MathDesk is a free, open math learning web app for students and the general public. It combines a curated directory of math tools/resources with an AI-powered chatbox (powered by Groq + n8n) that solves problems step-by-step, explains lessons, and generates practice problems. No account required. No paywall. No ads.

---

## Phase 1 — Discovery & Scoping

### What Chelsea wanted
- A website/app about mathematics to help students
- Two main sections: curated tools/websites + AI chatbox
- AI chatbox inspired by: Recall.it, NotebookLM, Claude, ChatGPT, Photomath
- Users can upload lessons → AI generates practice problems
- Users can upload/paste problems → AI solves with explanation
- Optional login to save progress (no account required to use)
- Name: **MathDesk**
- AI built in n8n

### Key decisions made
| Decision | Choice | Reason |
|---|---|---|
| Primary audience | Both students + general public | Broadest reach |
| Math levels | All levels (arithmetic to university) | No restrictions |
| AI API | Groq (free tier) | 14,400 req/day free, no credit card |
| Auth/DB | Supabase (free tier) | Later phase |
| Donations | Ko-fi | Zero platform fees |
| Frontend deployment | GitHub Pages + InfinityFree | Free, permanent |
| n8n | Self-hosted (already running) | Already connected via MCP |

### Curated tools list (locked in)
Photomath, Desmos, WolframAlpha, Paul's Online Math Notes, OpenStax, Khan Academy, GeoGebra, Symbolab, Mathway, LibreTexts, 3Blue1Brown, Integral Calculator, Matrix Calculator, Mathplanet, Brilliant.org

---

## Phase 2 — n8n Workflow Build

### Workflow: MathDesk AI Chat v3
- **ID:** `8v9cMQV7weB7sh8U`
- **Webhook path:** `/webhook/mathdesk-ai`
- **LLM:** Groq — `llama-3.3-70b-versatile`
- **Status:** Active (Published)

### Architecture
```
Webhook (POST /mathdesk-ai)
  → Route by Mode (Switch node)
    ├─ solve    → Build Solve Body → Groq: Solve → Format Solve JSON → Respond: Solve
    ├─ learn    → Build Learn Body → Groq: Learn → Format Learn JSON → Respond: Learn
    ├─ practice → Build Practice Body → Groq: Practice → Format Practice JSON → Respond: Practice
    └─ deskbot  → Build Deskbot Body → Groq: Deskbot → Respond: Deskbot
```

### Mode details
| Mode | Purpose | Output format |
|---|---|---|
| solve | Step-by-step problem solving | JSON: answer, steps, explanation, hint, practice |
| learn | Concept explanations with examples | JSON: answer, steps, explanation, hint, practice |
| practice | Generate 5 problems by difficulty | JSON: explanation, practice (2 easy, 2 med, 1 hard) |
| deskbot | FAQ assistant for site usage | Plain text (2-4 sentences) |

### Groq credential config
- Credential name in n8n: `Groq`
- Type: HTTP Header Auth
- Header: `Authorization: Bearer YOUR_GROQ_KEY`

### Key bugs fixed during build
1. **`$env` blocked in Code nodes** — n8n security restriction, switched to HTTP Request nodes with credential injection
2. **`geminiBody` bug** — Learn, Practice, Deskbot nodes were sending `$json.geminiBody` (wrong field), only Solve worked. Fixed to `$json.groqBody`
3. **Respond node empty body** — `respondWith: 'json'` with expression template broke; switched to `respondWith: 'text'` returning `formattedText`
4. **Version drift** — active workflow version was older than draft; fixed by republishing
5. **CORS missing** — `allowedOrigins: '*'` was not in active version; fixed
6. **`$env.GEMINI_API_KEY` access denied** — Code nodes cannot read env vars in this n8n setup; switched to credential-based HTTP Request nodes
7. **Docker env var not loading** — `N8N_GENERIC_TIMEZONE: Asia/Manila` used wrong YAML syntax (colon instead of equals); fixed to `=`

### Workflow evolution
- **v1** — Gemini API via Code nodes with `$env.GEMINI_API_KEY` (failed: env access denied)
- **v2** — Gemini API via HTTP Request nodes with credential (partially worked)
- **v3** — Switched to Groq API, all modes fixed, text/plain responses (working)

---

## Phase 3 — Frontend Build

### Stack
- Single HTML file (`index.html`)
- HTML + CSS + vanilla JavaScript
- No build step, no framework, no dependencies (except Supabase SDK + Desmos/GeoGebra iframes)

### Key features implemented
- **AI Chatbox** — 3 modes (Solve, Learn, Practice) + Deskbot FAQ bot
- **Tools directory** — 15 curated math resources with category filter
- **Math symbols panel** — 5 categories: Basic, Algebra, Calculus, Greek, Sets
- **Graphing calculator** — embedded GeoGebra iframe in modal
- **Image upload** — base64 encoded, sent to n8n workflow
- **Ctrl+V paste** — paste screenshots directly into chat
- **Drag and drop** — drop images anywhere on page
- **Pending image preview** — thumbnails above input with × remove button (like Claude/ChatGPT)
- **Deskbot** — floating FAQ chatbot (bottom right corner)
- **Login/Sign Up modal** — wired to Supabase (Phase 7)
- **Ko-fi donate button** — floating, bottom right (Phase 8)
- **System dark/light mode** — respects OS preference
- **Responsive** — works on mobile and desktop

### Webhook integration
```javascript
const N8N_WEBHOOK_URL = 'https://overwilling-janina-unsatisfactorily.ngrok-free.dev/webhook/mathdesk-ai';
```
Response: `text/plain` — frontend uses `res.text()` not `res.json()`

### callN8N function
```javascript
async function callN8N(message, mode, extraData = {}) {
  const controller = new AbortController();
  const timeout = setTimeout(() => controller.abort(), 30000);
  try {
    const res = await fetch(N8N_WEBHOOK_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ message, mode, ...extraData }),
      signal: controller.signal
    });
    clearTimeout(timeout);
    if (!res.ok) throw new Error('n8n returned ' + res.status);
    return await res.text();
  } catch (e) {
    clearTimeout(timeout);
    if (e.name === 'AbortError') throw new Error('Request timed out after 30 seconds.');
    throw e;
  }
}
```

---

## Phase 4 — Deployment

### GitHub
- **Repo:** https://github.com/Tselseya/MathDesk
- **Branch:** main
- **File:** index.html
- **Note:** Git push via terminal blocked (port 443 issue on local network). Use browser upload: repo → index.html → pencil edit → commit.

### InfinityFree (primary hosting)
- **URL:** https://mathdesk.page.gd
- **Domain:** mathdesk.page.gd (free .gd domain)
- **Deploy method:** File Manager → htdocs → upload index.html
- **Account:** chelseandrea99@gmail.com

### GitHub Pages (backup hosting)
- **URL:** https://tselseya.github.io/MathDesk
- **Deploy method:** Automatic from main branch

### Cloudflare (considered, deferred)
- Cloudflare Tunnel considered for stable webhook URL
- Deferred — currently using ngrok
- **Risk:** ngrok URL resets on Docker restart; update `N8N_WEBHOOK_URL` in HTML when this happens
- **Future plan:** Cloudflare Tunnel with free subdomain (e.g. freedns.afraid.org)

### Google Search Console
- **Property added:** https://mathdesk.page.gd
- **Verification:** HTML file upload to InfinityFree htdocs
- **Sitemap:** sitemap.xml submitted

---

## Phase 5 — Claude Project Setup

### Project created
- **Name:** MathDesk
- **Files added:** `MathDesk AI Chat v3.json`, `mathdesk-workflow-notes.md`, `index.html`
- **Instructions:** Full context including tech stack, webhook URL, workflow structure, deployment details, rules for making edits

### Files in project
| File | Purpose |
|---|---|
| `index.html` | Current working frontend — update on major changes |
| `MathDesk AI Chat v3.json` | n8n workflow export |
| `mathdesk-workflow-notes.md` | Workflow architecture, credential names, known fixes |

---

## Phase 7 — Supabase Auth

### Status: Added to HTML, Supabase project not yet created

### What was implemented
- Login with email + password
- Signup with name + email + password
- Session persistence (auto-login on page refresh)
- Logout with account menu
- Chat history auto-saved to Supabase after every AI response (logged-in users only)
- Last 5 chats loaded into sidebar on login
- Graceful degradation — site works fully for guests if Supabase not configured

### Supabase setup (still needed)
1. Create project at supabase.com (free tier)
2. Get Project URL and anon key from Settings → API
3. Run this SQL in Supabase SQL Editor:

```sql
create table chat_history (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users(id) on delete cascade,
  mode text,
  user_message text,
  ai_reply text,
  created_at timestamptz default now()
);

alter table chat_history enable row level security;

create policy "Users can manage their own chats"
on chat_history for all
using (auth.uid() = user_id);
```

4. Update in index.html:
```javascript
const SUPABASE_URL = 'https://YOUR_PROJECT.supabase.co';
const SUPABASE_ANON_KEY = 'YOUR_ANON_KEY';
```

### Known fix needed
Two console errors appear until fixed:
- `initSupabase is not defined` — move Supabase SDK script tag to bottom of body
- `Identifier 'supabase' has already been declared` — remove duplicate `let supabase = null;`

Fix: In index.html, find and delete the `<script>` tag with `onload="initSupabase()"` from `<head>`, then add before `</body>`:
```html
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/dist/umd/supabase.min.js"></script>
<script>
  window.addEventListener('load', function() {
    if (typeof window.supabase !== 'undefined') {
      initSupabase();
    }
  });
</script>
```

---

## Phase 8 — Ko-fi Donations

### Status: Added to HTML

### What was implemented
- Floating red "Support MathDesk" button, bottom-right (above Deskbot)
- Opens Ko-fi page in new tab
- Hover animation

### Still needed
1. Create Ko-fi account at ko-fi.com
2. Replace `YOUR_KOFI_USERNAME` in index.html with your real Ko-fi username

---

## Remaining Roadmap

| Phase | Description | Status |
|---|---|---|
| Phase 6 | Stable webhook — Cloudflare Tunnel with free subdomain | Deferred |
| Phase 7 | Supabase auth + chat history | Code added, Supabase not yet created |
| Phase 8 | Ko-fi donations | Code added, username not yet set |
| Phase 9 | Growth — Search Console monitoring, social sharing | Search Console submitted |

---

## Docker + n8n Environment

### docker-compose.yml key env vars
```yaml
- N8N_GENERIC_TIMEZONE=Asia/Manila
- GEMINI_API_KEY=...        # kept for reference, not used by MathDesk v3
- WEBHOOK_URL=https://overwilling-janina-unsatisfactorily.ngrok-free.dev
- N8N_EDITOR_BASE_URL=https://overwilling-janina-unsatisfactorily.ngrok-free.dev
```

### Important notes
- n8n runs in Docker on Windows (self-hosted-ai-starter-kit)
- Exposed via ngrok free tier (URL changes on restart)
- `$env` variables are NOT accessible inside n8n Code nodes (security restriction)
- All credentials must be stored in n8n credential store, not env vars
- Postgres is used as n8n database backend

---

## Key Lessons Learned

1. **n8n Code nodes cannot access `$env`** — use n8n credential store instead
2. **SDK body fields may not persist** — configure HTTP Request body manually in n8n UI when SDK validation warns
3. **Publish after every update** — n8n keeps draft and active versions separate; always publish or the live version won't update
4. **`respondWith: 'text'`** is the most reliable Respond to Webhook mode for this setup
5. **GitHub push via terminal blocked** — use browser upload for all deployments
6. **YAML syntax matters** — `- KEY: value` is wrong in env lists, must be `- KEY=value`
7. **ngrok URL is temporary** — update `N8N_WEBHOOK_URL` in HTML every time Docker restarts until Cloudflare Tunnel is set up

---

## Contact & Accounts

| Service | Account |
|---|---|
| GitHub | github.com/Tselseya |
| InfinityFree | chelseandrea99@gmail.com |
| Google Search Console | chelseandrea99@gmail.com |
| n8n | Self-hosted (local Docker) |
| Groq | (get key at console.groq.com) |
| Supabase | (create at supabase.com) |
| Ko-fi | (create at ko-fi.com) |

---

*Build log generated from full conversation history. Last updated: May 2026.*
