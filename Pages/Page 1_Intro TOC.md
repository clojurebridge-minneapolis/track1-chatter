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

