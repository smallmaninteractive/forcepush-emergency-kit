# Force Push Emergency Kit
If you're here, you probably ran the dreaded `git push --force` and then realized you destroyed your repository. In such a situation, you may be panicking or losing your sanity (likely both). Don't fret. Just follow these instructions:
## Step 1. Uncover what you thought was lost (no, not your dad who went to get the milk)
A force push doesn't actually delete anything, it just changes references. So the first step is to find the last "good" commit hash.
```sh
git reflog show remotes/origin/main
# Or for those of you who thought the switch to main was "too heated":
git reflog show remotes/origin/master
```
The output should look something like this.
```
89b85b3 remotes/origin/master@{0}: fetch --append --prune origin: forced-update
32c0dbf remotes/origin/master@{1}: update by push
15c8e79 remotes/origin/master@{2}: update by push
b4ff13f remotes/origin/master@{3}: update by push
```
In this specific example, the last non-screwed up commit is the one with the hash `32c0dbf`.
## Step 2. Restore your spaghetti code
Run this command, replacing `32c0dbf` with the hash you found:
```sh
git checkout 32c0dbf
```
Now, just run the following to start a new branch based off of the commit:
```
git checkout -b temp_branch
```
## Step 3. Fight force with force
Now, run this command to push your fixed branch. Interestingly, you *have* to use force pushing for this.
```sh
git push --force origin temp_branch:main
# Or for those of you who thought the switch to main was "too heated":
git push --force origin temp_branch:master
```
## Step 4. Delete your screwed up branch
At this point, your remote `main`/`master` branch is fixed. But your local `main`/`master` branch is still ruined. So let's delete it!
```sh
git branch -D main
# Or for those of you who thought the switch to main was "too heated":
git branch -D master
```
If you still need some of the code in your ruined `main`/`master` branch, cherry-pick it instead of deleting it.
```
git branch -m main messed_up_main
# Or for those of you who thought the switch to main was "too heated":
git branch -m master messed_up_master
```
## Step 5. Finish up the process
Now, simply run this command to make your temporary branch your main one.
```sh
git branch -m temp_branch main
# Or for those of you who thought the switch to main was "too heated":
git branch -m temp_branch master
```
## Step 6. Thank the original creator of this guide (not me)
I simply organized instructions here for your convenient reference. The original guide is available at https://www.borfast.com/blog/2014/10/19/how-to-undo-a-git-push-force-and-undelete-things/.
