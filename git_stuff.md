# Revert

Git revert undoes changes made in a commit by creating a new commit.
This shows up as a new commit and leaves the original commit in the history.

```bash
git log --oneline | head

git revert 18f5020
```

