# Souvik Samanta — Resume Deployment Guide

## File Structure

```
your-repo/
├── Souvik_Samanta_Resume.html    ← the resume
├── netlify.toml                   ← Netlify config
└── netlify/
    └── functions/
        └── chat.js                ← serverless function (API key lives here)
```

## Step-by-Step Deployment

### 1. Create a GitHub repo
- Go to github.com → New repository
- Name it: `souvik-resume`
- Set to Private (recommended)
- Upload all three files maintaining the folder structure above

### 2. Connect to Netlify
- Go to app.netlify.com → "Add new site" → "Import an existing project"
- Connect your GitHub account
- Select the `souvik-resume` repo
- Build settings will auto-detect from netlify.toml
- Click "Deploy site"

### 3. Set the API key as an environment variable
- In Netlify dashboard → Site configuration → Environment variables
- Click "Add a variable"
- Key:   `OPENAI_API_KEY`
- Value: `sk-...` (your actual OpenAI key)
- Click Save
- Go to Deploys → "Trigger deploy" → "Deploy site"

### 4. Set your custom domain (optional)
- Netlify dashboard → Domain management
- Change site name to: `souvik-resume`
- Your resume will be live at: https://souvik-resume.netlify.app

### 5. Set a spending limit on OpenAI (important)
- Go to platform.openai.com → Settings → Billing → Usage limits
- Set monthly hard limit to $10
- This protects you even if the key were ever exposed

---

## Updating the Resume

1. Edit `Souvik_Samanta_Resume.html` — the chatbot auto-reads the new content
2. Push to GitHub → Netlify auto-deploys in ~30 seconds
3. No other files need to change

## How the Chatbot Works

```
Recruiter asks question
        ↓
Browser scrapes resume text from live HTML DOM (~3,600 tokens)
        ↓
Browser sends {systemPrompt, messages} to /.netlify/functions/chat
        ↓
Netlify function adds the secret API key and calls OpenAI GPT-4o
        ↓
OpenAI response comes back through the function to the browser
        ↓
API key is never visible in the browser at any point
```

## Security

- API key lives ONLY in Netlify environment variables
- The Netlify function only accepts POST requests
- CORS is restricted to souvik-resume.netlify.app only
- Set a spend limit on OpenAI as a backup safety net
