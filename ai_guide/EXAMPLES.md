# Words Examples

Each section is copyable Words code. Runnable files are also provided in
`examples/`. Do not add Python syntax to `.wo` files.

## Hello, variables, conditions, and loops

```text
name = "Ada".
count = 0.

while count < 3.
    print("Hello", name, count).
    count = count + 1.

if count =? 3.
    print("Finished").
else.
    print("Unexpected result").
```

## Lists and numeric conversion

```text
values = [10, 20, 30].
values.append(number("40")).
values[0] = values[0] + 1.
print(values).
print("calculation:", calculate("2 * (3 + 4)")).
```

## Local Words modules

`examples/modules/main.wo`:

```text
import "greeting".
print(greeting.message).
```

`examples/modules/greeting.wo`:

```text
message = "Hello from a local Words module".
```

Imports must be at the beginning of the importing file. Python modules cannot
be imported this way.

## SQLite

```text
db = sql.open(":memory:").
db.execute("CREATE TABLE tasks (title TEXT, done INTEGER)").
db.execute("INSERT INTO tasks (title, done) VALUES (?, ?)", ["Write docs", 0]).
rows = db.query("SELECT title, done FROM tasks WHERE done = ?", [0]).
print(rows[0].title).
db.close().
```

Use parameter lists and `?` placeholders for values. `query` is read-only;
`execute` is for changes.

## Positioned GUI

```text
graphik "Greeting" [
    color window = "#101820".
    color surface = "#202830".
    color accent = "#315EFB".

    text prompt = "Your name" at (20, 20).
    textbox visitor = "" at (20, 52).
    button greet = "Say hello" at (20, 100).
    text result = "Waiting" at (20, 150).

    on greet [
        set(result, "Hello " + read(visitor)).
    ]
]
```

Coordinates are non-negative integer pixels relative to the content panel's
top-left corner. Elements without `at` use vertical automatic layout. GUI
programs need an interactive desktop session.

## Game loop and keyboard input

```text
game.create(640, 360, "Move the square").
game.set_fps(60).
game.background("#182030").
x = 280.
y = 140.
speed = 180.

while game.running.
    game.loop().
    if key.down("left").
        x = physics.velocity(x, -speed, game.delta_time()).
    elseif key.down("right").
        x = physics.velocity(x, speed, game.delta_time()).
    game.clear().
    draw.rect(x, y, 48, 48, "#55AAFF").
    draw.text(16, 16, "Use left and right arrow keys", "#FFFFFF").
    game.draw().
```

Close the game window to end `while game.running.`.

## Common invalid patterns

Invalid because Words has no `for` loop:

```text
for item in items.
    print(item).
```

Use a `while` loop and a manually updated index instead:

```text
items = ["a", "b", "c"].
index = 0.
while index < 3.
    print(items[index]).
    index = index + 1.
```

Invalid equality syntax:

```text
if answer == 42.
    print("yes").
```

Use `=?`:

```text
if answer =? 42.
    print("yes").
```

Invalid Python import:

```text
import "math".
```

Only a local Words file can be imported. For square roots, `calculate` does
not support named functions; use a different Words-compatible calculation or
implement an appropriate algorithm using supported arithmetic.

## Runnable example files

- [basics.wo](./examples/basics.wo)
- [sqlite.wo](./examples/sqlite.wo)
- [gui_positions.wo](./examples/gui_positions.wo)
- [game_loop.wo](./examples/game_loop.wo)
- [modules/main.wo](./examples/modules/main.wo)
- [modules/greeting.wo](./examples/modules/greeting.wo)
