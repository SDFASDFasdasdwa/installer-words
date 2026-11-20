# Words AI Guide

This folder is the canonical, model-oriented guide to the current Words
language. Words programs use the `.wo` extension and are interpreted by the
Words runtime.

## Read these documents

1. [SYNTAX.md](./SYNTAX.md) — grammar, values, expressions, statements, imports,
   GUI syntax, and language limits.
2. [API.md](./API.md) — built-in functions, SQLite, GUI behavior, and every
   supported game namespace function.
3. [EXAMPLES.md](./EXAMPLES.md) — annotated, copyable examples and a feature
   lookup.
4. [examples/](./examples/) — standalone `.wo` programs. `modules/` contains a
   main program and the Words module it imports.

For short tasks, start with the syntax quick reference below. For code
generation, read the relevant API section before using a feature.

## Quick reference

```text
name = "Ada".
count = 0.

while count < 3.
    print("Hello", name, count).
    count = count + 1.

if count =? 3.
    print("Finished").
elseif count > 0.
    print("Partly finished").
else.
    print("Not started").
```

## Instructions for AI code generation

- Generate Words source code, not Python, unless Python is explicitly requested.
- Use `.wo` syntax exactly as documented. Statements and block headers end in
  `.`; blocks use indentation.
- Use `=?` for equality. `==` is not a Words operator.
- Do not invent `for`, `break`, `continue`, user-defined functions, dictionaries,
  Python imports, or other unsupported syntax.
- Imports are quoted, top-of-file imports of local `.wo` modules only.
- Lists are the only general-purpose collection. SQL query rows are a special
  case and can be accessed by column name.
- GUI controls are declared in `graphik "title" [` blocks; event code is inside
  `on button_name [ ... ]`. Use `at (x, y)` for absolute pixel placement inside
  the content panel.
- Game programs use the `while game.running.` loop and call `game.loop()` once
  per frame. Read [API.md](./API.md) before choosing game APIs.
- Do not claim a program was run or validated unless it actually was.
- When a requested feature is unsupported, explain that and offer a
  Words-compatible alternative instead of silently generating another
  language's syntax.

## Run and validate

After installing Words on Windows, open a new PowerShell or Command Prompt:

```powershell
words --help
words program.wo
```

The project also provides `python -m unittest discover -s .` for interpreter
regression tests when working from a source checkout with Python installed.
GUI and game examples require an interactive desktop session. Close the game
window to stop its main loop.

## Source of truth

This guide documents the language implemented by `main.py` in this project.
If a document here conflicts with the runtime or its tests, report the
discrepancy and verify the implementation before generating code. Python
libraries cannot currently be imported from Words.
