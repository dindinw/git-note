# Git types

There are 4 kinds of Object types in Git :
  * [blob type](#blob-type)
  * [tree type](#tree-type)
  * [commit type](#commit-type)
  * [tag type](#tag-type)

## Blob type
the blob type is just the content of your file.

create a empty git repository for testing. 
    
    $ mkdir test1
    $ cd test1/
    $ git init
    Initialized empty Git repository in /home/alex/works/tools/git-sandbox/test1/.git/
    
look up the `.git/objects` folders changes

    $ find .git/objects/
    .git/objects/
    .git/objects/pack
    .git/objects/info
    $ echo "first file" > first.txt

after execute `git add`, a new SHA1 hash `303ff981c488b812b6215f7db7920dedb3b59d9a` created.

    $ git add .
    $ find .git/objects/
    .git/objects/
    .git/objects/pack
    .git/objects/30
    .git/objects/30/3ff981c488b812b6215f7db7920dedb3b59d9a
    .git/objects/info

use `303f` as a shorthand of `303ff981c488b812b6215f7db7920dedb3b59d9a`. (the `303f` is the 
shortest accepted one under the context, when the repository get bigger, the longer hash 
shorthand may be required.


    $ git rev-parse 303
    303
    fatal: ambiguous argument '303': unknown revision or path not in the working tree.
    Use '--' to separate paths from revisions, like this:
    'git <command> [<revision>...] -- [<file>...]'
    $ git rev-parse 303f
    303ff981c488b812b6215f7db7920dedb3b59d9a
    $ git rev-parse 303ff
    303ff981c488b812b6215f7db7920dedb3b59d9a
    $ git rev-parse 303ff9
    303ff981c488b812b6215f7db7920dedb3b59d9a

show the object type is blob.

    $ git cat-file -t 303ff9
    blob

show the length of content of the blob.

    $ git cat-file -s 303ff9
    11

show the content of the blob 

    $ git cat-file -p 303ff9
    first file

clone a new resposity test1-clone from test1

    $ git clone test1/ test1-clone
    Cloning into 'test1-clone'...
    warning: You appear to have cloned an empty repository.
    done.
    
    $ diff -r test1 test1-clone/
    Only in test1: first.txt
    diff -r test1/.git/config test1-clone/.git/config
    5a6,11
    > [remote "origin"]
    >     fetch = +refs/heads/*:refs/remotes/origin/*
    >     url = /home/alex/works/tools/git-sandbox/test1/
    > [branch "master"]
    >     remote = origin
    >     merge = refs/heads/master
    Only in test1/.git: index


## Tree Type

## Commit Type

## Tag Type
