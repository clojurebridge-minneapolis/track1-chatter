
## def

def is a way of defining variables.  The format is:

```clojure
(def symbol doc-string? init?)
```

1) The symbol ```def``` comes at the beginning of the expression.
2) symbol is the name you want to give the variable.
3) doc-string? is an optional description on what the variable is and how it is
meant to be used.
4) init? is an optional value the variable will be set to.  If unset, the value is unbound.

Clojure is a dynamic language; the variable has a value but does not
itself have a type.  The value has a type.

Variables must be def'd before they can be used.  As a convenience, it is legal to
def a variable, then def the variable to a different value, even a value of a completely
different type, but this is considered bad style.

After it has been def'd, a variable is visible throughout the file.
It's also possible to have access to the variable from other files but we have to
bring it into that namespace or use the variables full namespace name.

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

## defn

## maps

## maps

## map, fn

## atom, swap!, do

## lists


