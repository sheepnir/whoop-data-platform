## Simple Git Workflow for this Project

### **Basic Rules:**
1. Never commit directly to `main`
2. Create a feature branch for each task
3. Open a PR when ready
4. Merge after quick self-review

---

## Step 1: Minimal Branch Protection

**On GitHub:**
1. Go to **Settings** → **Branches** → **Add branch protection rule**
2. Branch name: `main`
3. Check **only** these boxes:
   - ☑ **Require a pull request before merging**
   - ☑ **Require linear history** (keeps git log clean)

That's it! This forces PRs but doesn't require approvals (since you're working solo).

---

## Step 2: Simple Pre-commit Hook (Optional but Recommended)

Just **one** hook to prevent committing directly to main.

### **File:** `.git/hooks/pre-commit`
**Purpose:** Remind you to use feature branches

```bash
#!/bin/sh
# Simple pre-commit hook: prevent commits to main branch

branch="$(git rev-parse --abbrev-ref HEAD)"

if [ "$branch" = "main" ]; then
  echo "❌ You can't commit directly to main branch."
  echo "✅ Create a feature branch instead:"
  echo "   git checkout -b feature/your-feature-name"
  exit 1
fi
```

**Make it executable:**
```bash
chmod +x .git/hooks/pre-commit
```

---

## Step 3: Your Daily Workflow

### **Starting New Work:**

```bash
# Make sure you're on main and up to date
git checkout main
git pull origin main

# Create a feature branch (use descriptive names)
git checkout -b feature/bronze-layer-setup

# Work on your code...
# Commit as you go
git add .
git commit -m "Add Snowflake bronze schema setup"
```

### **Opening a PR:**

```bash
# Push your branch to GitHub
git push origin feature/bronze-layer-setup

# Go to GitHub - it will show a banner to "Create Pull Request"
# Click it, add a description, and create the PR
```

### **Merging Your PR:**

1. On GitHub, review your own changes
2. Click **"Squash and merge"** (keeps history clean)
3. Delete the feature branch after merging
4. Pull the updated main locally:

```bash
git checkout main
git pull origin main
git branch -d feature/bronze-layer-setup  # Delete local branch
```

---

## Step 4: Branch Naming Convention

Keep it simple:

```
feature/short-description    # New features
  ├─ feature/bronze-layer
  ├─ feature/dbt-silver-models
  └─ feature/power-bi-dashboard

bugfix/short-description     # Bug fixes
  ├─ bugfix/snowflake-timeout
  └─ bugfix/date-dimension

docs/short-description       # Documentation only
  └─ docs/setup-guide
```

---

## Step 5: Simple Commit Message Format

No need for fancy conventions. Just be clear:

```bash
# Good commits:
git commit -m "Add bronze layer Snowflake schema"
git commit -m "Create dim_date dbt model with tests"
git commit -m "Fix OAuth token refresh logic"
git commit -m "Update README with setup instructions"

# Avoid vague commits:
git commit -m "updates"
git commit -m "fixed stuff"
git commit -m "wip"
```

---
This workflow benefits are:
- Clean git history
- Protection against accidental main commits  
- PR-based workflow for your portfolio
- Simple, lightweight, no complexity

### **Quick Reference Card:**

```bash
# Start new work
git checkout main && git pull
git checkout -b feature/my-feature

# Commit work
git add .
git commit -m "Clear description"

# Push and create PR
git push origin feature/my-feature
# (Go to GitHub and click "Create Pull Request")

# After merging PR
git checkout main && git pull
git branch -d feature/my-feature
```

