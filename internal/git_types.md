## Git types

4 kinds of Object types:
  * blob type
  * tree type
  * commit type

### Blob type
the blob type is just the content of your file.

1. create a empty git reposity for testing. 
 
alex@alex-ubuntu-vm:~/works/tools/git-sandbox$ mkdir test1
alex@alex-ubuntu-vm:~/works/tools/git-sandbox$ cd test1/
alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ git init
Initialized empty Git repository in /home/alex/works/tools/git-sandbox/test1/.git/

2. look up the .git/objects folders changes

alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ find .git/objects/
.git/objects/
.git/objects/pack
.git/objects/info
alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ echo "first file" > first.txt

3. after git add, a new SHA1 hash 303ff981c488b812b6215f7db7920dedb3b59d9a created.

alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ git add .
alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ find .git/objects/
.git/objects/
.git/objects/pack
.git/objects/30
.git/objects/30/3ff981c488b812b6215f7db7920dedb3b59d9a
.git/objects/info

4. show the object type is blob.

alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ git cat-file -t 303ff9
blob

5. show the length of content of the blob.

alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ git cat-file -s 303ff9
11

6. show the content of the blob 
alex@alex-ubuntu-vm:~/works/tools/git-sandbox/test1$ git cat-file -p 303ff9
first file


