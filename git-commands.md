# GIT commands

GIT handbook

https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes

GitHub fork readme

https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/configuring-a-remote-for-a-fork


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

Changed files between commits
>git diff --name-only HEAD~1

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

![interactive rebase](https://raw.githubusercontent.com/calleboketoft/co-quick-start-guides/master/git-commands-i-rebase.png "Interactive Rebase")

Show what happened in a commit
>git show [commit hash]

## Fork work

GIT see remote branch
>git remote -v

Get information about remote
>git remote show <remote>

Fetch newest stuff from remote
>git fetch

Pull and merge from remote
>git pull

Push to remote
>git push <remote> <local>

HEAD simply refers to the last commit in the current branch for the user

See remotes that are configured
>git remote -v

Add upstream (and name it "upstream")
>git remote add upstream repo git@git.build.ingka.ikea.com:ONE/onecheckout-client.git

Fetch upstream branches, need these to set tracking branch
>git fetch upstream

Set upstream tracking for branch in fork
>git branch cfb1.3 -u upstream/cfb1.3
(-u is same as --set-upstream-to)

Pull changes from remote
>git pull [remote name] [branch name]

>git pull upstream cfb1.3

Pull changes from tracked branch
>git pull

Rebase changes from tracked branch
>git rebase upstream/cfb1.3

Push changes to specific branch
>git push origin testbranch
