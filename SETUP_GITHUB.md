# GitHub Setup Instructions

## 1. Create Repository on GitHub

1. Go to https://github.com/new
2. Repository name: `solana-deep-forensic-demo`
3. Description: `Solana token forensic analysis examples and documentation`
4. Choose **Public** (recommended for visibility)
5. **DO NOT** check "Initialize with README"
6. Click "Create repository"

---

## 2. Push Local Files

After creating the repo on GitHub, run these commands in this folder:

```bash
# Initialize git (if not already done)
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit: Solana deep forensic documentation"

# Add remote (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/solana-deep-forensic-demo.git

# Push to GitHub
git branch -M main
git push -u origin main
```

---

## 3. Verify

Visit: https://github.com/YOUR_USERNAME/solana-deep-forensic-demo

You should see all files with proper formatting.

---

## 4. Post-Setup

- Pin the repo to your GitHub profile
- Update your Twitter bio with the GitHub link
- Start sharing the repo in relevant communities
