# Git cheatsheet (WSL)

Two separate repos:

| What | Local path | Remote |
|---|---|---|
| Notebooks and data | `~/cosmology` | `robertwaltersgr/Cosmic-Birefringence-Notebooks` |
| CLASS changes | `~/cosmology/class_v3.4.0` | `robertwaltersgr/class_public`, branch `cosmic-birefringence` |

The CLASS folder is its own repo. `cosmology/.gitignore` hides it from the outer repo.

## Notebooks and data -> Cosmic-Birefringence-Notebooks

```bash
cd ~/cosmology
git status                          # see what changed
git add '*.ipynb' '*.txt'           # quote the patterns so they match in subfolders
git add -f data/small_file.dat      # force-add something .gitignore hides
git add -n '*.npz'                  # dry run: preview without adding
git status                          # confirm what's staged
git commit -m "Short description"
git push
```

## CLASS changes -> fork (cosmic-birefringence branch)

```bash
cd ~/cosmology/class_v3.4.0
git branch --show-current           # should say cosmic-birefringence
git status
git add source/perturbations.c      # add the files you changed, not "."
git commit -m "Short description"
git push origin cosmic-birefringence
```

Run `git remote -v` inside the CLASS folder once to confirm `origin` points to `robertwaltersgr/class_public`. If it says "not a git repository", that copy was downloaded rather than cloned, so it needs to be pointed at the fork first.

## Handy checks

| Command | Purpose |
|---|---|
| `git status -sb` | One-line summary, shows ahead/behind vs. remote |
| `git diff --cached --stat` | What's staged and how big |
| `git restore --staged <file>` | Unstage a file |
| `git log --oneline -5` | Recent commits |
| `git ls-files \| xargs du -ch \| tail -1` | Total size of tracked files |
| `git remote -v` | Where this repo pushes to |
| `gh auth status` | Check GitHub login |

## Rules of thumb

- Stage files by name or pattern. Avoid `git add .` in `cosmology`, since `data/` and `research/` are ignored for a reason.
- GitHub rejects files over 100 MB. Check sizes before adding chains or `.npz` files.
- Clear notebook outputs if they're bloated: `nbstripout notebook.ipynb`.
- Pull before you push if you've edited from another machine: `git pull`.
- Don't `git push --force` unless you're certain nothing on the remote is worth keeping.
- Staged by mistake, before committing: `git restore --staged <file>`.
- Committed but not pushed yet: `git reset --soft HEAD~1` undoes the commit and keeps your changes.

## Setup notes

- Auth is through the GitHub CLI over HTTPS (`gh auth login`). Keep remotes as `https://github.com/...` URLs, not `git@github.com:...`, unless an SSH key is set up.
- If apt complains that dpkg was interrupted: `sudo dpkg --configure -a`, then retry.
