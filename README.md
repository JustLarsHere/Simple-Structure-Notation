# SNS - Simple Notation System 1.1
SNS is a very simple notation system that allows one to describe relational and data-driven structures.

Since 1.1, SNS has been split into two sub-languages to better encompass more fields:
- SSN (Simple Structure Notation) is meant to describe any sort of relational graph, such as social networks, knowledge graphs, semantic trees, ...
- SDN (Simple Data Notation) is meant to target databases and other forms of data structures.

Regardless of the language, SNS features the following syntax rules:
- All data relationships are written in an RTL format to avoid confusion (e.g. `a←(b,c)`, not `b→a←c`).
- Expressions are built from labels/operands (e.g. `u`, `user`), bound together with operators (such as `←`), and grouped with `()`.

## Simple Structure Notation
### Labels
Represent the different objects in a structure. In a family tree, this might be each individual person (`Mark`, `Jenny`, ...) or the different fulfilled roles.

### Operators
- `x←y` establishes a direct relationship between the operands, where `y` is subjected to, a child of, or a descendant of `x`.
- `(x,y,z)` groups multiple items together. This is useful, for example, when a parent has more than one child: `x←(y,z)`.
- `x : R` defines a specific role that the item has.
- `x|y` establishes that either `x` or `y` can fill the given position.
- `x&y` establishes that `x`, `y`, or both may fill the given position.
- `x ←p (expr)` establishes a recursive relationship in which occurrences of `x` inside the expression refer back to the same structure, allowing the relationship to propagate recursively.
- `x?` / `(expr)?` establishes that the item or group might not exist, or that it is unclear whether it is present.
- `x↔y` establishes that the two items are bidirectionally related.
- `[x,y,z]` establishes that all items in the group are related to each other.
- `x...` establishes that a construct can occur multiple times.

### Combined operator examples
Sometimes more than one operator acts upon an item or expression. Here are some examples:
- `parents ← ((x|y)?)` — the item might not exist, and when present it may be either `x` or `y`.
- `parents? : R` — the item might not exist and, when present, has role `R`.

### Examples
- `filesystem ← (folder... ←p (folder...,file...), file...)` — a filesystem in which folders may recursively contain further folders and files.

### Extra Notes
Due to the complexity of graphs and graph-like relationships, a parser may sometimes fail to parse an especially complex expression or may produce an unexpected result. It is difficult to encompass all possible graph combinations without introducing too many operators or making their interactions excessively complicated.

In SSN, a label refers to the same item everywhere it appears within a structure. Reusing a label does not create a new item. For example:

```
a←b
a←c
```

Both b and c are related to the same a.
## Simple Data Notation
### Labels
Represent the different components of a data structure. In a user database, these might include `uuid`, `username`, `email`, `password_hash`, and other fields belonging to a `user`.

### Operators
- `x←y` establishes a direct relationship between the operands, where `y` belongs to `x`.
- `(x,y,z)` groups multiple items together. This is useful, for example, when a data structure contains multiple fields: `x←(y,z)`.
- `x...n` establishes that the data item may hold up to `n` instances or values.
- `x : T` defines the type of the item.
- `x|y` establishes that either `x` or `y` can fill the given position.
- `x?` establishes that the item is nullable.

### Combined operator examples
Sometimes more than one operator acts upon an operand or expression. Here are some examples:
- `user ← ((x|y)?)` — the item may be null and, when present, may be either `x` or `y`.
- `2fa? : T` — the item is nullable and has type `T`.

### Examples
- `user ← (uuid : int, username : string, 2fa? : bool)` — a user data structure containing an integer UUID, a string username, and a nullable Boolean `2fa` field.

### Extra Notes
Due to the diversity of existing data formats, SDN does not strictly differentiate between categories and items. This is not an accidental limitation, but an intentional feature. It is up to the user to define whether an item that contains other items represents an array, an object, a record, a placeholder name, or another kind of structure.

Unlike SSN, repeated labels in SDN do not inherently refer to the same item. Each occurrence starts its own independent data branch unless the surrounding structure explicitly defines otherwise. For example:

```
a←b
a←c
```


describes two separate a structures: one containing b and another containing c.
