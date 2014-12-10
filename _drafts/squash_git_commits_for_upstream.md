```bash
git fetch upstream
git checkout branch-to-squash
git rebase -i upstream/master
# Pick the last commit, squash the rest.
# The last commit is the most recent.
# The commit that is picked is the commit that will show up 
# in the upstream repo. So pick the last commit to make 
# sure the most recent timestamp is used.

git push upstream branch-to-squash
```
