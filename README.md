# Ledger — Personal Expense Tracker

A single-file expense tracker that stores your data as a JSON file inside your own GitHub repo. No backend, no database service, no monthly cost — host it free on GitHub Pages and edit your finances from any device.

## How it works

- The whole app is one `index.html` file (HTML + CSS + JS, no build step).
- When you add or delete an entry, it calls the GitHub API directly from your browser and commits the updated data to `expenses.json` in this same repo.
- Your GitHub username, repo name, and a Personal Access Token are entered once in the Settings modal and saved only in your browser's `localStorage` — never written into any file you commit.

## 1. Create the repo

```bash
# from this folder
git init
git add .
git commit -m "Initial commit: expense ledger"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

## 2. Enable GitHub Pages

1. Go to your repo on GitHub → **Settings** → **Pages**
2. Under "Build and deployment", set **Source** to `Deploy from a branch`
3. Branch: `main`, folder: `/ (root)` → **Save**
4. Wait ~1 minute, then your site is live at:
   `https://<your-username>.github.io/<repo-name>/`

## 3. Create a Personal Access Token (PAT)

Use a **fine-grained token** scoped to only this repo — not a classic token with broad access.

1. GitHub → click your profile photo → **Settings**
2. **Developer settings** (bottom of left sidebar) → **Personal access tokens** → **Fine-grained tokens**
3. **Generate new token**
4. Set:
   - **Repository access**: "Only select repositories" → choose this repo
   - **Permissions** → **Repository permissions** → **Contents**: set to **Read and write**
5. Generate, then **copy the token immediately** (you won't see it again)

## 4. Connect the app

1. Open your live GitHub Pages URL
2. The Settings modal opens automatically on first visit
3. Enter your GitHub username, the repo name, and the token
4. Click **Connect**

That's it — every add/delete now syncs straight to `expenses.json` in your repo, and you can open the same URL on your phone, laptop, anywhere, paste the same token, and see the same data.

## Security notes (read this)

- The token lives only in your browser's local storage. It is **never** committed to the repo or sent anywhere except `api.github.com`.
- Because it's a static site, the token is technically visible to anyone with access to that browser's devtools/storage. That's fine for a personal tool on your own devices — just don't use this on a shared/public computer, and don't use a broad classic token.
- If a token ever leaks, revoke it instantly from GitHub → Settings → Developer settings → Personal access tokens.
- Every change is a real git commit, so your full expense history is recoverable from the repo's commit log even if something gets accidentally deleted in the app.

## Editing the data directly

Since it's just a JSON file, you can also open `expenses.json` in GitHub's web editor and tweak entries by hand if you ever need to. Format:

```json
[
  {
    "id": 1719400000000,
    "date": "2026-06-25",
    "desc": "Groceries",
    "category": "Food",
    "amount": 850.00,
    "type": "expense"
  }
]
```

`type` is either `"expense"` or `"income"`.
