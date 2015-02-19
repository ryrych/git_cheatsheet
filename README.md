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
