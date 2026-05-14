# Git Setup Guide for SQL Study Repository

This guide will help you initialize this repository on your local machine and push it to GitHub (or another Git hosting service).

## Prerequisites

- Git installed on your machine ([Download Git](https://git-scm.com/))
- GitHub account (or alternative: GitLab, Bitbucket, etc.)
- Basic command line knowledge

## Step-by-Step Setup

### Step 1: Initialize Local Repository

Open your terminal/command prompt and navigate to your project folder:

```bash
# Navigate to your project directory
cd path/to/your/sql-study-guide

# Initialize git repository
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial commit: Add SQL study guide and documentation"
```

### Step 2: Create Repository on GitHub

1. Go to [GitHub.com](https://github.com) and sign in
2. Click the **+** icon in the top right → **New repository**
3. Enter repository name: `sql-study-guide`
4. Add description: "SQL for Any IT Professional - Complete Study Guide"
5. Choose **Public** (so others can learn from it) or **Private** (for personal use)
6. **Do NOT** initialize with README, .gitignore, or license (you already have these)
7. Click **Create repository**

### Step 3: Connect Local Repository to GitHub

After creating the repository on GitHub, you'll see instructions. Follow these:

```bash
# Add the remote repository (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/sql-study-guide.git

# Rename branch to main (if needed)
git branch -M main

# Push your code
git push -u origin main
```

Replace `YOUR_USERNAME` with your actual GitHub username.

### Step 4: Verify Upload

1. Visit your repository: `https://github.com/YOUR_USERNAME/sql-study-guide`
2. Check that all files are visible
3. Verify the README is displaying correctly

---

## File Structure After Setup

Your repository should look like this:

```
sql-study-guide/
├── README.md                      # Repository introduction
├── SQL_Study_Guide.md             # Main study guide (comprehensive)
├── SQL_QUICK_REFERENCE.md         # Quick syntax reference
├── LICENSE                        # MIT License
├── .gitignore                     # Git ignore rules
└── .git/                          # Git metadata (hidden)
```

---

## Common Git Commands

### After Initial Setup

```bash
# Check status
git status

# See commit history
git log

# View changes before committing
git diff

# Add specific files
git add filename.md

# Add all changes
git add .

# Commit changes
git commit -m "Your commit message"

# Push changes to GitHub
git push

# Pull latest changes
git pull

# Create new branch
git branch branch-name
git checkout branch-name

# Or create and checkout in one command
git checkout -b branch-name
```

### Updating Your Repository

After you finish more lessons or add content:

```bash
# Make your changes to files
# Then:

git status                    # See what changed
git add .                     # Stage all changes
git commit -m "Add Lesson 15 notes and examples"
git push                      # Upload to GitHub
```

---

## Tips for Maintaining Your Repository

### Commit Message Best Practices

Use clear, descriptive commit messages:

```bash
# Good
git commit -m "Add comprehensive examples for Lesson 10 - Joins"
git commit -m "Fix typo in Lesson 5 normalization section"
git commit -m "Add SQL quick reference cheat sheet"

# Avoid
git commit -m "update"
git commit -m "stuff"
git commit -m "fix"
```

### Keep Your Repository Organized

```
Structure suggestions:
- Keep all study guide files in root
- Add /examples folder if you create practice queries
- Add /diagrams folder if you add ERD images
- Add /notes folder for personal learning notes
```

### Create Meaningful Branches

If you want to experiment:

```bash
# Create a branch for new content
git checkout -b add-advanced-examples

# Make your changes
# Then push the branch
git push -u origin add-advanced-examples

# Create a Pull Request on GitHub to merge back to main
```

---

## Sharing Your Repository

### Option 1: Public GitHub Repository (Free)

**Best for:**
- Sharing knowledge with the community
- Portfolio building
- Collaborative learning

**How to share:**
- Direct GitHub link: `https://github.com/YOUR_USERNAME/sql-study-guide`
- QR code (GitHub generates this)
- Include in resume/portfolio

### Option 2: Private GitHub Repository

**Best for:**
- Personal learning notes
- Private team collaboration
- Work-specific content

**To invite collaborators:**
1. Go to repository Settings → Collaborators
2. Add collaborators by username or email

### Option 3: Other Sharing Methods

```bash
# Clone a specific branch (for others)
git clone -b main https://github.com/username/sql-study-guide.git

# Download as ZIP
# Click "Code" → "Download ZIP" on GitHub

# Create archive
git archive --format zip HEAD > sql-study-guide.zip
```

---

## Troubleshooting

### Problem: "Permission denied (publickey)"

**Solution:**
```bash
# Generate SSH key if you don't have one
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add SSH key to GitHub
# 1. Copy your public key: cat ~/.ssh/id_ed25519.pub
# 2. Go to GitHub Settings → SSH and GPG keys → New SSH key
# 3. Paste your key

# Then use SSH URL instead:
git remote set-url origin git@github.com:YOUR_USERNAME/sql-study-guide.git
```

### Problem: "fatal: destination path already exists"

**Solution:**
```bash
# You're in a directory that's already a git repository
# Use a different folder name or remove .git:
rm -rf .git
git init
```

### Problem: Want to Change Remote URL

**Solution:**
```bash
# Check current remote
git remote -v

# Change remote URL
git remote set-url origin https://github.com/NEW_USERNAME/sql-study-guide.git
```

### Problem: Accidentally Committed Sensitive Info

**Solution:**
```bash
# Remove file from last commit (file still exists locally)
git reset --soft HEAD~1
git reset HEAD sensitive-file.txt
# Edit .gitignore to exclude it
git add .gitignore
git commit -m "Remove sensitive file and update gitignore"

# Or remove completely (dangerous!)
git rm --cached sensitive-file.txt
git commit --amend
```

---

## Continuing Your Learning

### After Each Lesson:

```bash
# 1. Update SQL_Study_Guide.md with new notes
# 2. Update SQL_QUICK_REFERENCE.md if adding new syntax
# 3. Commit your changes

git add .
git commit -m "Add notes and examples from Lesson X"
git push
```

### Creating a Learning Log:

Consider creating `PROGRESS.md`:

```markdown
# Learning Progress

## Completed Lessons
- ✅ Lesson 1: Unmask the Simplicity of SQL
- ✅ Lesson 2: Get to Know Your Database
- 🔄 Lesson 3: Database Design (in progress)
- ⏳ Lesson 4: Entities and Relationships (not started)

## Key Achievements
- Mastered INNER and LEFT JOINs
- Comfortable with GROUP BY and aggregate functions

## Areas for Practice
- Complex subqueries
- Window functions
- Performance optimization

## Notes
- Remember to use EXPLAIN for query optimization
```

Then commit it:
```bash
git add PROGRESS.md
git commit -m "Update learning progress"
git push
```

---

## Useful Resources

- [Git Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Interactive Git Tutorial](https://learngitbranching.js.org/)
- [Git Cheat Sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)

---

## Quick Setup Checklist

- [ ] Git installed and configured
- [ ] GitHub account created
- [ ] Repository created on GitHub
- [ ] Local repository initialized (`git init`)
- [ ] All files added (`git add .`)
- [ ] Initial commit created (`git commit -m "..."`
- [ ] Remote URL added (`git remote add origin ...`)
- [ ] Changes pushed to GitHub (`git push -u origin main`)
- [ ] Repository verified on GitHub
- [ ] README displays correctly on GitHub

---

## Need Help?

**GitHub Issues:** If something breaks, check GitHub's issue templates  
**Stack Overflow:** Search for your Git problem  
**Git Documentation:** Run `git help command-name`  

---

## Next Steps

1. ✅ Set up your Git repository
2. 📚 Continue studying the SQL course
3. 📝 Update your study guide regularly
4. 🚀 Share your progress and help others learn

---

**Setup Guide Version:** 1.0  
**Last Updated:** 2026-05-14  

Happy learning and happy committing! 🎉
