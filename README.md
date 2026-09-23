# Simple Structure Notation

SSN is a very simple notation system that allows one to describe and represent most data structures. In addition, SDN (Simple Data Notation) is a version of SSN that allows you to represent small pieces of the data itself.

As of now this notation has no defined use but it may be used to:
- serve as a compact, human-readable way to describe data structures and relationships without needing full diagrams or complex schema languages.
- feed a machine-readable mini-language for representing data structures directly
- automatically generate diagrams/schemas

## Syntax

SSN 1.0 is made up of items, enumerations, and operators. Below is a list of these elements and how they are used to represent data structures.

### Expressions

`x`
Represented by any letter or word, an item works as an identifier for a singular piece of data in a structure.

To represent a collection of items that are not necessarily related to each other, but all participate in the same relationship towards another piece of data, SSN uses `(x,y,z)`. See the examples below for how this is used.

### Enumeration

`...`
Allows you to represent that the item or expression it follows (`x...`), or the expression it encompasses (`(expr)...`), exists more than once.

`n...` means that the item or expression can exist from a minimum of `n` times to, technically, infinity.

`n...m` means that the item or expression can exist from `n` to `m` times.

### Operators

`x ← y` means that `y` belongs to, is a child of, or participates in `x`. It can represent any sort of ownership, containment, or parent-child relationship between expressions.

## How to use

To properly represent data structures with SSN, you use items, enumeration, and operators to build expressions. You then combine those expressions until you have a single final expression representing the entire structure.

## Examples

Below are some examples with step-by-step instructions showing how the final expression is built.

### University Data

Imagine a database for a university. A university has courses. Each of these courses has students and a professor.

First, we must identify the master item. This can be the item that encompasses all the rest, or simply the item we want to organize the data structure around. In our case, everything is contained within the university, so we'll assign the university the item `u`.

This gives us our first expression:

`u`

Now the second expression will represent the courses, `c`. A course can have students `s` and a professor `p`. From this we can draw two conclusions:

* `c ← s` — students belong to the course.
* `c ← p` — the professor belongs to the course.

Since these both use the same relationship, we can merge them into:

`c ← (s,p)`

Now we only have one thing left to specify for courses. A course only has one professor, but can have any number of students:

`c ← (s...,p)`

Now that we have our two expressions, `u` for the university and `c ← (s...,p)` for a course, we can merge them like this:

`u ← (c ← (s...,p))`

This gives us a single expression representing the complete structure.

### Other Short Examples
`shop ← (customer..., order ← (item1..., payment))`
`user ← (name, email, post... ← (title, comment...))`

### FAQ

**Q: Why was the course expression put into parentheses?**

**A:** This is because, while `u ← c ← (s...,p)` could sometimes be interpreted correctly, it does not explicitly keep the course and its children together as a single expression.

For example, let's say a university has courses and has recently introduced late-night lessons `l` for adults.

If we write:

`u ← c ← (s...,p)`

there is no clear way to add `l` as another child of the university without making the structure ambiguous.

By grouping the course expression with parentheses, we can write:

`u ← (c ← (s...,p), l)`

Here, it is clear that both the complete course structure and `l` belong directly to `u`.

## SDN

SDN works the same as SSN in its syntax, except that it introduces instances.

You write an instance of data like this:

`x¹`, `x²`, and so on.

This allows you to represent individual instances of the same type of item or data structure rather than only describing its general structure.
