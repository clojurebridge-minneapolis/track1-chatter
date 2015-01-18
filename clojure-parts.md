
## def

def is a way of defining variables.  The format is:

```clojure
(def name doc-string? init?)
```

1. ```def``` - introduces the def expression.
2. ```name``` - the name you want to give the variable.
3. ```doc-string?``` -  an optional description on what the variable is and how it is
meant to be used.
4. ```init?``` - an optional value the variable will be set to.  If unset, the value is unbound.

A value is an expression that cannot be evaluated any further and a
type is a collection of values.  Clojure is a dynamic language; the
variable has a value but does not itself have a type.  The value has a
type.

Variables must be def'd before they can be used.  As a convenience, it is legal to
def a variable, then def the variable to a different value, even a value of a completely
different type, but this is considered bad style.

## vectors

Vectors are both collections and sequences, generally created using the square brackets.

```clojure
(def blue [1 2 3])
```

They are similiar to arrays in other languages.  The contents of the
vector do not have to be the same type.

```clojure
(def red [1 "hi there"])
```

Since vectors are collections, we can see how many elements it has using count:

```
user> (count red)
2
user> (count blue)
3
```

We can also check if it's empty:

```
user> (empty? red)
false
```

Since vectors are also sequentials, we can also get the first element, the
second element, or the last element.

```
user> (first blue)
1
user> (second blue)
2
user> (last blue)
3
```

Vectors also have a special trick where you can use it as a function that takes
and index and returns the element at that index.

```
user> (blue 1)
2
```

Like many languages, clojure vectors are zero-indexed.  The first
element is in the 0th place, so

```clojure
(blue 1)
```
Actually returns the second element.

Many languages allow you to change what's in an array.  Clojure does
not because when programs become large enough, mutating arrays (or
other things) makes it hard to write correct code that fully uses the
power of the computer.  Instead, clojure has ways of making it look like
the array is modified, but what it's really doing it returning a new vector
that reuses as much of the original information as it could.

If we want to add an element to the vector, we can use "conj", which
returns a new vector with the new element added.

```
user> (conj red "something new")
[1 "hi there" "something new"]
user> red
[1 "hi there"]
```

Notice that "conj" returned a vector that was just like red but with
"something new" added to the end.  When we check the value of red, it
is unchanged.  We can save the result of "conj" to a variable if we
wanted to reuse it.

```
user> (def green (conj red "something new"))
#'user/green
user> green
[1 "hi there" "something new"]
```

We can also use "assoc" to associate an index with a new value.

```
user> green
[1 "hi there" "something new"]
user> (assoc green 1 1001)
[1 1001 "something new"]
user> green
[1 "hi there" "something new"]
```

Like "conj", "assoc" returns a value that looks like the original
vector with our change but is really a new vector that reuses as much
of the original as possible.

## keywords

Keywords are symbolic identifiers that evaluate to themselves.
Keywords begin with a colon.

+ :x
+ :st-paul
+ :favorite-color

are all keywords.

## defn

Functions are the heart and soul of a clojure program.  They take
arguments and return a value.  We've been using functions but defn
gives us a way of creating our own and giving it a name.

The basic structure of ```defn``` is:

```clojure
(defn name
  doc-string?
  params-vector
  expression)
```

1. ```defn``` - introduces the defn expression.
2. ```name``` - the name you want to give the function.
3. ```doc-string?``` - an optional description of the function.
4. ```params-vector``` - a vector of symbols naming the functions arguments.
5. ```expression``` - the body of the function.

A simple function would,

```clojure
(defn hello-world
  "ye olde 'Hello, World'"
  []
  "Hello, World")
```

This creates a function called "hello-world", which takes no arguments and returns the string "Hello, World".

```
user> (hello-world)
"Hello, World"
```

The doc-string? is "ye olde 'Hello, World'".  We can see the doc-string?, when there is one, by calling the function "doc"
on the function we're curios about.

```
user> (doc hello-world)
-------------------------
user/hello-world
([])
  ye olde 'Hello, World'
nil
```

Saying hello to the World is a good thing but it would be useful to
say hello to other things too.  By adding an argument, we can say
hello to other things.  This time, we'll need to build up a string
since we won't be able to return a constant string.  The function
"str" will take an arbitrary number of arguments and convert them into
a string.  And since we'll update the name and doc-string? to reflect
the new functionality.

```clojure
(defn hello-thing
  "Takes something and returns `Hello, <thing>'"
  [thing]
  (str "Hello, " thing))
```

```
user> (hello-thing 13)
"Hello, 13"
user> (hello-thing "world")
"Hello, world"
user> (hello-thing (+ 10 20 30))
"Hello, 60"
```

That works but now we have two functions, one exclusively for saying
hello to the world and the other for everything else, and there's
actually a fair amount of overlap between the two.  We can reduce the
redundancy by combining them.  We'll just call this version hello.

```clojure
(defn hello
  "returns 'Hello, <thing>' when passed thing, otherwise returns 'Hello, World'"
  ([] "Hello, World.")
  ([thing] (str "Hello, " thing)))
```

```
user> (hello)
"Hello, World."
user> (hello 1001)
"Hello, 1001"
user> (doc hello)
-------------------------
user/hello
([] [thing])
  returns 'Hello, <thing>' when passed thing, otherwise returns 'Hello, World'
nil
```

## maps

Programs often need to associate keys with values and the usual data
structure for doing that are hash tables.  Clojure calls them maps and
they look like:

```clojure
(def cities
{
 "Tokyo" 37900000
 "Delhi" 26580000
 "Seoul" 26100000
 "Shanghai" 25400000
 "Mumbai" 23920000
 "Karachi" 23500000
 "Mexico" 22200000
 "Beijing" 21650000
 "Sao" 21390000
 "Jakarta" 20500000
})
```
The curly brace opens the map, the city names are the strings and the
populations are the values.

Use get to look up a value for a particular key:

```
user> (get cities "Delhi")
26580000
```
You can use anything you like as keys but the most common thing to use
are keywords.  As a convenience, you can use the keyword as a way of
accessing that particular value:

```
user> (def here {:x 2 :y 4})
#'user/here
user> (:x here)
2
user> (:y here)
4
```

Like vectors, you can use assoc to return a new map with changes:

```
user> (assoc here :y 20 :z -13)
{:y 20, :z -13, :x 2}
user> here
{:y 4, :x 2}
```

Again, assoc does not change the original data, it returns a copy that
reuses as much of the original as possible.

The reason assoc works with both vectors and maps is that they are
both considered "associate" data structures.  Maps associate their
keys with values, vectors associate their index with values.

Maps are used everyhwere in clojure and are often used where other
languages would use classes.

## map, fn

Map has another meaning in clojure, it's a way of walking over a
collection, apply a function to each element in the collection, and
returning a new collection of the results.

For example, if we had a function that squared numbers, we map across
a vector, and return a new collection.

```
user> (def numbers [1 2 3 4 5])
#'user/numbers
user> (defn square
        [x]
        (* x x))
#'user/square
user> (map square numbers)
(1 4 9 16 25)
user> numbers
[1 2 3 4 5]
```

Naming is hard and often when we want to map over a collection, the
function we're applying is a one-off that we aren't going to reuse.
In clojure, you can create an anonymous function using ```fn```

```clojure
(fn params-vector expression)
```

Without defining "square", we could have written:

```
user> (map (fn [x] (* x x)) numbers)
(1 4 9 16 25)
user> numbers
[1 2 3 4 5]
user>
```

## atom, swap!, do

Everything we've done up until this point has been purely functional.
We haven't been changing any data and our functions have simply taken
inputs and returned values.

In clojure, variables have values.  Instead of modifying the value,
you point the variable to an entirely different value.  Clojure
ensures that the change happens in a way that everything using the
data sees it in a consistent state, which is important when writing
programs that do more than one thing at a time.  But, to do this,
clojure requires hints from you that a varable can be pointed to
something else, and it needs to do the pointing in a safe way.

There are a couple of different ways to do this but, in this tutorial,
we're only going to use the mechanism called an "atom".

Let's use a map to represent the solar system.  Normally, planets
don't change and this matches perfectly clojure's view of the world.
But research astronomers change what we know all the time, so if we
were writing the program for them, we might want the planets to
change.  To do this, we'll put the map in an atom.

```
user> (def solar-system
        (atom
         {:mercury  {:position 1 :color :grey}
          :venus    {:position 2 :color :yellow}
          :earth    {:position 3 :color :blue}
          :mars     {:position 4 :color :red}
          :jupiter  {:position 5 :color :orange}
          :saturn   {:position 6 :color :yellow}
          :uranus   {:position 7 :color :blue}
          :neptune  {:position 8 :color :blue}
          :pluto    {:position 9 :color :brown}}))

#'user/solar-system
user> solar-system
#<Atom@6b37948d: {:mercury {:color :grey, :position 1}, :uranus {:color :blue, :position 7}, :mars {:color :red, :position 4}, :pluto {:color :brown, :position 9}, :neptune {:color :blue, :position 8}, :jupiter {:color :orange, :position 5}, :earth {:color :blue, :position 3}, :venus {:color :yellow, :position 2}, :saturn {:color :yellow, :position 6}}>
```

Notice how the repl displays it as an atom wrapping a map.  If we
try getting mercury, we'll get nil back.

```
user> (:mercury solar-system)
nil
```

Clojure is unhappy.  Keywords used as lookups expect to be given a map
but instead it was given an atom.  The atom is like a box and you have
to tell it that you want to look inside.  This is so it can ensure
that nobody is changing what's protected by the box while you're
looking at it.  To get inside, use the ```@``` sign:

```
user> (:mercury @solar-system)
{:color :grey, :position 1}
```

You can also use ```deref```, for "dereference":

```
user> (first (deref solar-system))
{:color :grey, :position 1}
```

Suppose we want to remove pluto.  ```disassoc``` returns a new map
with the pluto key/value pair removed but we want to change the solar
system itself.  We'll use ```swamp!```.  The explanation point is a
sign that something is changing and is a warning to programmers
reading the code.  The syntax is:

```clojure
(swap! atom fn x ...)
```

1. ```swap!``` - starts the call.
2. ```atom``` - the atom we want to update.
3. ```fn``` - a function.
4. ```x ...``` - zero or more additional arguments.

```swap!``` takes the atom, dereferences it, calls the function on the
dereferenced values along with any other additional arguments, then
changes variable protected by the atom to the result of the function.

```

user> (swap! solar-system dissoc :pluto)
{:mercury {:color :grey, :position 1}, :uranus {:color :blue, :position 7}, :mars {:color :red, :position 4}, :neptune {:color :blue, :position 8}, :jupiter {:color :orange, :position 5}, :earth {:color :blue, :position 3}, :venus {:color :yellow, :position 2}, :saturn {:color :yellow, :position 6}}
user> solar-system
#<Atom@6b37948d: {:mercury {:color :grey, :position 1}, :uranus {:color :blue, :position 7}, :mars {:color :red, :position 4}, :neptune {:color :blue, :position 8}, :jupiter {:color :orange, :position 5}, :earth {:color :blue, :position 3}, :venus {:color :yellow, :position 2}, :saturn {:color :yellow, :position 6}}>
user> @solar-system
{:mercury {:color :grey, :position 1}, :uranus {:color :blue, :position 7}, :mars {:color :red, :position 4}, :neptune {:color :blue, :position 8}, :jupiter {:color :orange, :position 5}, :earth {:color :blue, :position 3}, :venus {:color :yellow, :position 2}, :saturn {:color :yellow, :position 6}}
```


## lists


