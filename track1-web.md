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
Try adding this table to our sample html:

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
