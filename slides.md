---
title: (Yet Another) Introduction to Category Theory
theme: serif
css: style.css
revealOptions:
  margin: 0.2
  transition: fade
  center: false
  controls: false
---


### (Yet Another)
## Introduction to
## Category Theory

Rui Gonçalves <!-- .element: style="margin-top: 80px" -->

ShiftForward

June 20, 2017

Note:

This is a very short introduction to category theory focused on the base concepts for understanding the nature of abstract data types and how the underlying patterns are derived. It's by all means not complete; it misses some very important things in category theory and it most probably sacrifices formal correctness for ease of understanding.


---


## A Category

![](img/category_1.png)

Note:

A category is composed by a (possibly infinite) set of objects and directed arrows linking them. Some conditions related to composition are necessary for such a structure to be considered as a category:

- Every object $A$ must have an identity arrow called $id_A$ starting and ending in itself;
- Every time we have three (possibly non-distinct) objects $A$, $B$ and $C$ and arrows $f: A \rightarrow B$ and $g: B \rightarrow C$, there must exist the arrow from $A$ to $C$ called $f∘g$. This is called the composition of $f$ and $g$;
- The identity arrow must be the unit of composition;
- Composition must be associative.

The equality of arrows is different for different categories: apart from what the laws of categories tell us, we can't compare them directly.


---


## A Category

![](img/category_2.png)


---


## A Category

![](img/category_3.png)


---


## It's all about composition

![](img/composition.png)


---


## **Set** Category

![](img/set.png)

Note:

In the **Set** category, objects are sets of elements and arrows are functions between them. Identity and composition are trivially defined.

The concept of category is a kind of abstraction layer over the operations typically defined on sets. It allows us to use what we know from math in other domains, as long as their structure fits a category.


---


## **Set** Category

- $id(x) = x$
- $(g∘f)(x) = g(f(x))$


---


## **Hask** Category

![](img/hask.png)

Note:

The **Hask** category is the category of Haskell types. Objects are types (which can be seen as sets of instances) and arrows are Haskell functions.

Due to the nature of computing and the impossibility of detecting (and thus disallowing) non-terminating programs, there must exist an extra "bottom" instance in each type. Theoretically, this presents many issues; in practice, we can ignore that aspect and treat **Hask** just like **Set**.


---


## **Hask** Category

```haskell
id :: a -> a
id x = x

compose :: (b -> c) -> (a -> b) -> (a -> c)
compose g f x = g (f x)
-- compose g f = g . f
-- compose = (.)
```


---


## More Categories

- No objects (**0**)
- Single object (**1**)
- Orders
- Vector spaces (**Vect**)
- ...


---


## Universal Constructions

- Patterns of relationships between objects
- Allows reusing laws from one category in others
- A balance of precision and recall
  1. pick a pattern and look for all its occurrences
  2. rank the hits and pick the best fit


---


## Initial Object

![](img/initial.png)

Note:

The first universal construction is that of an object with arrows for every other object. There's no guarantee that such an object exists (which is OK), but there can be too much. If we restrict our pattern to require the initial object to have a *unique* arrow from it to each object in the category, such an object is now unique up to isomorphism (which is explained later).


---


## Initial Object

- The object is called $0$
- The unique arrow $0 \rightarrow A$ is called $0_A$
- The empty set is an initial object in **Set**
- Any empty type (such as `Void`) is an initial object in **Hask**


---


## Terminal Object

![](img/terminal.png)

Note:

By reversing all arrows in the initial object pattern, we have a terminal object - one object with an unique arrow pointing from each object of the category to itself.


---


## Terminal Object


- The object is called $1$
- The unique arrow $A \rightarrow 1$ is called $1_A$
- Any singleton set is a terminal object in **Set**
- Any type with a single instance (such as `()`) is a terminal object in **Hask**

Note:

Note that the rules are very similar to the initial object rules.


---


## Duality

- **C<sup>op</sup>** is a category obtained by keeping all objects and reversing the arrows of category **C**
- Automatically a category:
  - $id^{op} = id$
  - If $h = g∘f$, then $h^{op} = f^{op}∘g^{op}$


---


## Duality

- Constructions in the opposite category are prefixed with "co" – coproducts, comonads, colimits...
- **C<sup>op</sup>** may be equal to **C** (not in **Set** and **Hask**)
- The terminal object is an initial object in the opposite category!


---


## Isomorphism

- In universal constructions, the "shape" is the key
- It's enough to have a one-to-one mapping between objects
- In category theory, such a mapping is a pair of arrows, one being the _inverse_ of the other


---


## Isomorphism

![](img/iso.png)


---


## Isomorphism

- $f: A \rightarrow B$ is an isomorphism iff there is an arrow $f^{-1}: B \rightarrow A$ such that:
  - $f^{-1}∘f = id_A$
  - $f∘f^{-1} = id_B$


---


## Isomorphism

- Two initial objects must be isomorphic:
  - Let $0^A$ and $0^B$ be two initial objects with arrows $0^A_C: 0^A \rightarrow C$ and $0^B_C: 0^B \rightarrow C$ for every $C$
  - $0^A\_{0B} ∘ 0^B\_{0A}$ is an arrow $0^A \rightarrow 0^A$ <!-- .element: class="fragment" -->
  - But there can be only one arrow from $0^A$ to any other object, and we already have $id_A$! <!-- .element: class="fragment" -->
- No need to show the same thing for the terminal object! <!-- .element: class="fragment" -->


---


## Isomorphism

![](img/initial_iso.png)


---


## Products

- Goal: generalize the notion of a cartesian product of sets
- First idea:
  - three objects: $A \times B$ is the product object of $A$ and $B$
  - two arrows, $outl: A \times B \rightarrow A$ and $outr: A \times B \rightarrow B$, extracting the first and the second component


---


## Products

![](img/product.png)


---


## Products

```haskell
outl :: (Int, Bool) -> Int
outl (x, b) = x

outr :: (Int, Bool) -> Bool
outr (x, b) = b
```

Note:

We can show that the type `(Int, Bool)` is a valid product of `Int` and `Bool` according to our first definition by showing the `outl` and `outr` arrows for it.


---


## Products

```haskell
outl :: Int -> Int
outl x = x

outr :: Int -> Bool
outr _ = True
```

Note:

However, the definition also allows less suitable types to work as products, such as `Int` and `(Int, Int, Bool)`.


---


## Products

```haskell
outl :: (Int, Int, Bool) -> Int
outl (x, _, _) = x

outr :: (Int, Int, Bool) -> Bool
outr (_, _, b) = b
```


---


## Products

- Need to rank candidates
- Idea: find an arrow between candidates that reconstructs one in function of the other
- Let $P$ and $Q$ be two candidates for a product with arrows $outl_P$, $outr_P$, $outl_Q$ and $outl_Q$:
  - $P$ is "better" than $Q$ iff there exists an **unique** arrow $m: Q \rightarrow P$ such that $outl_Q = outl_P∘m$ and $outr_Q = outr_P∘m$
  - Similiar to factorization in mathematics


---


## Products

- Is `(Int, Bool)` better than `Int`?

```haskell
m :: Int -> (Int, Bool)
m x = (x, True)
```

- Is `Int` better than `(Int, Bool)`?

```haskell
m :: (Int, Bool) -> Int
-- m (x, b) = ???
```

Note:

Using our new ranking method, we show that neither `Int` nor `(Int, Int, Bool)` have suitable factorization functions to `(Int, Bool)`.

As `Int` cannot hold all the information in `(Int, Bool)`, there is no suitable function.


---


## Products

- Is `(Int, Bool)` better than `(Int, Int, Bool)`?

```haskell
m :: (Int, Int, Bool) -> (Int, Bool)
m (x, _, b) = (x, b)
```

- Is `(Int, Int, Bool)` better than `(Int, Bool)`?

```haskell
m :: (Int, Bool) -> (Int, Int, Bool)
-- m (x, b) = (x, ???, b)
```

Note:

`(Int, Int, Bool)` holds too much information, resulting in the existence of multiple factorization functions (the factorization function should be unique).


---


## Products

- With the factorization condition, every time a product exists it is unique up to a unique isomorphism
- `(a, b)` is the best match in **Hask** – A _factorizer_ can show it:

```haskell
factorizer :: (c -> a) -> (c -> b) -> (c -> (a, b))
factorizer outl outr = \x -> (outl x, outr x)
-- factorizer outl outr x = (outl x, outr x)
```


---


## Coproducts

![](img/coproduct.png)

Note:

A product in the opposite category is called a coproduct. By duality, all reasoning done in products can be ported to coproducts.


---


## Coproducts

- By duality, when it exists, a coproduct is unique up to unique isomorphism
- `Either a b` is the best match in **Hask**:

```haskell
-- Either a b = Left a | Right b

factorizer :: (a -> c) -> (b -> c) -> (Either a b -> c)
factorizer inl inr (Left a) = inl a
factorizer inl inr (Right b) = inr b
```


---


## Algebra of Data Types

- Most data structures in programming are built using products and coproducts
- Many properties are composable: equality, comparison, conversions...
- Treating data structures by their shape paves the way for automatic derivation


---


## Algebra of Data Types

- These types are isomorphic in **Hask**:

```haskell
(String, Int, Bool)       -- String * Int * Bool

((String, Int), Bool)     -- (String * Int) * Bool

(String, (Int, Bool))     -- String * (Int * Bool)

data Contact = Contact {  -- String * Int * Bool
  name :: String,
  age :: Int,
  gender :: Bool
}

(Int, Bool, String)       -- Int * Bool * String

(Int, Bool, String, ())   -- Int * Bool * String * 1
```

Note:

Products in **Hask** can be composed, are associative and have the terminal object as its unit. Note the similarity with the multiplication operation and the number 1.


---


## Algebra of Data Types

- These types are isomorphic in **Hask**:

```haskell
Either (Either String Int) Bool     -- (String + Int) + Bool

Either String (Either Int Bool)     -- String + (Int + Bool)

data JsValue =                      -- String + Int + Bool
  JsString String |
  JsNumber Int |
  JsBoolean Bool

Either (Either Int Bool) String     -- (Int + Bool) + String

data JsValue2 =                     -- String + Int + Bool + 0
  JsString String | JsNumber Int |
  JsBoolean Bool | Void
```

Note:

Coproducts are also composable, associative and have the initial object as its unit. Note once more the similarity with the addition operation and the number 0.


---


## Algebra of Data Types

- **Set** and **Hask** are monoidal categories:
  - With the product as binary operation and the terminal object as unit
  - With the coproduct as binary operation and the initial object as unit
- It can be shown that every category that _has_ products and a terminal object is also a monoidal category (and the dual proposition for coproducts)


---


## Algebra of Data Types

- These types are isomorphic in **Hask**:

```haskell
-- String * (Int + Bool)
(String, Either Int Bool)

-- (String * Int) + (String * Bool)
Either (String, Int) (String, Bool)
```


---


## Algebra of Data Types

- These types are isomorphic in **Hask**:

```haskell
-- String * 0
(String, Void)

-- 0
Void
```

Note:

Products in **Hask** distribute over coproducts and the initial object is the absorbing element of the product. Note once more the similarity with math.


---


## Algebra of Data Types

- **Set** and **Hask** form a _semiring_ under the product and coproduct
- Not every category with product and coproduct monoids is a semiring


---


## Algebra of Data Types

| Numbers      | Types
|--------------|--------
| $0$          | `Void`
| $1$          | `()`
| $a + b$      | <code>Either a b = Left a &#124; Right b</code>
| $a \times b$ | `(a, b)` or `Pair a b = Pair a b`
| $2 = 1 + 1$  | <code>data Bool = True &#124; False</code>
| $1 + a$      | <code>data Maybe = Nothing &#124; Just a</code>

Note:

Due to all the conditions presented, types can be mapped to natural numbers. The natural number corresponds to the number of instances of the respective type.


---


## Algebra of Data Types

- Other mathematical concepts with meaning in type theory: exponentials, infinite sums, derivatives


---


## What's More?

- Functors
- Natural transformations
- Function objects
- Everything else :)


---


Thank you! <!-- .element: style="text-align: left" -->

- https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface

- _Algebra of Programming_, Richard Bird and Oege de Moor (1997)
