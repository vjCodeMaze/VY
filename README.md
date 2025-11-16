# 🚀 Auto-Docs Agent — Automatic Documentation Updating with AI

Auto-Docs Agent is a GitHub Action that automatically updates your project documentation whenever you push code. It analyzes git diffs, scans your documentation folder, uses an AI model (OpenAI, Gemini, Groq, Claude, or xAI Grok) to rewrite only the relevant sections, and opens a pull request with updated docs. Developers simply commit code — documentation stays in sync on its own.

---

# ✅ Step-by-Step Guide (How Any User Can Use This)

### **STEP 1 — Add the workflow file**
Create:

```
.github/workflows/auto-docs.yml
```

Paste:

```yaml
name: Auto Docs Agent

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  auto-docs:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Run Auto Docs Agent
        uses: vjCodeMaze/auto-docs-agent@v1
        with:
          llm_api_key: ${{ secrets.LLM_API_KEY }}
          llm_model: "gpt-4o-mini"

      - name: Create Pull Request
        uses: peter-evans/create-pull-request@v6
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          commit-message: "Auto-update documentation"
          title: "📘 Auto-update documentation"
```

---

### **STEP 2 — Add your LLM API key**
Go to:

```
Repo → Settings → Secrets → Actions → New Secret
```

Create:

```
Name: LLM_API_KEY
Value: <your AI provider API key>
```

Supports:  
OpenAI, Gemini, Groq, Claude, Grok.

---

### **STEP 3 — Ensure documentation exists**
Default docs folder is:

```
docs/
```

Place your `.md`, `.mdx`, `.markdown` inside:

```
docs/
  getting-started.md
  api/authentication.md
  guide/notifications.md
```

If your docs live elsewhere:

```yaml
with:
  docs_path: "documentation"
```

---

### **STEP 4 — Push your code normally**
Run:

```
git add .
git commit -m "any message"
git push
```

Even if you write “2nd commit” or “update”, Auto-Docs still works.

Because it uses the **actual git diff**, not the commit message.

---

### **STEP 5 — Watch the GitHub Action run**
Go to:

```
Actions → Auto Docs Agent
```

You will see:

- Code diff detected  
- Docs scanned  
- AI updates generated  
- PR created  

---

### **STEP 6 — Review the Pull Request**
A PR will appear automatically:

```
📘 Auto-update documentation
```

Click "Merge" if everything looks correct.

---

### **STEP 7 — Done! Docs stay updated forever.**

No manual writing.  
No outdated docs.  
No forgetting.  
Auto-Docs takes care of everything.

---

# 🌟 Why Auto-Docs?

Engineering teams move fast and documentation becomes outdated quickly. Features evolve → docs remain old → onboarding slows → bugs increase.  
Auto-Docs Agent fixes this by automatically rewriting documentation whenever code changes happen.

---

# ⚙️ How It Works

```
Developer pushes code → GitHub Action runs
        ↓
Extract git diff (main...HEAD)
        ↓
Scan docs/ folder for Markdown files
        ↓
AI analyzes diff + docs
        ↓
Rewrite only the affected documentation sections
        ↓
Commit changes → open a Pull Request
```

Commit messages do NOT matter.  
Even if the commit message is:

```
"2nd commit"
"update"
"asdfg"
```

Auto-Docs still works — because detection is based on **code diff**, not commit text.

---

# 🧩 Supported LLM Providers

| Provider | Example Model | Prefix |
|---------|----------------|--------|
| **OpenAI** | gpt-4o-mini, gpt-4o | `gpt-` |
| **Google Gemini** | gemini-1.5-flash | `gemini-` |
| **Anthropic Claude** | claude-3-sonnet | `claude-` |
| **Groq / Llama 3** | llama3-70b | `llama-` / `groq-` |
| **xAI Grok** | grok-beta | `grok-` |

⚡ Provider is auto-detected from model prefix.

---

# 🧠 Intelligent Doc Updating

Auto-Docs Agent:

- Reads every `.md`, `.mdx`, `.markdown` file in `docs/`
- Extracts keywords from code diffs
- Matches relevant documentation using heuristics
- Updates only the needed sections
- Preserves formatting, headings, tone, and structure
- Creates clean PRs ready for merge

---

# 📝 Example Scenario

Developer pushes:

```
"my second commit"
```

Diff:
```js
console.log("Added payment processing integration");
```

AI extracts:

```
payment, processing, integration
```

It updates:

- docs/payments.md  
- docs/billing.md  

Commit message does NOT matter — only code matters.

---

# 🧰 Inputs Supported

| Input | Description | Default |
|-------|-------------|---------|
| `llm_api_key` | Your LLM provider key | required |
| `llm_model` | Model name | gpt-4o-mini |
| `docs_path` | Custom docs folder | docs |

---

# 📤 Output (Pull Request)

Auto-Docs opens a PR:

```
📘 Auto-update documentation

This PR updates 2 documentation files based on recent changes.
Files updated:
- docs/api/payments.md
- docs/integrations/gateway.md
```

---

# 🛠 Troubleshooting

### ❗ “No diff detected”
Ensure:
```
fetch-depth: 0
```

### ❗ “Invalid API key”
Check provider prefix:
- OpenAI: `sk-`
- Gemini: `AI…`
- Groq: `gsk_...`
- Grok: `xai-...`
- Claude: `claude-...`

### ❗ “No documentation found”
- Create a `docs/` folder  
- OR configure: `docs_path: "your-folder"`

---

# 🚨 Limitations

- Only Markdown docs supported  
- Requires GitHub Actions  
- LLM accuracy varies  
- Cannot update before commit  

---

# 🏆 Pitch (For Hackathon)

Auto-Docs Agent solves documentation drift forever. It rewrites documentation automatically whenever code changes. It reads diffs, matches docs, updates only relevant sections using AI, and creates PRs. It supports OpenAI, Gemini, Groq, Claude, and Grok. With only one workflow file and one secret, any developer can enable self-updating documentation.

---

# ⭐ Final Note

Users need only:

1. One workflow file  
2. One secret  
3. Push code  

Documentation stays updated forever.

⭐ Star this repo if you like this project!
