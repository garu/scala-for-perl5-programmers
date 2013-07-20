Scala for Perl 5 Programmers
============================

So, you're a Perl hacker dipping your toes into Scala. Maybe you want to contribute to [Moe]. Maybe you want to improve your Perl programming skills by stepping out of your comfort zone. Maybe you're doing it just for kicks, or as a second language, or... well, it doesn't really matter.

When I decided to learn a bit of Scala, I was devastated to find that most tutorials are for people coming from Java. Well, I'm not really a Java person, so I decided to write a tutorial for people with mostly a Perl 5 background. As I was learning (and writing), I realized Scala feels *very* Perlish, with anonymous methods, more than one way to do things, weird operators and even a context variable! You'll feel right at home soon enough :)

Now, I'm not going to try and teach you how to program, nor go through basic programming knowledge. Instead, I'm going to draw several parallels between Perl 5 and Scala 2.10.0 to get you on your feet as soon as possible.

I also made sure to add a lot of Perl names and references throughout this tutorial, so you can search it later on for specific keywords, like "perldoc", "eval", "Test::More", etc.

Finally, please bare in mind I'm still learning Scala and it's a pretty big language, so there are likely some errors and misconceptions on my part around here. If you happen to stumble across any, please let me know so I can update this guide and learn too :)


Alright, let's get started!


Setting Up Your Environment
---------------------------

I'm not gonna go through setting up an IDE like Eclipse, NetBeans or whatever. It may boost your productivity in the future, but you're a Perl hacker so you likely already have a favorite text editor. Stick to it :)

On Windows? Install the [Scala 2.10.0 MSI] and move to the next session.

On Linux or Mac OS X? Read on:

  1. Download the [Scala 2.10.0 tgz] and unpack it. Find "bin/scala" under the unpacked directory and run it. If you see a "scala>" prompt, yay! Now type ":q" (without the quotes) to quit that terminal and move to the next session. Got some weird Java error? Read on:

  2. Scala runs on the JVM, and we need to install both JRE and JDK. JRE is the runtime environment, and JDK is the development kit (which includes the JRE so don't worry). Go to the [JavaSE website], download and install the JDK, and go back to step 1.


Running your Scala programs
---------------------------

Save the text file as 'app.scala' and run it through 'bin/scala' program you just installed (can be just 'scala' or a full path, depending on which directory you are and whether or not you set your PATH environment variable accordingly):

    bin/scala app.scala

Not many Perl developers are into REPLs, but just in case you're one of the few, running `bin/scala` without arguments runs Scala in REPL mode, like Perl's Devel::REPL or Reply, letting you quickly experiment with code. Take it for a spin!


Basic Syntax
------------

### Comments

You're new to Scala, so you likely want to add several comments in your code. Here's how:

```scala
// this is a single line comment, like Perl's "#"

/* and this
   is for multiline comments */
```

Note, however, that multiline comments need to be set precisely with **/\*** and **\*/** (unlike C, you can't write something like **/\*\*\*\*\*\***) and, if nested, comment tokens cannot be left unbalanced. In other words:

```scala
/**** this gives a compile error ****/

/* **** this works fine *** */

/* this also goes kaboom because of /* <== this, which
   should be balanced within the comment somewhere */
```

### Basic Output

```scala
print( "Scala <3 Perl\n" )   // like 'print' in Perl

println( "Scala <3 Perl" )   // like 'say' in Perl
```

Enough said for now. Oh, and sorry, you need to keep the parenthesis :-/

### Variables

Unlike Perl, Scala is a statically typed language, so all variables and functions have their types fully defined at compile time. Available types for your "scalars" are:


  * **String** - a sequence of Unicode characters (BMP charset)
  * **Char** - 16-bit unsigned Unicode character
  * **Int** - 32-bit signed 2's complement integer (-2,147,483,648 to 2,147,483,647 inclusive)
  * **Short** - 16-bit signed 2's complement integer (-32,768 to 32,767 inclusive)
  * **Long** - 64-bit signed 2's complement integer (-2^63 to 2^63-1, inclusive)
  * **Float** - 32-bit IEEE 754 single-precision float
  * **Double** - 64-bit IEEE 754 double-precision float
  * **Byte** - 8-bit signed 2's complement integer (-128 to 127 inclusive)
  * **Boolean** - *true* or *false* (yup, you type them as barewords, just like that)

When you start off, you will probably only care about *Int* for numbers, *String* for strings and characters, *Double* if you're doing decimals and *Boolean* for true/false values.

You declare variables like so:

```scala
var foo: String = "42"
var bar: Int = 42
var isSomething = true
val immutableVar: Double = 3.14 + bar
```

There are several thing to notice here:

  * **no semicolon separating statements!** Yup, Scala uses *newline* or { } blocks do separate statements. Breathe. Seriously, I can feel you fighting your inner Perler right now. Don't worry, this is still not Python, and spaces are still irrelevant. Just breathe for a while, it's gonna be ok :-)

  * Number notation is very much like Perl's. This means *42* is an integer, *3.14* is a real (you can use 1.23e-4 syntax too), *0644* is octal, *0x3f* (or *0x3F*) is hexadecimal. Sorry, no "0b" notation in Scala, but did you ever use it in Perl?

  * When giving types to variables, use the "name: Type" convention. That's *varname-colon-space-typename*. If you never played with a statically typed language before, you may be tempted to think of variable types as scalar attributes, like "my $foo :String", except Scala would check these at compile time. If it helps to think like that, it's fine, just don't put the colon next to the "attribute", as you (likely) would in Perl.

  * **You don't *need* to specify the type** (at least not usually). This means most of the time you can simply write "var Foo = 42" and Scala will figure its type for you, kinda like Perl does. *Don't be fooled by this, though!* Scala is statically typed and **will** complain at compile time if you try and assign a different type to the same variable later on. It is recommended that you do **NOT** specify types for variables unless you really have to (e.g. the compiler complains).

  * You may have noticed, the last statement uses **val** instead of **var**. That's not a typo, Scala lets you use **var** for variables that will be assigned to new values, and **val** for values that won't change. Note that I'm referring only to actual "=" assignments, not changes within an object. As a Perler, you may be tempted to think of **val** as a constant or readonly variable, but that's not exactly the case (like I said, objects can change internally, and in Scala *everything is an object*). It is, in fact, recommended that you **use *val* for all your variables**, unless you really really *really* need to use var. See [here][1] for a more comprehensive discussion on *var* x *val*.

  * **val**ues and **var**iables are named in *camelCase*, with the first letter lowercase. No, you can't use underscores (\_), they have a special meaning in Scala (it's somewhat similar to Perl's $_ variable, just read on).

Declaring multiple variables on the same line works exactly like Perl:

```scala
val (x, y, z) = (1, 3.14, "foo")
```

**Tip:** If you need to separate a statement into several lines, just make sure to finish each line with a token that requires another argument. For example.

```scala
val (x, y, z) =
      (1, 3.14, "foo")

var fibonacci: Int = 0 + 1 + 1 + 2 +
  3 + 5 + 8 + 13 +
  21 + 34
```

This will avoid weird compile-time errors while still letting you tidy your code.

### Manipulating variables

    + - / * % work as expected
    logical operators &, |, ! and ^ (xor) also work
    <<  shift bits to the left
    >>  shift bits to the right preserving sign
    >>> shift right and zero left bits

**Note:** As mentioned before, in Scala *everything is an object*, so even though it may look like independent tokens and functions to a Perl developer, all manipulation is in reality calling a method for you. Thankfully, Scala is clever enough to realize what you mean just so you don't have to add parentheses and dots everywhere (yeah, it uses "." for method calling, not "->"). In other words, when you write something like:

```scala
foo + bar - baz
```

You're actually doing:

```scala
foo.+(bar.-(baz))
```

And this is true not just for variables, but for raw numbers and strings as well. For example, when you write:

```scala
2 + 1
```

You're actually doing this:

```scala
(2).+(1)
```

Note that you would need to write *(2).* instead of *2.* because *2.* is type Double, not type Int, so we must remove the ambiguity.

Aside from the plain arithmetic and bitwise operators mentioned above, numbers have the following methods to help you interoperate data from variables of different types:

    .toByte
    .toChar
    .toDouble
    .toFloat
    .toInt
    .toLong
    .toShort
    .toString

So you can have something like:

```scala
val x = 3.14
val y = x.toInt + 9
```

... and *y* will be initialized as 12 (Int) instead of 12.14 (Double).


Strings have several helper methods to let you manipulate them. The ones that you're more likely to be interested right now are:

    .length                      // string size, in characters.
    .isEmpty                     // true if string has 0 length.

    .reverse()                   // like Perl's "reverse".
    .trim()                      // removes whitespace characters from left or right.
    .split("regex")              // like Perl 5's "split" (can even set a limit as a second arg).
    .concat("append me")         // concatenates new string to the end of original string.
    .toLowerCase()               // like Perl's "lc".
    .toUpperCase()               // like Perl's "uc".

    .charAt(2)                   // returns the character at the specified index.
    .indexOf("subString")        // similar to "index" in Perl 5.
    .contains("subString")       // similar to "index >= 0" in Perl 5.
    .substring(begin, end)       // like Perl 5's "substr", but uses start/end indices.
                                 // rather than index + size. Doesn't let you replace though.

    .equals("otherString")       // similar to "eq" in Perl 5.
    .equalsIgnoreCase("...")     // same as above but ignoring case. Similar to "eq fc" in Perl 5.
    .compareTo("otherString")    // similar to "cmp" in Perl 5 (Unicode aware).
    .compareToIgnoreCase("...")  // same but ignoring case.
    .startsWith("subString")     // true if string starts with given "subString".
    .endsWith("subString")       // true if string ends with given "subString".

Sorry, I couldn't find ready-to-use "chop", "chomp", "chr", "crypt", "hex", "lcfirst", "ucfirst", "oct", "ord", "rindex" or "sprintf" - at least not for plain String objects anyway. Let me know if there are any so I can update this!

Side-note: I found it very interesting how Scala (or, in this case, Java itself) implemented split() just like Perl 5. They even let (in fact, demand) you write the regex as a string and not as a regex object.


#### Heredocs and Quote Operators

As we saw earlier, newlines separate statements in Scala. But, in this case, how would you create a multiline string?

Scala provides a rudimentary form of heredocs for this: just enclose your strings with three sequential quotes:

```scala
val longString = """This is my long string.
  it includes any "newlines" and there's no
  need for me to escape special characters
  like " and \. See?"""
```

This syntax also works a bit like Perl's quote operators, since you don't have to escape any characters inside.


### Arrays

```scala
val myStuff = Array(-42, "foo bar", 3.1415926)

myStuff.length         // total size of array
myStuff(0) = "meep!"   // replaces 1st element
val pi = myStuff.last  // there's no myStuff(-1)
val reversed = myStuff.reverse
```

Notice, in the example above, that we created our array using **val**, and yet we were able to change its elements. Like I said before, val/var refers to the container itself, not its contents. What this means is that I can change the elements, but not the size of this array. In Scala, you're advised to keep your data nice and separate and reuse your data structures as little as possible in order to make your intentions clearer. If, however, you want/need to change arrays dynamically, just use **var** and you're all set:

```scala
var data = Array(1, 2, "Fizz", 4, "Buzz")
```

In Perl there's a clear separation between lists and arrays. Scala does that too, and you can even store lists for later reuse:

```scala
val myList = List(1, 2, 3)
```

Those behave just like Array objects, except the contents are also immutable.


Let's do some array manipulation

```scala
var data = Array(1, 2, "Fizz", 4, "Buzz")

// use '++' to concatenates arrays
val bigger = data ++ Array("Fizz", 7, 8, "Fizz")
data ++= Array("Fizz", 7, 8, "Fizz") // like Perl's push or unshift
```

For just one element, you can also:

```scala
val bigger = data :+ "w00t!"  // adds to the end of the array
data :+= "w00t!"              // same, but does it in-place

val bigger = data +: "w00t!"  // adds to the beginning of the array
data +:= "w00t!"              // same, but does it in-place
```

How would you define an array without initializing it?

```scala
val emptyArray = new Array[Any](3)  // (null, null, null)
```

Note the **new** keyword, and the *[Type]\(size\)* syntax. Type "Any" means each slot in the array may hold any type of value. Of course, if you already know the type of data it will hold, it's probably best to constrain it to that. Finally, remember *val* arrays do not change sizes, but *var* arrays can increase or decrease at will.

Oh! You know how Perl has range operators that let you create lists like:

```perl
my @range = 1..10;
```

Well, in Scala you can do the same thing!

```scala
val range = 1 to 10
```

Scala also offers easy ways to *map()* and *grep()*:

```scala
val data = Array(1, 4, 9, 7, 3, 7, 6, 4, 42)

val double = data.map { _ * 2 }      // (2, 8, 18, 13, ...)

val bigger = data.filter { _ > 6 }   // (9, 7, 7, 42)
```

These methods expect functions and, even though you can write them as above (which should look very familiar to a Perler), simple statements like that are idiomatically written using parenthesis instead of curly brackets, like so:

```scala
val double = data.map( _ * 2)
val bigger = data.filter( _ > 6 )
```

You'll also often see *map()* and friends implemented in a more functional manner, kinda like a for/foreach in Perl (check below for more on loops):

```scala
val double = data.map( x => x * 2 )   // same as .map( _ * 2 ) above
```

Meaning "for each element *x* in *data*, return `x * 2`". You can name *x* anything you like as long as it doesn't collide with a function name or reserved word (if you try naming it "val", for example, you'll get a lot of weird compilation errors), so make sure it's a meaningful name.

One thing, though: since Scala uses objects for everything, you can't make *map()* return more than one element like you can in Perl. This means that if you write something like:

```scala
val data = List(1, 13, 42)
val moarElements = data.map { x => List( x-1, x, x+1 ) }
```

then *moarElements* will contain just 3 elements - one list object for each item in *data* - and not 9 elements as you could originally expect:

```scala
// moarElements: List( List(0, 1, 2), List(12, 13, 14), List(41, 42, 43) )
```

To get a flattened list, all you have to do is use the *flatMap()* method instead of just *map()*:

```scala
val moarElements = data.flatMap { x => List( x-1, x, x+1 ) }

// moarElements: List(0, 1, 2, 12, 13, 14, 41, 42, 43)
```

#### Useful methods for array manipulation

Now, if you do arrays in Perl, you're probably looking for ways to replicate all the cool stuff that you can do with List::AllUtils. Here's a compiled cheatsheet with everything I could find (let me know if you have updates!)

    Perl 5      ## Scala
    --------------------
    scalar @a   ## .size
    map         ## .map { condition }
    grep        ## .filter { condition }
    join        ## .mkString("separator")
    reverse     ## .reverse()
    sort        ## .sort()    // List() objects only, not Array()

    pop         ## .takeRight(1) gets the element, .dropRight(1) reduces the list
    push        ## :+ (for one element) and ++ (for another list)
    shift       ## .take(1) gets the element, .drop(1) reduces the list
    unshift     ## +: (for one element) and :: (for another list)
    splice      ## .slice(index1, index2) (see also: .patch() for replacing the slice data)

    # List::Util
    first { $_ > 5 }   ## .find { _ > 5 }
    max                ## .max
    maxstr             ## .max
    min                ## .min
    minstr             ## .min
    reduce { $a + $b } ## .reduce { _ + _ }
    shuffle @list      ## scala.util.Random.shuffle(list)
    sum                ## .sum

    # List::MoreUtils
    any { $_ < 3 }         ## .exists { _ < 3 }
    all { $_ > 0 }         ## .forall { _ > 0 }
    none                   ## ??? (can be implemented with "! .exists")
    notall                 ## ??? (can be implemented with "! .forall")
    true { $_ < 3 }        ## .count { $_ < 3 }
    false                  ## ??? (can be implemented negating the condition in .count)

    firstidx/first_index   ## .indexWhere { _ > 0 }  (also returns -1 if not found)
    lastidx/last_index     ## .lastIndexWhere { _ > 0 } (same deal as above)

    insert_after           ## ???
    insert_after_string    ## ???
    apply                  ## .map
    indexes                ## ??? (no indicesWhere yet...)
    after                  ## ???
    after_incl             ## ???
    before                 ## ???
    before_incl            ## ???

    firstval/first_value      ## .find { _ > 5 }
    lastval/last_value        ## ???
    each_array/each_arrayref  ## ???
    pairwise                  ## ??? (might be done with .zip and another method)
    natatime                  ## .grouped(n).foreach  // each group is a Vector object
    mesh/zip                  ## .zip (for 2 lists. Keep chaining .zip calls for extra lists)
    uniq/distinct             ## .distinct()
    minmax                    ## ???
    part                      ## .partition { cond }  // but works a bit differently

There's also an efficient way to check if the list/array contains a given element: ```.contains( element )``` usually done in Perl 5 with an any() that looks for the element explicitly. Another cool method is ```list.diff(anotherList)``` which is similar to distinct() but returns a list of elements in the original array which are not present in "anotherList".

As shown above, some methods might are exclusive of one type and thus not available to others. Scala statically typed nature encourages developers to seek the object that most closely relates to whatever it is they want to do with the data. In fact, Scala provides a gigantic amount of classes and trait for that very reason. For example, if you have a stack, you're encouraged to use "scala.collection.immutable.Stack()" objects - that provide efficient push() and pop() methods - instead of plain Array() or List() objects. Whenever your code requires a collection of data, check out [this nice description](http://docs.scala-lang.org/overviews/collections/concrete-immutable-collection-classes.html) of the main collection classes available to you.

One last thing: as you dip your toes into Scala, you'll soon realize there is not only many specific classes for handling your data, but also that each class provides a huge amount of methods and method variations. For example, in Perl 5 we use Scalar::Util's reduce(), and if we wanted to apply it from right to left we'd just write "reduce {...} reverse @list". Scala rather provide you with a "reduceLeft" method (and several others in fact). Whenever you catch yourself adding a lot of logic to your data mangling in Scala, make sure to check the [Scala official API reference] to make sure there isn't a specific method to help you make things a bit simpler and more efficient.

Are you a performance freak? With so many classes available for you to pick, which suits your project best? Check out this [cool performance table] to see a feature/performance comparison between the major Scala collections available.

### Hashes


In Scala, hashes are collections called [Maps].

```scala
val hash = Map(
  "name" -> "foo",
  42     -> -3
)
```

Unlike Perl, the separator is '->' and not '=>'. Also, remember in Scala you can't finish the key/value pair with a comma or you'll get a compile error. Yeah, it sucks. I know :/

The big difference you'll notice, however, is that in Scala your keys can be **anything**, not just strings. In fact, you can have actual *variables* as keys to your hash-...um...Map. Even though you might not find it very useful at first, it might still bite you as you're likely used to having barewords as keys. Just remember that if you do something like this:

```scala
val hash = Map(foo -> "bar")
```

you're actually saying that the key to "bar" is a *variable* called foo, not the string "foo".

Adding elements to a Map is easy:

```scala
val newHash = myHash + (anotherkey -> "another value")
```

You can also easily join two Maps:

```scala
val joinedHash = myHash ++ otherHash
```

in this case, just like when you do it in Perl, any duplicate keys will point to the latest value.

Deleting keys is also pretty simple:

```scala
myHash - "someKey"
```

There's also a cool thing: we often want to fetch values from Maps or set a default. In Perl, you could
do something like this:

```perl
my $param = $hash{'param'} // 'default value';
```

you can do this in Scala like so:

```scala
val param = hash getOrElse ("param", "default value")
```

You can also set a default value for non-existing keys in your entire Map variable!

```scala
val hash = Map( "answer" -> 42 ).withDefault( x => "sorry, dude." )

hash("answer")        // 42
hash("someOtherKey")  // "sorry, dude."
hash("meep")          // "sorry, dude."
```

The '.keys' and '.values' methods are of course available for Scala Maps, just as you'd expect for Perl hashes. For reference, here are the basic equivalences to manipulate hashes/Maps between Perl 5 and Scala:

```scala
// in Scala (variable is 'hash'):         // in Perl 5 (variable is %hash):

val data = hash get "somekey"          // my $data = $hash{somekey}
val data = hash("somekey")             // my $data = $hash{somekey} // die;
val data = hash getOrElse ("k", 42)    // my $data = $hash{k} // 42;

hash.keys                              // keys %hash
hash.size                              // scalar keys %hash
hash.values                            // values %hash
hash contains "somekey"                // exists $hash{somekey}

val otherHash = hash + ("k" -> 1)      // my %other_hash = (%hash, k => 1)
val newHash = hash ++ otherHash        // my %new_hash = (%hash, %other_hash)

/* in the code below, hash must be 'var', not 'val',
 * otherwise you must assign to another variable
 */
hash += ("k1" -> 1, "k2" -> 1)         // @hash{'k1', 'k2'} = (1, 1)
hash -= "somekey"                      // delete $hash{somekey}

/* code below only available
 * in *mutable* hashes
 */
hash("k") = 1                          // $hash{k} = 1
val data = hash remove "somekey"       // my $data = delete $hash{somekey}
```

There are a *lot* of methods in [Maps], so you're advised to check the API docs before manipulating them manually, as there is likely a native method that either does what you need or can greatly help (and simplify/speed up) your code.

Note that some operations listed above (like "remove") are only available in mutable Map objects. To get them, just add the following line to your code:

```scala
import scala.collection.mutable.Map
```

Or use the fully qualified ```collection.mutable.Map()``` when creating your mutable Maps. Be advised though that Scala's best practices strongly recommend you *avoid reusing a variable as much as you can*. And yes, this also means mangling with keys on a Map. Changing Map variables from immutable to mutable - and even using 'var' instead of 'val' is considered by many as code smell unless you really have to. So always consider using new 'val' variables whenever you need to change a Map object.

Finally, it might be worth noticing that, similar to Perl 5 hashes, Maps in Scala can be created/manipulated somewhat like regular arrays. This means that you can create a Map like so:

```scala
val hash = Map( ("foo", "bar"), ("meep", "moop") )

hash contains "meep"  // true
```

Keep in mind that, if you use *list syntax*, then each element of the Map needs to be a pair of values in parentheses. Scala will convert each of those into a "key -> value" format.

Oh, and one final tip: if you have an array of keys and an array of values, you can turn them into a single variable like so:

```scala
val hash = arrayOfKeys.zip(arrayOfValues).toMap
```

### Complex Data Structures

Since everything is a typed object, you can think of them as Perl references and mix and match them at will to create complex data structures, like matrices, hashes of lists, lists of hashes of lists, and so on. For example:

```scala
val data = List(1, 2, Map( 3 -> List("foo", "bar"), 4 -> 5), 3, 4)
```

### Basic I/O

All I/O processing is done by the bundled [Console] object. Check it out for a full list of functions. You already know about print() and println(). But how to you read data from STDIN?

```scala
val answer = readLine
```

You can even bundle your question with format strings, printf() style:

```scala
val name = readLine("What's your name?")
val age = readLine( "And how old are you, %s?", name )
```

The Console object provides read* methods to read not just lines, but any kind of input into any kind of variable, such as readLong(), readChar(), even readByte().

You can also colorize your output, as you would with Term::ANSIColor's exported colors in Perl:

```scala
print( Console.RED + "DANGER!" + Console.WHITE )
```

Check out [Console]'s documentation for a full list of colors and methods.


### Conditionals

```scala
// use the good old operators: &&, ||, <, >, >=, <=, ==, != and ()
// string operators are inside the string objects as seen above
if (foo > 42) {
  println("yay!")
}
```

Scala lets single-statement "if" clauses to be written without curly-braces, but whenever there is no "else" (like the example above) it is considered good style to always add the curly-braces (like Perl 5, except in Perl 5 the curly braces are mandatory).

On the other hand, whenever there **is** an 'else' statement and they are single-line expressions, you should omit the curly braces for good style (it's not mandatory by the parser, of course):

```scala
if (foo > 42)
  println("yay!")
else
  println("nay...")
```

Couple of things to notice here:

  * Indenting style says **two** whitespace characters for each level.
  * It is considered good style to *add a single space after flow-control keywords* like "if", "else", "for" and "while".
  * Sorry, no "unless" and no post-conditionals in Scala.
  * Also, no "elsif". You have to nest your second "if" inside the "else", or use the *case* keyword (see below)

There is no ternary conditional in Scala. Instead, the 'if' always returns the last evaluated value so you are encouraged to write extremely brief conditionals all in the same line, like so:

```scala
val res = if (foo) bar else baz
```

As mentioned above, Scala also has a given/when operator, used within calls to the *match* method:

```scala
val foo = 42

foo match {
  case 2  => println("perfect number")
  case 3  => println("crow")
  case 42 => println("The Answer")
  case _  => println("the rest")
}
```

Good style says you should *NOT* use curly braces in a 'case' statement unless the expression does not fit a single line. Also, the _ (underscore) character is a wildcard in Scala, and in this context matches anything.

Oh, one more thing: since Scala is statically typed, there is no 'eq' (or 'ne', 'gt', etc. for that matter). When you write something like `someVar == otherVar`, it will first check the type, then the value. So `100 > 99` is true but `"100" > "99"` is false and they both work. But if you try`"100" > 99`, Scala won't DWIM like Perl and you'll get a type mismatch error. To fix this, you must explicitly convert one of the types to the other with `.toString()` and friends.

### Loops, in a nutshell

There are tons of methods available in Scala to loop through elements. Let's go through a few of the most popular ones, starting with our good friend *while()*, whose syntax you're already very familiar with:

```scala
while (true) {
  println( "Scala <3 Perl!" )
}
```

For good style, braces should never be omitted in *while* loops, even though they can be for single statements.

Scala also lets you take advantage of do-while loops:

```scala
do {
  println( "Perl <3 Scala!" )
} while (true)
```

This behaves just like Perl's do-while construct, except there is no standalone *do()* function - it must always precede a *while()*.

The *foreach()* method is also available, and let you access the current element via the ```_``` variable (sounds familiar?)

```scala
val numbers = List(1, 13, 42)
val sum = 0

numbers.foreach {
  sum += _
  println      // same as println(_)
}
```

What about *for()*?

```
for (i <- 1 to 3) {
  println(i)
}
```

if you're not using number sequences, just group them in a regular List:

```scala
for (i <- List("answer", 42, "meep")) {
  println(i)
}
```

Now, there are several cool things you can do with a *for* loop in Scala. For instance, the "A to B" range operator can be changed to "until" if you don't want the last element of the list to be included:

```scala
for (i <- 1 until 4) {    // 1, 2, 3
  ...
}
```

You can also iterate through any collection, including strings (which are character collections):

```scala
for (c <- "Perl") {
  println(c)
}
```

Not good enough? How about adding conditionals to your loops so you only get the elements that fit a criteria?

```scala
val languages = List("Perl", "Scala", "Ruby", "Python")

for (name <- languages if name.startsWith("P")) {
  println(name)
}
```

But, to me, the *coolest* thing is how easy Scala makes it to walk through multidimensional iterations: just separate each range with a semicolon!

```scala
for (i <- 1 to 10; j <- 0 to 3) {
  println(i, j)
}
```

Pretty cool, huh? No more deeply nested loops if you can help it!

As you could see throughout this section, there are several ways to iterate over lists. More than often (and you know this from Perl itself) you could achieve the same results of a *for* loop with a more idiomatic method call on the target list itself, like foreach(), map(), flatMap(), filter() or withFilter(). In fact, the *for* construct in Scala is nothing but syntatic sugar that the compiler translates into calls to those methods!

Another idiomatic way to express *for* loops, for instance, uses the `yield()` idiom. Now, english is not my native tongue so I always had trouble with this one. If you have a Ruby background, note that Scala's yield() behaves differently, so read on. "To yield" in this context means "to give something (to someone else)", and you always see it in combination with *for* loops:

```scala
val planets = List("earth", "mars", "jupiter")
val upper = for (name <- planets) yield name.capitalize
```

And now *upper* is a List containing "Earth", "Mars" and "Jupiter".

Best Practice: if you choose to use *yield()* in a *for*-comprehension with more than one generator, good style dictates that you use curly braces instead of parenthesis, and put one generator per line, avoiding the ";" between generators and the extra parenthesis for the function block:

```scala
for {
  x <- 1 to 10
  y <- 0 to 3
} yield (x, y)
```

Now, why should you use *yield* instead of *map* or something else? Good style suggests that, since it's syntatic sugar, *for* comprehensions should be preferred over chained calls to map, filter, etc., as those can get difficult to read and it's one of the main reasons *for* comprehensions exist in the first place. In practice, you should just use whatever syntax makes you feel better and your code more readable. TIMTOWTDI :)

One final note about loops: in Scala, it is expected that you do **not** break from loops, so you won't find things like *next*, *break* nor *continue*. If you really need them - hint: you don't - you can try importing [scala.util.control.Breaks] (read on for how to "use" modules).


### File I/O

The [io.Source] class is going to be your best friend:

```scala
val fh = io.Source.fromFile("filename") // not really a handle but relates well
val lineIterator = fh.getLines
```

The "lineIterator" variable can be used in a for() loop:

```scala
for (line <- lineIterator) {
  println(line)
}
```

And you can also access its contents directly:

```scala
val lines = lineIterator.toList
val contentString = lineIterator.mkString
```

Of course, no one really goes through all that trouble with temporary variables. Instead, you just do something like:

```scala
for (line <- io.Source.fromFile("filename").getLines) {
  println(line)
}
```

Or even:

```scala
io.Source.fromFile("filename").getLines.toList.foreach(println)
```

Same goes if you want to quickly retrieve a file's content:

```scala
val lines = io.Source.fromFile("filename").getLines.toList
val contentString = io.Source.fromFile("filename").getLines.mkString
```

Oh, one cool thing about [io.Source]: it doesn't read just from files!

```scala
// native web crawlers FTW!
val dom = io.Source.fromURL("http://www.perl.org").getLines.mkString
```

#### How about writing to files?

I... I... You know what? You got me. In Perl this is *so* easy, and yet in Scala it... doesn't exist. I honestly don't know why, and I still hope I just overlooked some documentation (did I? Send me a patch!), but I really checked and even though Scala makes it easy to read files, even the latest version of Scala still doesn't support writing. Ugh.

Not all is lost, though. Since it runs under the JVM and lets you use Java classes seamlessly, we just use `java.io.FileWriter` and off we go:

```scala
val out = new java.io.FileWriter("some_filename")
out.write("hello, world!")
out.close
```

This is a great example on how to take advantage of Java's libraries from Scala. But yeah, I much rather see Scala's own version :-/


### Invoking External Programs

In Perl you're used to calling qx(), \`\` or system(). For extra points, you might use something like the IPC::Run\* family of modules or Capture::Tiny. In Scala, external process execution is done via [scala.sys.process]. This trait incorporates 3 main modules in string objects: `.!`, `.!!` and `.lines`:

```scala
import scala.sys.process._

// .! returns the exit code of the process
val retCode = "ls".!

/* .!! returns the output (stdout only) of the
   process instead of printing it on the screen */
val output = "ls".!!

// .list returns the output, separated by lines
for (line <- "ls".lines) {
  println(line)
}
```

A clearer (or less noisy) solution is also available from `scala.sys.process`:

```scala
val cmd = Process("ls")
cmd.run
```

As with Perl, passing program + arguments as a single string in Scala is accepted, but not recommended for security reasons - specially when one of the arguments comes from user input. Instead, whenever you need to pass arguments, you should use a List() - or a Seq(), which is just a simpler version of a list:

```scala
"ls -l".!           // ok
Seq("ls", "-l").!   // better!
```

Check out [scala.sys.process] and [ProcessBuilder](http://www.scala-lang.org/api/current/index.html#scala.sys.process.ProcessBuilder) for extra information, including how to combine processes via pipes, redirect input/output, and a lot more.


### Parsing Command Line Arguments

Perl's `@ARGV` is Scala's `args()`, so you get `$ARGV[0]` as `args(0)` and so on.

If you want extra control over your program's command line arguments, like Getopt::Long does, try packages like [scopt](http://github.com/scopt/scopt) and [scallop](https://github.com/Rogach/scallop).

### Eval and Exception Handling

If you're familiar with Try::Tiny (and you should!) or TryCatch, then Scala's exception handling should be a no-brainer:

```scala
try {
  // some code
} catch {
  // here the special "_" variable contains the thrown exception
} finally {
  // if you have this block, it's always called
}
```

To throw an exception in Perl you'd use die() or croak(). In Scala, you use the *throw*. Unlike Perl, however, you can't just throw anything you want, be it an object, a string or even a function. In Scala everything is an object, but *throw* expects you to throw a 'Throwable' object, which helps expose details on whatever error you faced. The most common one is the 'Exception' object, which you can construct with a string describing your error:

```scala
throw new Exception("I can't let you do that, Dave.")
```

Throwable objects contain some useful methods like toString(), getMessage() and printStackTrace() that you can use to debug your program.

In Scala, you are expected to create different exception classes for every different error you app may face, and treat them accordingly in your try/catch block. Remember our *case* notation? It's time to point out that *case* also lets you take advantage of Scala's static typing to dispatch our data according to its type:

```scala
try {
  throw new Exception("I can't let you do that, Dave.")
  println("this line is never executed")
} catch {
  case _: IOException => doSomething
  case error: FileNotFoundException => doSomethingElse(error)
  case a: AssertionError => doYetAnotherThing(a)
  case unknown => println("Unkown error: " + unknown)
}
```

Now, if the thrown object is an IOException, *doSomething* is going to be called. If, on the other hand, it's a FileNotFoundException, then *doSomethingElse* will be called instead, with the error object passed as argument. And so on.

Note that in every *case* check I use a different variable name. This is not really recommended, I did it just to show you that it doesn't matter, you're just labeling "_" for easier reference. You should probably name them all "error", "exception" or just "e", and name the last one "default" or "unknown" - since we don't know its type and are treating it as a generic error.

As mentioned earlier, you are expected to create different exception classes for every different error in your app. Fortunately for us, there are pre-built classes for some common errors inherited from Java, like Exception, Error, RuntimeException, AssertionError, IllegalArgumentException, and many more. Check out Java's [Exception] class for a list of "direct known subclasses". Notice, however, that if you use an exception that doesn't come from the "java.lang" namespace, you must reference it through the full namespace. For example:

```scala
throw new java.security.GeneralSecurityException("move along, now!")
throw new java.io.IOException("error writing to file")
```

### Subroutin... erm... functions!

Instead of "sub", you'll use **def** to create functions in Scala:

```scala
def myCoolFunction {
   println("coolness!")
}
```

You call functions by name, WITHOUT PARENTHESES:

```scala
myCoolFuncion   // prints "coolness!"
```

Note that this function just prints something on the screen, it doesn't return anything. This means the function is of the *Unit* type, and really cannot return anything - otherwise you'll trigger a compile-time error. If you want to return something, you must specify the type during declaration and use the "=" sign before starting the block:

```scala
def randomInteger: Int = {
  return scala.util.Random.nextInt
}

println( randomInteger )  // prints a random integer!
```

Like Perl, the 'return' keyword is optional. Scala will return the last evaluated expression in your function.

The "=" sign before the start of the block is an important visual hint in Scala that this function will return something. You can also use the "=" for functions that do not return anything (like 'myCoolFunction' above), but the best practices strongly recommend you use "=" only if your function/method is actually returning something. This is particularly cool for simple functions like the one above, which can be written in a single line without the cruft of curly brackets, like so:

```scala
def randomInteger: Int = scala.util.Random.nextInt
```

Also, just like plain variables, if Scala can infer the return type during compile time you don't need to make it explicit.In fact, also like variables, it is considered good style if you omit the return type whenever possible:

```scala
def randomInteger = scala.util.Random.nextInt
```

What about passing arguments? Well, in Scala you can't unfold @_ like you do in Perl. Being statically typed means it must know during compile-time the number of elements the function is expecting, and the type of each one, via a function signature:

```scala
def add(a: Int, b: Int) = a + b

println( add(3, 4) )  // prints '7'
```

You can also set default values:

```scala
def user(age: Int = 18): Unit {
  // do something
}
```

It is, of course, ok to make recursive calls to your function (but recursive functions always need a result type):

```scala
def gcd(x: Long, y: Long): Long =
  if (y == 0) x else gcd(y, x % y)
```

There is a LOT of cool stuff you can do with functions in Scala using special sugary syntax (like higher-order and function values), but this should be more than enough to get you going.


Regular Expressions
-------------------

Yeah, I know, nothing really beats Perl's regexes, so don't expect to find zero-width negative look-behind assertions or code execution in Scala regexes. But think of them as simple-case regexes, the 20% of features required to fit 80% of all your regex needs.

You create regexes either via "new Regex( stringWithPattern )" or with the ".r" method in String objects. Since most regexes require you to use the backslash character a lot, use triple quote to avoid having to escape it:

```scala
val datePattern = new Regex("""\d\d\d\d-\d\d-\d\d""")
val yetAnotherDatePattern = """\d\d\d\d-\d\d-\d\d""".r   // same thing as above
```

You find matches via the "findFirstIn" and "findAllIn" methods:

```scala
val lottery = "4 8 14 16 23 42"
val numberPattern = """\d+"""

val firstNum = numberPattern.findFirstIn(lottery)  // firstNum gets '4'

val numberSet = numberPattern.findAllIn(lottery)
numberSet.foreach( println )
```

What about named captures? Scala's syntax for this is actually kinda cool:

```scala
// let's create a pattern for a fictitious log file,
val logPattern = """(\d+) minutes \( \w+://mysite(.*) \) [1-3] \S+ (OK|FAIL)?""".r

// this is the line we want to parse
val line = "17 minutes ( http://mysite/foo/bar ) 2 some_data OK"

// apply logPattern regex, with those 3 named lexical variables, to "line"
val logPattern(time, path, status) = line

// done! Now 'time', 'path' and 'status' are lexical variables (values):
println( path )  // <-- prints "/foo/bar"
```

You probably noticed we didn't wrap the regexes in "if" blocks. Well, in Scala a failed match will raise an exception, so you test them by putting them in *try* blocks. A saner approach, if you're just looking to see if it matches, is to do call ".pattern.matcher(variable).matches". For the example above, we could do something like:

```scala
if (logPattern.pattern.matcher(line).matches)
  // do something
else
  // do something else
```

### Substitutions

You won't find something as simple as Perl's s/// syntax, but Scala provides the *replaceAllIn* method which takes two arguments: the original string and a function which takes the match object itself (which we labelled "m" below) and returns the new string:

```scala
val newLine = logPattern.replaceAllIn(line, m => "getting path " + m.group(2) + " took " + m.group(1))
```

Note that we were able to access the named captures via the "group()" method. Running the code above, the "line" variable will remain the same, but "newLine" will contain the string "getting path /foo/bar took 17". Thankfully, a much easier to read solution can be written like so:

```scala
val newLine = logPattern.replaceAllIn(line, "getting path $2 took $1")
```

Classes and Objects
-------------------

If you're familiar with Stevan Little's p5-mop project, or even Class::MOP and Moose in general, you'll find Scala class definition somewhat familiar. Otherwise, if you're just used to bare-bones OO, think of "class" as a special kind of "package" for classes, one that already creates the "new" constructor for you.

```scala
class Point {
  var x = 0
  var y = 0
}
```

Note that class naming convention follows the same rule as in Perl. From that simple class definition, you can already create "Point" objects:

```scala
val p = new Point
p.x = 10
p.y = -3

println(p.x, p.y)
```

As you could see, variables declared inside a class work as attributes, just like the ones you would define in Moo(se) with the 'has' DSL. These attributes are always public, unless you prefix their declaration with "private":

```scala
class Point {
  private var x = 0
  private var y = 0
}
```

Of course, private attributes can't be accessed from outside:

```scala
val p = new Point
p.x = 10            // error: variable x in class Point cannot be accessed in Point
```

What if you want to pass values for 'x' and 'y' during construction? That's simple, just use the same signatures you did when creating functions! Here's how our new "Point" class would look like:

```scala
class Point(initialX: Int, initialY: Int) {
  var x = initialX
  var y = initialY
}
```

And now you define your objects during construction:

```scala
val p = new Point(10, -3)
```

Remember that good style in Scala asks you to refrain from adding spaces between the class name and the parentheses. Methods are functions defined inside a "class":

```scala
class Point(initialX: Int, initialY: Int) {
  var x = initialX
  var y = initialY

  def add(p: Point): Point = new Point(x+p.x, y+p.y)
}
```

Now we can call the "add" method:

```scala
val p1 = new Point(1, 3)
val p2 = new Point(-3, 7)

val p3 = p1.add(p2)
println(p3.x, p3.y)  // -2, 10
```

Not cool enough? How about defining "add" as an actual "+"?

```scala
class Point(initialX: Int, initialY: Int) {
  var x = initialX
  var y = initialY

  def +(p: Point): Point = new Point(x+p.x, y+p.y)
}
```

You can check: the only change we made above was replace "add" with "+" as the method name. And now we can add our points as we would in plain math:

```scala
val p1 = new Point(1, 3)
val p2 = new Point(-3, 7)

val p3 = p1 + p2     // same as p1.+(p2) remember?
println(p3.x, p3.y)  // -2, 10
```

#### $self

Another thing you might have noticed is that, unlike Perl 5, as variables become attributes they don't get shared among different objects. This means there is much less need for the "$self" variable in Scala: you simply call your methods and attributes within your class directly, like we just did.

What if you want/need a reference to your object anyway? Instead of "$self", use "this" and you're all set :)

### method calling convention

Usually, in Scala, functions and methods need to be called with the parentheses after the function/method name. The exception are methods that take *no parameters*, which can be called with or without the parentheses. While Perl's best practices ask you to drop the parentheses for clarity whenever possible, Scala's best practices convention ask you to drop them *only* when it is working just as a representation of a particular property or state. If the method does I/O, writes to some variable or reads from anywhere other than the object itself, directly or otherwise, then you should always call it WITH parenthesis. For example:

```scala
"meep".length   // no () because no side-effect is involved
println()       // keep the () on this one because it does I/O
```

The parentheses will work as a visual queue of what's going on in your code. As stated in the Scala style guide, religiously observing this convention will dramatically improve code readability and make it much easier to understand at a glance the most basic operation of any given method. This might look like a silly thing to you but it's taken very seriously by the Scala police - much like the Perl police frowns upon seeing a two-argument open() call, or code without strict and warnings :)

#### Singletons

An interesting feature of Scala is that it provides a keyword just for instantiating singletons, called "object". It works the same as the "class" keyword, but instead of a class which can be instantiated multiple times, it creates an object of an anonymous class, guaranteed to be unique. This works as a singleton which you can use in your apps easily, if you need to. Of course, unlike classes, defined objects cannot take parameters, since there is no construction step (e.g. you don't call "new" on an object). As a beginner, you probably don't want to worry about this just yet, though.

#### Inheritance

In Perl 5 you might be used to work inheritance through "use base" or "use parent". In Scala, inheritance is handled much like it is in Moose, via the "extends" keyword:

```scala
// first we define our base class...
class Drink {
  var alcoholic = false
  def drink() {
    println("Cheers!")
  }
}

// then we inherit from it!
class Beer extends Drink {
  alcoholic = true
}
```

When you extend a given class, the subclass inherits all non-private methods and attributes (variables/values). Unlike Perl 5, private data is kept from the subclass. You should also note that the parent class above ("Drink") doesn't need any arguments to be instantiated. When you subclass from a parent that needs arguments for its constructor, you must provide them right there on the 'extends' declaration. For example, do you remember our "Point" class? Let's check it out one more time so you don't have to scroll back:

```scala
class Point(initialX: Int, initialY: Int) {
  var x = initialX
  var y = initialY

  def +(p: Point): Point = new Point(x+p.x, y+p.y)
}
```

This is what we could do to extend it to a 3-dimensional point, one with "x", "y" and "z" instead of just "x" and "y":

```scala
class Point3D(initialX: Int, initialY: Int, initialZ: Int) extends Point(initialX, initialY) {
  var z = initialZ

  def +(p: Point3D): Point = new Point(x+p.x, y+p.y)
}
```

Notice how we already inherit "var x" and "var y", so we just need to define "var z". Also, when had to override the "+" method to add only "Point3D" objects. Finally, when we say it extends "Point", we need to tell the compiler how to initialize (i.e. construct) our parent object. Since we defined the variables "initialX" and "initialY" in our new class, we can just pass them directly to the original Point class.

#### Traits

And if you're into roles (and you should), be in from Moo(se) or Role::Tiny, Scala provides a similar feature called 'traits'. You define a trait just like you define a class or singleton object, except you use the 'trait' keyword:

```scala
trait MyTrait {
  def something() {
    println("does something")
  }
  def somethingElse() {
    println("does something else")
  }
}
```

And you use it in your classes with the 'extends' keyword, just like you would use in plain inheritance:

```scala
class MyClass extends MyTrait {
  override def something() {
    println("not the same!")
  }
}
```

The "override" keyword is necessary when you override methods and variables' definitions (don't worry about this now, the compiler is smart enough to let you know if you miss it). If you're already have a superclass, you use 'extends' to define the superclass and 'with' to define the trait(s):

```scala
class Cocktail extends Drink with Umbrella {
  ...
}
```

And if you need more traits, just add more 'with' clauses:

```scala
class Mojito extends Drink with WhiteRum with Sugar with LimeJuice with Mint {
  ...
}
```

Of course, this is just a rough introduction of the basic syntax for OOP in Scala. There are entire books dedicated to object oriented programming and associated patterns and best practices in Scala, but this should be more than enough to get you going.


Scaladoc for all your POD needs
-------------------------------

Whenever you're in trouble, the main place to go is the [Scala official API reference]. But how do you *write* documentation in Scala?

One of the advantages of being statically typed is that it's really easy to automate documentation. The *scaladoc* command line tool does this pretty well, reading your classes and object definitions and generating API documentation as HTML. It also lets you describe anything you want by adding special block comments right before declaring classes/objects/variables/etc, looking like so:

```scala
/** Start your comment here (note the two '*' starting the 'scaladoc' comment)
  * use left stars followed by a whitespace on every line.
  *
  * yes, even on empty lines (which are rendered as paragraphs)
  *
  * also note that the star in each line is aligned with the second '*' in '/**'
  * so that the text aligns with the start of the comment.
  *
  * = some heading =
  * === some sub-heading ===
  *
  * You can format text using '''bold''', ''italic'', `monospace`, __underline__,
  * ^superscript^ and ,,subscript,,
  *
  * {{{
  *   this is for code, rendered as a monospaced multiline block
  * }}}
  *
  * [[http://scala-lang and this is a link]]. Entity links work too: [[scala.collection.Seq]]
  */
```

You're expected to help scaladoc by describing constructors, parameters and return values using @annotations:

```scala
/** A 2-dimensional coordinate
  *
  * @constructor creates a new point with x and y values
  * @param x the point's coordinate in the x axis
  * @param y the point's coordinate in the y axis
  */
class Point (initialX: Int, initialY: Int) {
  var x = initialX
  var y = initialY

  /** adds two points together
    *
    * @param p the Point object to be added
    * @return a new Point with the 2 points added
    */
  def +(p: Point): Point = new Point(x+p.x, y+p.y)
}
```

Not as fancy as Pod, but does the trick. You create your documentation by typing on the command line:

    scaladoc <options> <source files>

Check out scaladoc's man page for extra information. As for generic data such as contact details, license type, general description and code sections, I'm really not sure. Most people just seem to write README and markdown files without standards (so don't expect to find a Pod::Weaver equivalent in Scala), except maybe for keeping docs in the "docs" directory of your package.


Tests
-----

Perlers usually give a lot of importance to testing - and good testing, for that matter. Scala offers an inherited JUnit, but you should probably check [specs2] or the extremely popular [ScalaTest].

There are 3 different ways to write tests in ScalaTest:

#### Test-Driven Development (TDD) with ScalaTest's FunSuite

```scala
import org.scalatest.FunSuite

class ExampleSuite extends FunSuite {
   test("classToBeTested.someMethod should return 42") {
     val foo = new classToBeTested
     assert(foo.someMethod === 42)
   }

   test("some other test") {
      ...
   }
}
```

Check [here](http://www.scalatest.org/getting_started_with_fun_suite) for a more complete tutorial on ScalaTest's FunSuite.

#### Behavior-Driven Development (BDD) with ScalaTest's FunSpec:

```scala
import org.scalatest.FunSpec

class ExampleSpec extends FunSpec {
  describe("My Thingie") {
    it("should do something") (pending)
    it("should return 42 when someMethod is called") {
      val foo = new classToBeTested
      assert(foo.someMethod === 42)
    }
  }
}
```

Check [here](http://www.scalatest.org/getting_started_with_fun_spec) for a more complete tutorial on ScalaTest's FunSpec.

#### Functional, integration and acceptance testing with ScalaTest's FeatureSpec

```scala
import org.scalatest.FeatureSpec
import org.scalatest.GivenWhenThen

class SomeTest extends FeatureSpec with ShouldMatchers with GivenWhenThen {
  feature ("Basic math works"){
    scenario ("Adding two numbers"){
      given ("a number")
      val number = 2
      and ("another number")
      val otherNumber = 3
      when ("adding them both")
      val result = number + otherNumber
      and ("in the reverse order")
      val otherResult = otherNumber + number
      then ("the result will be correct")
      result should equal (5)
      and ("the order will not matter")
      result should equal (otherResult)
    }
  }
}
```

Check [here](http://www.scalatest.org/getting_started_with_feature_spec) for a more complete tutorial on ScalaTest's FeatureSpec.

To run your tests, make sure you have downloaded ScalaTest's `.jar` file and placed it under your project's path, then run:

    $ bin/scala scalatest-1.9.1.jar org.scalatest.run myTestFile


Is There a "CSAN"?
------------------

One of the reasons we love Perl is because of the CPAN. So you might be wondering, is there a Comprehensive Scala Archive Network, or any place where we can find (and maybe even upload) publicly available code to easily achieve... stuff?

You wish. Sadly, Scala is still a long way to go on that matter. There are, however, a few aggregators you can look at to check for cool projects and libraries:

Scala's "contributed libraries and tools": http://www.scala-lang.org/node/1209
Github's "Scala" page: https://github.com/languages/Scala
Implicit.ly's feed of Scala software: http://notes.implicit.ly/
Scala Wiki's "Tools and Libraries": https://wiki.scala-lang.org/display/SW/Tools+and+Libraries

Here are some very crude equivalences between popular CPAN modules/frameworks and their Scala counterparts:

   * Catalyst => [Play Framework]
   * Dancer/Mojolicious => [Finatra]
   * DBI => http://scalaquery.org
   * DBIx::Class => http://blog.xebia.com/2011/06/25/scala-orm-with-squeryl/
   * PDL => Saddle [http://saddle.github.io/] and the [scala.math](http://www.scala-lang.org/api/current/scala/math/package.html) core package
   * POE/AnyEvent => not sure :(
   * If you're into Coro, concurrent programming, synchronous/asynchronous messagings, future and whatnot, check out the [scala.actors](http://www.scala-lang.org/api/current/index.html#scala.actors.package) package
   * wxWidgets => I don't think there is a wrapper for it in Scala (or Java, for that matter). Most people seem to use the scala.swing package.


Creating Larger Projects
------------------------

So far we learned how to write our files manually, which is fine for simpler projects. However, like Perl, once your projects start moving away from trivial examples, you want a tool to write the boilerplate stuff for you. This is specially important in Scala if you want to compile your code before publishing, as `scalac` will just fill your directories with weird files. In Perl we put a Makefile.PL (or a cpanfile) in our project's root directory to manage all that.

In Scala the most popular tool for this is called [SBT]. This tool organizes your project's compiled files in a much nicer fashion and even gets them ready for packaging and publishing. Sadly, it's not a very simple tool to setup and use, so it goes a bit beyond the scope of this guide. Another alternative is Apache's [Maven], so make sure to check that out as well if you can't wrap your mind around SBT.

Note: I know, I know, even with SBT you still have to create a lot of things manually. I'm not saying there isn't a Module::Starter or Dist::Zilla for Scala, I just haven't found one. Yet :-)

If you find something simple and easy for beginners, send me a patch!


The Next Step
-------------

Hope you've enjoyed this tutorial! Now that you've finished it, there are several places to gather extra and more in-depth information on Scala. [Check 'em out!](http://www.scala-lang.org/node/197)

That said, the best way to make yourself comfortable with Scala is to code, so go get your hands dirty! Maybe you find it interesting to have this [Scala cheatsheet] with you ;)


### Perl Mongers? Scala Tribes! \o/

Scala offers a community centre called [Scala Tribes], built much in the same mindset as the Perl Mongers communities worldwide. Check them out to see if there's a "tribe" near you!

A lot of talk is done over on IRC, on #scala at irc.freenode.net. There are also plenty of specific [mailing lists] for you to join if you like.


That's it, you've done it! Now go play with your new skills!

[Moe]: http://moe.iinteractive.com/
[Scala 2.10.0 MSI]: http://www.scala-lang.org/downloads/distrib/files/scala-2.10.0.msi
[Scala 2.10.0 tgz]: http://www.scala-lang.org/downloads/distrib/files/scala-2.10.0.tgz
[JavaSE website]: http://www.oracle.com/technetwork/java/javase/downloads/index.html
[Exception]: http://docs.oracle.com/javase/7/docs/api/java/lang/Exception.html
[Play Framework]: http://www.playframework.org/documentation/2.1-RC2/ScalaTodoList
[Finatra]: https://github.com/capotej/finatra#readme
[1]: http://stackoverflow.com/questions/1791408/what-is-the-difference-between-a-var-and-val-definition-in-scala
[Maps]: http://www.scala-lang.org/api/current/index.html#scala.collection.Map
[scala.util.control.Breaks]: http://www.scala-lang.org/api/current/index.html#scala.util.control.Breaks
[cool performance table]: http://docs.scala-lang.org/overviews/collections/performance-characteristics.html
[Scala cheatsheet]: http://docs.scala-lang.org/cheatsheets/
[Console]: http://www.scala-lang.org/api/current/index.html#scala.Console$
[io.Source]: http://www.scala-lang.org/api/current/index.html#scala.io.Source
[specs2]: http://etorreborre.github.io/specs2/
[ScalaTest]: http://www.scalatest.org
[SBT]: http://www.scala-sbt.org
[Maven]: http://maven.apache.org
[scala.sys.process]: http://www.scala-lang.org/api/current/index.html#scala.sys.process.package
[Scala official API reference]: http://www.scala-lang.org/api/current/index.html
[Scala Tribes]: http://www.scala-tribes.org
[mailing lists]: http://www.scala-lang.org/node/199
[Schwartzian transform in Scala]: https://joelneely.wordpress.com/2008/03/29/sorting-by-schwartzian-transform-in-scala/

