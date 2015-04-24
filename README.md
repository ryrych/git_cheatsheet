How to make sure that file doesn't have bad white space?
```
git diff --check
```

How your commit message should look like?
- your messages should start with a single line that’s no more than about 50 characters
- followed by a blank line
- followed by a more detailed explanation
- instead of 'I added tests for' or 'Adding tests for', write 'Add tests for'

How to commit changes?
- commit per issue, with a useful message per commit
- if some of the changes modify the same file, partially stage files:
  `git add --patch`

How to review of all the commits that are in `feature` branch but that aren’t in your
master branch?
```
git log feature --not master
git log master..feature
```

How to display what you're about to push to a remote?
```
git log origin/master..HEAD
```
```
git log origin/master..
```

How to review all unique commits that `master` and `feature` have? (commits `master` and `feature` don't share)
```
git log master...feature
```

How to display all unique commits showing which side of the range each commit is in?
```
git log --left-right master...feature
```

How to remove remote branch?
```
git push origin --delete <branchName>
```

How to figure out the common ancestor of master and feature?
```
git merge-base feature master
```

How to prepare a release for people that don't use git?
```
git archive master --prefix='project/' --format=zip > `git describe master`.zip
```

How to display all branches?
```
git log --branches
```

How to display where your master branch was yesterday?
```
git show master@{yesterday}
```

And 2 months ago?
```
git show master@{2.months.ago}
```

How to display the parent of a commit (e.g HEAD)?
```
git show HEAD^
```

How to display the second parent of `d921970`?
```
git show d921970^2
```
Note: this is only useful for merge commits, which have move than one parent.

How to partially stage file?
```
git add -p
```
```
git add --patch
```

How to partially reset files?
```
git reset --patch
```

How to partially checkout file?
```
git checkout --patch
```

How to partially stash changes?
```
git stash save --patch
```

How to push a local branch to a different remote?
```
git push origin my_local_branch:feature_branch
```

How to accept only one side when resolving conflicts?
```
git checkout --ours PATH/FILE
git checkout --theirs PATH/FILE
```

`--ours` is the branch you're merging in, `--theirs` is the branch you're
merging into.

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

```
git checkout featureA
git reset --hard origin/featureA
```

That's it!

How to find line that was removed in the past?
git -SinplaceSelect

use case:
ownerTaskeditor was refactored - instead of input (select2), a avatar is
displayed. The previous params were different and are no longer availabled in
the app. How to find out what params were needed - needed to use select with
input in tasks overview page.
