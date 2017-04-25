# How to remove static assets from a Git repo

## Why?
1. Static assets are best served over CDN with streaming capability
2. Repo size inflates even if you delete binary file because Git has history of the data.
3. Inflated repo size inflates backup sizes, CI performance, and deployment speed.

## Setup
1. [Download BFG](https://rtyley.github.io/bfg-repo-cleaner/)
2. (Optional) Cteate an alias `bfg` for future use

## Run

1. Check your repo size and make a note of it.

2. Commit removal of static assets you no longer want. Notice that repo size still accounts for previously committed binary file.

3. Open your termianal, run BFG to remove specific files with following example command.

`$ java -jar bfg.jar --delete-files some-file-name.ext`

Check in git history to see if the file has been removed in previous commits. The repo size has not changed just yet because Git keeps your backup just in case.

4. Expire safety net Git history:

`$ git reflog expire --expire=now --all && git gc --prune=now --aggressive`

Check your repo size, it should have reduced repo size by the size of files you removed. The Git history should change at this point which you want to force push up to the origin.

## Note

* The repo size in remote server might still be large until remote reflog expires over time, or until you or admin can run the expire command.
