
How to replace the master by a branch
=====================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
$ git branch rep_branch
$ git checkout rep_branch
$ git merge --no-commit -s ours master
$ git commit -m "It's replacement for branch to master"
$ git checkout master
$ git merge rep_branch 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using GUI tool for diff and resove merge conflicts
==================================================
By using meld

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
$ meld . 
$ git config merge.tool meld
$ git mergetool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Checkout only single file
=========================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
$ git clone --no-checkout --depth 1 foo@bar.com:/path/to/your/repo my_repo
$ cd my_repo
$ git checkout HEAD your_file_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NOTE: the entires content of your remote repo is copied, it's can't be avoid, event you can shortage the history by using "--depth 1" 

Different between git-diff
==========================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
$ git diff 
    # show diff of unstaged changes
$ git diff --cached 
    # show diff of staged changes
$ git diff HEAD 
    # show diff of all staged or unstaged changes (between working and last commit)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Remove file form history
========================

1. remove file from your local history.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch $file_name' HEAD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2. force push back to the remote repo.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
$ git push origin master --force
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3. clean up 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
$ rm -rf .git/refs/original/
$ git reflog expire --expire=now --all
$ git gc --prune=now
$ git gc --aggressive --prune=now
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Using GitHub
============

Set up ssh key/account for github
---------------------------------

1. Create ssh isa public key by fellowing steps:

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
	$ cd ~/.ssh
	$ mkdir key_backup && cp is_rsa* key_backup
	$ rm id_rsa*
	$ ssh-keygen -t rsa -C "your_email@youremail.com"
	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/your_user_dir/.ssh/id_rsa): <press enter>
	Enter passphrase (empty for no passphrase): <enter your passphrase>
	Enter same passphrase again: <enter again>
	Your identification has been saved in /home/your_user_dir/.ssh/id_rsa.
	Your public key has been saved in /home/your_user_dir/.ssh/id_rsa.pub.
	The key fingerprint is:
	7f:ba:37:75:76:ad:24:e6:f4:36:f5:4a:d0:9e:32:ee your_email@youremail.com
	The key's randomart image is:
	+--[ RSA 2048]----+
	|                 |
	|                 |
	|                 |
	|             .   |
	|        S   . . .|
	|         .  +oo.*|
	|          .++=+=o|
	|           +++= .|
	|          o+Eo.o |
	+-----------------+
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2. now the id_ras.pub (isa public key) is created, go to the GitHub site ( “Account Settings” > “SSH Public Keys” > “Add another public key”) copy the content of the id_ras.pub. 

3. test your ssh connection with gibhub

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
	$ ssh -T git@github.com
	The authenticity of host 'github.com (207.97.227.239)' can't be established.
	RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
	Are you sure you want to continue connecting (yes/no)? yes
	Warning: Permanently added 'github.com,207.97.227.239' (RSA) to the list of known hosts.
	Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Initial repo setup on github
----------------------------
Global setup:

    $ git config --global user.name "alex wu"
    $ git config --global user.email wuyiding@gmail.com

Next steps:

    $ mkdir your-proj
    $ cd your-proj
    $ git init
    $ touch README
    $ git add README
    $ git commit -m 'first commit'
    $ git remote add origin git@github.com:dindinw/your-proj.git
    $ git push -u origin master

* NOTE: the '-u' option made the github repository the default repo when 
        you do 'git push' without any option. the 'u' means 'Upstream".       

Existing Git Repo?

    $ cd existing_git_repo
    $ git remote add origin git@github.com:dindinw/your-proj.git
    $ git push -u origin master
      


Syncing with private(myself) and public(github) repos
-----------------------------------------------------

1.	change the remote name

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
	$ cat .git/config
	[core]
		repositoryformatversion = 0
		filemode = true
		bare = false
		logallrefupdates = true
	[remote "origin"]
		url = git@github.com:dindinw/git-note.git
		fetch = +refs/heads/*:refs/remotes/origin/*
	[branch "master"]
		remote = origin
		merge = refs/heads/master
	
	$ git remote rename origin github
	$ cat .git/config
	[core]
		repositoryformatversion = 0
		filemode = true
		bare = false
		logallrefupdates = true
	[remote "github"]
		url = git@github.com:dindinw/git-note.git
		fetch = +refs/heads/*:refs/remotes/github/*
	[branch "master"]
		remote = github
		merge = refs/heads/master
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2.	add my private repo as origin, and set it back as default

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
	$ git remote add origin myrepo-url
	$ git config branch.master.remote origin
	$ cat .git/config
	[core]
		repositoryformatversion = 0
		filemode = true
		bare = false
		logallrefupdates = true
	[remote "github"]
		url = git@github.com:dindinw/git-note.git
		fetch = +refs/heads/*:refs/remotes/github/*
	[branch "master"]
		remote = origin
		merge = refs/heads/master
	[remote "origin"]
		url = alex@ubuntu-server-9i:~/gitrepo/git-note.git
		fetch = +refs/heads/*:refs/remotes/origin/*
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3.	add new remote 'all' for mutiple urls

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.bash}
	$ git config --add remote.all.url alex@ubuntu-server-9i:~/gitrepo/git-note.git
	$ git config --add remote.all.url git@github.com:dindinw/git-note.git
	$ cat .git/config
	[core]
		repositoryformatversion = 0
		filemode = true
		bare = false
		logallrefupdates = true
	[remote "github"]
		url = git@github.com:dindinw/git-note.git
		fetch = +refs/heads/*:refs/remotes/github/*
	[branch "master"]
		remote = origin
		merge = refs/heads/master
	[remote "origin"]
		url = alex@ubuntu-server-9i:~/gitrepo/git-note.git
		fetch = +refs/heads/*:refs/remotes/origin/*
	[remote "all"]
		url = alex@ubuntu-server-9i:~/gitrepo/git-note.git
		url = git@github.com:dindinw/git-note.git
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now, we can use 'git push all' to publish my changes to all repos.


