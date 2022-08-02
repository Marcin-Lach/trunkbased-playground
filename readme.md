# Trunk-based development playground

This repo is to verify some operations before deciding on using TBD in commercial projects.

## More info

Extensive overview can be found on [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com/).

## Scenarios

### Branching and releasing - on-prem + SaaS

Most complicated scenario is when you need to support customers on multiple versions.
This happens when you offer both on-prem and SaaS solution.

For this case, we can use following rules:

1. Tags must follow [semantic versioning](https://semver.org/) to provide information about expected breaking changes between versions
1. Every major and minor bump should be done on `main` branch
1. Every change pushed to remote branch should be buildable, tested and overall working - continuous deployment could be triggered on every push to `main`
1. If a hotfix is needed, it should be implemented on `main`, then a branch from specific tag should be created (if does not exist already) and hotifx should be cherry-picked from main to the release branch. Then tag should be created on release branch.
Example:
    - tag 0.1.0 was created on main and version was released
    - development on main is constantly flowing
    - hotfix for release 0.1.0 is required
    - defect still exists on main
    - hotfix is implemented on main
    - branch r/0.1.x is created from tag 0.1.0
    - hotfix is cherry-picked from main to branch r/0.1.x
    - `git describe --tags` is executed to suggest what next tag should be (result is something similar to `0.1.0-1-ge94280e` which says that from last know tag - `0.1.0` - 1 commit was added)
    - tag is created on r/0.1.x branch and release can be created

1. In case of conflicts/compatilibity issues while cherry-picking or defect no longer being present on main, fix can be implemented directly on release branch, should be tagged there and **should not be merged** anywhere.

>Hint:  
To push all tags and commits to remote, execute following command  
`git push --tags`

### Upgrading from old version to not-newest version

Let's assume that customer still uses some old version (let's say from tag 0.1.3, meaning this version has already been patched 3 times). Lastest release is 0.4.0. We should strongly suggest to upgrade to latest release. But customer realy wants to just update to version 0.2.0.

In such case, we should identify changes, that are different between those versions and carefully apply hotfixes implemented on r/0.1.x to branch r/0.2.x. Regression is highly probable - new version of a feature could be present on 0.2.0 that could be overwritten by cherry-picking changes from 0.1.3. Something that was fixed in one of patches to 0.1.x version could also be missing in 0.2.x version.

We can use `git log` with [double-dot operator](https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection#_commit_ranges) to list differences. It should be done in both ways, to show changes that are present on 0.2.0 and not present on 0.1.3, and also changes that are present on 0.1.3 and are not present on 0.2.0.

```bash
git log 0.1.3..0.2.0 # show what's on 0.2.0 that is not on 0.1.3
git log 0.2.0..0.1.3 # show what's on 0.1.3 that is not on 0.2.0
```

## Sequence of operations done in this repo

1. initial commit + tag 0.0.0
1. add more commits and tags on main (0.0.1, 0.1.0, 0.2.0 - annotated)
1. add a hotfix to tag 0.1.0 on main, create branch r/0.1.x from tag 0.1.0, cherry-pick hotfix from main, tag 0.1.1 on r/0.1.x
1. tag main with 0.3.0
1. add some more commits on main (build up release 0.4.0)
1. add hotfix for 0.3.0 and 0.1.0 on main
1. cherry-pick hotfix to r/0.1.x branch, tag as 0.1.2
1. create branch r/0.3.x from 0.3.0 tag, cherry pick hotfix from main to r/0.3.x, tag as 0.3.1
1. add some more commits on main
1. cherry-pick last commit from main to r/0.1.x, create tag 0.1.3 on branch r/0.1.x (with merge conflict - this is the same as if a feature was required on old version)

```js
❯ git reflog
2d8fcad (HEAD -> main, origin/main) HEAD@{0}: commit: final info about this repo
4a562ed HEAD@{1}: commit: info about usage and hardcore cases
261ef08 HEAD@{2}: checkout: moving from r/0.1.x to main
e94280e (tag: 0.1.3, origin/r/0.1.x, r/0.1.x) HEAD@{3}: commit (cherry-pick): merge conflict when cherry-picking 261ef0816060268b9ebfdbed92a4975072036f09, replaced readme.md
9ae2307 (tag: 0.1.2) HEAD@{4}: checkout: moving from main to r/0.1.x
261ef08 HEAD@{5}: commit: final info to readme.md
2cff2e6 HEAD@{6}: commit: changed readme.md to utf-8
ed77465 HEAD@{7}: commit: added git attributes
fd6d347 HEAD@{8}: checkout: moving from r/0.3.x to main
b0a2455 (tag: 0.3.1, origin/r/0.3.x, r/0.3.x) HEAD@{9}: cherry-pick: hotfixing 0.3 and 0.1
04f7438 (tag: 0.3.0) HEAD@{10}: reset: moving to HEAD
04f7438 (tag: 0.3.0) HEAD@{11}: checkout: moving from 04f7438318e636c35eb432d846399389ad7a2c5a to r/0.3.x
04f7438 (tag: 0.3.0) HEAD@{12}: checkout: moving from r/0.1.x to 0.3.0
9ae2307 (tag: 0.1.2) HEAD@{13}: cherry-pick: hotfixing 0.3 and 0.1
ba58520 (tag: 0.1.1) HEAD@{14}: checkout: moving from main to r/0.1.x
fd6d347 HEAD@{15}: commit: hotfixing 0.3 and 0.1
a9f043a HEAD@{16}: commit: some more working on 0.4.0
38429e0 HEAD@{17}: commit: working on 0.4.0
04f7438 (tag: 0.3.0) HEAD@{18}: commit: add readme.md
2d7d83c HEAD@{19}: checkout: moving from r/0.1.x to main
ba58520 (tag: 0.1.1) HEAD@{20}: cherry-pick: hotfix that is required on 0.1.0
f0c018e (tag: 0.1.0) HEAD@{21}: checkout: moving from main to r/0.1.x
2d7d83c HEAD@{22}: commit: hotfix that is required on 0.1.0
2886e96 (tag: 0.2.0) HEAD@{23}: checkout: moving from r/0.1.x to main
f0c018e (tag: 0.1.0) HEAD@{24}: checkout: moving from f0c018e6143a0f3807d1eccffe7db2f5bb916d75 to r/0.1.x
f0c018e (tag: 0.1.0) HEAD@{25}: checkout: moving from main to 0.1.0
2886e96 (tag: 0.2.0) HEAD@{26}: commit: working on release 0.2.0 - some more
30f3c6d HEAD@{27}: commit: working on release 0.2.0
f0c018e (tag: 0.1.0) HEAD@{28}: Branch: renamed refs/heads/master to refs/heads/main
f0c018e (tag: 0.1.0) HEAD@{30}: commit: file 2 added
468b2c1 (tag: 0.0.1) HEAD@{31}: commit: change txt to md
84589ea (tag: 0.0.0) HEAD@{32}: commit (initial): init
```