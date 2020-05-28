# GIT commands

Undo commits and keep changes
>git reset --soft HEAD~1

Undo commits and remove changes
>git reset --head HEAD~1

Fix commit message of a specific commit
>git commit --amend

Compare code changes between two commits
>git diff k73ud^..dj374

Compare code changes between last commit and now
>git diff HEAD~1

for a specific file
>git diff HEAD~1 filename

Replace local branch commits with remote, forced
>git reset --hard origin/master

See global config
>git config --list --global

See local config
>git config --list

Clean workspace of untracked files
>git clean -f -d

Reset workspace to previous commit
>git reset --hard

Get out of broken rebase
>git rebase --quit

Interactive rebase
>git rebase -i HEAD~4

Show what happened in a commit
>git show [commit hash]
