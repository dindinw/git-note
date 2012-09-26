# Git types

There are 4 kinds of Object types in Git :
  * blob type
  * tree type
  * commit type
  * tag type

## Blob type
the blob type is just the content of your file.

create a empty git reposity for testing. 
    
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

show the object type is blob.

    $ git cat-file -t 303ff9
    blob

show the length of content of the blob.

    $ git cat-file -s 303ff9
    11

show the content of the blob 

    $ git cat-file -p 303ff9
    first file
