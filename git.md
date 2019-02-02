
# Refresher
```sh
$ git status
$ git add .
$ git commit -m ""
$ git pull origin <branch>

# see if there are any conflicts
# fix the conflicts, remove all the code with "====" and ">>"
# remove duplicate code
# run the app and see if everything is intact
# commit again
$ git add .
$ git commit -m "fixed merge conflicts"
$ git pull origin <branch>

$ git push origin <branch>
```

# Advanced
```sh
$ git -m old-branch-name new-branch-name  # rename branch
$ git branch new-branch-name origin/old-branch-name
$ git push origin new-branch-name

$ git push origin :old-branch-name  # OLD: delete branch remotely
$ git push origin old-branch-name --delete # OLD VERBOSE: delete branch remotely

$ git branch -D branch-name # delete branch locally
$ git branch -d branch-name # delete branch remotely

$ git remote prune origin # delete (or "prune") local branches that are not in the remote repo/origin

# know merged branches and delete them locally/remotely
$ git branch --merged # will list all the branches merged with the current branch
$ git branch -D branch-name-1 branch-name-2 # will delete all the branches at once locally
$ git branch -d branch-name-1 branch-name-2 # will delete all the branches at once remotely and locally
$ git push origin --delete branch-name-1 branch-name-2 # will delete all the branches at once remotely and locally

# get all the remote branches locally
$ git branch -r | grep -v '\->' | while read remote; do $ git branch --track "${remote#origin/}" "$remote"; done

# http://stackoverflow.com/questions/1139762/gitignore-file-not-ignoring
# If you are trying to ignore changes to a file that's already tracked in the repository (e.g. a dev.properties file that you would need to change for your local environment but you would never want to check in these changes) than what you want to do is:
$ git update-index --assume-unchanged <file>

# If you wanna start tracking changes again
$ git update-index --no-assume-unchanged <file>

# remove cached version of a file or dir
$ git rm --cached vendor/gems/rdf

# pull all the remote branches
$ git fetch

# revert merge conflicts
$ git reset --merge ORIG_HEAD
$ git merge --abort

# revert commit which is not pushed
$ git reset HEAD~1

# switch branch with uncommited changes
$ git stash                # save changes
$ git stash apply          # apply saved changes when back on the same branch
$ git stash list           # list all stashes
$ git stash apply stash{0} # applying a particular stash
$ git stash pop stash{0}   # poping a particular stash

# move last n commits to a new branch and remove n commits from master
# assuming that you are on master branch, and you want to undo last 6 commits
$ git checkout -b new_branch
$ git push origin new_branch -f
$ git checkout master
$ git reset --hard HEAD~6        # remove last 6 commits from head
$ git push origin HEAD --force   # push to master by forcing it

# add an origin to an empty repository
$ git init
$ git remote add origin git@github.com:raghuboosetty/excel_merge.git
$ git config user.name
# raghu
$ git config user.email
# raghu@rubyeffect.com
$ git config user.name raghuboosetty
$ git config user.email raghu.boos@gmail.com

# $ git commit with message and description
$ git commit -m "title/message" -m "description"

# set remote origin
# get remote origin URL:
$ git remote -v
$ git remote show origin
# remove & add
$ git remote remove origin
$ git remote add origin git@github.com:raghuboosetty/excel_merge.git
# set new or override existing one
$ git remote set-url origin git@github.com:raghuboosetty/excel_merge.git

$ git remote add origin git@github.com-raghu-rubyeffect:rubyeffect/ckeditor.git
$ git remote set-url origin git@github.com-raghu-rubyeffect:rubyeffect/ckeditor.git

# update a commit message
$ git commit --amend

# $ git origins
$ git remote show origin
$ git remote rm origin
$ git remote rename origin myNewAlias

## duplicate a $ git repository
$ git clone --bare https://github.com/exampleuser/old-repository.$ git # Make a bare clone of the repository
cd old-repository.git
$ git push --mirror https://github.com/exampleuser/new-repository.$ git # Mirror-push to the new repository
$ cd ..
$ rm -rf old-repository.$ git # Remove our temporary local repository

```
# Move old repository to new subdirectory
```sh
# Assume the current directory is where we want the new repository to be created
# Create the new repository
$ git init

# Before we do a merge, we have to have an initial commit, so we'll make a dummy commit
$ dir > deleteme.txt
$ git add .
$ git commit -m "Initial dummy commit"

# Add a remote for and fetch the old repo
$ git remote add -f old_a <OldA repo URL>

# Merge the files from old_a/master into new/master
$ git merge old_a/master --allow-unrelated-histories

# Clean up our dummy file because we don't need it any more
$ git rm .\deleteme.txt
$ git commit -m "Clean up initial file"

# Move the old_a repo files and folders into a subdirectory so they don't collide with the other repo coming later
$ mkdir old_a
# Ubuntu
$ dir -exclude old_a | %{$ git mv $_.Name old_a}
$ dir –exclude old_a,old_b | %{$ git mv $_.Name old_b}
# Mac
$ git ls-tree -z --name-only HEAD | xargs -0 -I {} $ git mv {} old_a/

# Commit the move
$ git commit -m "Move old_a files into subdir"
```
