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


