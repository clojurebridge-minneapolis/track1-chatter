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

