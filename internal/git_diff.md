Git Diff
========

Unix Diff
---------


4 kinds of Git Diff
-------------------

There are 4 types of git diff, they are :

* Type 1 : `git diff` , WD (working dictionary) vs. Index

* Type 2 : `git diff <commit>`, WD vs. <commit>.

* Type 3 : `git diff --cached`, index vs. HEAD
           `git diff --cached <commit>`, index vs. <commit>

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

2. Before any changes, WD,Index,HEAD(master) both clean

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

   (1) Edit hello.txt   : WD != Index ; Index = HEAD
   (2) Add hello.txt    : WD = Index ; Index != HEAD
   (3) Commit hello.txt : WD = Index = HEAD 

  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  $ sed -ie 's/world/My world!/' hello.txt 
  
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
  
  # The index is not touched, index is same with HEAD(master). 
  $ git diff --cached
  $ git diff --cached HEAD
  $ git diff --cached master
  
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
  
  # commit changes, now HEAD(master) updated, WD=Index=HEAD, clean again.
  $ git commit -a -m "change hello.txt, world->My world"
  [master 1b0ffec] change hello.txt, world->My world
   1 file changed, 1 insertion(+), 1 deletion(-)
  $ git diff
  $ git diff --cached
  $ git diff --cached HEAD
  $ git diff --cached master
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


