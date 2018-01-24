---
layout: post
title: "笔记: Programming in Scala, third edition"
description: ""
category: misc
tags: []
modify: 2017-02-26 16:00:00
---


前段时间看了此书，简要书摘如下，以便个人快速回顾。


### 4. Classes and Objects
`extends App`: don't need write `main` function.

#### 5.2 Literals
literal: a way to write a constant value directly in code.

##### Inter literals

##### Floating point literals

##### Character literals

##### String literals
raw string:

```scala
println("""|Welcome to Ultamix 3000.
           |Type "HELP" for help.""".stripMargin)
```

##### Symbol literals
``ident`

```scala
val s = 'aSymbol
val nm = s.name
```

##### Boolean literals
`true` and `false`

#### 5.3 String Interpolation
```scala
s"Hello, $name"
f"${math.Pi}%.5f"
raw"No\\\\escape!"
```

#### 5.4 Operators are Methods
```scala
val sum = 1 + 2 // operator is method: 1.+(2)
s indexof ('o', 5) // method is operator: s.indexof('o', 5)
```

Operators:

+ infix: `7 + 2`
+ prefix: unary
  - only four identifiers: `+`, `-`, `!`, `~`

  ```scala
  scala> -2.0
  scala> (2.0).unary_-
  ```
+ postfix: unary
  - empty parameter `s.toLowerCase`

#### 5.5 Arithmetic Operations

#### 5.6 Relational and Logical Operations
short-circuit?

operator = method

+ normal arguments: evaluated before entering a method
+ by-name parameters: delay

#### 5.7 Bitwise Operations

#### 5.8 Object Equality
comparing value equality: `==`(`equals`), `!=`

comparing reference equality: `eq` and `ne`

#### 5.9 Operator Precedence and Associativity

#### 5.10 Rich Wrappers


### 6. Functional Objects

#### 6.2 Constructing Rational
any code which isn't part of a field or a method definition -> primary constructor.

#### 6.4 Checking Preconditions
```scala
class Rational(n: Int, d: Int) {
  require(d != 0)
}
```

#### 6.5 Adding Fields
```scala
class Rational(n: Int, d: Int) { // class parameters
  val numer: Int = n // field
}
```

#### 6.7 Auxiliary Constructors
```scala
class Rational(n: Int, d: Int) {
  def this(n: Int) = this(n, 1)
}
```

+ primary constructor: single point of entry of a class.
+ only the primary constructor can invoke a superclass constructor.

#### 6.8 Private Fields and Methods

#### 6.10 Identifiers in Scala
+ alphanumeric identifier
+ operator identifier
+ mixed identifier: `unary_+`
+ literal identifier: `` `yield` ``

#### 6.11 Method Overloading
chosen overloaded version: best matches the static types of the arguments.

#### 6.12 Implicit Conversions


### 7. Built-in Control Structures
only control structures: `if`, `while`, `for`, `try`, `match`, and function calls.

`()`: unit value
+ assignment in scala *always* results in the **unit value**.

#### 7.3 For Expressions
`to` and `until`

```scala
for (file <- filesHere
     if file.isFile // filter
	 if file.getName.endsWith(".scala")
) print(file)
```

```scala
def scalaFiles = for (file <- filesHere
     if file.isFile // filter
	 if file.getName.endsWith(".scala")
) yield {file} // return collectons
// or `yield file`
```

#### 7.4 Exception Handing with Try Expressions
```scala
try {
  val f = new FileReader("input.txt")
} catch {
  case ex: FileNotFoundException => //....
}
```

avoid returning values from finally clauses.

#### 7.7 Variable Scope
an inner variable will *shadow* a like-named outer variable.


### 8. Functions and Closures

#### 8.2 Local Functions
local functions can access the parameters of their enclosing function.

#### 8.5 Placeholder Syntax
```scala
val f = (_: Int) + (_: Int)
```

#### 8.6 Partially Applied Functions
```scala
def sum(a: Int, b: Int, c: Int) = a + b + c

val a = sum _
val b = sum(1, _: Int, 3)

```

```scala
someNumbers.foreach(println _)
someNumbers.foreach(println)
```

#### 8.7 Closures

+ closures capture variables themselves, not the value to which

  ```scala
  var more = 1
  val addMore = (x: Int) => x + more // dynamical bind
  // function value (the object) addMore is a closure
  more = 9999
  ```

+ Changes: inside <==> outside

  ```scala
  var sum = 0
  someNumbers.foreach(sum += _)
  //or
  val closure = x => {x + sum; ()}
  someNumbers.foreach(closure)
  ```

#### 8.8 Special Function Call Forms
```scala
// Repeated parameters
echo (arr: _*)

// Named arguments
speed(time = 10, distance = 100)
```

#### 8.9 Tail Recursion
Often, a recusive solution is more elegant and concise than a loop-based one.

Note: recusive call is the *last* thing that happens in the *evaluation* of function approximate's body.


### 9. Control Abstraction

#### 9.3 Currying
```scala
def curriedSum(x: Int)(y: Int) = x + y

val onePlus = curriedSum(1)_
```

#### 9.5 By-name Parameters
```scala
def myAssert(predicate: () => Boolean) = //...

def byNameAssert(predicate: => Boolean) = //...
```


### 10. Composition and Inheritance

#### 10.2 Abstract Classes
```scala
abstract class Element {}
```

#### 10.3 Defining Parameterless Methods
```scala
def width: Int // access mutable state, computed on each method call

val width: Int // precomputed when initialized
```

#### 10.5 Overriding Methods and Fields
field and methods belong to the same namespace => overrided by each other

```scala
class WontCompile {
  private var f = 0 // Won't compile, because a field
  def f = 1         // and methd have the same name
}
```

scala's two namespaces are:

+ values: fields, methods, packages, and singleton objects;
+ types: class and trait names.

#### 10.6 Defining Parameteric Fields
```scala
class Cat {
  val dangerous = false
}

class Tiger {
  override val dangerous: Boolean, // parametric field definition
  private var age: Int
} extends Cat
```

#### 10.7 Invoking Superclass Constructors
```scala
class LineElements(s: String) extends ArrayElement(Array(s)) {}
```

#### 10.8 Using Overriding Modifiers

#### 10.9 Polymorphism and Dynamic Binding
dynamically bound: actual method implementation invoked <= runtime based on the class of the object, not the type of the variable or expression.

notice: override method.

#### 10.10 Declaring Final Members
final: cannot be ovverriden or subclassed

```scala
final class ArryElement {
  final overriden def demon() = {}
}
```

#### 10.11 Using Composition and Inheritance
inheritance relation:

+ whether it models an `is-a` relationship?
+ whether clients will want to use the subclass type as a superclass type?


### 11. Scala's Hierarchy

#### 11.1 Scala's Class Hierarchy
individual classes can tailor what `==` or `!=` means by overriding the `equals` method.

class AnyRef defines an `eq` mothod: reference equality.


### 12. Traits

#### 12.1 How Traits Work
A trait also defines a type.

trait definition = class definition with only two exceptions:

+ a trait cannot have any "class" parameter.

  ```scala
  trait NoPoint(x: Int, y: Int) // Does not compile
  ```
+ super calls are statically bound in classes, whereas they are dynamically bound in traits.

#### 12.2 Thin Versus Rich Interfaces
traits can enrich a thin interface, making it into a rich interface.

traits:

- a small number of abstract methods;
- a potentially large number of concrete methods.

#### 12.4 The Ordered Trait
type paramter

```scala
class Rational(n: Int, d: Int) extends Ordered[Rational] {}
```

#### 12.5 Traits As Stackable Modifications
super call in a trai are dynamically bound(even for abstract method):traits futhest to the right take effect first, roughly speaking.

#### 12.6 Why not Multiple Inheritance?
mutliple inheritance: the method called by a supercall can be determined right where the call appears.

traits: the method called is determined by a **linearization** of the classes and traits that are miexed into a class.

#### 12.7 To Trait or not to Trait?

+ If the behavior will not be reused ==> concrete class.
+ If it might be reused in multiple, unrelated classes ==> trait.
+ If you want to inherit from it in java code ==> abstract class.
+ If you plan to distribute it in complied form, and expected others inheritting from it ==> abstract class.
+ If you still do not know ==> trait.


### 13. Packages and Imports
```scala
// 1
package bobsrockets.navigation
class Navigator

// 2
package bobsrockets.navigation {
  class Navigator
}

// 3
package bobsrockets {
  package navigation {
    class Navigator
	// all names accessible in scopes outside the package
  }
}
```

`_root_`: outside any packagea user can write.

#### 13.3 Imports
three principal differences with Java:

1. may appear anywhere.
2. may refer to objects(singleton or regular) in addition to packages.
3. let you rename and hide some of the imported members.

```scala
import Fruits.{Apple, Orange}
import Fruits.{Apple => Mcintosh, Orange}
import Fruits.{Apple => _, Orange}
import Fruits._
import Fruits.{Apple => _, _}
```

#### 13.4 Implict Imports
Scala adds some imports implicitly to every program:

```scala
import java.lang._
import scala._
import Predef._
```

special: later imports overshadow earlier ones.

#### 13.5 Access Modifiers

+ private: visible only inside. It applies also for inner classes.
+ protected: only accessible from subclass.
+ public: not labeled.

A class shares all its access rights with its companion object and vice versa.

#### 13.6 Package Objects
```scala
// In file bobsdelights/package.scala
pacakge object bobsdelights {
  def helpMethods = {} // package-wide type aliases
                       // or implicit conversions.
}
```

### 14. Assertions and Tests
ScalaTest is the most flexible Scala test framework.

### 15. Case Classes and Pattern Matching

#### 15.2 Kinds of Patterns

+ Wildcard patterns `case _ =>`
+ Constant patterns `case 5 =>` or `case Pi =>`
+ Variable patterns `case x => `
  a simple name starting with a lowercase letter ==> variable;
  other => constant.

  ```scala
  import math.Pi
  val pi = Pi

  E match {
    case Pi => // constant
	case pi => // variable
	case `pi` => // constant
  }
  ```
+ Constructor patterns

  ```scala
  expr match {
    case Binop("+", e, Number(0)) =>
  }
  ```
+ Sequence patterns

  ```scala
  expr match {
    case List(0, _, _) => // Or,
	case List(0, _*) =>
  }
  ```
+ Tuple patterns: `case (a, b, c) => `
+ Typed patterns

  ```scala
  x match {
    case s: String => s.length
	case m: Map[_, _] => m.size //type erasure, exception: Array
	case _ => -1
  }
  ```
+ Variable binding

  ```scala
  expr match {
    case Unop("abs", e @ Unop("abs"), _)) => e
  }
  ```

#### 15.3 Pattern Guards
```scala
e match {
  case BinOp("+", x, x) => // Error
  case Binop("+", x, y) if x == y => // OK

  case n: Int if 0 < n =>
  case s: String if s(0) == "a" => 
}
```

#### 15.5 Sealed Classes
sealed: A sealed class cannot have any new subclass added except the ones in the same file. useful for case class.

```scala
(e: @unchecked) match {
  case Number(_) =>
  case Var(_) =>
}
```

#### 15.7 Patterns Everywhere
+ Patterns in variable definitions.

  ```scala
  val (number, string) = (123, "abc")
  val BinOp(op, left, right) = new BinOp("*", Number(5), Number(1))
  ```
+ Case sequences as partial functions.
+ Patterns in for expressions.

  ```scala
  for (Some(fruit) <- results) println(fruit)
  ```


### 16. Working with Lists
List:

+ immutable
+ recursive structure
+ homogeneous
+ covariant

```scala
val List(a, b, c) = fruit

x :: xs == ::(x, xs)

xs ::: ys

xs.length
xs.head :: xs.tail == xs.init :: xs.last
xs.reverse.reverse == xs
xs splitAt n == (xs take n, xs drop n)
xs apply 2 == xs.drop(2).head
xs.indices

xs.flatten
(xs zip ys).unzip == (xs, ys)

xs partition p == (xs filter p, xs filter(!p(_)))

xs span p == (xs takeWhile p, xs dropWhile p)

xs.forall(_ > 0)
xs.exists(_ > 0)

(0 /: xs)(_ + _)
(xs :\ 0)(_ + _)

xs.sortWith(_ < _) //merge sort
```

#### 16.8 Methods of the List Object
```scala
List.apply
List.range
List.fill
List.tabulate
List.concat
```

#### 16.10 Understanding Scala's Type Inference Algorithm
Type inference is Scala is flow based.

curried function: non-function arguments at first, and then function arguments.


### 17. Working with Other Collections

#### 17.1 Sequences
+ Lists
+ Arrays
+ List buffers
+ Array buffers
+ Strings

#### 17.2 Sets and Maps
```scala
var c: scala.collection.immutable.Set[String]
val c: scala.collection.mutable.Set[String]
```

#### 17.4 Initializing Collections
`xs.toArray`: order is same with iterator's. copying is expensive.


### 18. Mutable Objects
```scala
var hour = 12 // equals

private[this] var h = 12
def hour: Int = h
def hour_=(x: Int) = {h = x}
```

```scala
var celsius: Float = _ // init, default value, 0
var celsius: Float // abstract field, cannot init
```


### 19. Type Parameterization

### 20. Abstract Members

#### 20.5 Initializing Abstract Vals
+ class paramter: evaluated before passed to class constructor.
+ abstract fields: evaluated only after the superclass has been initalized.

solutions:

+ Pre-initialized fields:

  ```scala
  new {
    val n = 1 * x
	val d = 2 * x
  } with RationalTrait
  ```
+ Lazy vals:

  ```scala
  private lazy val g = {
    require(d != 0)
	// ....
  }
  ```

#### 20.6 Abstract Types
```scala
class Food
abstract class Animal {
  type SuitableFood <: Food
  def eat(food: SuitableFood)
}
```

#### 20.8 Refinement Types
```scala
class Pasture {
  var animals: List[Animal {type SuitableFood = Grass}] = Nil
}
```


### 21. Implicit Conversions and Parameters

#### 21.2 Rules for Implicits
1. Only definitions marked `implict` are available.
2. An inserted implict cnversion must be in scope as a single identifier, or be associated with the source or target type of the conversion.

One-at-a-time rule: Only one implicit is inserted.

usage:

+ conversions to an expected type
+ conversions of the receiver of a selection
+ implicit parameters (provide information, useful for generic functions)

#### 21.5 Implicit Parameters
compiler replace `someCall(a)` with `someCall(a)(b)`, or `new SomeClass(a)` with `new SomeClass(a)(b)`.

```scala
def maxListImpParm[T](elements: List[T])
      (implicit ordering: Ordering[T]): T = {}
```

#### 21.7 When Multiple Conversions Apply
If one of the available conversions is strictly more specifi than the others, then the compiler will choose the more specific one.

one implicit conversion is more specific than another if one of the following applies applies:

+ subtype.
+ Both conversions are methods, and the enclosing class of the former extends the enclosing class of the latter.

Think twice and use implicits.


### 22. Implementing Lists
ListBuffer track the pointer of tail of List.


### 23. For Expressions Revisited
all `for` expressions that yield a result ==> combinations of `map`, `flatMap`, and `withFilter`.

all `for` expressions without yield ==> `foreach` and `withFilter`

#### 23.1 For Expressions
`for (seq) yield expr`
and
`for (seq) body`


### 24. Collections in Depth

#### 24.3 Trait Traversable
`foreach` is the basis of the implementation of all operations in Traversable, so its performance matters.

#### 24.4 Trait Iterable

#### 24.5 The Sequence Traits Seq, IndexedSeq, and LinearSeq
`update`: mutable sequences
`updated`: always return a new sequence.

#### 24.13 Equality
Collections in different categories are always unequal.

Collections with the same category are equal if and only if they have the same elements.

#### 24.14 Views
A view is a special kind of collection that represents some base collection, but implements all of its transformers lazily.

reason to use:

+ performace: get rid of intermediate data strucure.
+ hope to update some mutable sequences.


### 25. The Architecture of Scala Collections
same-result-type


### 26. Extractors
`apply` and `unapply`


### 27. Annotations

#### 27.1 Why have Annotations?
+ Automatic generation of documentation as with Scaladoc.
+ Pretty printing code so that it matches your preferred style.
+ Checking code for common errors.
+ Experimental type checking.


### 28. Working with XML


### 29. Modular Programming Using Objects


### 30. Object Equality


### 31. Combining Scala and Java


### 32. Futures and Concurrency


### 33. Combinator Parsing
