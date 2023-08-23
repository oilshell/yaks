Table of Language Projects
========

- Looking for static / static pairs.  Metalanguage matters.
- See OCaml vs. Rust post, and TypeScript


MOTIVATIONS: static TYPES make code go fast!!  mycpp leverages the C++
compiler.

I would like to write something like mycpp -- "Pea".  MyPy's type system is
incredibly useful and complex, but we arguably want something simpler.

Meta: TypeScript's type system is awesome, but it's unclear how to translate it
to fast code !!!
  Maybe something like AssemblyScript

## Intro

Interpreter involves 2 languages:

- Language being interpreted (e.g. Python)
- Language that implements the interpreter (C)

Compiler involves 3 languaegs:

- Language being compiled (e.g. C++)
- Language that implements the compiler (C++)
- Language being translated to (Assembly)

---

- https://mukulrathi.com/create-your-own-programming-language/intro-to-type-checking/
  - best one, it's in OCaml and longer.
- Make-a-Lisp: Much bigger codebase to read
- Norvig's Lispy -- both languages dynamically typed, s.split('(') etc.
- ocamlscheme -- Uses ML lex and yacc

But the reader is worth comparing.

- Types and Programming Languages, e.g. Chapter 11 Lambda Calculus
  - Doesn't have examples!
  - No parser!  Hard to write tests!


## Table

Test

<table>
 <tr>
   <td>

Does *Github* support Markdown in cells?  CommonMark does.  [TODO.md](TODO.md)
     
   </td>
 </tr>
</table>

## Books

- Crafting Interpreters pt 1: Java (static) interpreting Lox (dynamic)
- Crafting Interpreters pt 2: C (static) compiling Lox (dynamic) to bytecode (dynamic)

- Dragon Book: Shockingly, doesn't have code for a type checker!
  - Somehow I didn't realize this for awhile

- Appel Tiger
  - ML/Java/C -- procedural tiger?

- TAPL
  - OCaml (Static) and static languages, but no real parser or scaffolding required
  - link to course material

- Terrence Parr
  - Java/ANTLR (more like a DSL) - Not sure there is a complete one

- Writing a Compiler in Go - https://compilerbook.com/
  - Static / Dynamic

### Lisp

- TODO: look at Essentials of Programming Languages?
  - Does not have parser?
  - example code and scaffolding?

- SICP chapters
  - Lisp interpreting Lisp -- metacircular

## Compiler Courses

- Coursera?
  - Standard ML (static)  ???

## Web

- ocamlscheme
  - OCaml (static) interpreting Lisp (dynamic)
    - or was there a compiler too?
    - I hacked on this codebase to learn OCaml
- https://github.com/schani/ocamlscheme
  - "This is a very efficient interpreter for a small statically scoped subset of Scheme"
  - "Most importantly, no symbol lookup needs to happen during execution"

- Let's build a compiler
  - https://compilers.iecc.com/crenshaw/
  - PASCAL compiling ???

### Bolt - static/static

- Follow Bolt, an OO language implemented in OCaml
  - TAPL doesn't seem to have an implementation
  - <https://mukulrathi.com/create-your-own-programming-language/intro-to-type-checking/>
  - Although he uses a repetitive AST style, not the map
  - Also typing environment is a list, not a Dict !
  - Dicts are missing from OCaml!

- <https://github.com/mukul-rathi/bolt/blob/master/src/frontend/typing/type_classes.ml>
  - 143 lines

- Does Bolt have function types?  I think we need those
  - or maybe we don't need first class ones?

- <https://mukulrathi.com/create-your-own-programming-language/inheritance-method-overriding-vtable/>
  - OK this is good, I want inheritance!
  - Copy all this in TypeScript

### PL Zoo - OCaml/static

- <https://plzoo.andrej.com/>
  - "sub" language: eager, mutable records, statically typed, subtyping
  - but syntax?
  - examples don't have subtyping!!!  Bad

## Real Codebases

- C interpreting femtolisp 
- femtolisp front end for Julia

- OPy
  - Python compiling Python to Bytecode (dynamic)
- byterun
  - Python interpreting bytecode

## Lisps

- Norvig Lispy:
  - Python interpreting Lisp

- Make-a-lisp
  - Lisp and X  -- are they all interpreters?

# vim: sw=2
