
### Readme

Let's start with something small and fix the readme.

#### Branch the code

Branching tells git that we want to start working on some changes and
we're going to give them a name.

    $: git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working directory clean

To create a branch, use the "git branch" command and give it the name
for the new branch.

    $: git branch fix-readme

Without the branch name, `git branch` lists what branches exist.

    $: git branch
    fix-readme
    * master

Notice there's now two branches, `master` and `fix-readme`.  Master has
an asterix by it because we're still on master.  We need to switch to
`fix-readme`.

    $: git checkout fix-readme
    Switched to branch 'fix-readme'


    $: git branch
    * fix-readme
    master


    $: git status
    On branch fix-readme
    nothing to commit, working directory clean

Now we're on the `fix-readme` branch.

It's important to check out the branch.  Git won't do it automatically,
and you can find yourself making commits into the `master` branch.  This
usually isn't disastrous, but it's often very messy to clean up if
things go wrong.

#### Changing README.md

Open the `README.md` file and replace the two FIXMEs.  Save the file.

Now, if you ask git for the status, it will show that `README.md` has
changed.

    $: git status
    On branch fix-readme
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

    no changes added to commit (use "git add" and/or "git commit -a")

We can ask git to show us exactly what changed.


    $: git diff
    diff --git a/README.md b/README.md
    index 9493433..718893f 100644
    --- a/README.md
    +++ b/README.md
    @@ -1,6 +1,6 @@
    # chatter

    -FIXME
    +This is a web app that will display posted messages.

     ## Prerequisites

    @@ -16,4 +16,4 @@ To start a web server for the application, run:

     ## License

    -Copyright © 2014 FIXME
    +Copyright © 2014 clojurebridgemn.org


`git diff` is telling us that the `README.md` file changed.  I removed
the line that said "FIXME" and added a line saying "This is a web app
that will display posted messages."  I also changed the FIXME in the
copyright to my email address.  You should see the changes you made.

Since we changed the description in the `README.md`, we might as well
change the description in `project.clj` file.  Make the change and save
that file too.  Now the git status should be:


    $: git status
    On branch fix-readme
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md
        modified:   project.clj

    no changes added to commit (use "git add" and/or "git commit -a")

#### Adding and Committing the changes

Let's add and commit the changes.

    $: git add README.md project.clj


    $: git status
    On branch fix-readme
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
	modified:   project.clj


    $ git commit README.md project.clj -m "fixing README.md description"
    [fix-readme 57aff88] fixing README.md description
     2 files changed, 3 insertions(+), 3 deletions(-)

Now git status reports no uncommitted changes, but we're still on the
fix-me branch.

    $: git status
    On branch fix-readme
    nothing to commit, working directory clean

Let's the check the log.

    $: git log
    commit 57aff889c81698394faf8568b63f14130d32599a
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 17:33:41 2014 -0600

        fixing README.md description

    commit 44a560f1653770afac01aea2c9279a7af46a46eb
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 16:43:37 2014 -0600

        initial commit

Two commits, the last commit from our originating branch and our
new commit.

#### Merging the changes

Let's merge our changes into the `master` branch.  First, let's checkout
`master` and check its log.

    $: git checkout master
    Switched to branch 'master'
    Your branch is up-to-date with 'origin/master'.

    $: git log
    commit 44a560f1653770afac01aea2c9279a7af46a46eb
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 16:43:37 2014 -0600

        initial commit

Note that `master` is lagging behind.  It doesn't have the commit with
the README changes.  We can fix that by merging our `fix-me` branch into
the `master` branch.

    $: git merge fix-readme
    Updating 44a560f..57aff88
    Fast-forward
     README.md   | 4 ++--
     project.clj | 2 +-
     2 files changed, 3 insertions(+), 3 deletions(-)

The merge brought in the changes.  If we check the log, we'll see two
commits now.

    $: git log
    commit 57aff889c81698394faf8568b63f14130d32599a
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 17:33:41 2014 -0600

        fixing README.md description

    commit 44a560f1653770afac01aea2c9279a7af46a46eb
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 16:43:37 2014 -0600

        initial commit

#### Deleting the fix-readme branch

Now that we've pulled the changes from the fix-readme branch into
master, we no longer need the `fix-readme` branch, so let's delete it.

    $: git branch -d fix-readme

#### Pushing to GitHub

The final step will be to push our changes to GitHub.

    $: git push origin master
    Username for 'https://github.com': crkoehnen
    Password for 'https://crkoehnen@github.com':
    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 572 bytes | 0 bytes/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    To https://github.com/crkoehnen/chatter.git
       44a560f..57aff88  master -> master

Now go back to the repository page in GitHub and refresh the page.
You should see the text and the commit count change.

That's all the git we'll need for this tutorial.  GitHub maintains a list of
resources where you can learn more about git and GitHub.

[Good Resources for Learning Git and GitHub](https://help.github.com/articles/good-resources-for-learning-git-and-github).
