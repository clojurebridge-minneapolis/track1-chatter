
# chatter

This is a web app that will display posted messages.

## Prerequisites

You will need [Leiningen][] 2.0.0 or above installed.

[leiningen]: https://github.com/technomancy/leiningen

## Running

To start a web server for the application, run:

    lein ring server

## Copyright and License

Copyright © 2015 Chris Koehnen

Licensed under the [MIT](http://opensource.org/licenses/MIT) [LICENSE](LICENSE)

## The Chatter App Tutorial

### Goal

We're going to introduce Clojure by creating and deploying a web app
where you and your friends can post messages to each other.

_insert image of app_

### Meta-Goal

* Understanding HTTP and the web.
* Basic Clojure syntax.
* REPL for fun and profit.
* Leiningen for project automation.
* Git for version control.
* Heroku deployment.

### Requirements

* Java 1.7+
* Leiningen 2.5+
* Git 2.1+
* Firefox with Firebug
* The Heroku Toolbelt

Everything should be set up the night before during our install-fest.
Please ensure you have everything working _before_ you show up for
ClojureBridge on Saturday.

First, we're going to verify that everything is set up correctly.
We'll do this by entering some commands in the terminal.

When we demonstrate terminal commands, we'll show a snippet from our
terminal.  Our prompt is the dollar sign; everything after the dollar
sign is the command we typed.  The result of the command is printed on
the following lines.  Sometimes we'll have a sequence of commands in
the same snippet — look for the dollar signs!  The terminal you're
using will probably have different prompts but the commands should
work the same.


    $: lein -version
    Leiningen 2.5.1 on Java 1.8.0_40-internal OpenJDK 64-Bit Server VM

    $: git --version
    git version 2.1.4

    $: heroku --version
    heroku-toolbelt/3.25.0 (x86_64-linux-gnu) ruby/2.1.5
    You have no installed plugins.

With the tools installed, we can begin working on our web app.

### Clojure, the Big Picture

Clojure is a modern Lisp with a focus on functional programming.

<img src="http://griffsgraphs.files.wordpress.com/2012/07/programming-languages_label.png"/>

There are lots of programming languages, and Clojure is just one of
them.  Clojure is great because:

* The core language is small and easy to learn.
* The design makes it easier to write correct programs.
* Clojure makes it easier to write concurrent programs, programs that
do more than one thing at a time.
* Clojure programs are fast.
* Clojure programs can build on Java libraries.

Most programmers have to use multiple languages to get their jobs
done.  Web applications often use HTML, CSS, and JavaScript.  We'll
touch on each of those as we build our web app.

### The Web

The Internet is a bunch computers all over the world communicating with
each other using a variety of computer programs.  Some of those programs
are servers that listen for requests and respond with data.

Your browser is program that sends requests over HTTP (Hypertext
Transfer Protocol).  Entering, [https://github.com](https://github.com)
into your browser's address bar tells your browser that you want to see
that page.

[https://github.com](https://github.com) is actually a human-readable
alias for the numerical address of GitHub's servers.  Since your
computer isn't directly connected to GitHub, it asks the computers it
is connected to to forward the request.

On Linux or a Mac, `traceroute` will show you the number of hops a
request takes to get to GitHub.  (On Windows, the command is called
`tracert`.)

On my machine, working from home:

        $: traceroute github.com
        traceroute to github.com (192.30.252.130), 30 hops max, 60 byte packets
        1  192.168.1.1 (192.168.1.1)  5.913 ms  5.908 ms  6.000 ms
        2  * 96.120.49.33 (96.120.49.33)  30.033 ms  30.066 ms
        3  te-0-3-0-1-sur02.webster.mn.minn.comcast.net (68.85.167.149)  30.553 ms  32.776 ms  32.785 ms
        4  te-0-7-0-12-ar01.roseville.mn.minn.comcast.net (69.139.219.134)  32.778 ms  36.059 ms te-0-7-0-13-ar01.roseville.mn.minn.comcast.net (68.87.174.185)  35.686 ms
        5  he-1-13-0-0-cr01.350ecermak.il.ibone.comcast.net (68.86.94.81)  42.354 ms *  43.213 ms
        6  be-10206-cr01.newyork.ny.ibone.comcast.net (68.86.86.225)  63.574 ms  59.894 ms  62.441 ms
        7  pos-2-5-0-0-cr01.dallas.tx.ibone.comcast.net (68.86.85.25)  64.643 ms  64.659 ms  64.653 ms
        8  he-3-14-0-0-cr01.dallas.tx.ibone.comcast.net (68.86.85.1)  67.039 ms  67.079 ms  67.056 ms
        9  he-0-10-0-0-pe07.ashburn.va.ibone.comcast.net (68.86.83.66)  63.013 ms  67.045 ms  88.273 ms
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

+ Line 1 is my computer
+ Line 2 is my router
+ On lines 3-30, the request is moving through Comcast's network.

When you enter [https://github.com](httpss://github.com) in the address
bar, your browser makes a GET request to the GitHub server.  There are
several types of HTTP requests, but GET is the one for simply asking the
server to send back some information.  The server sends back an HTML
page.  In a process called rendering, the Web browser turns this HTML
into the page you see, but if you want to see the HTML itself, right
click on the page and select `View Page Source`.

### HTML Proper


> HTML stands for "HyperText Markup Language"

Hypertext means documents can contain links to other pages or images.
The structure of the document was encoded using a markup language mainly
consisting of opening and closing elements (alternatively, tags).

Save the following as sample.html:

```HTML
<!DOCTYPE html>
<html>
  <head>
    <title>Sample HTML Page</title>
  </head>
  <body>
    This is a sample html page.  This is more text.
  </body>
</html>
```

Open the file in the browser.  On Windows or a Mac, double-clicking
the file should open it in the default browser, and you should see
something like, _insert screenshot_

Notice the address bar.  Instead of making an HTTP request to a server
over the Internet, the browser just opened a local file and displayed it
as HTML.  If you right-click and `View Page Source`, you should see
something very similiar to the file you saved.

The first line of the source, the DOCTYPE, announced that the
document was html.  The document proper is enclosed between the
opening and closing `html` tags.

Inside the HTML document, we have a `head` and `body`.

The `head` contains the `title`, "Sample HTML Page".  Notice that the
title doesn't actually show up when you open the page in the browser.
The title is used in the tab name and when you bookmark the page.

If we wanted to add a title on the page that's displayed, we need to
add it to the body.  Let's use "Our HTML".  HTML provides a number of
heading tags to indicate titles, ranging from the smallest `<h6>`
to the largest `<h1>`.  Let's start with `h2`.  Change the file so
the body looks like:

```HTML
  <body>
    <h2>Our HTML</h2>
    This is a sample HTML page. This is more text.
  </body>
````

Save the file and refresh the browser.  Experiment with different size
heading tags.

HTML supports tables which we're going to use later to display
structured data.  Try adding this table to our sample HTML within the
body:

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

`table` encloses the entire table.

`tr` wraps a table row.

`td` wraps table data, a cell within a row.

When you save and refresh the page, you should see the table in the
first paragraph.  You should also see that it looks pretty bad.  The
HTML we've been using describes the structure of the document and leaves
the display entirely up to the browser.  Another language, called CSS,
is used to provide hints to the browser, so it can display the page in a
more pleasing way.  We'll touch on CSS later.

The HTML file we've got is called static HTML.  The HTML is always just
what's in the file.  Static HTML works great for some kinds of pages,
but our page will change depending on the messages people post to it.
For that, instead of having a file with HTML, we'll have a program
listening for requests that generates the HTML.  As people make requests
and post messages, it will generate HTML that reflects the posts.

There's a lot more to HTML, but this is (almost) just enough for our
chat app.

[w3schools.com](http://www.w3schools.com) is a great source for
learning more about html.  I suggest starting with their
[HTML Introduction](http://www.w3schools.com/html/html_intro.asp).

Another useful resource is [Mozilla Developer Network](http://developer.mozilla.org/).

### Creating a Clojure project

We're going to start by asking Leiningen to stub out a web application for
us.

```
  $ : lein new compojure chatter
```


> Lein is a tool for managing Clojure projects.

We've asked to create a new compojure project called chatter.  This
should result in the creation of a directory `chatter`.

Move into the project directory:

```
  $: cd chatter
```

Start the server with the command:

```
  $: lein ring server
```

Lein will download a bunch of stuff off the Internet the first time it
is run.  After this finishes, it should cause your default browser to
open to a page saying, "Hello World".

Notice the address bar.

> "localhost" is an alias for your computer.

3000 is the port number where your server is listening for requests.

If you take another look at the terminal, you'll see the message,
"Started server on port 3000".

Right-click on the page and ```View Page Source```.  You'll see that's
not even an HTML document, it's just the String(*) "Hello World".

Let's stop the server by going to the terminal and typing the key
combination "Control-c".

In Light Table, let's take a closer look at what's in the chatter directory.  It
looks like:

```
.
├── project.clj
├── README.md
├── resources
│   └── public
├── src
│   └── chatter
│       └── handler.clj
└── test
    └── chatter
        └── handler_test.clj

6 directories, 4 files
```

```project.clj``` is a Clojure file describing what our project does and
what other programs it needs to run.

```README.md``` is a Markdown file describing the program's
prerequisites, how to run the program, and the license.

```resources``` is a folder where we would store static HTML, CSS, and
images.

```src``` is where our source code will live.

```test``` is where our tests will live.  We're going to set an awful
example and ignore testing for this tutorial.

### A closer look at the src directory

Let's open the file `src/chatter/handler.clj`.

Ending the file's name with ".clj" tells us (and your editor) that
this is a Clojure file.  Clojure programs are made up of expressions.

> Expressions are either a single name, number, string, or a list of expressions beginning with a paren (or parenthesis).

The first expression,

```clojure
(ns "chatter.handler"...)
```

tells Clojure what we want to call the namespace being defined in this
file.  The subexpression beginning with

```clojure
(:require...
```

is importing the ring and compojure libraries.  Those are low-level
Clojure libraries for building web apps.

The second expression is

```clojure
(defroutes app-routes...
```

defroutes is specific to Clojure web apps.  It's creating a set of
routes and giving them the name `app-routes`.  The expressions after
symbol `app-routes` are the route definitions.  There are two.

The first is

```clojure
(GET "/" [] "Hello World")
```

There are four parts to the expression:

1. `GET`- this is which type of HTTP request we want to handle.  GETs
   are requests for information or data.
2. `"/"`- this is the name of the web page.  `/` means the top
   level.
3. `"[]"`- this is an empty parameter vector.  When you do a search or fill
   out a form, the parameters narrow the result.
4. `"Hello World"`- the result that gets sent back to the requesting
   browser.

After the GET is

```clojure
(route/not-found "Not Found")
```

This means that when the server gets any other kind of request it should
return "Not Found."

Right click on the page in the browser and select `Inspect Element
With Firebug.`

_insert screenshot_

The `HTML` tab shows what the HTML document looks like. The default is
an empty head and a body with the string "Hello World".  This is
different from what we saw when we used `View Page Source`.  The
browser requested html but only got a string back, and it fleshes
out a legal page from this information.

Click on the `Net` tab and refresh the page.  You'll see the request was
actually a GET request and the responce contained a status code of 200.
That indicates the request was successful.

In your browser address bar, try changing the url to
`http://localhost:3000/non-existent-page`.  Now you should see the GET
request is in red and has a status of 404, which indicates that the
server couldn't find the page.  This was handled by the line,

```clojure
(route/not-found "Not Found")
```

Back in the `handler.clj` file, the third and final expression begins

```clojure
(def app ...
```

> ```def``` is how you declare a variable in clojure.
> The format is:
>
> ```clojure
> (def name doc-string? init?)
> ```
>
> 1. `def` - introduces the def expression.
> 2. `name` - the name you want to give the variable.
> 3. `doc-string?` -  an optional description on what the variable is and how it is
> meant to be used.
> 4. `init?` - an optional value the variable will be set to.  If unset, the variable is unbound.

The name of the variable is `app` and it's being assigned the
result of

```clojure
(wrap-defaults app-routes site-defaults)
```

`app-routes` is what we're calling our set of routes; `site-defaults`
are default request/response handling details imported into the
namespace from ring library.  `wrap-defaults` is also imported from
ring, and it combines our routes and the defaults.  When we start the
server, it's going to look for the routes associated with the `app`
variable and use those to decide how to handle HTTP requests.

Let's stop the server by going back to the terminal and holding the
Control and letter c keys down at the same time, "Control-c".

### git

We haven't made any changes yet, so this is a good time to put the
code under version control.  Version control allows developers to keep
track of their changes over time.  It makes it easier to experiment
and coordinate work with others.

Inside the chatter directory, enter the command, `git init`

    $ git init
    Initialized empty Git repository in /home/crk/chatter/.git/

Now git is keeping an eye our directory.  Try asking git the status of
our directory.

    $: git status
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

We're on the master branch.  The master branch is the main place where
our code will be.  It says "Initial commit" because we're just
initializing git.  Git doesn't know anything about the files in our
project, but it has spotted the `README.md` and `project.clj` files as
well as the src and test directories.  It also spotted a file called
`.gitignore`.  Files beginning with a dot are normally hidden unless
you specifically ask to see them.  `.gitignore ` is a special file;
it tells git what kind of files we don't want git to track.  These will
mostly be compiled code, test reports, and log files.

Git follows a two-step process.  First you add the changes; they
become staged.  Then, you commit all of the staged changes.  Let's add
everything.

    $: git add .

The "." tells git the current directory and below.

Now when we ask git for the status:

    $: git status
    On branch master

    Initial commit

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
	new file:   README.md
	new file:   project.clj
	new file:   src/chatter/handler.clj
	new file:   test/chatter/handler_test.clj

All of our stuff is ready to be commited.  That requires a commit
message.

    $: git commit . -m "initial commit"
    [master (root-commit) 44a560f] initial commit
    5 files changed, 66 insertions(+)
    create mode 100644 .gitignore
    create mode 100644 README.md
    create mode 100644 project.clj
    create mode 100644 src/chatter/core/handler.clj
    create mode 100644 test/chatter/core/handler_test.clj

Now when we ask git for the status:

    $: git status
    On branch master
    nothing to commit, working directory clean

OK, there haven't been any changes since our last commit, so there's
nothing to see.  Let's check the log.

    $: git log
    commit 44a560f1653770afac01aea2c9279a7af46a46eb
    Author: crkoehnen <crkoehnen@gmail.com>
    Date:   Sun Dec 28 16:43:37 2014 -0600

        initial commit


We see there's been one commit, the commit hash (which uniquely identifies
every commit), the author's name, the date, and the commit comment.

By keeping track of changes, git makes it easy to go back to an
earlier point.  It's a safety net.  By itself, it won't do much if our
hard drive suddenly dies.  But git allows you to have repositories on
other computers.  If your computer dies, your code lives on.  GitHub
is a company hosting source code, free if you don't mind that other
people can see your code.  As a safety measure, we're going to put our
code on GitHub.

Log into [https://github.com](https://github.com) and click, "create
repository" (the "+" sign on the top menu).  Name it "chatter".  That
will open a page for your new repository.  We want to push an existing
repository, so (be sure to use the url that GitHub gives you instead
of mine!) :

    $: git remote add origin https://github.com/crkoehnen/chatter.git
    $: git push -u origin master
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

Back on GitHub, click on the "chatter" link and you'll go to the main
page for the repository.  Note there is a single commit and the text
is identical to what's in our `README.md` file.

<note: tagged section2>

### Workflow

Now we're going to start changing the templated code to make it our
web app.  We're going to follow a certain workflow:

1. branch the code
2. write some code
3. try the code
4. repeat 2-3 until we're happy with the changes
5. merge the branch into master
6. push the changes to GitHub

This methodology allows us to isolate changes in their own branch.  If
we change our minds or discover we've made a mistake, it's easy to
back up to an earlier version using git.

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

### hiccup

Now let's change the app's main page from "Hello, World" to something
a little more chatty.

First, let's create and checkout a new branch called, `view-messages`."

We need to write code that will generate HTML.  To do this, we're going to use a
library called `hiccup`.  Adding a new library requires two steps:

1) Add the library to the dependency section of the `project.clj` file.
   This tell lein that your program needs another program.

   Adding hiccup makes ours look like:

```clojure
  :dependencies [[org.clojure/clojure "1.6.0"]
                 [compojure "1.3.1"]
                 [ring/ring-defaults "0.1.2"]
                 [hiccup "1.0.5"]]
```

2) Import the library into the namespace you need by adding the import
   to the `ns` declaration.  We're going to be using it in
   `handler.clj`.  Our ns declaration will look like:

```clojure
(ns chatter.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [hiccup.page :as page]))
```

Let's use hiccup to generate the html by changing `app-routes`:


```clojure
(defroutes app-routes
  (GET "/" []
       (page/html5
        [:head
         [:title "chatter"]]
        [:body
         [:h1 "Our Chat App"]]))
  (route/not-found "Not Found"))
```

Start the server:

    $: lein ring server

You'll see that `http://localhost:3000` now proudly displays "Our Chat
App".  And if you `View Page Source`, you'll see that now it's
proper HTML complete with `head`, `title`, and `body`.

The hiccup function(*) `page/html5` generates an HTML page.  It expects
Clojure vectors with symbols representing corresponding HTML tags.
Hiccup will automatically add the closing tag when it reaches the end
of the vector.

_include screenshot_

Compare the hiccup to HTML in view-source to the HTML we wrote by hand
earlier.

> Vectors are a Clojure data structure used to contain sequences of
> things, including other vectors.  Vectors are often written using
> square brackets.  `[1 2 3]` is a vector containing the numbers
> 1, 2, and 3.  Hiccup uses vectors of keywords to represent sections
> of HTML.

> Keywords are names that begin with a colon.  `:title`, `:x`,
> and `:favorite-color` are all keywords.  Clojure often uses
> keywords where other languages use strings.  If you were to use
> Clojure to query a database, Clojure would probably use keywords to
> represent the column names.  Hiccup uses keywords to represent the
> names of HTML elements.  Where HTML uses `<body>`, hiccup would
> expect `:body`.  Where HTML uses `<title>`, hiccup uses
> `:title`.  Because the keywords are enclosed in a vector, the
> closing of the HTML tag is unnecessary.  The closing of the
> surrounding vector signals where the HTML section ends.

A problem with our new `app-routes` is that it's doing two different
things.  Its main role is to take the incoming request and decide
what to do.  Now it's doing that, but it's also generating a full HTML
page.  As we add more and more pages, this will become too complicated
to manage.  We'll get ahead of the game by splitting out the task of
generating the HTML into a helper function.

> Clojure defines a function using this syntax:
>
> ```clojure
> (defn name
>   doc-string?
>   params-vector
>   expression)
> ```
>
> 1. `defn` - introduces the defn expression.
> 2. `name` - the name you want to give the function.
> 3. `doc-string?` - an optional description of the function.
> 4. `params-vector` - a vector of symbols naming the functions arguments.
> 5. `expression` - the body of the function.
>
> `hello-world`, a traditional first function, might be programmed
> in Clojure like:
>
> ```clojure
> (defn hello-world
>   "ye olde 'Hello, World'"
>   []
>   "Hello, World")
> ```
>
> `hello-world`  takes no arguments and returns the string "Hello, World".
>
> `
> user> (hello-world)
> "Hello, World"
> `

Our new code should look like:

```clojure
(defn generate-message-view
  "This generates the HTML for displaying messages"
  []
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]]))

(defroutes app-routes
  (GET "/" [] (generate-message-view))
  (route/not-found "Not Found"))
```

`generate-message-view` is a function that takes no arguments.  It
calls a hiccup function `page/html5` to generate html from a
vector representing the `head` sections and a vector representing the
`body` elements of the html.

Save `handler.clj`, and let's refresh the browser to make sure our page
still works.  From the outside, we shouldn't see a change.  The page
should still display "Our Chat App" and the html should be identical.
Now, let's double check our git status:


    $: git status
    On branch view-messages
    Changes not staged for commit:
       (use "git add <file>..." to update what will be committed)
       (use "git checkout -- <file>..." to discard changes in working directory)

          modified:   project.clj
          modified:   src/chatter/core/handler.clj

    no changes added to commit (use "git add" and/or "git commit -a")

That looks right so let's add, commit, merge the changes back to
master, and then push to GitHub.  Then, delete the `view-messages` branch.
You should see the commit numbers go up on GitHub.

<note: git tag, section 3>

### Adding Messages

We still aren't displaying messages, nor do we have a way of
adding them.  Now, we'll make that happen.

Create and check out a branch to work on.

Let's change the app to display messages.  We'll represent the
messages as a vector of maps.  Each map will have a `:name` and `:message`
key and the corresponding value, in quotes.  For example:

```clojure
{:name "blue" :message "blue's first post"}
```

will represent blue's first post.  This is a map with two keys.  `:name`
is "blue", because blue posted it, `:message` is the content of the post
and its value is "blue's first post".

> Programs often need to associate keys with values and the usual data
> structure for doing that are hash tables.  Clojure calls them maps and
> they look like:
>
> ```clojure
> (def cities
> {"Tokyo" 37900000
>  "Delhi" 26580000
>  "Seoul" 26100000})
> ```
> `cities` is a hash table whose keys are strings (the names of
> cities) and the values are the populations of each city.
>
> To get a value from a map, pass the map and key into the
> `get` function.
>
> ```clojure
> (get cities "Tokyo")
> ```
> returns 37900000.
>
> When the keys are keywords, you can also use the keyword as a
> function that takes the map and returns the values.
>
> ```clojure
> (:name {:name "blue" :message "blue's first post"})
> ```
> returns "blue".
>
> ```clojure
> (:message {:name "blue" :message "blue's first post"})
> ```
> returns "blue's first post".
>
> Maps are everywhere in Clojure and are used for many things where
> other languages might use objects.

Let's call the vector simply `chat-messages` and hard code(*) some
samples to get started.  Add a chat-messages variable to `handler.clj`.

After the ns expresion, add:

```clojure
(def chat-messages [{:name "blue" :message "hello, world"}
                    {:name "red" :message "red is my favorite color"}
                    {:name "green" :message "green makes it go faster"}])
```

Next, we'll modify the HTML to display the messages.  We will also add a
parameter to the `generate-message-view` function so that we can give
it the messages we want displayed.

```clojure
(defn generate-message-view
  "This generates the HTML for displaying messages"
  [messages]
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p messages]]))

(defroutes app-routes
  (GET "/" [] (generate-message-view chat-messages))
  (route/not-found "Not Found"))
```

Save `handler.clj` then go back to the browser and refresh the page.

This blows up spectacularly.

![blowups happen, illegal argument exception](images/illegal-argument-exception.jpg "Illegal Argument Exception")

This is a stack trace, which gives an idea what the program was
doing when it hit the problem.  Ignore all the files that aren't ones
you wrote for the project.  In my case, the first file of interest is
`handler.clj`, line 14, the `generate-message-view` function.

The exception message on the top, "... is not a valid element name",
is a clue to what's wrong.  Elements are what fragments of html are
called.  Hiccup is responsible for generating html from Clojure
symbols.  The problem is that we've got a map with symbols in it and
hiccup thinks they're html.  They're not, so it blows up.

We can finesse the issue by converting our maps to strings.

```clojure
(defn generate-message-view
  "This generates the HTML for displaying messages"
  [messages]
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p (str messages)]]))
```

Save `handler.clj`, then refresh the browser.

This fixes the exception but is ugly.

![ugly hack](images/ugly.jpg "converting messages to a string")

Let's take the messages and put them in a table using HTML's `table`,
`tr`, and `td` elements.  We're going to write a function that takes a
message and creates an HTML row.  Then, inside a `table`, we're going to
apply that function to all of our messages.


> Clojure uses `defn` to create a function, but those functions are named.
> Sometimes, we want a specialized function that isn't reusable.
> For those cases, Clojure has a way of creating an anonymous function.
>
> ```clojure
> (fn params-vector expression)
> ```

Our function is going to take a message, we'll call it "m" within the function,
and extract both the `:name` and `:message`, wrapping them in `:td` to make
table cells and putting them both within a `:tr` to make the row.  Since the keys to
the message hashmap are keywords, we can use them as functions to get the values.  In Clojure, the function looks like:

```clojure
(fn [m] [:tr [:td (:name m)] [:td (:message m)]])
```

> Making a new collection by applying a function to all of the elements of a collection
> is such a common thing to do that Clojure has that functionality predefined.  It's a
> function called `map`, which can be confusing when you're talking about "mapping"
> (the function) over a collection of maps (hash tables), which we are.
>
> The syntax is:
>
> ```clojure
> (map fn coll)
> ```
>
> `map` - signifies that we're going to be invoking the map function.
>
> `fn` - the function we're going to apply to every element.
>
> `coll` - the collection containing the elements.

Mapping our anonymous function over our vector of messages looks like:

```clojure
(map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)
```

Now our `generate-message-view` looks like:

```clojure
(defn generate-message-view
  "This generates the HTML for displaying messages"
  [messages]
  (page/html5
     [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p
     [:table
      (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]]]))
```

Save `handler.clj`, then refresh the browser.  Our hard-coded messages should
now display in the page.

![hard coded messages](images/hardcoded.jpg "hard coded messages")

<note: section4>

### Forms

We still don't have a way of adding messages.  This requires HTML forms
and importing the form functions from hiccup.  The form allows the user
to send messages in the parameters of an HTML `POST`.  We will need
to extract the message and add it to our collection of messages.  This
will be the most complicated set of changes in our app.


> In HTML, a `form` is used to send input from the browser
> to the server.  The `form` element contains a pair of
> attributes.
>
> `action` - which specifies the route that should handle the input.
>
> `method` - which specifies the type of request.

Up until now, we've only used `GET`.  To send messages, we'll need to
add a `POST`.

> `forms` contain text and `input` elements.  The `input`
> elements define the content the `form` will send to the
> server.  `input` elements have a number of attributes:
>
> `id` - a way of identifing the input
>
> `name` - the name of the input
>
> `type` - the kind of input
>
> `value` - the default value for the input

We're going to use hiccup to generate html that looks like,

```html
<form action="/" method="POST">
  Name: <input id="name" name="name" type="text">
  Message: <input id="msg" name="msg" type="text">
  <input type="submit" value="Submit">
</form>
```

First, we need to import some libraries into our `handler.clj` file.  Add:

```clojure
[hiccup.form :as form]
```

to the `:require` section of the `ns` declaration.  It should now look like:

```clojure
(ns chatter.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [hiccup.page :as page]
            [hiccup.form :as form]))
```

Now that we have imported the hiccup form function, we can use it to
generate the HTML form.

```clojure
(form/form-to
 [:post "/"]
 "Name: " (form/text-field "name")
 "Message: " (form/text-field "msg")
 (form/submit-button "Submit"))
```

`form/form-to` is a hiccup function for generating the form.

`[:post "/"]` - is a vector with the keyword `:post` and the string "/".
This tells hiccup to make the method a `POST` to the `/` location.

`"Name: "` - a string that will be the text displayed before the input field.

`form/text-field` - is a hiccup function for generating an input field of type "text".  We're passing in the string "name".

`"Message: "` - a string that will be the text displayed before the input field.

`form/submit-button` - a hiccup function for for generating the submit button.

We want to generate the form button below the title but above the list
of messages in the `generate-message-view` function.

Finally, we're going to change our definition of the app

from:

```clojure
(def app
  (wrap-defaults app-routes site-defaults))
```

to:

```clojure
(def app app-routes)
```

This is a simplification for the tutorial.

Now our code looks like:

```clojure
(ns chatter.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [hiccup.page :as page]
            [hiccup.form :as form]))

(def chat-messages [{:name "blue" :message "blue's first post"}
                    {:name "red" :message "red is my favorite color"}
                    {:name "green" :message "green makes it go faster"}])

(defn generate-message-view
  "This generates the HTML for displaying messages"
  [messages]
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p
     (form/form-to
      [:post "/"]
      "Name: " (form/text-field "name")
      "Message: " (form/text-field "msg")
      (form/submit-button "Submit"))]
    [:p
     [:table
      (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]]]))

(defroutes app-routes
  (GET "/" [] (generate-message-view chat-messages))
  (POST "/" [] (generate-message-view chat-messages))
  (route/not-found "Not Found"))

(def app app-routes)
```

Save ```handler.clj``` and refresh the browser.  We should now have a form on the
page where a user could submit a new message.

![unwired form](images/unwired-form.jpg "unwired form")

<note: section5>

### Wiring the form

We see the form now, but submitting it does nothing.  The problem now is
that we're extracting the params during the `POST` but aren't actually
doing anything with them.  To fix this, we have to extract the
parameters from the form, build a message, and store the message in our
messages vector.  Actually, this might be the hardest part of our app.

First, we need to import a library to extract the information sent by
the form.  Add this to the ```:require``` section,

`[ring.middleware.params :refer [wrap-params]]`

Next, change the `app` definition from:

```clojure
(def app app-routes)
```

to:

```clojure
(def app (wrap-params app-routes))
```

This enables us to have access to the information sent back in our `form`.

We want to be able to add new messages to our `messages` vector.
Clojure was designed from the ground up to make it easier to write
concurrent programs, programs that do more than one thing at a time.  It
does that by having having data structures that do not change.
Variables can be changed to point to something else, but Clojure
requires that doing so happens using particular functions, so it can
ensure the program stays in a safe state.  We're going to use the
`atom` mechanism to allow us to update our `messages`.


> An `atom` is like a box that protects information from being changed in
> an unsafe way.  You simply pass the information into the `atom`.

Instead of having the `chat-messages` variable point to our vector of messages,
we're going to have it point to the `atom` protecting the vector.

Instead of:

```clojure
(def chat-messages [{:name "blue" :message "blue's first post"}
                    {:name "red" :message "red is my favorite color"}
                    {:name "green" :message "green makes it go faster"}])
```

We'll use:

```clojure
(def chat-messages
     (atom [{:name "blue" :message "blue's first post"}
            {:name "red" :message "red is my favorite color"}
            {:name "green" :message "green makes it go faster"}]))
```

Now `chat-messages` is pointing to the `atom` protecting our vector
of hashes.

Because `chat-messages` is pointing to the `atom`, we can't simply
`map` over it in `generate-message-view`.  Now, we have to tell
Clojure that we want to generate HTML for the contents of the atom.  This
allows Clojure to ensure the messages are always read in a consistent
state, even though something could be modifiying them.

> Reading what's stored in an `atom` is called "dereferencing" and
> is represented by the `@` character.

We will dereference the `chat-messages` atom just before it is passed to the
`generate-message-view` function.  We can do this by changing our routes from:

```clojure
(defroutes app-routes
  (GET "/" [] (generate-message-view chat-messages))
  (POST "/" [] (generate-message-view chat-messages))
  (route/not-found "Not Found"))
```

to:

```clojure
(defroutes app-routes
  (GET "/" [] (generate-message-view @chat-messages))
  (POST "/" [] (generate-message-view @chat-messages))
  (route/not-found "Not Found"))
```

If you save `handler.clj` and refresh the browser, the hard coded examples
should display as before.  We still won't see any new messages because we still
need to extract the information from the form and modify `chat-messages`.

To add messages to `chat-messages`, we will need to introduce two more
functions: `conj` and `swap!`.

> #### conj
> There are many ways to work with collections of values in Clojure.  One commonly
> used function is `conj`.  The name is short for "conjoin".  This function
> takes a collection and one or more item(s) to add to the collection.  It then
> returns a _new_ collection without modifying the original collection.
>
> ```clojure
> (conj [:one :two] :three)
> => [:one :two :three]
>
> (conj [:one :two :three] :four :five)
> => [:one :two :three :four :five]
> ```

> #### swap!
> To modify an `atom`, Clojure provides `swap!`.
>
> ```clojure
> (swap! atom update-function arguments...)
> ```
>
> `atom` - the atom to be updated.
>
> `update-function` - the function that is applied to the value
> protected by the atom.  It returns a new value which will replace the
> original.
>
> `arguments...` - zero or more arguments to be passed to the `update-function`.
>
> The `swap!` function will:
>  1. Dereference the atom
>  2. Pass this dereferenced value to the `update-function`
>     along with any additional arguments<br/>
>     You can think of it like this: `(update-function @atom arguments...)`
>  3. Safely replace the inner content of the atom with the
>     value returned from the `update-function`, and finally...
>  4. Return the new content of the atom.
>
> ```clojure
> (def a-number (atom 1))
>
> @a-number
> => 1
> (swap! a-number + 2)
> => 3
> @a-number
> => 3
> ```

In our case, we're going to "swap" the content of ```chat-messages``` by
"`conj`ing" a new message onto the vector of messages.

We'll also put it in a helper function to make it easier to maintain.

```clojure
(defn update-messages!
  "This will update a message list atom"
  [messages name new-message]
  (swap! messages conj {:name name :message new-message}))
```

Now, we have to modify our `app-routes`.  We have to make two
changes; it needs to extract the form information when somebody
`POST`s a new message, and it needs to add the new message to our
`chat-messages` before returning the page to the user.  Both of
these changes need to happen in the `POST` route.

The new `app-routes` looks like

```clojure
(defroutes app-routes
  (GET "/" [] (generate-message-view @chat-messages))
  (POST "/" {params :params} (generate-message-view
                               (update-messages! chat-messages
                                 (get params "name") (get params "msg"))
                               ))
  (route/not-found "Not Found"))
```
1. `{params :params}` is a shorthand notation that tells Clojure to extract
   all of the data submitted from the HTML form and call that data `params`.
2. `(get params "name")` and `(get params "msg")` extract the values of
   the "name" and "msg" fields from the form data.
3. The `update-messages!` function is then called with the `chat-messages` atom
   and the values of the "name" and "msg" fields from the form.
4. After `update-messages!` has added the new message to the inner content of
   `chat-messages` it returns the new, dereferenced, vector of messages held by
   the atom.
5. `generate-message-view` is called with the updated collection of messages and
   builds the HTML response for the user.

Another way of writing this, which may make the intent more clear, is to
name some of the intermediate values using `let`.  This will allow us to
temporarily provide names for the results of some of the expressions.

```clojure
(defroutes app-routes
  (GET "/" [] (generate-message-view @chat-messages))
  (POST "/" {params :params}
    (let [name-param (get params "name")
          msg-param (get params "msg")
          new-messages (update-messages! chat-messages name-param msg-param)]
      (generate-message-view new-messages)
      ))
  (route/not-found "Not Found"))
```

1.  Extract the "name" field from the form data in `params` and name it `name-param`.
2.  Extract the "msg" field from the form data in `params` and name it `msg-param`.
3.  Execute the `update-messages!` function for the chat-messages atom and the values of
    the previously established `name-param` and `msg-param` names.
4.  Assign the name `new-messages` to the result of `update-messages!`.
5.  Execute `generate-message-view` for the new collection of messages now called `new-messages`.
6.  Return the HTML produced by `generate-message-view` and forget about the names
    `name-param`, `msg-param`, and `new-messages`.

> #### let
> `let` expressions are used to temporarily associate names with the results of other
> expressions, similar to how a function assigns names to its arguments.  These named
> values can also be re-used without the cost of re-evaluating the expression that
> generated them.
>
> (let [name-one expression-one]
>       name-two expression-two]
>   (some-function name-one name-two))
>
> 1. `name-one` - a name for the result of evaluating `expression-one`.
> 2. `name-two` - a name for the result of evaluating `expression-two`.
> 3. Call `some-function` and pass it the values assigned to `name-one`
>    and `name-two`.
> 4. Return the result of the last expression within the `let`, and
>    forget about the names we had created.
>
> ```clojure
> (let [two   2
>       three (+ two 1)]
>   (* two three))
> => 6
> ```

If you save `handler.clj`, we should be able to use the form to
add messages to the page.

Since we can add messages through the form, we can remove our hard-coded messages.  Change
the messages to an empty vector.

```clojure
(def chat-messages (atom []))
```

Now, the app is taking our new messages, but it's adding new messages to
the end.  That's going to be hard to read.  We can fix that by
changing from a vector to a list.


```clojure
(def chat-messages (atom '()))
```

> Like vectors, lists are sequential collections.  Vectors are better for accessing random
> elements fast (which we aren't doing).  Lists are better at adding an element to the front,
> which we want to do.  Since they are both collections, `conj` works with either.

Our app now looks like:


```clojure
(ns chatter.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [ring.middleware.params :refer [wrap-params]]
            [hiccup.page :as page]
            [hiccup.form :as form]))

(def chat-messages (atom '()))

(defn generate-message-view
  "This generates the HTML for displaying messages"
  [messages]
  (page/html5
   [:head
    [:title "chatter"]]
   [:body
    [:h1 "Our Chat App"]
    [:p
     (form/form-to
      [:post "/"]
      "Name: " (form/text-field "name")
      "Message: " (form/text-field "msg")
      (form/submit-button "Submit"))]
    [:p
     [:table
      (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]]]))

(defn update-messages!
  "This will update a message list atom"
  [messages name message]
  (swap! messages conj  {:name name :message message}))

(defroutes app-routes
  (GET "/" [] (generate-message-view @chat-messages))
  (POST "/" {params :params}
    (let [name-param (get params "name")
          msg-param (get params "msg")
          new-messages (update-messages! chat-messages name-param msg-param)]
      (generate-message-view new-messages)
      ))
  (route/not-found "Not Found"))

(def app (wrap-params app-routes))
```

Add, commit, merge the changes to master, push master to GitHub, and delete the branch.

### Bootstrap

The app is working but is ugly.  We can improve it by using CSS and JavaScript from a
package of software called Twitter Bootstrap.

Create and checkout a new branch.

In the head section of our HTML, we're going to include Bootstrap:

```clojure
   [:head
    [:title "chatter"]
    (page/include-css "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css")
    (page/include-js  "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js")]
```

Start the server again, and you should see the fonts change.  You'll also see the table get smashed together.

Now, let's change the table element from `:table` to `:table#messages.table`.

```clojure
    [:table#messages.table
     (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]
```

This tells hiccup that we want the table to have an id of `messages` and a class of `table`.
CSS works by looking for combinations of classes and structure and changing the appearance
when an element matches a pattern.  Bootstrap uses a set of predefined CSS to look for a common
set of classes.  One of them is table.  Save, then refresh the browser.  It should look better.
Examine the HTML that's now generated.  You should see an id and class field inside the table element.

Let's make the table entries stripped by adding an additional class.  Change the table
element to `:table#messages.table.table-striped`.

```clojure
    [:table#messages.table.table-striped
     (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]
```


What do you think would happen if you changed `table-striped` to `table-bordered`?  Try it and refresh your brower!

HTML elements can have multiple classes and CSS uses this to create more complex
effects.  Try adding `table-hover` to the table element: `:table#messages.table.table-bordered.table-hover`.


```clojure
    [:table#messages.table.table-hover
     (map (fn [m] [:tr [:td (:name m)] [:td (:message m)]]) messages)]
```

Now, when you move the mouse over a row, the entire row becomes
highlighted.  Dynamic effects in the browser are implemented using a
language called JavaScript.  We won't talk about JavaScript except to
say that it exists and the JavaScript part of Bootstrap was what we
imported into the page with the `include-js` call.

Let's create our own CSS file to center the heading.  Create a file `chatter.css` in the `resources/public` directory.  Inside, paste:


```css
h1 {
    text-align: center;
}
```

CSS works using pattern matching.  In this case, we're saying that if
the element is an `h1` element, center the text.  Save the file and add
another `page/include-css` expression to `handler.clj` to pull in
`chatter.css`.

```clojure
   [:head
    [:title "chatter"]
    (page/include-css "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css")
    (page/include-js  "//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js")
    (page/include-css "/chatter.css")]
```

Refresh the page.  We want to see the `h1` tag centered.  It won't
though.  Open Firebug and watch the traffic as you refresh the page.
We're getting a 404 when it's trying to download the css.

The problem is in our `defroutes`.  We have a route handling browser GET
or POST requests, but anything else is falling through to our
`route/not-found` call.  We need to tell `defroutes` where to find our
resources.  Change the defroutes to:

```clojure
(defroutes app-routes
  (GET "/" [] (generate-message-view @chat-messages))
  (POST "/" {params :params}
    (let [name-param (get params "name")
          msg-param (get params "msg")
          new-messages (update-messages! chat-messages name-param msg-param)]
      (generate-message-view new-messages)
      ))
  (route/resources "/")
  (route/not-found "Not Found"))
```
### Heroku

Up until now, we've been running the server on our computer,
`localhost`(*).  This works great while writing or developing the
program because it makes testing changes fast and easy, but eventually
we'll want to put it on the internet for others to see and use.

To put it on the internet, we're going to use a company called Heroku
for hosting(*).  They're going to run our program on a machine visible
to anybody on the Internet.  We'll use git to send them our program
The advantage of using a company like Heroku is that they'll handle
the work of actually maintaining a server.  One of the disadvantages
is that Heroku can be expensive, but for a little program like ours
it's free.

First, we need to make a couple of changes to our app so it plays nicely
with Heroku.

In `handler.clj`, we'll add a couple of functions to print log
messages when Heroku starts and stops the program.


```clojure
(defn init []
  (println "chatter is starting"))


(defn destroy []
  (println "chatter is shutting down"))
```

Then we need to add a main function.  This is what Heroku will actually
invoke(*) to start our program.

```clojure
(defn -main [& [port]]
  (let [port (Integer. (or port (env :port) 5000))]
    (jetty/run-jetty #'app {:port port :join? false})))
```

Finally, we need to change our `project.clj` so it knows how to prepare our
application for Heroku.

Our new `project.clj` should look like:

```clojure
(defproject chatter "0.1.0-SNAPSHOT"
  :description "clojure web app for displaying messages"
  :url "http://example.com/FIXME"
  :min-lein-version "2.0.0"
  :dependencies [[org.clojure/clojure "1.6.0"]
                 [compojure "1.3.1"]
                 [ring/ring-defaults "0.1.2"]
                 [ring/ring-jetty-adapter "1.3.2"]
                 [hiccup "1.0.5"]
                 [hickory "0.5.4"]
                 [environ "1.0.0"]]
  :plugins [[lein-ring "0.8.13"]
            [lein-environ "1.0.0"]]
  :ring {:handler chatter.handler/app
         :init chatter.handler/init
         :destroy chatter.handler/destroy}
  :aot :all
  :profiles
  {:dev
   {:dependencies [[javax.servlet/servlet-api "2.5"]
                   [ring-mock "0.1.5"]]}
   :production
   {:ring
    {:open-browser? false, :stacktraces? false, :auto-reload? false}
    :env {production true}}}
  :uberjar-name "chatter-standalone.jar")
```

The `ns` of `handler.clj` should look like:

```clojure
(ns chatter.core.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [ring.middleware.params :refer [wrap-params]]
            [ring.adapter.jetty :as jetty]
            [hiccup.page :as page]
            [hiccup.form :as form]
            [ring.util.anti-forgery :as anti-forgery]
            [environ.core :refer [env]]))
```

We should still be able to start the app using `lein ring server` but
now we can also start it with `lein run -m chatter.handler`.  When we
start it this way, it'll default to port 5000, and you should see that
in the browser.  Port 3000 won't work anymore, but you'll see the app if
you switch to 5000.

If we create a jar with `lein uberjar`, we can aso start the app with 
`java $JVM_OPTS -cp target/chatter-standalone.jar clojure.main -m chatter.handler`

These new methods of starting the app are closer to what Heroku will use to start the app.

We also need a Procfile in the top directory containing the line
`web: java $JVM_OPTS -cp target/chatter-standalone.jar clojure.main -m chatter.handler`

We'll deploy to heroku with the following commands:
+   `heroku create`
+   `git push heroku master`
+   `heroku ps:scale web=1`
+   `heroku open`

The final command will open the browser and point it at your app on Heroku.

Try the traceroute command again against the address Heroku assigned your app:

    $: traceroute obscure-brushlands-9918.herokuapp.com
    traceroute to obscure-brushlands-9918.herokuapp.com (50.16.239.160), 30 hops max, 60 byte packets
    1  192.168.1.1 (192.168.1.1)  15.820 ms  15.797 ms  15.773 ms
    2  96.120.49.33 (96.120.49.33)  23.895 ms  25.196 ms  25.192 ms
    3  te-0-2-0-6-sur01.webster.mn.minn.comcast.net (68.85.167.145)  25.179 ms  25.168 ms  25.362 ms
    4  te-0-7-0-13-ar01.crosstown.mn.minn.comcast.net (68.87.174.189)  30.743 ms  31.979 ms te-0-7-0-12-ar01.crosstown.mn.minn.comcast.net (69.139.219.129)  31.979 ms
    5  pos-0-0-0-0-ar01.roseville.mn.minn.comcast.net (68.87.174.194)  31.967 ms * *
    6  he-1-12-0-0-cr01.350ecermak.il.ibone.comcast.net (68.86.94.77)  39.269 ms  25.568 ms  26.422 ms
    7  be-10206-cr01.newyork.ny.ibone.comcast.net (68.86.86.225)  45.276 ms  63.727 ms  66.090 ms
    8  he-0-11-0-0-pe03.111eighthave.ny.ibone.comcast.net (68.86.83.98)  58.386 ms  58.374 ms  58.358 ms
    9  as16509-3-c.111eighthave.ny.ibone.comcast.net (50.242.148.118)  56.143 ms  58.280 ms  65.988 ms
    10  54.240.229.76 (54.240.229.76)  65.977 ms 54.240.229.82 (54.240.229.82)  65.982 ms *
    11  54.240.228.190 (54.240.228.190)  65.874 ms 54.240.228.202 (54.240.228.202)  65.888 ms 54.240.228.196 (54.240.228.196)  65.969 ms
    12  54.240.229.223 (54.240.229.223)  42.112 ms 54.240.229.200 (54.240.229.200)  45.620 ms 54.240.229.221 (54.240.229.221)  47.673 ms
    13  54.240.228.173 (54.240.228.173)  47.630 ms 54.240.228.177 (54.240.228.177)  52.382 ms 54.240.228.173 (54.240.228.173)  45.498 ms
    14  72.21.220.100 (72.21.220.100)  52.589 ms 72.21.220.116 (72.21.220.116)  52.527 ms 54.240.228.139 (54.240.228.139)  50.825 ms
    15  72.21.220.135 (72.21.220.135)  56.875 ms 72.21.220.167 (72.21.220.167)  59.011 ms 72.21.220.151 (72.21.220.151)  59.014 ms
    16  72.21.220.108 (72.21.220.108)  48.813 ms 205.251.245.242 (205.251.245.242)  49.425 ms  48.614 ms
    17  * * *
    18  * * *
    19  * * *
    20  216.182.224.85 (216.182.224.85)  55.704 ms 216.182.224.223 (216.182.224.223)  66.895 ms 216.182.224.227 (216.182.224.227)  54.629 ms
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

Try going to each other's app.

To delete the app from Heroku, select the app in the dash board, click settings, delete app is on the bottom.  Then, "git remote remove heroku".

