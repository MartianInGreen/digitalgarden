---
{"dg-publish":true,"permalink":"/wiki/tech-and-engineering/git-and-dev-infra/git-essentials/","title":"Git essentials","tags":["Git","Coding","Commands","Bash"],"dg-note-properties":{"type":"Page","collections":"Knowledge & Guides","title":"Git essentials","aliases":null,"description":null,"icon":null,"createdAt":"2025-12-04T06:46:38.782Z","lastUpdated":"2025-12-15T13:13:53.086Z","tags":["Git","Coding","Commands","Bash"],"coverImage":null}}
---


# Git essentials

## Commits
To recover a commit after it failed, use 
```bash
git commit -eF $(git rev-parse --git-dir)/COMMIT_EDITMSG
```

## Branch switching
To switch to a different branch you can use
```bash
git switch <branch> 
```

To fetch the latest branch status use 
```bash
git fetch (origin)
```

and to pull the latest updates 
```bash
git switch master #Alternative stay in current branch to only fetch current
git pull
```

## Push 
To push to a specific branch use 
```bash
git push --set-upstream origin <branch>
```


