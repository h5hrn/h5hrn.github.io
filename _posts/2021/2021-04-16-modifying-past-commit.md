---
layout: post
title:  "Git における過去コミットの修正"
categories: Git
---

### 注意点

過去のコミットを修正すると、修正したコミット以降のハッシュ値が変わる。そのため、GitHub 等のリモートリポジトリに `push` するときは、`push -f` とする必要がある。これは強制的に上書きする行為なので、複数人で管理しているときなどは注意すること。

### 初期状態

以下のファイルを上から順番にコミットしたとする。

- hello
- morning
- night

```sh
$ ls
hello  morning  night

$ cat hello
hallo, world!

$ cat morning
god morning!

$ cat night
good night

$ git log --oneline
564bd9c (HEAD -> master) Add night
97e326f Add morning
b739670 first commit
```

### 直前のコミットを修正

直前のコミットを修正したい場合は、`commit` コマンドに `--amend` オプションを付ける。

以下の例では、`night` ファイルに `sweet dreams` というメッセージを追加し、コミットメッセージも変更している。

```sh
$ echo 'sweet dreams' >> night

$ git diff
diff --git a/night b/night
index d55621b..461a258 100644
--- a/night
+++ b/night
@@ -1 +1,2 @@
 good night
+sweet dreams

$ git add night

$ git commit --amend -m "Add night messages"
[master a8e822b] Add night messages
 Date: Fri Apr 16 01:13:50 2021 +0900
 1 file changed, 2 insertions(+)
 create mode 100644 night

$ git log --oneline
a8e822b (HEAD -> master) Add night messages
97e326f Add morning
b739670 first commit
```

### 2つ以上前のコミットを修正

2つ前にコミットした `morning` ファイルの内容が `god morning` になっているので `good morning` に修正する。

`commit --amend` は直前のコミットにしか使えないので、`rebase` コマンドを用いる。

`git log` でコミットログを出力し、修正したいコミットの1つ前のコミットを指定して `rebase` を実行する。

```sh
$ git log --oneline
a8e822b (HEAD -> master) Add night messages
97e326f Add morning
b739670 first commit

$ git rebase -i b739670
Stopped at 97e326f...  Add morning
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue
```

以下のファイルが開かれるので、修正したいコミットの `pick` を `edit` に変更して保存する。

```
- pick 97e326f Add morning
+ edit 97e326f Add morning
pick a8e822b Add night messages

# Rebase b739670..a8e822b onto b739670 (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

ファイルを閉じると、修正したいコミットをした直後の状態になる。

```sh
$ git log --oneline
97e326f (HEAD) Add morning
b739670 first commit
```

あとは直前のコミットを修正するのと同じように修正する。

```sh
$ git diff
diff --git a/morning b/morning
index c94a8fd..527cb14 100644
--- a/morning
+++ b/morning
@@ -1 +1 @@
-god morning!
+good morning!

$ git add morning

$ git commit --amend -m "Add morning"
[detached HEAD 4667fe5] Add morning
 Date: Fri Apr 16 01:12:51 2021 +0900
 1 file changed, 1 insertion(+)
 create mode 100644 morning

$ git status
interactive rebase in progress; onto b739670
Last command done (1 command done):
   edit 97e326f Add morning
Next command to do (1 remaining command):
   pick a8e822b Add night messages
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'master' on 'b739670'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

nothing to commit, working tree clean
```

このコミット以外は修正しないので、以下コマンドで最新コミットまで進める。

```sh
$ git rebase --continue
Successfully rebased and updated refs/heads/master.

$ git status
On branch master
nothing to commit, working tree clean

$ git log --oneline
63c4c92 (HEAD -> master) Add night messages
4667fe5 Add morning
b739670 first commit
```

### 初回コミットを修正

初回にコミットした `hello` ファイルの内容を `hallo world!` から `hello, world!` に修正する。

しかし、`rebase -i` では初回コミットの前のコミットハッシュを指定することができない。

この場合は、`rebase -i --root` とすると、初回コミットを修正することができる。

```sh
$ git rebase -i --root
Stopped at b739670...  first commit
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue
```

初回コミットを `edit` に変更し、保存して閉じる。

```
- pick b739670 first commit
+ edit b739670 first commit
pick 4667fe5 Add morning
pick 63c4c92 Add night messages
```

初回コミット直後の状態になるので、先ほどと同じような手順で修正を反映する。

```sh
$ git diff
diff --git a/hello b/hello
index 7ed9b8c..270c611 100644
--- a/hello
+++ b/hello
@@ -1 +1 @@
-hallo, world!
+hello, world!

$ git add hello

$ git commit --amend -m "Add hello"
[detached HEAD 2390055] Add hello
 Date: Fri Apr 16 01:11:31 2021 +0900
 1 file changed, 1 insertion(+)
 create mode 100644 hello

$ git status
interactive rebase in progress; onto dc37feb
Last command done (1 command done):
   edit b739670 first commit
Next commands to do (2 remaining commands):
   pick 4667fe5 Add morning
   pick 63c4c92 Add night messages
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'master' on 'dc37feb'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

nothing to commit, working tree clean

$ git rebase --continue
Successfully rebased and updated refs/heads/master.

$ git log --oneline
078325e (HEAD -> master) Add night messages
f56e0d9 Add morning
2390055 Add hello
```
