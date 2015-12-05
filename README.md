## How to make sure that file doesn't have bad white space?

```bash
git diff --check
```

## How your commit message should look like?

- your messages should start with a single line that’s no more than about 50 characters
- followed by a blank line
- followed by a more detailed explanation
- instead of 'I added tests for' or 'Adding tests for', write 'Add tests for'

## How to commit changes?

- commit per issue, with a useful message per commit
- if some of the changes modify the same file, partially stage files:
  `git add --patch`

## How to review of all the commits that are in `feature` branch but that aren’t in your master branch?

```bash
git log feature --not master
git log master..feature
```

## How to display what you're about to push to a remote?

```bash
git log origin/master..HEAD
```

```bash
git log origin/master..
```

## How to review all unique commits that `master` and `feature` have? (commits `master` and `feature` don't share)

```bash
git log master...feature
```

## How to display all unique commits showing which side of the range each commit is in?

```bash
git log --left-right master...feature
```

## How to remove remote branch?

```bash
git push origin --delete <branchName>
```

## How to figure out the common ancestor of master and feature?

```bash
git merge-base feature master
```

## How to prepare a release for people that don't use git?

```bash
git archive master --prefix='project/' --format=zip > `git describe master`.zip
```

## How to display all branches?

```bash
git log --branches
```

## How to display where your master branch was yesterday?

```bash
git show master@{yesterday}
```

And 2 months ago?

```bash
git show master@{2.months.ago}
```

## How to display the parent of a commit (e.g HEAD)?

```bash
git show HEAD^
```

## How to display the second parent of `d921970`?

```bash
git show d921970^2
```

Note: this is only useful for merge commits, which have move than one parent.

## How to partially stage file?

```bash
git add -p
```

```bash
git add --patch
```

## How to partially reset files?

```bash
git reset --patch
```

## How to partially checkout file?

```bash
git checkout --patch
```

## How to partially stash changes?

```bash
git stash save --patch
```

## How to push a local branch to a different remote?

```bash
git push origin my_local_branch:feature_branch
```

## How to accept only one side when resolving conflicts?

```bash
git checkout --ours PATH/FILE
git checkout --theirs PATH/FILE
```

`--ours` is the branch you're merging in, `--theirs` is the branch you're
merging into.

## How not to confuse `git checkout --ours / --theirs` in rebase?

```bashshell
git checkout feature_foo_bar
git rebase master
# some conflict, accept change from `feature_foo_bar`
git checkout --theirs some_file
```

Rebase **replays** the current branch's commits one at a time on top of the
master. This makes `master` the "base" (*ours*) branch, and `feature_foo_bar`,
*theirs*.

[More info](http://inlehmansterms.net/2014/12/14/resolving-conflicts-in-git-with-ours-and-theirs/)

## How to display what pr you going to merge to master?

```bash
git le origin/master --not master --merges
```

## How to preview previous revision of a file?

```bash
git show e051eff~1:app/assets/javascripts/file.js
```

Modify `~1` to get previous revisions

## How to check out file in a given revision?

```bash
git checkout e051eff~1 app/assets/javascripts/file.js
```

## Restoring deleted files

### Resetting to `origin/featureA`

You have been working on featureA on 2 different machines. 2 changes were pushed
to to origin/featureA before going home:

- C1
- C2

At home you finished the feature adding one small change:

- C3

To make the history simple (e.g in pull requests workflow), you squashed all the
3 commits and pushed the changes.

The history diverged and you can't pull with fast forward on your 1st machine.
To force pull you can do:

```bash
git checkout featureA
git reset --hard origin/featureA
```

### Using `git reflog`

Your local branch diverged from PR. You reseted it with origin, forgetting
about fix you didn’t push.
 
```bash
git reset --hard origin/feature_foo_bar
```

Doing the same work is a waste of time. Local history is empty.

```bash
git reflog feature_coffeelint@{60.minutes.ago}
```

You can look for the missing commit and cherry-pick it after:

```bash
git cherry-pick ff0000
```
