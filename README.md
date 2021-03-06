# git-submodule-practice

## サブモジュールの作成

```
# サブモジュールの作成
$ git submodule add git@github.com:twbs/bootstrap.git bootstrap

# ファイルの内容やコミットの差分などを表示する
$ cd bootstrap
$ git show
commit 8bb68b04b35765c30038923bd620ee28b21553c8 (HEAD -> main, origin/main, origin/HEAD)
Author: Patrick H. Lauke <redux@splintered.co.uk>
Date:   Wed Jul 13 22:31:15 2022 +0100

    Add accNames to all progress bar examples (#36732)
    
    Co-authored-by: Julien Déramond <juderamond@gmail.com>
    Co-authored-by: Abdullah Alaqeel <abdullah.t.aqeel@gmail.com>

diff --git a/site/content/docs/5.2/components/progress.md b/site/content/docs/5.2/components/progress.md
index 8fbba3be7..aa793683b 100644
--- a/site/content/docs/5.2/components/progress.md
+++ b/site/content/docs/5.2/components/progress.md
@@ -13,25 +13,25 @@ Progress components are built with two HTML elements, some CSS to set the width,
 - We use the `.progress` as a wrapper to indicate the max value of the progress bar.
 - We use the inner `.progress-bar` to indicate the progress so far.
 - The `.progress-bar` requires an inline style, utility class, or custom CSS to set their width.
-- The `.progress-bar` also requires some `role` and `aria` attributes to make it accessible.
+- The `.progress-bar` also requires some `role` and `aria` attributes to make it accessible, including an accessible name (using `aria-label`, `aria-labelledby`, or similar).
 
 Put that all together, and you have the following examples.
 
 {{< example >}}
 <div class="progress">

# コミットする
$ git commit -m "Add Twitter Bootstrap as a submodule"
```

## サブモジュールの更新

```
$ cd bootstrap
$ git branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/basic-sass-tests
  remotes/origin/carousel-rtl-css
  remotes/origin/collapse-parent-ex-link-accordion
  remotes/origin/cssvar-function
  remotes/origin/darkmode

# 作業ツリーを異なるブランチに切り替える
$ git checkout darkmode
branch 'darkmode' set up to track 'origin/darkmode'.
Switched to a new branch 'darkmode'

# ワークツリーにあるファイルの状態を表示する
$ cd ..
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   bootstrap (new commits)

no changes added to commit (use "git add" and/or "git commit -a")

# コミット同士やコミットと作業ツリーの内容を比較する
$ git diff
diff --git a/bootstrap b/bootstrap
index 8bb68b0..85be61d 160000
--- a/bootstrap
+++ b/bootstrap
@@ -1 +1 @@
-Subproject commit 8bb68b04b35765c30038923bd620ee28b21553c8
+Subproject commit 85be61d34b8d1d0dcc86ba9f7ab6a831b3ce5d20

# コミットする
git add bootstrap
git commit -m "Update submodule: Bootstrap"
```

## 別のコミットをチェックアウトする & git submodule update


- 過去のコミットにチェックアウトしたり別のブランチにチェックアウトした場合、「submoduleを使用しているリポジトリ」と「submoduleの内容」が連動しない点に注意
- 連動させるための方法を以下に記す

```
# コミット時のログを表示する
$ git log --oneline
39aac1d (HEAD -> main, origin/main, origin/HEAD) Update readme
668103d Update submodule: Bootstrap
866910d サブモジュールの作成
74c9f52 Initial commit

# 過去のコミットにチェックアウトする
$ git checkout 866910d
M       bootstrap
Note: switching to '866910d'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 866910d サブモジュールの作成

# statusで確認すると、bootstrapディレクトリでdiffが発生した
# checkoutが連動していないのが分かる
$ git status
HEAD detached at 866910d
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   bootstrap (new commits)

no changes added to commit (use "git add" and/or "git commit -a")

# submoduleを更新して連動させる
$ git submodule update
Submodule path 'bootstrap': checked out '8bb68b04b35765c30038923bd620ee28b21553c8'

# statusを確認すると連動してるのが確認できた
git status          
HEAD detached at 866910d
nothing to commit, working tree clean
```