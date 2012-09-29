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

  * clone will add remote repository config (where it comming from)
  * clone will not copy uncommit file and content (the `first.txt`)
  * clone will not copy uncommit stages (the `index` file) 

## Tree Type

create new empty repository `test2`

    $ mkdir test2
    $ cd test2
    $ git init
    Initialized empty Git repository in /Users/alexwu/dev/git-sandbox/test2/.git/

add a file `second.txt` (the blob `1c59427adc4b205a270d8f810310394962e79a8b`) and commit it this time

    $ echo "second file" > second.txt
    $ git add . 
    $ find .git/objects/
    .git/objects/
    .git/objects//1c
    .git/objects//1c/59427adc4b205a270d8f810310394962e79a8b
    .git/objects//info
    .git/objects//pack
    $ git commit -a -m "test commit file"
    [master (root-commit) 50d84af] test commit file
     1 file changed, 1 insertion(+)
     create mode 100644 second.txt

now 2 addtional hashes found: `50d84` and `77a48` 
    
    $ find .git/objects/
    .git/objects/
    .git/objects//1c
    .git/objects//1c/59427adc4b205a270d8f810310394962e79a8b
    .git/objects//50
    .git/objects//50/d84af15d1ab38453ba09a6c2f27dc26f1ab904
    .git/objects//77
    .git/objects//77/a48e384b1588e0979c0e132e9bffa2a874668f
    .git/objects//info
    .git/objects//pack

check the tree type (`77a48`)

    $ git cat-file -t 77a48
    tree
    $ git cat-file -p 77a48
    100644 blob 1c59427adc4b205a270d8f810310394962e79a8b    second.txt


## Commit Type

The `50d84` is the commit type

    $ git cat-file -t 50d84
    commit
    $ git cat-file -p 50d84
    tree 77a48e384b1588e0979c0e132e9bffa2a874668f
    author Alex Wu <wuyiding@gmail.com> 1348665401 +0800
    committer Alex Wu <wuyiding@gmail.com> 1348665401 +0800


commit `50d84` => tree `77a48` => blob `lc594`

 * The `blob` references nothing and referenced only by `tree`. 
 * The `tree` references `blob` or `tree`.  
 * The `commit` references `tree` and(or if not parent) a parent `commit` 


## Tag Type
