# Statements

A *statement* is a component of a [block], which is in turn a component of an
outer [expression] or [function].

Rust has two kinds of statement: [declaration
statements](#declaration-statements) and [expression
statements](#expression-statements).

## Declaration statements

A *declaration statement* is one that introduces one or more *names* into the
enclosing statement block. The declared names may denote new variables or new
[items][item].

The two kinds of declaration statements are item declarations and `let`
statements.

### Item declarations

An *item declaration statement* has a syntactic form identical to an
[item declaration][item] within a [module]. Declaring an item within a statement
block restricts its scope to the block containing the statement. The item is not
given a [canonical path] nor are any sub-items it may declare. The exception to
this is that associated items defined by [implementations] are still accessible
in outer scopes as long as the item and, if applicable, trait are accessible.
It is otherwise identical in meaning to declaring the item inside a module.

There is no implicit capture of the containing function's generic parameters,
parameters, and local variables. For example, `inner` may not access
`outer_var`.

```rust
fn outer() {
  let outer_var = true;

  fn inner() { /* outer_var is not in scope here */ }

  inner();
}
```

### `let` statements

A *`let` statement* introduces a new set of variables, given by a pattern. The
pattern may be followed by a type annotation, and/or an initializer expression.
When no type annotation is given, the compiler will infer the type, or signal
an error if insufficient type information is available for definite inference.
Any variables introduced by a variable declaration are visible from the point of
declaration until the end of the enclosing block scope.

## Expression statements

An *expression statement* is one that evaluates an [expression] and ignores its
result. As a rule, an expression statement's purpose is to trigger the effects
of evaluating its expression. An expression that consists of only a [block
expression][block] or control flow expression and that does not end a block 
can also be used as an expression statement by omitting the trailing semicolon.

```rust
# let mut v = vec![1, 2, 3];
v.pop();          // Ignore the element returned from pop
if v.is_empty() {
    v.push(5);
} else {
    v.remove(0);
}                 // Semicolon can be omitted.
[1];              // Separate expression statement, not an indexing expression.
```

[block]: expressions/block-expr.html
[expression]: expressions.html
[function]: items/functions.html
[item]: items.html
[module]: items/modules.html
[canonical path]: path.html#canonical-paths
[implementations]: items/implementations.html