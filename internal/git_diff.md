Git Diff
========

Unix Diff
---------


4 kinds of Git Diff
-------------------

There are 4 types of git diff, they are :

* Type 1 : `git diff` , `WD` (working dictionary) vs. `Index`

* Type 2 : `git diff <commit>`, `WD` vs. <commit>.
           `git diff HEAD` , `WD` vs. `HEAD`

* Type 3 : `git diff --cached`, `Index` vs. `HEAD`
           `git diff --cached <commit>`, `Index` vs. <commit>

* Type 4 : `git diff <commit1> <commit2>`, <commit1> vs. <commit2> 

1. Prepare a empty repository. And test stuffs

  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  $ mkdir test && cd test
  $ git init
  Initialized empty Git repository in /Users/alexwu/dev/git-sandbox/test/.git/
  $ echo -e "hello\nworld" > hello.txt
  $ cat hello.txt 
  hello
  world
  $ git add hello.txt && git commit -a -m "initialize hello."
  [master (root-commit) a36828c] initialize hello.
   1 file changed, 2 insertions(+)
   create mode 100644 hello.txt
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2. Before any changes, `WD`,`Index`,`HEAD`(master) both clean

  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  $ git diff
  $ git diff --cached
  $ git diff --cached master
  $ git diff --cached HEAD
  $ git diff
  $ git ls-files -s
  100644 94954abda49de8615a048f8d2e64b5de848e27a1 0	hello.txt
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3. a normal edit,add,commit working flow.

  (1) Edit hello.txt   : `WD` != `Index` ; `Index` = `HEAD`
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  $ sed -i '' -e's/world/My world!/' hello.txt 
  
  # the WD is dirty, WD vs Index, shows 1 insertion, 1 deletion
  $ git diff
  diff --git a/hello.txt b/hello.txt
  index 94954ab..38e4f48 100644
  --- a/hello.txt
  +++ b/hello.txt
  @@ -1,2 +1,2 @@
   hello
  -world
  +My world!
  
  # The `Index` is not touched, index is same with `HEAD`(master). 
  $ git diff --cached
  $ git diff --cached HEAD
  $ git diff --cached master
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
  (2) Add hello.txt    : `WD` = `Index` ; `Index` != `HEAD`
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  # staged changes, now index updated, WD = Index, index != HEAD(master)
  $ git add hello.txt
  $ git diff
  $ git diff --cached
  diff --git a/hello.txt b/hello.txt
  index 94954ab..38e4f48 100644
  --- a/hello.txt
  +++ b/hello.txt
  @@ -1,2 +1,2 @@
   hello
  -world
  +My world!
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  (3) Commit hello.txt : `WD` = `Index` = `HEAD` 
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  # commit changes, now HEAD(master) updated, WD=Index=HEAD, clean again.
  $ git commit -a -m "change hello.txt, world->My world"
  [master 1b0ffec] change hello.txt, world->My world
   1 file changed, 1 insertion(+), 1 deletion(-)
  $ git diff
  $ git diff --cached
  $ git diff --cached HEAD
  $ git diff --cached master
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Git Diff when Merge Conflict
-----------------------------

Still use the previous example, we have two commits under `master`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ git show-branch --more=2
[master] change hello.txt, world->My world
[master^] initialize hello.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

now, we create a branch `br01` under `master^`(the initial commit)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ git checkout -b br01 master^
Switched to a new branch 'br01'
$ git show-branch                                        (0)
* [br01] initialize hello.
 ! [master] change hello.txt, world->My world
--
 + [master] change hello.txt, world->My world
*+ [br01] initialize hello.
$ sed -i '' -e's/world/My world under br01/' hello.txt
$ git diff                                               (1)
diff --git a/hello.txt b/hello.txt
index 94954ab..dcf51d0 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1,2 +1,2 @@
 hello
-world
+My world under br01
$ git diff --cached                                      (2)
$ git diff --cached HEAD                                 (3)
$ git diff --cached master                               (4)
diff --git a/hello.txt b/hello.txt
index 38e4f48..94954ab 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1,2 +1,2 @@
 hello
-My world!
+world
$ git diff master                                        (5)
diff --git a/hello.txt b/hello.txt
index 38e4f48..dcf51d0 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1,2 +1,2 @@
 hello
-My world!
+My world under br01
$ git diff HEAD master                                   (6)
diff --git a/hello.txt b/hello.txt
index 94954ab..38e4f48 100644
--- a/hello.txt
+++ b/ello.txt
@@ -1,2 +1,2 @@
 hello
-world
+My world!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 * (0): git show-branch will stop at the most recent commit which can be seen in all branches. so the [master] is the commit. note, there are two part of output of show-branch, the upper side is the list of branches (aka. `br01` and `master`), the lower part is the list of commits. 
 * (1): `WD` vs. `Index`, a line removed, a line inserted
 * (2): `Index` vs. `HEAD`, since changes not be staged, no difference
 * (3): Same with (2), (2) is the default version of (3). 
 * (4): `Index` vs. `master`, `Index`=`HEAD` --> `HEAD` vs. `master` -> `master^` vs. `master`
 * (5): `WD` vs. `master`
 * (6): Same with (4).


Now add(stage) the change to `Index`, and commit changes.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ git add hello.txt
$ git diff
$ git diff --cached
diff --git a/hello.txt b/hello.txt
index 94954ab..dcf51d0 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1,2 +1,2 @@
 hello
-world
+My world under br01
$ git commit -m "change world to My world under br01"
[br01 952dcf7] change world to My world under br01
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git show-branch                                          (1)
* [br01] change world to My world under br01
 ! [master] change hello.txt, world->My world
--
*  [br01] change world to My world under br01
 + [master] change hello.txt, world->My world
*+ [br01^] initialize hello.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * (1) git show-branch will stop at the most recent commit which can be seen in all branch, this time, the `[br01]` is the most recent commit.


Switch to `master`, and try to merge `br01` into `master`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ git checkout master
Switched to branch 'master'
$ git ls-files -s                                                        (1)
100644 38e4f4809c981377e79497d339a37a41adab023f 0	hello.txt
$ cat hello.txt 
hello
My world!
$ git log --oneline
3fe6062 change hello.txt, world->My world
1c9313b initialize hello.
$ git rev-parse --short HEAD
3fe6062

$ git merge br01                                                         (2)
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.

$ git ls-files -s                                                        (3)
100644 94954abda49de8615a048f8d2e64b5de848e27a1 1	hello.txt
100644 38e4f4809c981377e79497d339a37a41adab023f 2	hello.txt
100644 dcf51d0a6ab1f5c3aea17f8a8db5bc6aa3848bd8 3	hello.txt

$ git cat-file -p 94954                                                  (4)
hello
world
$ git cat-file -p 38e4f                                                  (5)
hello
My world!
$ git cat-file -p dcf51                                                  (6)
hello
My world under br01

$ git log --all --oneline --graph                                        (7)
* 952dcf7 change world to My world under br01
| * 3fe6062 change hello.txt, world->My world
|/  
* 1c9313b initialize hello.

$ git cat-file -p 1c9313b                                                (8)
tree b2ebf159fb11cd612e1c3e62636be14ce60424ad
author Alex Wu <wuyiding@gmail.com> 1349658897 +0800
committer Alex Wu <wuyiding@gmail.com> 1349658897 +0800

initialize hello.
$ git cat-file -p b2ebf15
100644 blob 94954abda49de8615a048f8d2e64b5de848e27a1	hello.txt


$ git cat-file -p 3fe6062                                                (9)
tree 2d21e0a53dbdc79b40a11c04a7ba0a92f9da6b74
parent 1c9313b4c403618801d753346a0a14d9ba27fcc6
author Alex Wu <wuyiding@gmail.com> 1349658978 +0800
committer Alex Wu <wuyiding@gmail.com> 1349658978 +0800

change hello.txt, world->My world
$ git cat-file -p 2d21e0a
100644 blob 38e4f4809c981377e79497d339a37a41adab023f	hello.txt


$ git cat-file -p 952dcf7                                                (10)
tree dbf53240cfb4013db87acda53b5f44ec861f97f4
parent 1c9313b4c403618801d753346a0a14d9ba27fcc6
author Alex Wu <wuyiding@gmail.com> 1349662968 +0800
committer Alex Wu <wuyiding@gmail.com> 1349662968 +0800

change world to My world under br01
$ git cat-file -p dbf5324
100644 blob dcf51d0a6ab1f5c3aea17f8a8db5bc6aa3848bd8	hello.txt

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * (1) There is only one version under `Index`, it means `master` is clean. the blob of hello.txt is `38e4f`, the current `HEAD` is `3fe6062`
  * (2) merge `br01` into `master` , got conflict! 
  * (3) `Index` shows 3 versions of hello.txt.
  * (4) Version 1 (`94954a`) is the initailized version. the merge-base, see (8)
  * (5) Version 2 (`38e4f4`) is master (HEAD) version. see (9)
  * (6) Version 3 (`dcf51d`) is br01 branch version. see (10)
  * (7) list all commits we have, 
  * (8) 1c9313b->b2ebf15->94954a , see (4)
  * (9) 3fe6062->2d21e0a->38e4f4 , see (5)
  * (10) 953dcf7->dbf5324->dcf51d, see (6) 

Now let look at the `git diff` result for merge conflict
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ cat hello.txt
hello
<<<<<<< HEAD
My world!
=======
My world under br01
>>>>>>> br01

$ git diff                                                                (1)
diff --cc hello.txt
index 38e4f48,dcf51d0..0000000                                            (2)
--- a/hello.txt
+++ b/hello.txt
@@@ -1,2 -1,2 +1,6 @@@
  hello                                                                   (3)
++<<<<<<< HEAD                                                            (4)
 +My world!                                                               (5)
++=======                                                                 (6)
+ My world under br01                                                     (7)
++>>>>>>> br01                                                            (8)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * (1) `WD`(cat hello.txt) vs `Index`,  `Index` has three commits: merge_base (init 000000), ours(master), theirs(br01)
  * (2) `38e4f48` is the `master`, `dcf51d0` is the `br01`.  
  * (3)  empty : not changes for both.
  * (4)  `++` : add for both.  
  * (5)  ` +` : plus under second column, changes for `theirs`: add a line to `dcf51d0` (br01)
  * (6)  same with (4)
  * (7)  `+ ` : plus under first column, changes for `ours`, add a line to `38e4f48` (master) 
  * (8)  same with (4)


The `git diff -cached` shows only a warning.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ git diff --cached
* Unmerged path hello.txt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now look at `--ours` and `--theirs`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ git diff --ours
* Unmerged path hello.txt                                                 (1)
diff --git a/hello.txt b/hello.txt
index 38e4f48..369afc9 100644                                             (2)
--- a/hello.txt
+++ b/hello.txt
@@ -1,2 +1,6 @@
 hello
+<<<<<<< HEAD
 My world!
+=======
+My world under br01
+>>>>>>> br01

$ git cat-file -p 369afc9                                                 (3)
hello
<<<<<<< HEAD
My world!
=======
My world under br01
>>>>>>> br01

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * (1) shows a warning, since the conflict still not resoved.
  * (2) `38e4f48` is the `master` version, `369afc9` is the blob for hello.txt under `WD`, see (3) 
  * `--ours` means `HEAD`(`master` aka ours verison) vs. current `WD`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ git diff --theirs
* Unmerged path hello.txt
diff --git a/hello.txt b/hello.txt
index dcf51d0..369afc9 100644                                             (4)
--- a/hello.txt
+++ b/hello.txt
@@ -1,2 +1,6 @@
 hello
+<<<<<<< HEAD
+My world!
+=======
 My world under br01
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  * (4) `dcf51d0` is the blob under `br01`
  * `--theirs` means branch (`br01` aka theirs version) vs. current `WD`


