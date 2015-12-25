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

[More info][1]

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

## Workflow

### Safe usage of `git push --force` for feature branches

Prefix branches with initials:

> At thoughtbot we prefix our branches with our initials, signaling that those
commits may get rewritten and others shouldn’t add commits to the branch. When
those commits land into master or a shared branch we never rewrite them again.
[thoughtbot blog][2]

## Finding commits

### Using keywords

```console
git log :/typo
```

Finds most recent commit that contains word ‘typo’ in the commit message:

```console
47db47b playing with typography
```

You can use it with other git commands like:

```console
git commit --fixup :/second
```

[Source thoughtbot blog][3]

### Using pickaxe

Pickaxe finds any commit that introduced or removed the string `reflog`:

```console
git log --oneline -S reflog
```

To search by a **regex**:

```console
git log --oneline -S re..og --pickaxe-regex
```

```console
git log --oneline -S"[bash|console]" --pickaxe-regex
```

Note: use can use [extended POSIX regular expression syntax][4].

## Formatting output

How to format `reflog` as `git log`?

```console
git log -g
```

## Stashing changes

How to reapply changes that would cause conflicts when applied?

```console
git stash branch changes-from-stash
```

## How to clean working directory and get rid of stashed changes?

```console
git clean -i
```

Note: use `git clean --dry-run` (`-n` for short) to see what’s going to be
removed; it doesn’t actualy remove anything

## Working with working directory

How to ignore files (applies to all contributors)?

```console
echo "some-file.txt" >> .gitignore
```

How to ignore personal files?

```console
echo "some-work-in-progress-file.txt" >> .git/info/exclude
```

How to add files except those that match pattern defined in `.gitingore`?

```console
git add -A
```

## Resources I’ve found especially helpful

- [Auto-squashing Git Commits][5]
- [Git Interactive Rebase, Squash, Amend and Other Ways of Rewriting History][6]
- [GitHub Cheat Sheet][7]
- [Six cool features of the Git 2.x series][8]
- [The git pickaxe][9]


[1]: http://inlehmansterms.net/2014/12/14/resolving-conflicts-in-git-with-ours-and-theirs/
[2]: https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history
[3]: https://robots.thoughtbot.com/autosquashing-git-commits
[4]: https://en.wikibooks.org/wiki/Regular_Expressions/POSIX_Basic_Regular_Expressions
[5]: https://robots.thoughtbot.com/autosquashing-git-commits
[6]: https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history
[7]: https://github.com/tiimgreen/github-cheat-sheet
[8]: https://developer.atlassian.com/blog/2015/10/cool-features-git-2.x/
[9]: http://www.philandstuff.com/2014/02/09/git-pickaxe.html
