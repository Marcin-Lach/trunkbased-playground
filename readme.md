# Trunk-based development playground

This repo is to verify some operations before deciding on using TBD in commercial projects.

## More info

<https://trunkbaseddevelopment.com/>

## Sequence of operations done in this repo

1. initial commit + tag 0.0.0
1. add more commits and tags on main (0.0.1, 0.1.0, 0.2.0 - annotated)
1. add a hotfix to tag 0.1.0
1. tag main with 0.3.0

❯ git reflog
ba58520 (HEAD -> r/0.1.x, tag: 0.1.1, origin/r/0.1.x) HEAD@{0}: cherry-pick: hotfix that is required on 0.1.0
f0c018e (tag: 0.1.0) HEAD@{1}: checkout: moving from main to r/0.1.x
2d7d83c (origin/main, main) HEAD@{2}: commit: hotfix that is required on 0.1.0
2886e96 (tag: 0.2.0) HEAD@{3}: checkout: moving from r/0.1.x to main
f0c018e (tag: 0.1.0) HEAD@{4}: checkout: moving from f0c018e6143a0f3807d1eccffe7db2f5bb916d75 to r/0.1.x
f0c018e (tag: 0.1.0) HEAD@{5}: checkout: moving from main to 0.1.0
2886e96 (tag: 0.2.0) HEAD@{6}: commit: working on release 0.2.0 - some more
30f3c6d HEAD@{7}: commit: working on release 0.2.0
f0c018e (tag: 0.1.0) HEAD@{8}: Branch: renamed refs/heads/master to refs/heads/main
f0c018e (tag: 0.1.0) HEAD@{10}: commit: file 2 added
468b2c1 (tag: 0.0.1) HEAD@{11}: commit: change txt to md
84589ea (tag: 0.0.0) HEAD@{12}: commit (initial): init
