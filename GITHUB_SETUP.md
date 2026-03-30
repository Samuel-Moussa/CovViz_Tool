# GitHub Repository Setup Instructions

This document provides step-by-step instructions for setting up the CovViz repository on GitHub.

## Prerequisites

- GitHub account (https://github.com)
- Git installed locally
- Access to Samuel-Moussa GitHub account

## Step 1: Create Repository on GitHub

1. Go to https://github.com/new
2. Fill in the details:
   - **Repository name:** `CovViz`
   - **Description:** `Zero-dependency QuestaSim coverage report visualizer. Interactive dashboards, multi-run analysis, CI/CD ready. Single HTML file, offline-capable.`
   - **Visibility:** Select **Private** (can be changed to public later)
   - **Initialize with:** Leave unchecked (we have local files)
3. Click **Create repository**

## Step 2: Add Remote and Push

From the repository directory (`/home/ubuntu/CovViz-Repo`):

```bash
# Add GitHub as remote
git remote add origin https://github.com/Samuel-Moussa/CovViz.git

# Rename branch to main (optional but recommended)
git branch -M main

# Push to GitHub
git push -u origin main
```

**Note:** You'll be prompted for GitHub credentials. Use:
- Username: `Samuel-Moussa`
- Password: Personal Access Token (PAT) or GitHub CLI

### Creating a Personal Access Token (Recommended)

1. Go to https://github.com/settings/tokens
2. Click **Generate new token** → **Generate new token (classic)**
3. Set:
   - **Note:** `CovViz Repository`
   - **Expiration:** 90 days or custom
   - **Scopes:** Select `repo` (full control of private repositories)
4. Click **Generate token**
5. Copy the token (you won't see it again)
6. Use token as password when pushing

## Step 3: Configure Repository Settings

### Branch Protection (Optional but Recommended)

1. Go to repository → **Settings** → **Branches**
2. Click **Add rule**
3. Set:
   - **Branch name pattern:** `main`
   - **Require pull request reviews before merging:** ✓
   - **Require status checks to pass before merging:** ✓
   - **Require branches to be up to date before merging:** ✓
4. Click **Create**

### Enable GitHub Pages (Optional)

For hosting documentation:

1. Go to repository → **Settings** → **Pages**
2. Set:
   - **Source:** `main` branch
   - **Folder:** `/docs`
3. Click **Save**
4. Documentation will be available at: `https://samuel-moussa.github.io/CovViz/`

### Configure Actions

1. Go to repository → **Actions**
2. Workflows should automatically enable
3. Verify `validate.yml` runs on push/PR

## Step 4: Add Repository Topics (Optional)

1. Go to repository → **About** (gear icon)
2. Add topics:
   - `coverage-analysis`
   - `questasim`
   - `verification`
   - `verilog`
   - `systemverilog`
   - `coverage-report`
   - `ci-cd`
3. Click **Save changes**

## Step 5: Create Release

1. Go to repository → **Releases**
2. Click **Create a new release**
3. Set:
   - **Tag version:** `v4.1`
   - **Release title:** `CovViz v4.1 - Production Release`
   - **Description:** Copy from CHANGELOG.md [4.1] section
   - **Attach binaries:** Upload `CovViz-Enhanced-Fixed.html`
4. Click **Publish release**

## Step 6: Verify Setup

Check that everything is working:

```bash
# Verify remote
git remote -v

# Check branch
git branch -a

# View commits
git log --oneline

# Check GitHub Actions
# Go to repository → Actions tab
```

## Step 7: Update Local Configuration

```bash
# Set default branch to main (if not already)
git config --global init.defaultBranch main

# Optional: Set up SSH for future pushes
# See: https://docs.github.com/en/authentication/connecting-to-github-with-ssh
```

## Continuous Integration Setup

The repository includes GitHub Actions workflow (`validate.yml`) that:
- Validates HTML file structure
- Checks for required sections
- Validates example reports
- Checks documentation completeness
- Verifies file sizes
- Runs on every push and pull request

**Status badge** for README:
```markdown
[![Validate CovViz](https://github.com/Samuel-Moussa/CovViz/actions/workflows/validate.yml/badge.svg)](https://github.com/Samuel-Moussa/CovViz/actions/workflows/validate.yml)
```

## Making Changes

For future updates:

```bash
# Create feature branch
git checkout -b feature/your-feature-name

# Make changes
# ... edit files ...

# Commit changes
git add .
git commit -m "feat: description of changes"

# Push to GitHub
git push origin feature/your-feature-name

# Create Pull Request on GitHub
# Go to repository → Pull requests → New pull request
```

## Troubleshooting

### "fatal: remote origin already exists"
```bash
git remote remove origin
git remote add origin https://github.com/Samuel-Moussa/CovViz.git
```

### "Permission denied (publickey)"
Use HTTPS instead of SSH, or set up SSH keys:
https://docs.github.com/en/authentication/connecting-to-github-with-ssh

### "fatal: 'origin' does not appear to be a 'git' repository"
```bash
git remote add origin https://github.com/Samuel-Moussa/CovViz.git
```

### Workflow not running
1. Check repository → **Actions** tab
2. Verify `.github/workflows/validate.yml` exists
3. Check Actions permissions in Settings → Actions → General

## Next Steps

1. **Announce Release:**
   - Share GitHub link with team
   - Post on relevant forums/communities
   - Create announcement blog post

2. **Gather Feedback:**
   - Monitor GitHub Issues
   - Respond to feature requests
   - Fix reported bugs

3. **Plan Roadmap:**
   - Create GitHub Projects for Phase 1, 2, 3, 4
   - Add issues to projects
   - Track progress

4. **Community Building:**
   - Add contributing guidelines (already done)
   - Create discussion forum
   - Engage with users

## Resources

- GitHub Docs: https://docs.github.com
- Git Basics: https://git-scm.com/book
- GitHub Actions: https://docs.github.com/en/actions
- Markdown Guide: https://guides.github.com/features/mastering-markdown/

---

**Repository:** https://github.com/Samuel-Moussa/CovViz  
**Status:** Ready for GitHub  
**Last Updated:** March 30, 2026
