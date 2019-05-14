# Git

-
-

## What we'll cover
<p class="fragment fade-up">What is Git?</p>
<p class="fragment fade-up">Basic Git Commands</p>
<p class="fragment fade-up">Intro to merge conflicts</p>
<p class="fragment fade-up">Fork/Pull</p>
<p class="fragment fade-up">.gitignore</p>

-
-

##What Is Git?
According to Wikipedia:

> Git is a version control system (VCS) for tracking changes in computer files and coordinating work on those files among multiple people. It is primarily used for software development, but it can be used to keep track of changes in any files.

-
-

##Basic Git Concepts

-
###Repository

The overall package or project.  There is typically a remote or origin repository, and a local copy of it at a certain time on your local machine.
-
###Branch

An extension of a repository.  All repos start with a `master` branch.  From there, however, you can create a new branch.  This is typically done to keep master clean and working, and give you a place to work on new features.
-
###Commit

A commit is a change to the repository.  For every commit made, you have a record of the state of the repository at that given point in time.

-
-

##Git Commands
-
* `git init` -- Creates a new local repository in the current directory.
  * NOTE: This isn't really that common of a command.  So if you find yourself typing it with any regularity, you're probably doing something wrong.

-
* `git clone {project}` -- This is how you will clone a repository from a remote resource (Github, Gitlab, internal Git server, etc).  This will create a directory inside your current directory, and put your project in there.  So if you are in an empty directory called `dev` and you do `git clone https://github.com/zipcoder/git-tester.git`, the `dev` directory should now contain only a directory called `GitTester` and that directory is where the project you cloned lives.

-
Running `git status` from `dev` should return 

> fatal: Not a git repository (or any of the parent directories): .git

But, if you go into the `GitTester` directory, it should return:

> On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean

-
* `git status` -- This is the command you will run the most.  What it does is tell you what files you've added, deleted, and modified.  It will also tell you if your local branch is behind the remote version, or if your version is ahead of the remote repository.  If it's behind, then you need to pull or merge the remote repository into yours, and if it's ahead, then you need to push your changes.

-
* `git branch {branchname}` -- This creates a new branch on your local machine.  It does not check out the branch, nor does it put it on the server.
* `git branch -a` -- This shows all branches, both local and remote, that are available.
* `git checkout {branchname}` -- This sets your current, local branch to that given branch.

-
* `git add {file, directory, ., or other}` -- This stages changes to be committed.  This, however does not commit that change.  This essentially means that you're about to commit some changes.  By specifying a file, it will just add that one file.  By specifying a directory, every change from that directory down will be added.  By specifying `.`, that says "Add all changes from this directory downward."  Meaning that if you're in the root of a git project (like `dev/GitTester` from our example above, all changes in `GitTester` and every directory within it will be added.

-
* `git commit -m "{commit message}"` -- This commits your added changes locally and attaches the commit message to the commit.  Quick note on commit messages, they should be short but descriptive of all the changes you've made.  Be serious about commit messages.  If someone wants to know what you've done, they should be able to understand your changes strictly from the commit message.  Also, note that doing a `commit` does not apply that commit to the remote repository.  It only applies it locally and you must push it to the remote repository.  It also only applies to the branch you are on.

-
* `git push` -- This will take the locally committed changes for a given branch and apply them to the remote repository.  So, if you've committed a change with `git commit`, the only way for everyone else to be able to use your changes is to run `git push`.
* `git push -u origin {branch}` -- This is how you put a new branch on the remote repository.  If you create a branch, the server has no knowledge of it.  It, essentially, tells the remote repository "Hey, I'm gonna be making changes on a branch, but I want you to keep track of those, too.  So here's where to put it and what to call it."

-
* `git pull` -- This takes the changes that have been made to the remote repository and applies them to your local version.  So, if someone else pushed a change, you must run `git pull` to be able to view and use their changes.

-
* `git merge {branch}` -- This command is pretty confusing and kind of dangerous if you don't know what you're doing.  What it does is applies all of the commits from the branch specified and applies them to your current branch.

-
-

## Example Time

-
The commands might not make a bunch of sense without context.  So here's some context of a workflow that uses git and makes use of the commands.

-
Consider that you've started working for a new company that is building a calculator.  They have most of the functionality built out, but they need you to implement the division function of the calculator.  So, you have some directory called `dev` where you do all your development work.  Your first step is:

`git clone https://github.com/Zipcoder/Calcul8r.git`

-
Now, `dev` has a `Calcul8r` directory, and a `git status` in `dev` shows that there is no git project in `dev`.  Now, you go into `Calcul8r` and do a `git status` to prove that you're up to date with the master branch of the company's `Calcul8r` repo.

-
Next, you run `git branch -a` to see exactly what branches exist on this company's repository.

> \* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/add
  remotes/origin/subtract
  remotes/origin/multiply


-
So, from here, you can see that your new collegues have made branches for `add`, `subtract`, and `multiply`.  That's good.  Now you know that since you're implementing divide, you should make a `divide` branch.  To do this, you run:

`git branch divide`

then

`git checkout divide`

-
Now, you have a local branch, but you want to cover your bases and tell Github to start tracking a remote version of it.  So you run:

`git push -u origin divide`

-
Now, a `git branch -a` shows:

> 
  master<br/>
  \* divide
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/add
  remotes/origin/subtract
  remotes/origin/multiply
  remotes/origin/divide
  
  
-
Now you're ready to start developing.  You modify a file called `Operations` and add your divide function.  You also modify `OperationsTest` and make unit tests for your new divide function.  You're ready to get everything up on the remote server, so you run:

`git add .`

`git commit -m "Added divide function and unit tests"`

`git push`

-
A `git status` should tell you that you're up to date.

Now you're ready to merge your stuff into your master branch to make sure that everything's cool.


-
First,
`git pull` to make sure that you're up to date with the remote version.  This way, if someone made a change to master, you won't have issues with your merge.

Next, `git checkout master` and `git merge divide`.

If everything's good, then you will now have all of your commits on top of the master commits.  Otherwise, you'll have to handle a merge conflict.  We'll touch on this later.

-
-

##Git TL;DR
* `git clone {remote project}`
* `git branch {branchname}`
* `git checkout {branchname}`
* `git push -u origin {branchname}`
* Make changes to the project
* `git add .`
* `git commit -m "{commit messages}"`
* `git push`
* When you're ready to merge
* `git pull`
* `git checkout master`
* `git merge {branchname}`

-
-
##Merge Conflicts

-
A merge conflict is what happens when you try to merge a file over another file that have both been changed.  Consider this scenario:

-
That division function that you wrote, well, it turns out Matt Multiplication decided it would be cool to implement it himself.  When you checkout master, the file you changed was in state A.  When Matt decided to make the change, his version was also in state A.  You make your changes on your branch, but before you can merge it into your up-to-date version of master, Matt pushed his change.  This means that the file is now in state B, and you're trying to put it in state B.  You can't.  Plus, considering that keep keeps a running log of the state of a file, you now need to make your changes as if they came after Matt's change (since it keeps the order).  Congrats!  Matt just helped usher you into the world of merge conflicts.

-
Now, I know merge conflicts are scary.  Keep in mind, though, that YOU CANNOT BREAK ANYTHING!  Git exists partially so that when people screw stuff up, you can just go back to a previous, safe version of the software.  So be strong.  We'll get through this.

-
Another thing to notice about merge conflicts is what happens to your conflicted files.  Notice the syntax of:
```
<<<<<<< Branch / HEAD
Some
changes
=======
Some different changes
>>>>>>> Other branch
```

-
There are a bunch of different ways to handle a merge conflict.  The easiest one, however, is to use `git mergetool` and use a tool to handle it.  This is hard to describe doing because each machine might use a different tool.  So, my tips for merge conflicts are these:

-
1. Commit early and commit often.  Your merge conflicts won't be all the large if you're constantly making small changes.
2. Try and not work on the same files as other people.
3. Be calm and Google things.  That's how most of us figured out how to fix merge conflicts.

-
-
## Pull Requests

-
Since a lot of repositories are protected, to get your changes done in master you must submit a PR (pull request).  Think of these are merges that someone has to approve.  To do them from a fork (or your personal copy of a repo that you can do work on), go to your git provider (Github, Gitlab, or a proprietary git server) and submit a PR.  My adivce is to get your master branch to where you want it, merge in the original project's changes, and then submit the pull request.  Each place will be a little different with pull requests, so follow the directions that apply to you.  But, remember, they're basically just protected merges so treat them the way you treat merges.

-
-

##Git Setup

-

We're going to use `git bash` for a few reasons:
* To actually learn the git commands
* To cover multiple systems and environments
* To teach you to be able to teach others

-
#Walkthrough

-
##Set up a Github account
https://www.github.com

-
##Fork this repo
https://github.com/zipcoder/calcul8r

-
##Open the project in Eclipse
* Set your workspace to the git directory
* Open the project from the file system
* Let maven work its magic
* Show the dotfiles

-
##Add your divide function

-
##Commit and push it

-
##Make your PR

-
##We'll cover resolving merge conflicts later.
