# Git Cheat Sheet

Personal cheat sheet. Slightly inspired by [this *Git Cheat Sheet*](https://gist.github.com/cholung/f418b3c078bbd4120318)

## <a id="toc"></a>TOC

* [Branching](#branching)
* [Committing changes](#committing-changes)
* [Comparing changes](#comparing-changes)
* [Finding commits](#finding-commits)
* [Formatting output](#formatting-output)
* [Ignoring files](#ignoring-files)
* [Restoring deleted files](#restoring-files)
* [Solving conflicts](#solving-conflicts)
* [Workflow](#workflow)
* [Working directory](#working-directory)
* [Resources I’ve found especially helpful](#resources)

## <a id='branching'></a>Branching

### How to remove remote branch?

```console
git push origin --delete <branchName>
```

### How to figure out the common ancestor of master and feature?

```console
git merge-base feature master
```
### How to display all branches?

```console
git log --branches
```
### How to push a local branch to a different remote?

```console
git push origin my_local_branch:feature_branch
```

## <a id='committing-changes'></a>Committing changes

### How to make sure that file doesn’t have bad white space?

```console
git diff --check
```

### How your commit message should look like?

- Your messages should start with a single line that’s no more than about 50 characters
- Followed by a blank line
- Followed by a more detailed explanation
- Instead of ‘I added tests for’ or ‘Adding tests for’, write ‘Add tests for’

### How to commit changes?

- Commit per issue, with a useful message per commit
- If some of the changes modify the same file, partially stage files with `git add --patch`

## <a id='comparing-changes'></a>Comparing changes

### How to review commits from `feature` branch that aren’t merged into master?

```console
git log feature --not master
```

or

```console
git log master..feature
```

### How to review what you’re about to push to a remote branch?

```console
git log origin/master..HEAD
```

```console
git log origin/master..
```

### How to review all unique commits that `master` and `feature` have?

```console
git log master...feature
```

### How to review all unique commits displaying which branch commit is from?

```console
git log --left-right master...feature
```

### How to display where was your master branch yesterday?

```console
git show master@{yesterday}
```

And 2 months ago?

```console
git show master@{2.months.ago}
```

### How to display the parent of a commit (e.g HEAD)?

```console
git show HEAD^
```

### How to display the second parent of `d921970`?

```console
git show d921970^2
```

Note: this is only useful for merge commits, which have move than one parent.

### How to preview previous revision of a file?

```console
git show e051eff~1:app/assets/javascripts/file.js
```

Modify `~1` to get previous revisions

## <a id='finding-commits'></a>Finding commits

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

## <a id='formatting-output'></a>Formatting output

How to format `reflog` as `git log`?

```console
git log -g
```

## <a id='ignoring-files'></a>Ignoring files

### How to ignore files (applies to all contributors)?

```console
echo "some-file.txt" >> .gitignore
```

### How to ignore personal files?

```console
echo "some-work-in-progress-file.txt" >> .git/info/exclude
```

### How to add files except those that match pattern defined in `.gitignore`?

```console
git add -A
```

## <a id='restoring-files'></a>Restoring deleted files

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

```console
git checkout featureA
git reset --hard origin/featureA
```

### Using `git reflog`

Your local branch diverged from PR. You reseted it with origin, forgetting
about fix you didn’t push.
 
```console
git reset --hard origin/feature_foo_bar
```

Doing the same work is a waste of time. Local history is empty.

```console
git reflog feature_coffeelint@{60.minutes.ago}
```

You can look for the missing commit and cherry-pick it after:

```console
git cherry-pick ff0000
```

## <a id='solving-conflicts'></a>Solving conflicts

### I didn’t solved conflicts properly. Can I re-generate conflict marks?

```console
git checkout --conflict=diff3 some-file
```

or

```console
git checkout --conflict=merge some-file.txt
```

### How to accept only one side when resolving conflicts?

```console
git checkout --ours PATH/FILE
git checkout --theirs PATH/FILE
```

`--ours` is the branch you're merging in, `--theirs` is the branch you're
merging into.

### How not to confuse `git checkout --ours / --theirs` in rebase?

```console
git checkout feature_foo_bar
git rebase master
# some conflict, accept change from `feature_foo_bar`
git checkout --theirs some_file
```

Rebase **replays** the current branch's commits one at a time on top of the
master. This makes `master` the "base" (*ours*) branch, and `feature_foo_bar`,
*theirs*.

[More info][1]

## <a id='workflow'></a>Workflow

### Safe usage of `git push --force` for feature branches

Prefix branches with initials:

> At thoughtbot we prefix our branches with our initials, signaling that those
commits may get rewritten and others shouldn’t add commits to the branch. When
those commits land into master or a shared branch we never rewrite them again.
[thoughtbot blog][2]

## <a id='working-directory'></a>Working directory

### How to partially stage file?

```console
git add --patch
```

### How to partially reset files?

```console
git reset --patch
```

### How to partially checkout file?

```console
git checkout --patch
```

### How to partially stash changes?

```console
git stash save --patch
```

### How to check out file in a given revision?

```console
git checkout e051eff~1 app/assets/javascripts/file.js
```

### How to reapply changes that would cause conflicts when applied?

```console
git stash branch changes-from-stash
```

### How to clean working directory and get rid of stashed changes?

```console
git clean -i
```

Note: use `git clean --dry-run` (`-n` for short) to see what’s going to be
removed; it doesn’t actually remove anything

## <a id='resources'></a>Resources I've found especially helpful

- [Auto-squashing Git Commits][5]
- [Code Archaeology With Git][6]
- [Git Interactive Rebase, Squash, Amend and Other Ways of Rewriting History][7]
- [GitHub Cheat Sheet][8]
- [Learn More About the History of a Line with Git Blame][9]
- [Six cool features of the Git 2.x series][10]
- [The git pickaxe][11]


[1]: http://inlehmansterms.net/2014/12/14/resolving-conflicts-in-git-with-ours-and-theirs/
[2]: https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history
[3]: https://robots.thoughtbot.com/autosquashing-git-commits
[4]: https://en.wikibooks.org/wiki/Regular_Expressions/POSIX_Basic_Regular_Expressions
[5]: https://robots.thoughtbot.com/autosquashing-git-commits
[6]: http://jfire.io/blog/2012/03/07/code-archaeology-with-git/
[7]: https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history
[8]: https://github.com/tiimgreen/github-cheat-sheet
[9]: http://zsoltfabok.com/blog/2012/02/git-blame-line-history/
[10]: https://developer.atlassian.com/blog/2015/10/cool-features-git-2.x/
[11]: http://www.philandstuff.com/2014/02/09/git-pickaxe.html
