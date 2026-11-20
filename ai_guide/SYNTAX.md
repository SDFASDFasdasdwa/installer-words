# Words Syntax Reference

Words is a small, case-sensitive, English-keyword language. Source files are
UTF-8 text files, conventionally named with the `.wo` extension.

## Program structure

A program is a sequence of statements. Blank lines are allowed. Imports, when
present, must be at the beginning of the file.

```text
import "helpers".

message = "Hello".
print(message).
```

Simple statements end with a period (`.`). A period is also required after
`if`, `elseif`, `else`, and `while` headers. The indented body starts on the
next line.

```text
if ready.
    print("Go").
```

Indentation defines blocks. Tabs are expanded to four spaces. Each dedent must
match an earlier indentation level.

## Comments

```text
// Single-line comment.

/!
Block comment.
It can cover more than one line.
//!
```

The opening and closing block markers are `/!` and `//!`.

## Names and case

Identifiers match `[A-Za-z_][A-Za-z0-9_]*`. Names are case-sensitive:
`score` and `Score` are different.

Boolean literals are `true` and `false`. Words has no `null` or `None` literal.
Avoid using language keywords as variable names. Built-in namespaces such as
`random`, `sql`, `game`, `draw`, `key`, `mouse`, `collision`, and `physics` are
reserved for their built-in behavior.

## Values and literals

| Value kind | Examples | Notes |
| --- | --- | --- |
| Integer | `0`, `42`, `-7` | A leading sign is a unary operator. |
| Decimal | `3.14`, `-0.5` | Exponential notation such as `1e3` is unsupported. |
| Text | `"hello"`, `'hello'` | Single-line, single- or double-quoted strings. |
| Boolean | `true`, `false` | Lowercase only. |
| List | `[]`, `[1, "two", [3]]` | Lists can contain mixed supported values. |

Strings use Python-compatible literal escapes such as `\n`, `\t`, `\\`, and
`\"` (or `\'` in a single-quoted string). A string cannot span source lines.

Words does not provide general dictionary literals. SQL query rows are
dictionary-like special values.

## Variables and assignment

Assignment creates or replaces a variable:

```text
score = 10.
score = score + 5.
```

The left side must be a variable or a list element:

```text
values = [10, 20].
values[1] = 25.
```

You cannot assign to a module member or function call. Assignment uses `=`;
`==` is not supported.

## Lists and indexing

```text
items = ["red", "green", "blue"].
first = items[0].
last = items[-1].
items.append("gold").
```

Indexes must be integers (not booleans). Indexing supports negative indexes.
An out-of-range index is a runtime error. List methods currently supported are
`append(value)` and indexed assignment. There is no built-in `len`, `pop`, or
general-purpose dictionary.

## Expressions and operators

Supported binary operators, from lowest to highest precedence:

| Precedence | Operators | Meaning |
| --- | --- | --- |
| 1 | `or` | Logical OR, short-circuiting |
| 2 | `and` | Logical AND, short-circuiting |
| 3 | `=?`, `!=`, `<`, `<=`, `>`, `>=` | Equality and comparisons |
| 4 | `+`, `-` | Addition and subtraction |
| 5 | `*`, `/`, `%` | Multiplication, division, remainder |
| 6 | `**` | Exponentiation |

Unary operators: `+`, `-`, and `not`. Exponentiation associates right to left:
`2 ** 3 ** 2` means `2 ** (3 ** 2)`. Use parentheses to make precedence clear.
Equality is `=?`, for example `answer =? 42`.

`and` and `or` short-circuit and return a boolean result. Arithmetic and
comparison operations generally follow Python value behavior; incompatible
types or invalid arithmetic produce a Words runtime error.

## Calls, members, and indexes

Expressions support function calls, member access, list creation, and indexing:

```text
print(number("42")).
result = calculate("2 + 3").
message = module_name.value.
item = items[0].
```

Member access is available on imported Words modules and supported built-in
namespaces/objects. Arbitrary Python object access is not a language feature.

## Output

`print` is a statement with zero or more comma-separated expressions:

```text
print().
print("name:", "Ada", 42).
```

Values are printed separated by spaces.

## Conditions

```text
if score >= 90.
    print("Excellent").
elseif score >= 60.
    print("Passed").
else.
    print("Try again").
```

`elseif` can repeat; `else` is optional and appears last. Conditions use the
language's truthiness behavior. There is no `switch` or `match` statement.

## Loops

```text
count = 0.
while count < 3.
    print(count).
    count = count + 1.
```

Only `while` loops are supported. There is no `for`, `break`, or `continue`.

## Imports

Imports are quoted and must appear before executable statements:

```text
import "helpers".
import "tools/formatters.wo".
print(helpers.message).
```

The `.wo` suffix is optional. The imported module is named after its file stem;
its variables are accessed as `module_name.variable_name`. Module paths are
relative to the importing Words file and must resolve inside the folder of the
originally started program. Absolute paths and paths outside that folder are
rejected. A module is executed once per program run; circular imports are
errors.

`import "math".` does not import Python's `math` library. Words currently
supports imports of local Words files only.

## GUI statement grammar

```text
graphik "Window title" [
    text heading = "Welcome".
    textbox visitor = "".
    button greet = "Greet".
    on greet [
        set(heading, "Hello " + read(visitor)).
    ]
]
```

The GUI block contains element declarations, optional color declarations, and
optional button event blocks.

Supported elements:

```text
text name = "Label".
textbox name = "Initial text".
textarea name = "Initial multiline text".
button name = "Button label".
image name = "relative-or-absolute-image-path".
```

Names must be unique within the window. Element values must evaluate to text.
Image paths are relative to the current `.wo` source file (or can be absolute).
`on name [` attaches one event block to a declared button; it is an error to
attach it to another element.

Optional element properties can follow the value in any order:

```text
text heading = "Hello" foreground "#FFFFFF" background "#202830" at (20, 12).
button ok = "Continue" at (30, 70).
```

`foreground` and `background` require a six-digit hex color string such as
`"#315EFB"`. `at (x, y)` requires two non-negative integer expressions and
places that element at pixel coordinates relative to the top-left of the GUI
content panel. Elements without `at` retain the automatic vertical layout.
Positioned elements can overlap other elements; coordinate layout is not a
constraint solver.

Colors can be set for the whole window:

```text
graphik "Window" [
    color window = "#101820".
    color surface = "#202830".
    color accent = "#315EFB".
    text title = "Ready".
]
```

Valid color roles: `window`, `surface`, `border`, `title`, `subtitle`, `text`,
`muted`, `footer`, `input`, `input_text`, `button`, `button_text`,
`button_hover`, `accent`, `accent_text`, `accent_hover`, and
`accent_pressed`.

GUI blocks are statements; do not put a period after the closing `]`. GUI
requires Tkinter and an interactive desktop session.

## Statement grammar sketch

This sketch summarizes the supported statement shapes; it is not a formal
parser grammar:

```text
program       := import* statement*
import        := 'import' STRING '.'
assignment    := target '=' expression '.'
target        := IDENT | expression '[' expression ']'
print         := 'print' '(' [expression (',' expression)* [',']] ')' '.'
expression    := literals, lists, variables, calls, members, indexes,
                 unary operators, or binary operators
if            := 'if' expression '.' INDENT statement+ DEDENT
                 ('elseif' expression '.' INDENT statement+ DEDENT)*
                 ['else' '.' INDENT statement+ DEDENT]
while         := 'while' expression '.' INDENT statement+ DEDENT
start         := 'start' '(' expression ')' '.'
graphik       := 'graphik' STRING '[' GUI declarations/events ']'
```

There are no user-defined function or class declarations.
