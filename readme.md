# Words

Words ist eine englischsprachige Programmiersprache für kleine Programme,
Werkzeuge, Grafikanwendungen und einfache Spiele auf Windows.

Ein Programm wird in einer UTF-8-kodierten Datei mit der Endung `.wo`
geschrieben. Nach der Installation öffnet der Words-Interpreter diese Dateien
durch einen Doppelklick.

## Installation

1. Lade den aktuellen Windows-Installer herunter.
2. Starte `WordsInstaller.exe`.
3. Wähle den Installationsordner.
4. Bestätige die Installation für den aktuellen Windows-Benutzer.

Für die Nutzung von Words wird kein separates Python installiert. Der Installer
legt den benötigten Interpreter mit `.wo`-Dateizuordnung an.

Eine `.wo`-Datei nur aus vertrauenswürdigen Quellen öffnen. Beim Doppelklick
wird der enthaltene Programmcode ausgeführt.

## Erstes Programm

Erstelle eine Datei namens `hello.wo`:

```text
name = "Ada".

print("Hello", name).
```

Öffne die Datei mit einem Doppelklick oder über „Öffnen mit“ im Windows-
Explorer.

## Schnellstart

```text
name = "Ada".
count = 0.

while count < 3.
    print("Hello", name, count).
    count = count + 1.

if count =? 3.
    print("Finished!").
elseif count > 0.
    print("Partly finished.").
else.
    print("Not started.").
```


## Grundregeln

- Die Sprache unterscheidet Gross- und Kleinschreibung.
- Zuweisungen und Funktionsaufrufe enden mit einem Punkt.
- Bedingungs- und Schleifenköpfe enden ebenfalls mit einem Punkt.
- Danach folgt ein eingerückter Block.
- Ein Tabulator zählt wie vier Leerzeichen.
- Text steht in einfachen oder doppelte Anführungszeichen.
- Kommentare beginnen mit `//`.
- Mehrzeilige Kommentare beginnen mit `/!` und enden mit `//!`.

```text
// Dies ist ein Kommentar.
/! Dies ist ein
mehrzeiliger Kommentar. //!
```

## Werte und Variablen

Words unterstützt ganze Zahlen, Dezimalzahlen, Text, Wahrheitswerte und Listen:

| Typ | Beispiele |
| --- | --- |
| Ganze Zahl | `0`, `42`, `-7` |
| Dezimalzahl | `3.14`, `-0.5` |
| Text | `"Hello"`, `"Ada"` |
| Wahrheitswert | `true`, `false` |
| Liste | `[1, 2, 3]`, `[]` |

```text
score = 10.
score = score + 5.
message = "Score:".
print(message, score).
```

`=` weist einen Wert zu. Vergleiche verwenden `=?` statt `==`.

## Operatoren

| Operator | Bedeutung |
| --- | --- |
| `or`, `and`, `not` | Logische Verknüpfungen |
| `=?`, `!=`, `<`, `<=`, `>`, `>=` | Vergleiche |
| `+`, `-` | Addition, Subtraktion |
| `*`, `/`, `%` | Multiplikation, Division, Rest |
| `**` | Potenzierung |

```text
result = (2 + 3) * 4.
equal = result =? 20.
print(equal).
```

Klammern legen die Berechnungsreihenfolge fest.

### Zahlen aus Text

```text
text = "12".
age = number(text).
print(age + 1).

result = calculate("2 + 3 * (4 - 1)"). 
print(result).
```

`number(...)` wandelt numerischen Text in eine Zahl um.
`calculate(...)` wertet einen begrenzten mathematischen Ausdruck aus.

## Ausgabe

`print(...)` gibt Werte aus. Mehrere Werte werden mit Leerzeichen getrennt:

```text
print("Hello").
print("Name:", "Ada", "Score:", 42).
```

## Bedingungen

```text
score = 75.

if score >= 90.
    print("Excellent").
elseif score >= 60.
    print("Passed").
else.
    print("Try again").
```

`elseif` und `else` sind optional.

## Schleifen

```text
count = 0.

while count < 3.
    print(count).
    count = count + 1.
```

Aktuell gibt es keine `for`-Schleife sowie keine `break`- oder
`continue`-Anweisung.

## Listen

```text
numbers = [10, 20, 30].
print(numbers[0]).
print(numbers[-1]).

numbers[1] = 25.
numbers.append(40).
print(numbers).
```

Listenindizes beginnen bei `0`. Negative Indizes zählen vom Ende aus.
`append(...)` fügt am Ende ein Element hinzu.

## Words-Module

Eigene Words-Dateien können mit `import` eingebunden werden:

```text
import "helpers".
print(helpers.message).
```

Die Endung `.wo` kann angegeben oder weggelassen werden:

```text
import "tools/helpers.wo".
print(helpers.message).
```

Der Modulname entspricht dem Dateinamen ohne `.wo`. Werte werden über
`modulname.wert` verwendet:

```text
// helpers.wo
message = "Hello from a module".
```

Importzeilen müssen am Anfang des Programms stehen. Ein Modul wird pro
Programmlauf einmal geladen. Importpfade müssen innerhalb des Ordners des
gestarteten Programms liegen. Absolute Pfade und Pfade ausserhalb dieses
Ordners sind nicht erlaubt.

Python-Bibliotheken können in der aktuellen Words-Version noch nicht mit
`import` geladen werden.

## Zufallszahlen

```text
print(random.random()).      // Zahl zwischen 0 und 1
print(random.random(1, 10)). // ganze Zahl im Bereich
```

`random.random()` liefert eine Dezimalzahl. Mit zwei Ganzzahlen liefert
`random.random(min, max)` eine ganze Zahl zwischen beiden Grenzen.

## SQLite-Datenbanken

Words unterstützt SQLite:

```text
database = sql.open(":memory:").
database.execute("CREATE TABLE people (name TEXT, age INTEGER)"). 
database.execute(
    "INSERT INTO people (name, age) VALUES (?, ?)",
    ["Ada", 36],
).

rows = database.query(
    "SELECT name, age FROM people WHERE age > ?",
    [30],
).

print(rows[0]["name"], rows[0].age).
database.close().
```

Für eine Datenbankdatei:

```text
database = sql.open("data.sqlite").
```

`query(...)` liest Daten und gibt eine Liste von Zeilen zurück.
`execute(...)` verändert Daten. SQL-Werte immer über die optionale Parameterliste
übergeben und Platzhalter `?` verwenden.

Dateipfade müssen relativ zum Programmordner bleiben. SQLite-Erweiterungen,
verbundene Datenbanken sowie andere Datenbanktypen werden nicht unterstützt.

## Grafische Oberflächen

```text
graphik "Greeting" [
    text heading = "What is your name?".
    textbox visitor = "".
    button greet = "Say hello".
    text result = "Waiting for input.".

    on greet [
        set(result, "Hello " + read(visitor)).
    ]
]
```

Unterstützte Elemente:

| Element | Beschreibung |
| --- | --- |
| `text` | Textanzeige |
| `textbox` | einzeiliges Eingabefeld |
| `textarea` | mehrzeiliges Eingabefeld |
| `button` | Schaltfläche |
| `image` | Bilddatei |

Wichtige GUI-Funktionen:

- `read(field_name)` liest den Inhalt eines Eingabefelds.
- `set(widget_name, value)` ersetzt einen angezeigten Wert.
- `append(field_name, value)` hängt Text an ein Eingabefeld an.
- `clear()` leert alle Eingabefelder.
- `update()` aktualisiert das Fensterlayout.

Farben können mit `color` festgelegt werden:

```text
graphik "Colours" [
    color window = "#101820".
    color surface = "#202830".
    color accent = "#315EFB".
    text heading = "Welcome" foreground "#FFFFFF" background "#202830".
    button continue = "Continue".
]
```

Ein vollständiges GUI-Beispiel steht in [gui_example.wo](./gui_example.wo).
Für Grafiken wird eine verfügbare Windows-Desktop-Sitzung und Tkinter benötigt.

## Spiele

Für einfache 2D-Spiele stehen Canvas-Zeichenfunktionen und Eingaben zur Verfügung:

```text
game.create(640, 360, "Words game").
game.set_fps(60).
game.background("#182030").

x = 40.
y = 120.
speed = 160.

while game.running.
    game.loop().
    x = physics.velocity(x, speed, game.delta_time()).

    if key.down("left").
        x = x - speed * game.delta_time().

    game.clear().
    draw.rect(x, y, 40, 40, "#55AAFF").
    draw.text(12, 12, "Move with the arrow keys", "#FFFFFF").
    game.draw().
```

Verfügbare Bereiche umfassen unter anderem:

- `game.*` – Fenster, Loop, Zeit und FPS
- `draw.*` – Rechtecke, Kreise, Linien, Text und Bilder
- `key.*` – Tastenzustände
- `mouse.*` – Mausposition und Klicks
- `collision.*` – Überschneidungsprüfungen
- `physics.*` – Bewegung, Geschwindigkeit und Beschleunigung

Ein ausführbares Beispiel ist [game_example.wo](./game_example.wo).

## Dateien und Websites öffnen

```text
open("C:\\Users\\Ada\\Documents").
open("https://example.com").
```

Pfad- und URL-Text müssen gültig sein. Eine Datei oder Website wird mit der
unter Windows zugeordneten Anwendung geöffnet.

## Kommentare

Ein Kommentar bis zum Zeilenende:

```text
// Dieses Zeichen entfernt den Rest der Zeile aus der Ausführung.
```

Ein Kommentar über mehrere Zeilen:

```text
/!
Mehrere Kommentarzeilen.
//! 
```

## Aktuelle Grenzen

- Keine benutzerdefinierten Funktionen.
- Keine `for`-Schleifen, `break`- oder `continue`-Anweisungen.
- Keine allgemeinen Wörterbücher; SQL-Zeilen liefern allerdings benannte Werte.
- Keine Python-Bibliotheken über `import`.
- SQL ist auf SQLite beschränkt.
- Die GUI ist einfach und vertikal angeordnet.
- Graphische Anwendungen benötigen eine Desktop-Sitzung.

## Fehler

Fehler enthalten Zeile und Spalte der Quelle. Häufige Ursachen sind:

- fehlender Punkt am Ende einer Anweisung,
- falsche Einrückung,
- unbekannter Variablen-, Modul- oder Funktionsname,
- unzulässiger Datei- oder Verzeichnispfad,
- falscher Werttyp,
- nicht geschlossene Kommentare oder Zeichenketten.
