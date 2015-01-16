
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
since we won't be able to return a constant string.  And since we'll
update the name and doc-string? to reflect the new functionality.

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

## map, fn

## atom, swap!, do

## lists


