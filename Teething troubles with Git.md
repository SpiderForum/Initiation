It isn't clear to me what I am doing here. 
I tried to create a branch to deal with Git alone, but as far as I can see I am dealing with the old master branch.
(OK, the indications were misleading; when I changed the text, the old branch was unaffected.)
I am continuing to see whether I wind up with two functional distinct branches, which is the intention.
OK I suppose, that looks as though it worked and I now have a separate teething troubles branch inside (*I hope*) repository Initiation.
For me, moving around still is cumbersome. I got here  by going right back to the SpiderForum organisation and then back down. But as I write this I still haven't managed to get out of this plain edit mode.
I have however found editing instructions at [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
and how to wrap the lines (click on the top right pull-down button in this editing window and select soft-wrap).

The markups include **bold** and *italic* and presumably ***both*** (I bet you can break lines too with br inside angle-brackets)
You can add URLs inside single square brackets.
Still can't find the updated "readable" text, only edit mode after committing.

##Branching in the GitHub interface

To create a new branch, you can go to the [main project page](https://github.com/SpiderForum/Initiation) and click on the button that says "Branch: master" and type the name of the new branch.

With that said, you really shouldn't be doing much of your work online through the web interface. Did you already [download git](https://git-scm.com/download/win) onto your local machine, and clone this project? The git software comes with a gui, but another option is to use [Github's GUI](https://desktop.github.com/) - that will probably give you a closer match between the web interface and your local interface.

##An example of how branches work

I just checked out the project to my local machine, and had a look at the structure:



```
david@davelaptop:~/dev/Initiation$ git log --all --graph --decorate

* commit 81c2c4222c813db764706e01c01d2d4bfbb9db6b (origin/Atom-installation-1)
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:28:51 2017 +0200
| 
|     Update Atom installation.MD
|  
* commit c3342deea3bbcfde1d740a8536bd676277a377a1 (origin/jonrichfield-patch-1)
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:27:17 2017 +0200
| 
|     Update Atom installation.MD
|  
* commit 825067610613893bf88d6e843c53e13680514b8b
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:25:28 2017 +0200
| 
|     Rename Atom installation to Atom installation.MD
|  
* commit aeecb1b7585c607495311b4c9b2c16326d95c8f0
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 20:55:36 2017 +0200
| 
|     Create Atom installation
|  
* commit a6b64a1c48060a0eaf1ff36ddde299d1f126e24c (HEAD, origin/master, origin/HEAD, master)
| Author: David Richfield <davidrichfield@gmail.com>
| Date:   Sun Mar 12 10:39:16 2017 +0100
| 
|     Fullstop at end of sentence
|  
```

etc. What we see here is a list of commits, and the important thing to notice is that they're not branched: the label "master" (the main branch) points at the commit starting with a6b64a1. The label "jonrichfield-patch-1" points at the commit starting with  c3342de, and the label "Atom-installation-1" points at the commit starting with 81c2c4. That means that a bunch of changes were made to master, under the branch jonrichfield-patch-1, and another change was made under the branch Atom-installation-1.

OK, let's see the difference between the branches:

```
david@davelaptop:~/dev/Initiation$ git diff master jonrichfield-patch-1 
diff --git a/Atom installation.MD b/Atom installation.MD
new file mode 100644
index 0000000..47649e6
--- /dev/null
+++ b/Atom installation.MD      
@@ -0,0 +1,5 @@
+Jon has installed Atom. 
+Already it looks usable and is inrteresting because its design is based on the WWW browser Chromium rather than a text editor base.
+It looks usable and has Git and Python support to some extent, which might prove useful, though it is to early for me to say how.
+So far all I can say is that it is an impressive package but I don't yet know how to use it with Git (or very likely I would be using it here now.
+Just finding my way around here is a bind!
```

So between master and jonrichfield-patch-1, a new file was created with some content. Let's see the difference between jonrichfield-patch-1 and Atom-installation-1:

```
david@davelaptop:~/dev/Initiation$ git diff jonrichfield-patch-1 Atom-installation-1 
diff --git a/Atom installation.MD b/Atom installation.MD
index 47649e6..8b13789 100644
--- a/Atom installation.MD      
+++ b/Atom installation.MD      
@@ -1,5 +1 @@
-Jon has installed Atom. 
-Already it looks usable and is inrteresting because its design is based on the WWW browser Chromium rather than a text editor base.
-It looks usable and has Git and Python support to some extent, which might prove useful, though it is to early for me to say how.
-So far all I can say is that it is an impressive package but I don't yet know how to use it with Git (or very likely I would be using it here now.
-Just finding my way around here is a bind!
+
```

So in that last commit, the file Atom installation.MD was cleared out, but it still exists.

Now as a project member, I look at this, and I say "jonrichfield-patch-1 looks good, that should merge into master, but there's a typo. This other branch, Atom-installation-1, doesn't look so good."

So step 1: merge the patch:

```
david@davelaptop:~/dev/Initiation$ git checkout master 
Already on 'master'
Your branch is up-to-date with 'origin/master'.
david@davelaptop:~/dev/Initiation$ git merge jonrichfield-patch-1 
Updating a6b64a1..c3342de
Fast-forward
 Atom installation.MD | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 Atom installation.MD
```

So what happened here is that I made sure that I was on the master branch, and I told git to merge whatever was in jonrichfield-patch-1 into master. Git realised that there is no branching going on: it can just move the master label to the same commit that is currently labeled as "jonrichfield-branch-1". That's called a "fast-forward" merge.  Let's see what the tree looks like now:

```
* commit 81c2c4222c813db764706e01c01d2d4bfbb9db6b (origin/Atom-installation-1, Atom-installation-1)
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:28:51 2017 +0200
| 
|     Update Atom installation.MD
|  
* commit c3342deea3bbcfde1d740a8536bd676277a377a1 (HEAD, origin/jonrichfield-patch-1, master, jonrichfield-patch-1)
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:27:17 2017 +0200
| 
|     Update Atom installation.MD
|  
* commit 825067610613893bf88d6e843c53e13680514b8b
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:25:28 2017 +0200
| 
|     Rename Atom installation to Atom installation.MD
|  
* commit aeecb1b7585c607495311b4c9b2c16326d95c8f0
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 20:55:36 2017 +0200
| 
|     Create Atom installation
|  
* commit a6b64a1c48060a0eaf1ff36ddde299d1f126e24c (origin/master, origin/HEAD)
| Author: David Richfield <davidrichfield@gmail.com>
| Date:   Sun Mar 12 10:39:16 2017 +0100
| 
|     Fullstop at end of sentence
|  
```

So notice: I haven't yet pushed anything to GitHub at this point, so origin/master is still at the old place (a6b64a) but my own master branch is now one away from the tip of the tree. No problem, I'll push when I'm done with the typo correction.

```
david@davelaptop:~/dev/Initiation$ vim Atom\ installation.MD 
david@davelaptop:~/dev/Initiation$ git status 
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   Atom installation.MD

no changes added to commit (use "git add" and/or "git commit -a")
```

So I've edited the file, but it's still sitting around in my working directory, and hasn't yet been added to git. Notice that git helpfully tells me how to sort that out - whether I want to discard or keep the changes. git commit -a is quick and easy:

```
david@davelaptop:~/dev/Initiation$ git commit -a -m "Typo fix"
[master 8fb2a84] Typo fix
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Let's see what the tree looks like now:

```
* commit 8fb2a848149b3eb3a4ede4a9c20075ae408a122b (HEAD, master)
| Author: David Richfield <davidrichfield@gmail.com>
| Date:   Mon Mar 13 00:54:04 2017 +0100
| 
|     Typo fix
|    
| * commit 81c2c4222c813db764706e01c01d2d4bfbb9db6b (origin/Atom-installation-1, Atom-installation-1)
|/  Author: Jon Richfield <jonrichfield@gmail.com>
|   Date:   Sun Mar 12 21:28:51 2017 +0200
|   
|       Update Atom installation.MD
|  
* commit c3342deea3bbcfde1d740a8536bd676277a377a1 (origin/jonrichfield-patch-1, jonrichfield-patch-1)
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:27:17 2017 +0200
| 
|     Update Atom installation.MD
|  
* commit 825067610613893bf88d6e843c53e13680514b8b
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 21:25:28 2017 +0200
| 
|     Rename Atom installation to Atom installation.MD
|  
* commit aeecb1b7585c607495311b4c9b2c16326d95c8f0
| Author: Jon Richfield <jonrichfield@gmail.com>
| Date:   Sun Mar 12 20:55:36 2017 +0200
| 
|     Create Atom installation
|  
* commit a6b64a1c48060a0eaf1ff36ddde299d1f126e24c (origin/master, origin/HEAD)
| Author: David Richfield <davidrichfield@gmail.com>
| Date:   Sun Mar 12 10:39:16 2017 +0100
| 
|     Fullstop at end of sentence
|  
```

Note: the origin/xxx labels are still at the commits where they were when I started, but now my local master is sitting at the tip of the tree, and depends on jonrichfield-patch-1, and not on Atom-installation-1, which is now a side-branch. Time to push my changes to GitHub. The only branch that has changed is master. I haven't done anything to the other two branches.

```
david@davelaptop:~/dev/Initiation$ git push origin master 
Username for 'https://github.com': slashme
Password for 'https://slashme@github.com': 
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 292 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/SpiderForum/Initiation.git
   a6b64a1..8fb2a84  master -> master
david@davelaptop:~/dev/Initiation$ git status 
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

And now the GitHub project has, on the "master" branch, the changes that were made in jonrichfield-patch-1, as well as my typo fix.

By the way, I'm writing this after the fact, so it goes into a new commit on master :-D
