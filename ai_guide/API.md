# Words Built-in API

This document lists the built-ins implemented by the current runtime. All
examples are Words source code.

## Core functions

### `print(value, ...)`

Prints zero or more values separated by spaces.

```text
print("Score:", 42).
```

### `number(value)`

Converts numeric text to an integer or decimal number. Numeric inputs are
returned unchanged. Booleans and non-finite numbers are rejected.

```text
count = number("12").
price = number("3.50").
```

### `calculate(text)`

Evaluates a restricted arithmetic expression and returns a number. Accepted
syntax: numeric literals, parentheses, unary `+`/`-`, `+`, `-`, `*`, `/`, `//`,
`%`, and `**`. Names, calls, and general Python expressions are not evaluated.

Limits: 256 characters, 64 syntax nodes, exponent absolute value at most 100,
integer results at most 4096 bits, and finite numeric results only.

```text
result = calculate("2 + 3 * (4 - 1)").
```

### `start(path_or_url)`

Opens a file with the operating system's associated application or opens an
HTTP/HTTPS URL in the default browser. A relative path is resolved from the
current `.wo` file. Missing files and unsupported URL schemes are errors.

```text
start("notes.txt").
start("https://example.com").
```

## Random namespace

### `random.random()` and `random.random(lower, upper)`

With no bounds, returns a random decimal value in `[0, 1)`. With two numeric
bounds, returns a random value in the range. If both bounds are integers, the
result is an integer and both endpoints are inclusive; otherwise it is a
decimal number.

```text
fraction = random.random().
dice = random.random(1, 6).
```

The lower bound cannot exceed the upper bound. Bounds must be finite numbers
and cannot be booleans.

### `random.mous()` and `random.mous(lower, upper)`

Returns a random value seeded from recent physical mouse movement. Available
only on Windows. The program starts collecting movement when this API is
present and may wait for a mouse movement before it can return a value. The
optional bounds follow the same integer/decimal behavior as `random.random`.
Do not use this as a security or cryptographic random source.

`random.hot()` is not available; the feature was deferred.

## SQLite namespace and database objects

### `sql.open(path)`

Opens a SQLite database and returns a database object. Use `":memory:"` for an
in-memory database. File paths must be relative to the started program folder
and cannot escape it.

```text
db = sql.open(":memory:").
```

### `db.execute(sql, [parameters])`

Executes one SQL statement that does not return rows. Changes are committed
immediately. Returns a row-like value with `changes` and `last_id`.

### `db.query(sql, [parameters])`

Runs a read-only query and returns a list of rows. Each row supports both
string-key indexing and member syntax:

```text
rows = db.query("SELECT name FROM people WHERE age > ?", [18]).
print(rows[0]["name"]).
print(rows[0].name).
```

Parameters must be supplied as a Words list. Use `?` placeholders; do not
concatenate values into SQL strings.

`query` permits only read operations that return rows. `execute` must not return
rows. `ATTACH`, `DETACH`, `PRAGMA`, and SQLite extension loading are blocked.
PostgreSQL and MySQL are unsupported.

### `db.close()`

Closes the connection. Connections are also closed when the program exits.

## GUI functions

These are used from a GUI block or one of its button handlers.

### `read(field_name)`

Reads the current text from a `textbox` or `textarea`. The name can be an
unquoted identifier or a string. Exactly one argument is required.

```text
name = read(visitor).
name = read("visitor").
```

### `set(widget_name, value)`

Replaces the text in a `text`, `button`, `textbox`, or `textarea`. The value is
converted to text. Images cannot be changed with `set`.

### `append(field_name, value)`

Appends text to a `textbox` or `textarea`. The value is converted to text.

### `clear()`

Clears all textboxes and textareas in the open GUI.

### `update()`

Updates pending GUI layout/draw work. Requires an open GUI.

## Game namespaces

Games use a single Tkinter Canvas. Create the window before calling drawing,
input, or timing APIs. `game.loop()` processes events and updates frame timing.

### `game`

| API | Arguments | Result / behavior |
| --- | --- | --- |
| `game.create(width, height, title)` | integer dimensions `1..8192`, text title | Creates one non-resizable game window. |
| `game.running` | property, no parentheses | `true` while the window is open. |
| `game.loop()` | none | Processes one frame's events, waits to respect the FPS limit, updates `delta_time` and `fps`. |
| `game.set_fps(limit)` | number from `1..240` | Sets the frame-rate cap. |
| `game.delta_time()` | none | Seconds since the previous frame. |
| `game.fps()` | none | Measured frames per second. |
| `game.background(color)` | `#RRGGBB` | Sets the Canvas background. |
| `game.clear()` | none | Removes all Canvas items. |
| `game.draw()` | none | Flushes pending Canvas drawing. |
| `game.update()` | none | Processes Tk events and redraws immediately. |
| `game.close()` | none | Closes the game window. |

Typical frame loop:

```text
game.create(640, 360, "Demo").
while game.running.
    game.loop().
    game.clear().
    draw.text(16, 16, "Frame", "#FFFFFF").
    game.draw().
```

### `draw`

Coordinates use pixels from the top-left of the game Canvas. Colors must be
six-digit hex values.

| API | Arguments |
| --- | --- |
| `draw.rect(x, y, width, height, color)` | Rectangle origin, size, fill color |
| `draw.circle(x, y, radius, color)` | Center, radius, fill color |
| `draw.line(x1, y1, x2, y2, color [, width])` | Endpoints, color, optional positive width |
| `draw.polygon([x1, y1, x2, y2, x3, y3, ...], color)` | At least three coordinate pairs and fill color |
| `draw.text(x, y, text, color [, size])` | Text/number, color, optional size `1..256` (default 16) |
| `draw.image(x, y, path)` | Image at top-left; path relative to the `.wo` file |
| `draw.sprite(x, y, path)` | Alias for `draw.image` |
| `draw.pixel(x, y, color)` | One pixel; coordinates must be whole numbers |

`game.clear()` removes images and shapes too; draw them again on later frames.
Supported image formats depend on Tkinter's `PhotoImage` support (commonly GIF,
PNG, PGM, and PPM).

### `key`

Each function takes one key name:

- `key.down(name)` — currently held.
- `key.pressed(name)` — pressed since the last frame.
- `key.released(name)` — released since the last frame.

Common aliases: `"left"`, `"right"`, `"up"`, `"down"`, `"space"`, `"enter"`,
`"esc"`, `"ctrl"`, and `"shift"`. Other Tk key names may be used.

### `mouse`

- `mouse.position()` returns `[x, y]` in Canvas coordinates.
- `mouse.clicked()` returns whether the primary button was clicked since the
  previous frame.
- `mouse.clicked(button)` accepts mouse button `1`, `2`, or `3`.

### `collision`

- `collision.rect(x1, y1, width1, height1, x2, y2, width2, height2)` returns
  whether the two axis-aligned rectangles overlap.
- `collision.circle(x1, y1, radius1, x2, y2, radius2)` returns whether two
  circles overlap or touch.

Sizes and radii cannot be negative.

### `physics`

These helpers return updated velocity or position values. They do not store
state; assign the result yourself.

- `physics.gravity(velocity, gravity, delta_time)` returns
  `velocity + gravity * delta_time`.
- `physics.velocity(position, velocity, delta_time)` returns
  `position + velocity * delta_time`.
- `physics.force(velocity, force, mass, delta_time)` returns
  `velocity + force / mass * delta_time`; mass must be positive.
- `physics.friction(velocity, friction, delta_time)` reduces the magnitude of
  velocity without reversing its direction.

Inputs must be finite numbers. Friction, mass, and elapsed time are validated
as described above; negative elapsed time is rejected.

## Features not available

- Python standard-library or third-party imports.
- User-defined functions, classes, and recursion through user-defined calls.
- `for`, `break`, or `continue`.
- General-purpose dictionaries.
- `random.hot()`.
- Non-SQLite database engines.
