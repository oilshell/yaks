Yaks IR for Oils mycpp
===

## Top Level

- modules: import   (Import ?)
- func   
- class  (Class Fn Init Destroy)
  - including methods, inheritance
  - constructor and destructor for context manager

## Within a function

- var set
  - not def?  Because (set [a i] 0) is different and useful
  - I guess Yaks TAPL can use def
- scope  : with - for scope { }
- looping: for, while
- cond   : if switch (tagswitch)
- errors : try catch throw

## Infix Expressions?

I think these are the interesting ones:

- `(myfunc obj.field)`
- `(obj.method arg1 arg2)`
- Probably `(+ 1 2)` is OK -- this is a rough IR
  - Also I wonder about formatting
    - that could be `\()` or something?  But sometimes you want dynamic
      formats?  So printf can be OK

## Data Types
 
- `Bool Int Float Str`
- `(List T) (Dict K V)`

Basically I want to make an interpreter with the same semantics as mycpp We
already have that -- it's Python

But this is a static interpreter I guess

## TYPY - Subset of TypeScript and MyPy semantics?

### Issues

- TypeScript doesn't have 'with'
  - but it has classes -- does it have destructors?

- mylib has some shims, like mylib.iteritems()
  - well to be honest, this is what I want to clean up
  - the IR I guess decouples that from Python

- mylib.tagswitch papers over Python's lack of switch
  - well Python 3 has match, we could do it

### String Semantics

- `s[1]` in JavaScript will give the UTF-16 code unit `\u03bc`
- slice() works in 16-bit units as well

Can we simulate this on UTF-8 strings?

- In Python, our API is s[0] and s[1:2]

I think it could be `s.codeUnitAt() s.codeUnitSlice()`.

### Number Semantics

- JavaScript has BigInteger, but TypeScript only has `number`
- How to get `Int` for loop indices then?
  - rely on some kind of inference?
- Yeah this is an issue, TypeScript doesn't have the distinction

You could have a flag

- make number -> float
- make number -> int

OK that's probably OK.


