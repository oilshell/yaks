Syntax Notes
==

## Idea for Location info in YIR

`(+ 3 4)` =>

```
(Source (file foo.py))

(Tokens
  [ ( 1 1 ]    # ID, start, length
  [ + 2 1 ]    # I2
  [ ws 3 1 ]   
  [ NUM 4 1 ]  # 3 id 4
  [ ws 5 1 ]
  [ NUM 6 1 ]  # 4
  [ ) 7 1 ]
)
```

Then =>

```
(if (= a 0 |3 4 6|)
  1
  (+ a b |5 6 12)
|2 4 9 10|)  # location of if ( 1 (
```

- So yeah every node has N children, and there are N + 1 numbers at the end?
  Seems fine.

## Meaning of `[] ()` ?

Should they be synonyms?

    [+ 1 [* 3 4]]  # This is allowed

Or should you make [1 2 3] a synonym for '(1 2 3) perhaps?

It's unevaluated

So this makes more sense?

    (fn [x] (do (+ 1 2) (print a)))

Same with

    (fn (-> [x Int] [y (List Int)] (List Int))
      (+ x y))

What was that f-exprs thing?  Unifying functions and macros in Lisp?

### Clojure

- `do` is `begin`
  - `lambda` and `let` have implicit `do`
- `fn` is `lambda`
  - `defn` is `(def id (fn [x] x))`

### More

```lisp
(begin
  # Using syntax from book ?
  (deftype fib (-> [number] number)

  (define fib
    (lambda [n]
      (if (== n 0)
        1
        (+ (fib (- n 1)) (fib (- n 2))))))

  # Is print polymorphic
  (print (fib 10))
```

Types could be attached to set

```lisp

(set x 42)  # untyped

(set [x Int] 42)  # typed

# deftype might be better than set?
# or just type
(type IdType (-> [Int] Int))

(set IdType (-> [Int] Int))

# -> is a specal form like lambda, since first arg isn't evaluated
(set PlusType (-> [Int Int] Int))
(set EqType (-> [Int Int] Bool))
(set AndType (-> [Bool Bool] Bool))

(set
  (x IdType) 
  (lambda [x] x))

(set
  (x AndType) 
  (lambda [x y] (and x y)))
```

In JS 

```javascript
var fib = function(n: number): number {
  if (n === 0) {
    return 1
  } else {
    return fib(n - 1) + fib(n - 2)
  }
}

print((fib(10))
```

## WebAssembly Syntax

- <https://developer.mozilla.org/en-US/docs/WebAssembly/Understanding_the_text_format>

Has `$p1` instead of indices

```
(func (param $p1 i32) (param $p2 f32) (local $loc f64) ...)
```

- Has `(param ...)`
- `(result ...)`

We could do that

```lisp
(set x
  (lambda
    [(param x Int) (param y Int) (result Int)]
    (begin
      (set x y)
      z)))
```

Untyped:

```lisp
(set x
  (lambda [x y]
    (begin
      (set x y)
      z)))
```

### Grammar

Official:

- <https://webassembly.github.io/spec/core/text/values.html>
  - this has semantics with => , which is a little confusing

More readable referenec impl, with some differences

- <https://github.com/WebAssembly/spec/blob/master/interpreter/README.md#s-expression-syntax>

```
num:    <digit>(_? <digit>)*
hexnum: <hexdigit>(_? <hexdigit>)*
nat:    <num> | 0x<hexnum>
int:    <nat> | +<nat> | -<nat>
float:  <num>.<num>?(e|E <num>)? | 0x<hexnum>.<hexnum>?(p|P <num>)?
name:   $(<letter> | <digit> | _ | . | + | - | * | / | \ | ^ | ~ | = | < | > | ! | ? | @ | # | $ | % | & | | | : | ' | `)+
string: "(<char> | \n | \t | \\ | \' | \" | \<hex><hex> | \u{<hex>+})*"
```

- We can use their int and float syntax
  - leave out hex
- string syntax is pretty much what we want, it's double quoted and has \u{1234}
  - is hex really "\ff" ?  Not \xff


## Clojure Quoting

Make a lisp is inspired by Clojure

- <https://github.com/kanaka/mal/blob/master/process/guide.md#step7>


Quoting

    (quote +)
    => '+

    '+
    => '+

    (quote 1 2 3)
    => (1 2 3)

List evaluates its arguments

    (list '+ 1 (inc 1))
    => (+ 1 2)

Syntax quoting allows unquoting (evaluation)

    `(+ 1 ~(inc 1))
    => (+ 1 2)


I think this was 'unquote' in Elixir

### femtolisp Quoting

- https://github.com/JeffBezanson/femtolisp/blob/master/tests/ast/rpasses.lsp

Traditional syntax which I don't like

```
`(lambda ,(func-argnames n)
  (r-block ,@(gen-default-inits (cadr n))
    ,@(if (and (pair? (caddr n))
```

- It does use ,(f a b) not just ,a

### Macros

Key idea: all macros can be written with list !!!

    (defmacro code-critic
      [bad good]
      (list 'do
        (list 'println "bad"
              (list 'quote bad))
        (list 'println "good"
              (list 'quote good))))

    =>

    (do (println "1" (quote good)) (println "2") (quote bad))

But syntax quoting makes it a bit nicer

    (defmacro code-critic
      [bad good]
      `(do (println "bad" (quote ~bad))
           (println "good" (quote ~good))))

### unquote vs unquote-splicing

    (def items [1 2 3])

    `(0 ~@items 4)
    => (0 1 2 3 4)

    `(0 ~items 4)
    => (0 (1 2 3) 4)


### They're just Reader Macros

    `(+ ~x)
    (quasiquote (+ (unquote x))

    `(+ ~@x)
    (quasiquote (+ (unquote-splicing x))

Ok that's easy enough

I kind want to put that in

## C-ish / Shell-ish syntax

    (quote +)
    \+

And

    \(+ 1 $(inc 1))

    (defmacro code-critic
      [bad good]
      \(do (println "bad" (quote $bad))
           (println "good" (quote $good)))

So you have:

    \name
    \(a b)
    $name
    $(f a b)

    @splice

Do you need the difference between ' and backtick ?

- In Clojure, there is the difference of NAMESPACES
  - not sure about other Lisps

You can use perhaps make 

    \+   \(a b)

And also

    \\(a b $x @y)

So `\` is quoting, and `\\` is quasi-quote

MEH I want to reserve $ and @ for shell (and WebAssembly uses $ too)

We could have just `\\()` for quasi-quoting and explicit splicing

- And then maybe `~a` and `~~a` or `~@a` to be consistent
- Or `*a` and `**a`, too much like Python



## Julia Syntax

    ex.head
    ex.args

    :call

    ex = quote
      x = 1
      y = 2
      x + y
    end

Hm Elixir also has the quote.

I think for us Hay

    Package {
    }

is similar to quoting?  It produces a data structure you can access with
`_hay()`

Interpolation

    a = 1
    ex = :($a + b)
    :(1 + b)

Splicing is $(a...)

    a = [:x, :y, :z]
    ex = f(1, $(a...))
    :(f(1, x, y, z))

Can also have multiple `$$` for nested quote blocks




## Old



(Top level is only definitions.  No executable code.)

And then you can decorate nodes by index:

```

(+#2
  3#4
  4#6
)#1   # ID is one

```

How about something less noisy

```
(+,2
  3,4
  4,6
),1   # ID is one
```

```
(+,2 3,4 4,6),1 
```

Looks better

```
(+,2 a,4 b,6),1 
```

Looks worse:

```
(+,2 1.0,4 2.0,6),1 
```

Allow `.` or `:`

```
(+.2 1.0:4 2.0:6).1 
```

Or make it a suffix in ():

```
(+ 1 2 (f).7 {2 4 6}).1
```

Prefix:

```
1.({2 4 6} + a b 7.(f))
```

Or put everything inside ()

```
({2 4 6} 
  + a b
  ({7} f)
)

```

OK this isn't horrible.  It show you location of each node, without  messing up
readability of expression that much


```
(|2 4 6|
  + a b
  (|7| f)
)

(|2 4|
 if (|3 4 6| = a 0)
    1
    (|5 6 12| + a b)
```

