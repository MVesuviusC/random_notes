# Revert

Git revert undoes changes made in a commit by creating a new commit.
This shows up as a new commit and leaves the original commit in the history.

```bash
git log --oneline | head

git revert 18f5020
```

# Git commit message syntax conventions
https://www.conventionalcommits.org/en/v1.0.0/#summary

# Set email and user name
git config --global user.name "Your Name"
git config --global user.email "Your Email"
