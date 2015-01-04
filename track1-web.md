### Goal

We're going to introduce clojure by creating and deploying a web app
where you and your friends can post messages to each other.

_insert image of app_

### Meta-Goal

* Understanding HTTP and the web.
* Basic clojure syntax.
* REPL for fun and profit.
* Leiningen for project automation.
* Git for version control.
* Heroku deployment.

### Requirements

* java 1.7+
* lein 2.5+
* git 2.1+
* firefox with firebug

Everything should be set up the night before during our install-fest.
Please ensure you have everything working _before_ you show up for
ClojureBridge on Saturday.

You can verify that you have everything working by trying this out in
your terminal:

<div class= "console"><pre>
~/temp/CB $ lein -version
Leiningen 2.5.0 on Java 1.7.0_65 OpenJDK 64-Bit Server VM
~/temp/CB $ git --version
git version 2.1.4
</pre>
</div>

### Clojure, the Big Picture

<img
src="http://griffsgraphs.files.wordpress.com/2012/07/programming-languages_label.png">programming
languages</img>

There are lots of programming languages and clojure is just one of
them.  Clojure is great because:

+ The core language is small and easy to learn.
+ The design makes it easier to write correct programs.
+ Clojure makes it easier to write concurrent programs, programs that
do more than one thing at a time.
+ Clojure programs are fast.
+ Clojure programs can build on Java libraries.

Most programmers have to use multiple languages to get their jobs
done.  Web applications often use html, css, and javascript.  We'll
touch on each of those as we build our web app.

### The Web

The internet is a bunch of computer programs on computers all over the
world sending messages to each other.  Some of those programs are
servers, that listen for requests and respond with data.

Your browser is program that sends requests over "http".  Entering,
[https://github.com](https://github.com) into your browser's address bar
tells your browser that you want to see that page.

[https://github.com](https://github.com) is actually a human-readable
alias for the numerical address of github's servers.  Since your
computer isn't directly connected to github, it asks the computers it
is connected to to forward the request.

On linux or the mac, traceroute will show you the number of hops it
took to get to github.  (On windows, the command is called tracert.)
On my machine, working from home:


<div class= "console"><pre>

~/temp/CB $ traceroute github.com
traceroute to github.com (192.30.252.128), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  17.063 ms  17.049 ms  17.031 ms
 2  96.120.49.33 (96.120.49.33)  24.241 ms  25.139 ms  28.541 ms
 3  te-0-2-0-6-sur01.webster.mn.minn.comcast.net (68.85.167.145)  29.825 ms  29.820 ms  29.803 ms
 4  te-0-7-0-12-ar01.crosstown.mn.minn.comcast.net (69.139.219.129)  34.388 ms  34.371 ms  34.353 ms
 5  pos-0-0-0-0-ar01.roseville.mn.minn.comcast.net (68.87.174.194)  32.900 ms  34.284 ms *
 6  he-1-12-0-0-cr01.350ecermak.il.ibone.comcast.net (68.86.94.77)  40.186 ms  26.773 ms  27.484 ms
 7  be-10206-cr01.newyork.ny.ibone.comcast.net (68.86.86.225)  46.717 ms  60.557 ms  62.632 ms
 8  be-10002-cr01.ashburn.va.ibone.comcast.net (68.86.86.233)  68.517 ms  66.340 ms  68.452 ms
 9  he-0-11-0-0-pe07.ashburn.va.ibone.comcast.net (68.86.83.74)  71.955 ms  71.912 ms  75.097 ms
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *
~/temp/CB $

</pre>
</div>


+ 1) is my computer
+ 2) is my router
+ 3-30) the requests are bouncing around comcast.

When you enter [https://github.com](httpss://github.com) in the
address bar, your browser makes GET request to the github server.  The
server sends back an html page.  What you see is how the browser is
programmed to display the html but if you want to see the html itself,
right click on the page and select "view page source."

### HTML Proper


HTML stands for "HyperText Markup Language" Hypertext meant that
documents could have embedded within them links to other pages or
images.  The structure of the document was encoded using a markup
language mainly consisting of opening and closing "tags" or elements.

Save the following as sample.html:

```HTML
<!DOCTYPE html>
<html>
  <head>
    <title>sample html page</title>
  </head>
  <body>
    This is a sample html page.  This is more text.
  </body>
</html>
```

Open the file in the browser.  On windows or the mac, double clicking
the file should open it in the default browser and you should see
something like, _insert screenshot_

Notice the address bar.  Instead of making an http request to a server
over the internet, the browser just opened a local file and displayed
it as html.  If you right click and "view page source", you should see
something very similiar to the file you saved.

The first line of the source, the DOCTYPE, simply announced that the
document was html.  The document proper is enclosed between the
opening and closing ```html``` tags.

Inside the html document, we have a ```head``` and ```body```.

The ```head``` contains the ```title```, "sample html page".  Notice
the title doesn't actually show up when you open the page in the
browser.  The title is used when you bookmark the page though.  Try
bookmarking the page and confirm that the title is correct.


If we wanted to add a title on the page that's displayed, we need to
add it to the body.  Let's use "Our html".  Html provides a number of
heading tags to indicate titles; ranging from the smallest ```<h6>```
to the largest ```<h1>```.  Let's start with h2.  Change the file so
the body looks like:

```HTML
  <body>
    <h2>Our html</h2>
    This is a sample html page. This is more text.
  </body>
````

Save the file and refresh the browser.  Experiment with different size
heading tags.

Html supports tables. _explain why we're talking about tables_
Try adding this table to our sample html within the body:

```HTML
<table>
  <tr>
    <td>Hydrogen</td>
    <td>1</td>
    <td>H</td>
  </tr>
  <tr>
    <td>Helium</td>
    <td>2</td>
    <td>He</td>
  </tr>
  <tr>
    <td>Neon</td>
    <td>10</td>
    <td>Ne</td>
  </tr>
</table>
```

_insert screenshot_

When you save a refresh the page, you should see the list in the first
paragraph.  You should also see that it looks pretty bad.  The html
we've been using describes the structure of the document and leaves
the display entirely up to the browser.  Another language, called
"CSS", is used to provide hints to the browser so it can display the
page in a more pleasing way.  We'll touch on CSS later.

The html file we've got is called "static html".  The html is always
just what's in the file.  Static html works great for some kinds of
pages, but our page will change depending on the messages people post
to it.  For that, instead of having a file with html, we'll have a
program listening for requests that generate the html.  As people make
requests and post messages, it will generate html that reflects the
posts.

### Creating a clojure project

We're going to start by asking lein to stub out a web application for
us.

<div class= "console"><pre>
  ~/temp/CB: lein new compojure chatter
</pre>
</div>

Lein is a tool for managing clojure projects.  We've asked to create a
new compojure project called chatter and there should now be a
directory also called chatter.

Cd into the directory and start the server with the command:

<div class= "console"><pre>
  ~/temp/CB: lein ring server
</pre>
</div>


Lein will download a bunch of stuff off the internet the first time,
but then it should cause your default browser to open to a page
saying, "Hello World".

Notice the address bar; "localhost" is an alias for your computer.
3000 is the port number where your server is listening for requests.
If you take another look at the terminal, you'll see the message,
"Started server on port 3000"

Right click on the page and view the page source.  You'll see that's
not even an html document, it's just the string "Hello World".

Let's stop the server by going to the terminal and typing "control-c"
twice.

Now let's take a closer look at the chatter directory.  The whole
project tree looks like:


<div class= "console"><pre>

chatter
├── project.clj
├── README.md
├── resources
│   └── public
├── src
│   └── chatter
│       └── core
│           └── handler.clj
└── test
    └── chatter
        └── core
            └── handler_test.clj

8 directories, 4 files

</pre>
</div>


project.clj is a clojure file describing the what our project does and
how other programs does it build on.

README.md is a markdown file documenting how our project works.

resources is a folder where we would store static html, CSS, and
images.

src is where our source code will live.

test is where our tests will live.  We're going to set an awful
example and ignore testing for this tutorial.

### A closer look at the src directory


Let's open the files "src/chatter/core/handler.clj"

The "clj" extension tells us that this is a clojure file.  Clojure
programs are made up of expressions.  Expressions are either a single
name, number, or a list of expressions beginning with a paren.

The first expression, "(ns ..." is the namespace declaration.  It's a
way of importing libraries and other programs in a controlled way.  We
won't be covering it in much detail.  The first subexpression,
"chatter.core.handler" tells clojure what we want to call the name
space being defined in this file.  The next expression, beginning with
"(:require..." is importing ring and compojure libraries.  Those are
the low-level libraries for building web apps.

"(defroutes app-routes..." is next.  defroutes is specific to clojure
web apps.  It's creating a set of routes and giving them the name
"app-routes".  The expressions after symbol "app-routes" are the route
definitions.  There are two.

The first is
```clojure
(GET "/" [] "Hello World")
```

There are four parts to the expression:

1) "GET"; this is which type of http request we want to handle.  GET's
   are requests for information or data.
2) "/"; this is the name of the web page.  "/" means the top
   level.
3) "[]"; this is the parameters list.  When you do a search or fill
   out a form, the parameters narrow the result.
4) "Hello World"; the result that gets sent back to the requesting
   browser.

After the GET is

```clojure
(route/not-found "Not Found")
```

This means that, when the server gets any other request, it should
return "Not Found."

Right click on the page in the browser and select, "Inspect Element
With Firebug."  _insert screenshot_The "html" tab shows what the html
document looks like. The default is an empty head and a body with the
string "Hello World".  This is actually different from what we see
when we view the source.  The browser requested html but got only got
a string back and it fleshes out a legal page.

Click on the "Net" tab and refresh the page.  You'll see the request
was actually a GET request and returned a status code of 200.  That
indicates the request was successful.

Try changing the address to
"http://localhost:3000/non-existant-page".  Now you should see the GET
request is in red and has a status of 404, which indicates that the
server couldn't find the page.

Back in the handler.clj file, the third and final expression begins
"(def app ..."  "def" is how you declare a variable in clojure.  The
name of the variable is "app" and it's being assigned the result of

```clojure
(wrap-defaults app-routes site-defaults)
```

"app-routes" is what we're calling our set of routes, "site-defaults"
are the default routes imported into the namespace from ring library.
"wrap-defaults" is also imported from ring and it combines our routes
and the defaults.  When we start the server, it's going to look for
the routes associated with the "app" variable and use those to decide
how to handle http requests.

Let's stop the server by going back to the terminal and typing
"control-C" twice.

### git


We haven't made any changes yet, so this is a good time to put the
code under version control.  Version control allows developers to keep
track of their changes over time.  It makes it easier to experiment
and coordinate work with others.

Inside the chatter directory, enter the command, "git init"

<div class= "console"><pre>
~/temp/CB/chatter $ git init
Initialized empty Git repository in /home/crk/temp/CB/chatter/.git/
</pre>
</div>


Now git is keeping an eye our directory.  Try asking git the status of
our directory.


<div class= "console"><pre>
~/temp/CB/chatter $ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	README.md
	project.clj
	src/
	test/

nothing added to commit but untracked files present (use "git add" to track)
</pre>
</div>


We're on the master branch.  The master branch is the main place where
our code will be.  It says "Initial commit" because we're just
initializing git.  Git doesn't know anything about the files in our
project but it's spotted the README.md, project.clj files and the src
and test directories.  It also spotted a file called ".gitignore".
Files beginning with a dot are normally hidden unless you specifically
ask to see them.  .gitignore is a special file, it tells git what kind
of files we what it to not track.  These will mostly be compiled code,
test reports, and log files.

Git follows a two-step process.  First you add the changes; they
become staged.  Then you commit all of the staged changes.  Let's add
everything.

"git add ."  The "." tells git the current directory and below.

Now when we ask git for the status:


<div class= "console"><pre>
~/temp/CB/chatter $ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
	new file:   README.md
	new file:   project.clj
	new file:   src/chatter/core/handler.clj
	new file:   test/chatter/core/handler_test.clj
</pre>
</div>

All of our stuff is ready to be commited.  That requires a commit
message.


<div class= "console"><pre>
~/temp/CB/chatter $ git commit . -m "initial commit"
[master (root-commit) 44a560f] initial commit
 5 files changed, 66 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 README.md
 create mode 100644 project.clj
 create mode 100644 src/chatter/core/handler.clj
 create mode 100644 test/chatter/core/handler_test.clj

</pre>
</div>

Now when we ask git for the status,


<div class= "console"><pre>
~/temp/CB/chatter $ git status
On branch master
nothing to commit, working directory clean
</pre>
</div>

Ok, there haven't been any changes since our last commit, so there's
nothing to see.  Let's check the log.

<div class= "console"><pre>
~/temp/CB/chatter $ git log
commit 44a560f1653770afac01aea2c9279a7af46a46eb
Author: crkoehnen <crkoehnen@gmail.com>
Date:   Sun Dec 28 16:43:37 2014 -0600

    initial commit
</pre>
</div>

There's been one commit, it was by me, and the comment was "initial
commit."

By keeping track of changes, git makes it easy to go back to an
earlier save point.  It's a safety net.  By itself, it won't do much
if our hard drive suddenly dies.  But git allows you to have
repositories on other computers.  If your computer dies, your code
lives on.  Github is a company hosting source code, free if you don't
mind that other people can see your code.  As a safety measure, we're
going to put our code on Github.

Log into [https://github.com](https://github.com) and click, "create
repository".  Name it "chatter".  That will open a page for your new
repository.  We want to push an existing repository, so (be sure to
use the url that Github gives you instead of mine!) :

<div class= "console"><pre>
~/temp/CB/chatter $ git remote add origin https://github.com/crkoehnen/chatter.git
~/temp/CB/chatter $ git push -u origin master
Username for 'https://github.com': crkoehnen
Password for 'https://crkoehnen@github.com':
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (13/13), 1.54 KiB | 0 bytes/s, done.
Total 13 (delta 0), reused 0 (delta 0)
To https://github.com/crkoehnen/chatter.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.

</pre>
</div>

Click on the "chatter" link and you'll go to the main page for the
repository.  Note there is a single commit and the text is identical
to what's in our "README.md" file.

### Workflow

Now we're going to start changing the templated code to make it our
web app.  We're going to follow a certain workflow:

1. branch the code
2. write some code
3. try the code
4. repeat 2-3 until we're happy with the changes
5. merge the branch into master
6. push the changes to github

This methodology allows us to isolate changes in their own branch.  If
we change our minds or discover we've made a mistake, it's easy to
back up to an earlier version using git.

### Readme

Let's start with something small and fix the readme.

#### Branch the code

Branching tells git that we want to start working on some changes and
we're going to give the whole set a name.

<div class= "console"><pre>
~/temp/CB/chatter $ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
~/temp/CB/chatter $ git branch fix-readme
~/temp/CB/chatter $ git checkout fix-readme
Switched to branch 'fix-readme'
~/temp/CB/chatter $ git status
On branch fix-readme
nothing to commit, working directory clean
</pre>
</div>

I was on master, I created a branch named "fix-readme" with the
command, "git branch fix-readme".  Then I checked out the branch.

It's important to check out the branch.  Git won't do it automatically
and you can find yourself making commits into the root branch.  This
usually isn't fatal but it's often very messy to clean up if things go
wrong.

To checkout, "git checkout fix-readme".  Now git status reports that
I'm on the proper branch.

#### Changing README.md

Open the README.md file and replace the two FIXME's with reasonable
text.  Save the file.

Now, if you ask git for the status, it will show that README.md has
changed.


<div class= "console"><pre>
~/temp/CB/chatter $ git status
On branch fix-readme
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
</div>

We can ask git to show us exacty what changed.

<div class= "console"><pre>
~/temp/CB/chatter $ git diff
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
+Copyright © 2014 crkoehnen@gmail.com
</pre>
</div>

"git diff" is telling us that the README.md file change.  I remove the
line that said "FIXME" and added a line saying, "This is a web app
that will display posted messages."  I also changed the FIXME in the
copyright to my email address.

Since we changed the description in the README.md, we might as well
change the description in project.clj.  Save that file too.  Now the
git status should be:

<div class= "console"><pre>
~/temp/CB/chatter $ git status
On branch fix-readme
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md
	modified:   project.clj

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
</div>

#### Adding and Commmting the changes

Let's add and commit the changes.

<div class= "console"><pre>
~/temp/CB/chatter $ git add README.md project.clj
~/temp/CB/chatter $ git commit README.md project.clj -m "fixing README.md description"
[fix-readme 57aff88] fixing README.md description
 2 files changed, 3 insertions(+), 3 deletions(-)
</pre>
</div>

Now git status reports no uncommitted changes but we're still on the
fix-me branch.

<div class= "console"><pre>
~/temp/CB/chatter $ git status
On branch fix-readme
nothing to commit, working directory clean
</pre>
</div>

Let's the check the log.

<div class= "console"><pre>
~/temp/CB/chatter $ git log
commit 57aff889c81698394faf8568b63f14130d32599a
Author: crkoehnen <crkoehnen@gmail.com>
Date:   Sun Dec 28 17:33:41 2014 -0600

    fixing README.md description

commit 44a560f1653770afac01aea2c9279a7af46a46eb
Author: crkoehnen <crkoehnen@gmail.com>
Date:   Sun Dec 28 16:43:37 2014 -0600

    initial commit
~/temp/CB/chatter $
</pre>
</div>

Two commits, just what we expect.

#### Merging the changes

Let's merge our changes into the master branch.  First let's checkout
master and check its log.

<div class= "console"><pre>
~/temp/CB/chatter $ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
~/temp/CB/chatter $ git log
commit 44a560f1653770afac01aea2c9279a7af46a46eb
Author: crkoehnen <crkoehnen@gmail.com>
Date:   Sun Dec 28 16:43:37 2014 -0600

    initial commit
</pre>
</div>

Note that master is lagging behind.  It doesn't have the commit with
the README changes.  We can fix that by merging.

<div class= "console"><pre>
~/temp/CB/chatter $ git merge fix-readme
Updating 44a560f..57aff88
Fast-forward
 README.md   | 4 ++--
 project.clj | 2 +-
 2 files changed, 3 insertions(+), 3 deletions(-)
</pre>
</div>

The merge brought in the changes.  If we check the log, we'll see two
commits now.

<div class= "console"><pre>
~/temp/CB/chatter $ git log
commit 57aff889c81698394faf8568b63f14130d32599a
Author: crkoehnen <crkoehnen@gmail.com>
Date:   Sun Dec 28 17:33:41 2014 -0600

    fixing README.md description

commit 44a560f1653770afac01aea2c9279a7af46a46eb
Author: crkoehnen <crkoehnen@gmail.com>
Date:   Sun Dec 28 16:43:37 2014 -0600

    initial commit
</pre>
</div>

#### Deleting the fix-readme branch

Now that we've pulled the changes from the fix-readme branch into
master, we no longer need fix-readme.

"git branch -d fix-readme" will delete it.

#### Pushing to github

The final step will be to push our changes to github.


<div class= "console"><pre>
~/temp/CB/chatter $ git push origin master
Username for 'https://github.com': crkoehnen
Password for 'https://crkoehnen@github.com':
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 572 bytes | 0 bytes/s, done.
Total 4 (delta 2), reused 0 (delta 0)
To https://github.com/crkoehnen/chatter.git
   44a560f..57aff88  master -> master
</pre>
</div>

Now go back to the repository page in github and refresh the page.
You should see the text and the commit count change.

### hiccup

Now let's change the app's main page from "Hello, World" to something
a little more chatty.

First, let's create and checkout a new branch, "view-messages."

We need to write code that will generate html so we're going to use a
library called "hiccup".  Adding a new library requires two steps:

1) Add the library to the dependency section of the project.clj file.
   This tell lein that your program needs another program.

   Adding hiccup makes ours look like:

<div class= "console"><pre>
  :dependencies [[org.clojure/clojure "1.6.0"]
                 [compojure "1.3.1"]
                 [ring/ring-defaults "0.1.2"]
                 [hiccup "1.0.5"]]
</pre>
</div>

2) Import the library into the namespace you need by adding the import
   to the "ns" declaration.  We're going to be using it in the
   handler.clj.  Our ns declaration will look like:

<div class= "console"><pre>
(ns chatter.core.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [hiccup.page :as page]))
</pre>
</div>

Let's use hiccup to generate the html by changing our defroutes:


<div class= "console"><pre>
(defroutes app-routes
  (GET "/" []
       (page/html5
        [:head
         [:title "chatter"]]
        [:body
         [:h1 "Our Chat App"]]))
  (route/not-found "Not Found"))
</pre>
</div>

If you start the server using "lein ring server", you'll see that
"http://localhost:3000" now proudly displays "Our Chat App".  And if
you'll view source, you'll see that now it's proper html complete with
head, title, and body.

Hiccup function, "page/html5" generates an html page.  It expects
clojure vectors with symbols representing corresponding html tags.
Hiccup will automatically add the closing tag when it reaches the end
of the vector.  If the vector contains other vectors, hiccup digs into
those too.

Compare the hiccup to html in view-source to the html we wrote by hand
earlier.

_introduce clojure vectors and keywords_

A problem with our new app-routes is that it's doing two different
things.  It's main role is to take the incomming request and decide
what to do.  Now it's doing that but it's also generating a full html
page.  As we add more and more pages, this will become too complicated
to manage.  We'll get ahead of the game by splitting out the task of
generating the html into a helper function.

_introduce defn_

Our new code should look like:

<div class= "console"><pre>
(defn generate-message-view
  "this generates the html for displaying messags"
  []
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]]))

(defroutes app-routes
  (GET "/" [] (generate-message-view))
  (route/not-found "Not Found"))
</pre>
</div>

Let's fire up the server and make sure it still works.  From the
outside, we shouldn't see a change.  The page should still display
"Our Chat App" and the html should be identical.
Now let's double check our git status:

<div class= "console"><pre>

~/temp/CB/chatter $ git status
On branch view-messages
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   project.clj
	modified:   src/chatter/core/handler.clj

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
</div>

That looks right so let's add, commit, merge the changes back to
master, then push to github.  Then delete the view-messages branch.
You should see the commit numbers go up on github.

### Adding messages

We still aren't displaying messages, neither do we have a way of
adding them.  Now we'll make that happen.

Create and check out a branch to work in.

Let's fix the display first.  We'll represent the messages as a vector
of maps.  Each map will have a :name and :message key.  Let's call the
vector simply messages and hard code some samples to get started.

_explain clojure maps_

```clojure
(def messages [{:name "blue" :message "hello, world"}
               {:name "red" :message "read my lips"}
               {:name "green" :message "time makes more converts than reason"}])
```

Next we'll modify the html to display the messages.

```clojure
(defn generate-message-view
  "this generates the html for displaying messags"
  []
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p messages]]))
```

This blows up spectacularly.  The message is
"java.lang.illegalArgumentException"
"{:name "chris", :message "hello, world"} is not a valid element
name."

Then there's a stack trace.  That gives an idea what the program was
doing when it hit the problem.  Ignore all the files that aren't ones
you wrote for the project.  In my case, the first file of interest is
handler.clj, line 14, the generate-message-view function.  A couple of
helpful clues; elements are what fragments of html are called, and
hiccup is responsible for generating html from clojure symbols.  The
problem is that we've got a map with symbols in it and hiccup thinks
they're html.  They're not so it blows up.

We can finesse the issue by converting our maps to strings.

```clojure
(defn generate-message-view
  "this generates the html for displaying messags"
  []
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p (str messages)]]))
```

That works but is ugly.  Let's take the messages and put them in a
table using html's table, tr, and td elements.

Now our generate-message-view looks likes:

_explain clojure map and fn_

```clojure
(defn generate-message-view
  "this generates the html for displaying messags"
  []
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p
     [:table
      (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]]]))
```

We still don't have a way of adding messages.  This requires html
forms and adding importing the form functions from hiccup.

_explain html forms_

Now our code looks like:

```clojure
(ns chatter.core.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [hiccup.page :as page]
            [hiccup.form :as form]
            [ring.util.anti-forgery :as anti-forgery]))

(def messages [{:name "chris" :message "hello, world"}
               {:name "george" :message "read my lips"}
               {:name "tom" :message "time makes more converts than reason"}])

(defn generate-message-view
  "this generates the html for displaying messags"
  []
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p
     (form/form-to
      [:post "/"]
      (anti-forgery/anti-forgery-field )
      "Name: " (form/text-field "name")
      "Message: " (form/text-field "msg")
      (form/submit-button "Submit"))]
    [:p
     [:table
      (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]]]))

(defroutes app-routes
  (GET "/" [] (generate-message-view))
  (POST "/" {params :params} (generate-message-view))
  (route/not-found "Not Found"))

(def app app-routes)
```

But when we run it, we don't actually see our posts populating
anything.  The problem now is that we're extracting the params during
the post but aren't actually doing anything with them.

_explain atoms, swap!, and do_

_<note; much borkage.  the anti-forgery middleware is now default.  I'm
missing something and it's not, maybe the token needs adding to the
session or something.  anyway, to get that working, cut the
wrap-defaults stuff.  That broke params.  Adding wrap-params works but
now my params are strings instead of keywords./>_

Now the app is taking our new messages but it's adding new messages to
the end.  That's going to be hard to read.  We can fix that by
changing from a vector to a list.  We can also remove our stubbed out
data.

_explain lists_

And just to clean things up, let's put updating the message list into
a helper function to get the logic out of the routes.

Our app now looks like:


```clojure
(ns chatter.core.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [ring.middleware.params :refer [wrap-params]]
            [hiccup.page :as page]
            [hiccup.form :as form]
            [ring.util.anti-forgery :as anti-forgery]))

(def messages (atom '()))

(defn generate-message-view
  "this generates the html for displaying messags"
  []
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p
     (form/form-to
      [:post "/"]
      (anti-forgery/anti-forgery-field )
      "Name: " (form/text-field "name")
      "Message: " (form/text-field "message")
      (form/submit-button "Submit"))]
    [:p
     [:table
      (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) @messages)]]]))

(defn update-messages!
  "this will update the message list"
  [name message]
  (swap! messages conj  {:name name :message message}))

(defroutes app-routes
  (GET "/" [] (generate-message-view))
  (POST "/" {params :params} (do
                               (update-messages! (get params "name") (get params "message"))
                               (generate-message-view)))
  (route/not-found "Not Found"))

(def app (wrap-params app-routes))
```

Add, commit, merge the changes to master, push master to githup, and delete the branch.

### Bootstrap

The app is working but is ugly.  We can improve it by using CSS and a
package of software called Twitter Bootstrap.

Create and checkout a new branch.

In the head section of our html, we're going to invoke bootstrap:

```clojure
   [:head
    [:title "chatter"]
    (page/include-css "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css")
    (page/include-js  "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js")]
```

Start the server again and you should see the fonts change.  You'll also see the table get smashed together.

Now let's change the table element to:

":table#messages.table"

This tells hiccup that we want the table to have an id of "messages" and a class of "table."
CSS works by looking for combinations of classes and structure and change the appearance
when an element matches a pattern.  Bootstrap uses a set of predefined CSS to look for a common
set of classes.  On of them is table.  Save, then refresh the browser.  It should look better.
Examine the html that's now generated.  You should see and id and class field inside the table element.

Let's make the table entries stripped by adding an additional class.  Change the table
element to, ":table#messages.table.table-striped

What do you think would happen if you changed table-striped to table-bordered?

Html elements can have multiple classes and CSS uses this to create more complex
effects.  Try adding table-hover to the table element.
":table#messages.table.table-bordered.table-hover"

Now when you move the mouse of a row, the entire row becomes highlighted.  Dynamic effects in the browser are implemented
using a language called Javascript.  We won't talk about Javascript except to say that it exists and the javascript part of
Bootstrap was what we imported into the page with the include-js call.

Let's create our own css file to center the heading.  Create a file "chatter.css" in resources/public.  Inside,


```css
h1 {
    text-align: center;
}
```

Css works using pattern matching.  In this case, we're saying that if the element is an "h1" element, center the text.  Save
the file and import the file after importing bootstrap.

```clojure
   [:head
    [:title "chatter"]
    (page/include-css "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css")
    (page/include-js  "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js")
    (page/include-css "/chatter.css")]
```

Refresh the page.  We want to see the h1 tag centered.  It won't though.  Open firebug and watch the traffic
as you refresh the page.  We're getting a 404 when it's trying to download the css.

The problem is in our defroutes.  We tell what to do when a browser requests a GET or a POST but anything else is falling through
to our route/not-found call.  We need to tell defroutes where to find our resources.  Change the defroutes to:

```clojure
(defroutes app-routes
  (GET "/" [] (generate-message-view))
  (POST "/" {params :params} (do
                               (update-messages! (get params "name") (get params "message"))
                               (generate-message-view)))
  (route/resources "/")
  (route/not-found "Not Found"))
```
