# Trunk-based development playground

This repo is to verify some operations before deciding on using TBD in commercial projects.

## More info

<https://trunkbaseddevelopment.com/>

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
1. cherry-pick last commit from main to r/0.1.x, create tag 0.1.3 on branch r/0.1.x

```js
❯ git reflog

2cff2e6 (HEAD -> main, origin/main) HEAD@{0}: commit: changed readme.md to utf-8
ed77465 HEAD@{1}: commit: added git attributes
fd6d347 HEAD@{2}: checkout: moving from r/0.3.x to main
b0a2455 (tag: 0.3.1, origin/r/0.3.x, r/0.3.x) HEAD@{3}: cherry-pick: hotfixing 0.3 and 0.1
04f7438 (tag: 0.3.0) HEAD@{4}: reset: moving to HEAD
04f7438 (tag: 0.3.0) HEAD@{5}: checkout: moving from 04f7438318e636c35eb432d846399389ad7a2c5a to r/0.3.x
04f7438 (tag: 0.3.0) HEAD@{6}: checkout: moving from r/0.1.x to 0.3.0
9ae2307 (tag: 0.1.2, origin/r/0.1.x, r/0.1.x) HEAD@{7}: cherry-pick: hotfixing 0.3 and 0.1
ba58520 (tag: 0.1.1) HEAD@{8}: checkout: moving from main to r/0.1.x
fd6d347 HEAD@{9}: commit: hotfixing 0.3 and 0.1
a9f043a HEAD@{10}: commit: some more working on 0.4.0
38429e0 HEAD@{11}: commit: working on 0.4.0
04f7438 (tag: 0.3.0) HEAD@{12}: commit: add readme.md
2d7d83c HEAD@{13}: checkout: moving from r/0.1.x to main
ba58520 (tag: 0.1.1) HEAD@{14}: cherry-pick: hotfix that is required on 0.1.0
f0c018e (tag: 0.1.0) HEAD@{15}: checkout: moving from main to r/0.1.x
2d7d83c HEAD@{16}: commit: hotfix that is required on 0.1.0
2886e96 (tag: 0.2.0) HEAD@{17}: checkout: moving from r/0.1.x to main
f0c018e (tag: 0.1.0) HEAD@{18}: checkout: moving from f0c018e6143a0f3807d1eccffe7db2f5bb916d75 to r/0.1.x
f0c018e (tag: 0.1.0) HEAD@{19}: checkout: moving from main to 0.1.0
2886e96 (tag: 0.2.0) HEAD@{20}: commit: working on release 0.2.0 - some more
30f3c6d HEAD@{21}: commit: working on release 0.2.0
f0c018e (tag: 0.1.0) HEAD@{22}: Branch: renamed refs/heads/master to refs/heads/main
f0c018e (tag: 0.1.0) HEAD@{24}: commit: file 2 added
468b2c1 (tag: 0.0.1) HEAD@{25}: commit: change txt to md
84589ea (tag: 0.0.0) HEAD@{26}: commit (initial): init
```