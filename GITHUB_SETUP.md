# 🚀 Quick Start: How to Push to GitHub

This guide will help you share your knowledge base on GitHub - step by step!

## 📋 Prerequisites

1. **GitHub Account** - Create one at [github.com](https://github.com/signup) (free!)
2. **Terminal/Command Line** - Already have this on your Mac!

---

## 🎯 Step-by-Step Instructions

### Step 1: Initialize Git Repository

```bash
# Navigate to the knowledge-base directory
cd /Users/ekene.ejike/Tools/knowledge-base

# Initialize git (creates .git folder)
git init

# Check status (see what files are ready)
git status
```

### Step 2: Add Your Files

```bash
# Add all knowledge base files
git add .

# Check what's been staged
git status
```

### Step 3: Create Your First Commit

```bash
# Commit with a message describing what you're adding
git commit -m "Initial commit: Linux Fundamentals for beginners"
```

### Step 4: Create GitHub Repository

**Option A: Using GitHub Website (Easier for Beginners)**

1. Go to [github.com/new](https://github.com/new)
2. Fill in:
   - **Repository name:** `cybersecurity-knowledge-base` (or any name you like)
   - **Description:** "Beginner-friendly cybersecurity learning resources"
   - **Public** ✅ (so others can learn too!)
   - **Don't** check "Initialize with README" (we already have one)
3. Click **"Create repository"**

**Option B: Using GitHub CLI (Faster)**

```bash
# Install GitHub CLI if you haven't
brew install gh

# Login to GitHub
gh auth login

# Create repo and push in one command
gh repo create cybersecurity-knowledge-base --public --source=. --push
```

### Step 5: Connect Local to GitHub (Only for Option A)

After creating the repo on GitHub, you'll see instructions. Copy the commands:

```bash
# Add your GitHub repo as remote (replace YOUR-USERNAME)
git remote add origin https://github.com/YOUR-USERNAME/cybersecurity-knowledge-base.git

# Rename branch to main (if needed)
git branch -M main

# Push your files to GitHub
git push -u origin main
```

---

## ✅ Success! Your Knowledge Base is Live

Visit your repository at:
```
https://github.com/YOUR-USERNAME/cybersecurity-knowledge-base
```

---

## 🔄 Making Updates Later

When you add more content (like new OneNote sections):

```bash
# Navigate to your repo
cd /Users/ekene.ejike/Tools/knowledge-base

# See what changed
git status

# Add new/modified files
git add .

# Commit changes
git commit -m "Add network security fundamentals"

# Push to GitHub
git push
```

---

## 🎨 Making Your Repo Look Professional

### Add Topics (Tags)
On your GitHub repo page:
1. Click ⚙️ Settings (gear icon) near the "About" section
2. Add topics: `cybersecurity`, `linux`, `beginner-friendly`, `tutorial`, `learning-resources`

### Customize README
Your README.md is the first thing people see - make it welcoming!

### Enable GitHub Pages (Optional)
Turn your repo into a website:
1. Go to Settings → Pages
2. Source: Deploy from a branch
3. Branch: main
4. Save
5. Your site will be at: `https://YOUR-USERNAME.github.io/cybersecurity-knowledge-base/`

---

## ❓ Common Issues

### "Permission denied (publickey)"
**Solution:** Use HTTPS instead of SSH
```bash
git remote set-url origin https://github.com/YOUR-USERNAME/cybersecurity-knowledge-base.git
```

### "Branch 'master' or 'main'?"
**Solution:** Rename to main
```bash
git branch -M main
```

### "Files too large"
**Solution:** GitHub has 100MB file limit. Keep PDFs in `Notes/` (not tracked by git).

---

## 📚 Resources

- [GitHub Docs](https://docs.github.com/)
- [Git Basics](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics)
- [Markdown Guide](https://www.markdownguide.org/) - for editing .md files

---

**Need help?** Open an issue in your repo and the community can help! 🤝
