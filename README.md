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
