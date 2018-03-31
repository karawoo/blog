---
layout:     post
title:      Adding arguments to existing R functions, or A Weirdly Esoteric Thing I'll Probably Never Do Again
date:       2018-03-31 01:00
output: 
  md_document:
    preserve_yaml: TRUE
summary:    If you've never wondered how to add arguments to functions after they're defined, well, that's probably for the best. But here's how to do it anyway.
comments:   True
published:  True
tags:       ['R']
categories: ['R']
---

Typically when writing an R function you include any arguments to the function,
and default values if applicable, in the definition like so:


{% highlight r %}
add <- function(x = 4, y = 1) {
  x + y
}

add()
{% endhighlight %}



{% highlight text %}
## [1] 5
{% endhighlight %}



{% highlight r %}
add(6, 2)
{% endhighlight %}



{% highlight text %}
## [1] 8
{% endhighlight %}

Until recently I had never considered how one might add arguments to a function
_after_ it's defined. Given a function like the one below, how would we add the
arguments `x` and `y`?


{% highlight r %}
add <- function() {
  x + y
}

add(6, 2)
{% endhighlight %}



{% highlight text %}
## Error in add(6, 2): unused arguments (6, 2)
{% endhighlight %}

You can reassign the formal arguments of a function using `formals()`, though as
the documentation notes "this is advanced, dangerous coding". I will take this
moment to note that re-defining functions in this way is probably not something
you generally should need to do very often. But for various reasons that are
outside the scope of this post, I did. Here's an example: we create a list of
argument names and default values, and assign it to `formals(add)`.


{% highlight r %}
add <- function() {
  x + y
}

args <- list(x = 4, y = 1)
formals(add) <- args

# View the result
add
{% endhighlight %}



{% highlight text %}
## function (x = 4, y = 1) 
## {
##     x + y
## }
## <environment: 0x7fcce5b22988>
{% endhighlight %}



{% highlight r %}
add()
{% endhighlight %}



{% highlight text %}
## [1] 5
{% endhighlight %}

`alist()` is a special function for creating lists of function arguments. Unlike
`list()` it allows empty arguments.


{% highlight r %}
alist(x = , y = 1)
{% endhighlight %}



{% highlight text %}
## $x
## 
## 
## $y
## [1] 1
{% endhighlight %}



{% highlight r %}
list(x = , y = 1)
{% endhighlight %}



{% highlight text %}
## Error in list(x = , y = 1): argument 1 is empty
{% endhighlight %}

`alist()` is useful for creating function arguments with no defaults.


{% highlight r %}
formals(add) <- alist(x = , y = 1)
add
{% endhighlight %}



{% highlight text %}
## function (x, y = 1) 
## {
##     x + y
## }
## <environment: 0x7fcce5b22988>
{% endhighlight %}


Again, this is a pretty peculiar way to define a function. My situation was a
bit more complicated though: I needed to be able to generate the list of
arguments given their names, and do this for many functions. Constructing the
arguments by hand with `alist()` was out, so how do we construct a list of
arguments given a list of their names? Of course we need some information about
whether the arguments should have default values or not. If the arguments all
have defaults, and we know what they are, then we can create a list of the
defaults, assign the argument names as the names of the list elements, and
assign that list to `formals(add)`. But what if we don't want the arguments to
have default values?

To understand how to do this requires a brief detour into symbols. Symbols in R
(also called names) represent the name of an object. I'm not going to go into
too much detail on symbols, but you should check out the
[chapter on Expressions](https://adv-r.hadley.nz/expressions.html#symbols) in Hadley
Wickham's Advanced R book if you want to learn more about them. The important
thing for this post is that there is a special symbol, the empty symbol, that is
a sort of weird void of [nothing](https://youtu.be/CrG-lsrXKRM?t=47s)\* that
represents missing arguments.

You can create an empty symbol in base R with the very weird looking expression
`quote(expr = )`, or using the `rlang` package with `missing_arg()`.


{% highlight r %}
# Note the nothingness that's printed -- if you run this in an R console you'll
# see a blank line
quote(expr = )
{% endhighlight %}




{% highlight r %}
rlang::missing_arg()
{% endhighlight %}

Empty symbols cannot be assigned to variables! 😱


{% highlight r %}
x <- rlang::missing_arg()
x
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): argument "x" is missing, with no default
{% endhighlight %}

However, you can store an empty symbol inside another data structure, like a
list.


{% highlight r %}
x <- list(rlang::missing_arg())
x
{% endhighlight %}



{% highlight text %}
## [[1]]
{% endhighlight %}

Back to the problem at hand: we need to create a list of arguments based on
their names, and store an empty symbol as the values. First we'll create an
empty list of `NULL`s, then we'll use `purrr::map()` to assign an empty symbol
to each element of the list.


{% highlight r %}
# We have the funcion sans defaults + the argument names
# separately
add <- function() {
  x + y
}

arg_names <- list("x", "y")

# Create a list to hold our arguments
args <- vector("list", length(arg_names))
names(args) <- arg_names
args
{% endhighlight %}



{% highlight text %}
## $x
## NULL
## 
## $y
## NULL
{% endhighlight %}



{% highlight r %}
# Replace NULLs with empty symbols
(args <- purrr::map(args, function(x) rlang::missing_arg()))
{% endhighlight %}



{% highlight text %}
## $x
## 
## 
## $y
{% endhighlight %}

Then we can assign this list to the formal arguments of the function and view
the function definition.


{% highlight r %}
formals(add) <- args
add
{% endhighlight %}



{% highlight text %}
## function (x, y) 
## {
##     x + y
## }
## <environment: 0x7fcce5b22988>
{% endhighlight %}

Now, what if our list needs to contain a mix of arguments with defaults and
arguments without? This is a bit more tricky. Let's say for this example we have
argument names and default values stored separately. There may be more names
than defaults, in which case we want to match the defaults to the names from the
last argument backward, with any other remaining names having no default.

I'll add a third argument, `z` to our `add()` function. `z` has a default value
of 1, and the other arguments have no defaults. First we create a list based on
the names of the arguments.


{% highlight r %}
add <- function() {
  x + y + z
}

# Argument names and defaults
arg_names <- list("x", "y", "z")
defaults <- list(1)

# Create a list to hold our arguments #
args <- vector("list", length(arg_names))
names(args) <- arg_names
args
{% endhighlight %}



{% highlight text %}
## $x
## NULL
## 
## $y
## NULL
## 
## $z
## NULL
{% endhighlight %}

Then add the defaults to the last elements of the list. This is a bit more code
than is strictly needed, but it will come in handy later:


{% highlight r %}
n_args <- length(args)
n_defs <- length(defaults)

# Position of the last default-less argument
last_empty <- n_args - n_defs

# Add the defaults to the end
args[(last_empty + 1):n_args] <- defaults
args
{% endhighlight %}



{% highlight text %}
## $x
## NULL
## 
## $y
## NULL
## 
## $z
## [1] 1
{% endhighlight %}

To assign some other value to the first two elements of the list, I would
normally do something like `x[1:2] <- "some_value"`, but this does not work with
empty symbols:


{% highlight r %}
# Try to add empty symbols up to the last_empty index
# of the list
args[seq_len(last_empty)] <- rlang::missing_arg()
{% endhighlight %}



{% highlight text %}
## Warning in args[seq_len(last_empty)] <- rlang::missing_arg(): (symbol)
## object cannot be coerced to type 'list'
{% endhighlight %}



{% highlight r %}
args # uh oh
{% endhighlight %}



{% highlight text %}
## $z
## [1] 1
{% endhighlight %}



The final solution ended up being rather more complicated than I'd hoped, and
involved a for loop where I didn't expect to need one. Here it is wrapped in its
own function:


{% highlight r %}
create_args <- function(arg_names, defaults) {
  # Start a list based on the argument names
  args <- vector("list", length(arg_names))
  names(args) <- arg_names

  if (length(defaults) == 0) {
    args <- purrr::map(args, function(x) rlang::missing_arg())
  } else {
    n_args <- length(arg_names)
    n_defs <- length(defaults)
    last_empty <- n_args - n_defs

    # Add the defaults to the end
    args[(last_empty + 1):n_args] <- defaults

    # Replace NULL with empty symbol for first arg(s) 
    # with no defaults
    for (i in seq_len(last_empty)) {
      args[[i]] <- rlang::missing_arg()
    }
  }
  args
}
{% endhighlight %}

Now we can use `create_args()` to generate arguments and assign them to
`formals(add)`.


{% highlight r %}
# Use the function
formals(add) <- create_args(arg_names, defaults)
add
{% endhighlight %}



{% highlight text %}
## function (x, y, z = 1) 
## {
##     x + y + z
## }
## <environment: 0x7fcce5b22988>
{% endhighlight %}

So yeah...this was not exactly pretty, and again you probably don't want to do
it in 99% of scenarios, but it does seem to work. If anyone has ideas about how
to make it cleaner, I'd love to hear!

---

\* Without the expanding-to-destroy-all-everything-in-its-path part.
