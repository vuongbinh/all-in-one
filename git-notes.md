
# 📘 Git for Work: Best Practices & Workflow

---

## 🔧 Initial Setup

```bash
# Configure Git identity (only once per machine)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Clone your forked repo
git clone https://github.com/your-username/project.git
cd project
```

---

## 🔁 Fork & Remote Setup

```bash
# Add the upstream remote (original repo)
git remote add upstream https://github.com/original-owner/project.git

# View remotes
git remote -v
```

---

## 🔄 Sync Your Fork

```bash
# Fetch all branches from upstream
git fetch upstream

# Update your local main branch
git checkout main
git merge upstream/main

# Push updated main to your fork
git push origin main
```

> You can replace `main` with `develop` or any base branch.

---

## 🌿 Branching Strategy

```bash
# Create a new branch for your feature or bugfix
git checkout -b feature/your-feature-name

# Or track an existing remote branch
git checkout --track upstream/branch-name
```

Branch naming conventions:
- `feature/<name>` – new features
- `bugfix/<name>` – bug fixes
- `hotfix/<name>` – urgent fixes
- `chore/<name>` – maintenance or cleanup

---

## 💻 Making Changes

```bash
# Make edits, then stage changes
git add .

# Commit with a clear message
git commit -m "feat: Add user login flow"
```

Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):
- `feat:` – new feature
- `fix:` – bug fix
- `docs:` – documentation
- `style:` – formatting only
- `refactor:` – code refactor
- `test:` – adding tests
- `chore:` – tooling/configuration

---

## 🚀 Pushing Changes

```bash
# Push your branch and set upstream tracking
git push -u origin feature/your-feature-name
```

---

## 🔁 Pull Request Workflow

1. Push your changes to your fork.
2. Open a **Pull Request (PR)** to the `develop` or `main` branch of the upstream repo.
3. Assign reviewers, add a description, link Jira/ticket if applicable.
4. Resolve feedback and push more commits to the same branch.

---

## 🧹 Clean Up

```bash
# Delete local branch
git branch -d feature/your-feature-name

# Delete remote branch
git push origin --delete feature/your-feature-name
```

---

## 🔍 Troubleshooting

| Problem | Fix |
|--------|-----|
| `fatal: 'upstream/branch' is not a commit` | Double-check branch name, run `git fetch upstream` |
| `error: failed to push refs` | Run `git pull --rebase`, resolve conflicts, then push |
| Wrong remote tracking | `git branch --set-upstream-to=origin/branch-name` |
| View all branches & tracking | `git branch -vv` |

---

## 🛠 Helpful Commands

```bash
git status           # See current changes
git log --oneline    # Compact history
git diff             # View unstaged changes
git stash            # Temporarily save changes
git cherry-pick <commit>  # Apply specific commit
```
