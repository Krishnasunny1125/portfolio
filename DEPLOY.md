# Krishna Vamsi Portfolio — Netlify Deployment

Everything in one folder. No separate backend server.
Frontend + AI chat function deploy together automatically.

## Folder structure

```
portfolio_netlify/           ← drag-and-drop this entire folder to Netlify
├── netlify.toml             ← tells Netlify: functions folder, redirects, headers
├── package.json             ← one dependency: @google/generative-ai
├── .gitignore
│
├── netlify/
│   └── functions/
│       └── chat.js          ← the entire AI backend (serverless function)
│
├── index.html
├── about.html
├── contact.html             ← chat calls /api/chat (same domain, no CORS)
├── projects.html
├── skills.html
├── resume.html
├── style.css
└── resume.pdf               ← ADD YOUR PDF HERE before deploying
```

─────────────────────────────────────────────────────
STEP 1 — Add your resume PDF
─────────────────────────────────────────────────────

Copy your resume PDF into the folder and name it exactly:
  resume.pdf

─────────────────────────────────────────────────────
STEP 2 — Deploy to Netlify (2 ways)
─────────────────────────────────────────────────────

OPTION A: Drag-and-drop (no Git required — 30 seconds)
  1. Go to https://app.netlify.com
  2. Click "Add new site" → "Deploy manually"
  3. Drag the entire portfolio_netlify/ folder into the upload area
  4. Netlify deploys it instantly with a random URL like:
       https://amazing-tesla-abc123.netlify.app

OPTION B: GitHub (recommended for updates)
  1. Create a new GitHub repo
  2. Push the portfolio_netlify/ folder contents as the root:
       cd portfolio_netlify
       git init
       git add .
       git commit -m "initial deploy"
       git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
       git push -u origin main
  3. In Netlify: "Add new site" → "Import from Git" → connect the repo
  4. Build settings (Netlify auto-detects from netlify.toml — leave defaults)

─────────────────────────────────────────────────────
STEP 3 — Add Gemini API key in Netlify (REQUIRED)
─────────────────────────────────────────────────────

  1. In your Netlify dashboard → Site → Site configuration
  2. Click "Environment variables" → "Add a variable"
  3. Add:
       Key:   GEMINI_API_KEY
       Value: your_actual_gemini_key_here

  4. ALSO add:
       Key:   ALLOWED_ORIGIN
       Value: https://your-site-name.netlify.app
              (or your custom domain if you have one)

  5. Click "Deploy site" → "Trigger deploy" to redeploy with the key

Get a free Gemini key: https://aistudio.google.com/app/apikey
Free tier: 15 requests/min, 1 million tokens/day — plenty for a portfolio.

─────────────────────────────────────────────────────
STEP 4 — (Optional) Add a custom domain
─────────────────────────────────────────────────────

  Netlify dashboard → Site → Domain management → Add custom domain
  → Follow the DNS instructions for your registrar

─────────────────────────────────────────────────────
How it works (for reference)
─────────────────────────────────────────────────────

  Browser
    │  POST /api/chat  (same domain, no CORS)
    ▼
  Netlify CDN
    │  netlify.toml redirect: /api/chat → /.netlify/functions/chat
    ▼
  netlify/functions/chat.js  (serverless, runs on AWS Lambda)
    │  GEMINI_API_KEY from Netlify env vars (never in browser)
    ▼
  Gemini 1.5 Flash API
    │
    ▼
  { reply } → back to browser

─────────────────────────────────────────────────────
Updating the site
─────────────────────────────────────────────────────

  Option A (drag-and-drop): re-drag the folder to Netlify
  Option B (GitHub):        git push → Netlify auto-redeploys in ~30s
