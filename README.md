# Racket Enhanced

A syntax definition for the [Racket](https://racket-lang.org) programming
language for Sublime Text 4.

This package ships no build system, REPL, completion, or
language-server integration. For those, use a Sublime build system of your
own or the [LSP](https://packagecontrol.io/packages/LSP) package with
[racket-langserver](https://github.com/jeapostrophe/racket-langserver).

Supported extensions: `.rkt`, `.rktl`, `.rktd` (Racket syntax) and `.scrbl`
(Scribble syntax: prose text with `@`-expressions, sharing every rule with
the Racket syntax). `.rkt` files understand `@`-expressions anywhere in the
file, matching `#lang at-exp racket` semantics.

<img src="docs/example.png" width="956" alt="Racket source highlighted by this package, using the Monokai Pro Light colour scheme">

## Installation

Via Package Control: `Package Control: Install Package`, then choose
**Racket Enhanced**.

If the older `Racket` package is also installed, both claim the
`source.racket` scope and the `.rkt` extension. Remove it
(`Package Control: Remove Package`) or add `"Racket"` to `ignored_packages`
in `Preferences: Settings`. Tabs that were already open keep their previous
syntax; reopen them.

## Why another Racket package

The existing `Racket` package on Package Control is a Sublime Text 2
`.tmLanguage` file, unchanged since 2019. It has nine rules: comments,
strings, numbers, a short hand-written keyword list, and a `define`/`struct`
pattern. Everything else in a file is unscoped, so a colour scheme can do
little with it.

This package is a `.sublime-syntax` written from scratch for the Sublime
Text 4 engine. The differences are:

- **Complete generated identifier lists.** All 413 special
  forms, 2135 procedures and 125 values exported by the `racket` module
  (v9.3) are recognised; the lists are produced by a script that queries
  Racket itself, so they can be regenerated for any future Racket version.
- **Works with Sublime's editing features.** `Goto Symbol` (Ctrl/Cmd+R)
  lists every `define`d function and `struct` in the file. `Toggle Comment`
  (Ctrl/Cmd+/) inserts `; ` and block comment inserts `#| |#`. Pressing
  Enter after an open bracket indents; typing a close bracket outdents.
  Double-clicking `string->list` selects the whole identifier, not
  `string` alone, and `?` or `!` at the end of a name is part of the word.
- **Scopes depend on position, not just on the word.** The operator of a
  form is scoped as a special form (`define`, `let`), a builtin procedure
  (`map`), an operator (`+`), or a user function call (`my-fn`), while the
  same identifier used as an argument is scoped as a value. Binding sites
  are distinguished too: the name in `(define (add x y) …)` is a definition
  and `x`, `y` are parameters; `x` in `(let ([x 1]) …)` is a binding;
  `point`, `x`, `y` in `(struct point (x y))` are a type name and fields.
- **Every kind of literal Racket can read is recognised.** Comments in all
  three forms: `; line`, `#| block |#` (which may nest), and `#;` followed
  by one expression, which comments out exactly that expression, however
  many lines it spans. Strings of every flavour: `"plain"`, `#"bytes"`,
  `#rx"regexp"`, `#px"regexp"`, and `#<<EOF` here-strings. Characters
  like `#\a`, `#\space`, `#\x41`. Keywords like `#:name`. Numbers with
  prefixes such as `#x1F`, `#b101`, `#e1.5`, fractions `1/3`, and
  `+inf.0`. Quote marks `'`, `` ` ``, `,`, `,@`, `#'`, `#,`. Vector and
  hash literals `#(1 2)`, `#hash((a . 1))`. The old package handles only
  comments, plain strings and integers.

Known limitations can be found in the [dev docs](./docs/dev.md).
