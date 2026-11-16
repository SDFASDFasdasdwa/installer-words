# Words – Sprachreferenz

Words ist eine kleine englischsprachige Programmiersprache. Der Interpreter
steht in `main.py`; Words-Programme werden als UTF-8-kodierte `.wo`-Dateien
gespeichert.

## Inhaltsverzeichnis

- [Installation und Start](#installation-und-start)
- [Schnellstart](#schnellstart)
- [Grundregeln und Syntax](#grundregeln-und-syntax)
- [Werte, Variablen und Ausdrücke](#werte-variablen-und-ausdrücke)
- [Anweisungen und Kontrollfluss](#anweisungen-und-kontrollfluss)
- [Listen](#listen)
- [Module importieren](#module-importieren)
- [SQL-Datenbanken](#sql-datenbanken)
- [Grafische Oberflächen](#grafische-oberflächen)
- [Dateien und Websites öffnen](#dateien-und-websites-öffnen)
- [Zufallszahlen](#zufallszahlen)
- [Kommentare](#kommentare)
- [Fehler und Einschränkungen](#fehler-und-einschränkungen)

## Installation und Start

Words braucht Python. Für grafische Oberflächen muss außerdem Tkinter in der
Python-Installation enthalten sein.

Ein Programm aus dem Projektordner starten:

```powershell
python main.py example.wo
```

Alternativ kann ein anderer `.wo`-Dateipfad angegeben werden:

```powershell
python main.py pfad\zu\programm.wo
```

Ohne Dateinamen startet eine einfache interaktive Eingabeaufforderung:

```powershell
python main.py
```

Mit `exit` wird die Eingabeaufforderung beendet. Jede eingegebene Zeile wird
als eigenes Programm ausgeführt; Variablen bleiben nicht für die nächste Zeile
erhalten. Für mehrzeilige Programme eine `.wo`-Datei verwenden.

### Windows-Installer erstellen

Der Installer bündelt Words zusammen mit der Python-Laufzeit. Daher muss auf
dem Zielcomputer Python nicht separat installiert sein. Zum Erstellen wird
Python auf dem Entwicklungscomputer benötigt:

```powershell
python -m pip install -r requirements-build.txt
.\build_installer.ps1
```

Danach liegt `WordsInstaller.exe` im Ordner `dist`. Beim Start kann der
Installationsordner ausgewählt werden. Der Installer bietet außerdem an,
Words für den aktuellen Windows-Benutzer zu installieren oder die
`.wo`-Dateizuordnung zu entfernen. Die Installation benötigt keine
Administratorrechte. Bei einer vorhandenen Zuordnung fragt der Installer vor
dem Ersetzen. Die Zuordnung startet danach den gebündelten Words-Interpreter
aus dem gewählten Ordner. Das Words-Logo aus `assets\words-logo.png` wird auch
als Symbol für Installer, Interpreter und `.wo`-Dateien verwendet.

Nach dem Erstellen kann die Installationsoberfläche zum Testen auch mit Python
gestartet werden:

```powershell
python install_words.py
```

Dabei muss `dist\WordsRuntime.exe` vorhanden sein.

Der Installer ändert nur Einstellungen des aktuellen Benutzers. Falls Windows
weiterhin ein anderes Programm öffnet, Words in den Windows-Einstellungen
unter „Öffnen mit“ als Standard auswählen. Eine `.wo`-Datei kann Programmcode
ausführen; per Doppelklick daher nur Dateien starten, denen du vertraust.

Ein automatischer Updater ist nicht enthalten. Die angegebene Raw-Quelle
liefert verschleierten Python-Code, der beim Ausführen dynamisch entschlüsselt
und mit `exec` gestartet wird. Der Installer lädt diesen Code nicht herunter
und führt ihn nicht aus.

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

Die Beispiel-Datei [example.wo](./example.wo) zeigt weitere Funktionen wie
Imports, Listen, Zufallszahlen und Datenbankzugriff.

## Grundregeln und Syntax

- Words-Schlüsselwörter und eingebaute Namen sind englisch und
  gross-/kleinschreibungssensitiv: `if` ist ein Schlüsselwort, `If` nicht.
- Anweisungen wie Zuweisungen und `print(...)` enden mit einem Punkt (`.`).
- Bedingungs- und Schleifenköpfe enden ebenfalls mit einem Punkt. Danach folgt
  ein eingerückter Block.
- GUI-Blöcke und Ereignisblöcke werden mit eckigen Klammern `[` und `]`
  umschlossen.
- Blöcke werden mit Leerzeichen eingerückt. Ein Tabulator wird vom Interpreter
  wie vier Leerzeichen behandelt. Die Einrückung muss zu einer vorherigen
  Blockebene passen.
- Leere Zeilen und reine Kommentarzeilen sind erlaubt.
- Namen beginnen mit einem ASCII-Buchstaben oder `_`; danach sind Buchstaben,
  Ziffern und `_` erlaubt. Namen unterscheiden Gross- und Kleinschreibung.
- Text steht in einfachen oder doppelten Anführungszeichen. Unterstützt werden
  Python-kompatible Escape-Sequenzen wie `\n` und `\"`. Zeichenketten können
  nicht über mehrere Quelltextzeilen gehen.
- Zahlen sind ganze Zahlen oder Dezimalzahlen, zum Beispiel `12` und `3.5`.
  Exponentialschreibweise wie `1e3` wird nicht unterstützt.

Beispiel für einen eingerückten Block:

```text
if true.
    print("This line is inside the block.").
```

## Werte, Variablen und Ausdrücke

### Datentypen

Words unterstützt:

| Typ | Beispiele |
| --- | --- |
| Ganze Zahl | `0`, `42`, `-7` |
| Dezimalzahl | `3.14`, `-0.5` |
| Text | `"Hello"`, `'Words'` |
| Wahrheitswert | `true`, `false` |
| Liste | `[1, 2, 3]`, `[]` |

Variablen werden durch Zuweisung erstellt und können später neu zugewiesen
werden:

```text
score = 10.
score = score + 5.
message = "Score:".
print(message, score).
```

`=` bedeutet Zuweisung. Der Gleichheitsvergleich lautet `=?`; `==` gehört nicht
zur Words-Syntax.

### Operatoren

Operatoren sind in der Tabelle von der niedrigsten zur höchsten Priorität
aufgeführt:

| Operatoren | Bedeutung |
| --- | --- |
| `or` | Logisches ODER |
| `and` | Logisches UND |
| `=?`, `!=`, `<`, `<=`, `>`, `>=` | Gleichheit und Vergleiche |
| `+`, `-` | Addition und Subtraktion |
| `*`, `/`, `%` | Multiplikation, Division und Restwert |
| `**` | Potenzierung |

Unäres `+`, unäres `-` und `not` sind ebenfalls verfügbar. Potenzierung
verknüpft sich von rechts: `2 ** 3 ** 2` wird als `2 ** (3 ** 2)` ausgewertet.
`and` und `or` werten ihre rechte Seite nur bei Bedarf aus.

Klammern können die Reihenfolge festlegen:

```text
result = (2 + 3) * 4.
```

Einige Operatoren nutzen direkt die passenden Python-Operationen. Dadurch
funktioniert zum Beispiel `"Hello " + "Words"` als Textverkettung. Nicht
passende Typen oder ungültige Rechenoperationen erzeugen einen Laufzeitfehler.

### Zahlen aus Text erzeugen

`number(value)` wandelt numerischen Text in eine ganze Zahl oder Dezimalzahl
um. Bereits numerische Werte werden unverändert zurückgegeben:

```text
text = "12".
age = number(text).
price_text = "3.50".
price = number(price_text).
print(age + 1, price).
```

Ungültiger Text, Wahrheitswerte und nicht-endliche Dezimalzahlen wie `"nan"`
erzeugen einen Words-Fehler.

## Anweisungen und Kontrollfluss

### Ausgabe

`print(...)` gibt null oder mehrere Werte aus. Mehrere Werte werden durch
Leerzeichen getrennt:

```text
print("Hello").
print("Name:", "Ada", "Score:", 42).
```

### Bedingungen: `if`, `elseif`, `else`

`if` führt seinen Block aus, wenn die Bedingung wahr ist. Beliebig viele
`elseif`-Zweige können folgen. Der optionale `else`-Block läuft, wenn keine
Bedingung wahr ist:

```text
if score >= 90.
    print("Excellent").
elseif score >= 60.
    print("Passed").
else.
    print("Try again").
```

Jeder dieser Blockköpfe endet mit einem Punkt und wird von einem eingerückten
Block gefolgt.

### Schleifen: `while`

`while` wiederholt seinen eingerückten Block, solange die Bedingung wahr ist:

```text
count = 0.
while count < 3.
    print(count).
    count = count + 1.
```

Es gibt derzeit keine `for`-Schleife oder `break`-/`continue`-Anweisung.

## Listen

Listen werden mit eckigen Klammern erstellt. Elemente können beliebige
unterstützte Werte einschließlich weiterer Listen enthalten:

```text
numbers = [10, 20, 30].
empty = [].
nested = [[1, 2], [3, 4]].
```

Indizes beginnen bei `0`. Negative Indizes zählen vom Ende. Elemente können
gelesen, geändert und mit `append(...)` ergänzt werden:

```text
first = numbers[0].
last = numbers[-1].
numbers[1] = 25.
numbers.append(40).
print(numbers).
```

Ein Index muss eine ganze Zahl sein. Ein Index ausserhalb der Liste oder ein
unpassender Indexwert erzeugt einen Fehler. Eine Funktion zum Entfernen von
Elementen oder zum Abfragen der Listenlänge gibt es derzeit nicht.

## Module importieren

Eine Words-Datei kann am Anfang eines Programms mit `import` eingebunden
werden:

```text
import "helpers".
print(helpers.message).
```

Das lädt `helpers.wo` aus dem gleichen Ordner wie das importierende Programm.
Die Endung `.wo` kann angegeben oder weggelassen werden. Unterordner sind
ebenfalls möglich:

```text
import "tools/helpers.wo".
print(helpers.message).
```

Der Modulname entspricht dem Dateinamen ohne Endung. Werte des Moduls werden
über `modulname.wert` verwendet. Alle Variablen des Moduls sind über diesen
Namensraum erreichbar. Importzeilen müssen vor ausführbaren Anweisungen stehen.
Ein Modul kann weitere Module importieren. Ein Modul wird pro Programmlauf nur
einmal ausgeführt; zyklische Importe erzeugen einen Fehler.

Importe sind auf `.wo`-Dateien innerhalb des Ordners des gestarteten Programms
und seiner Unterordner begrenzt. Absolute Pfade und Pfade, die aus diesem
Ordner hinausführen, sind nicht erlaubt. Python-Module können nicht mit
`import` geladen werden.

## SQL-Datenbanken

Words unterstützt SQLite über `sql.open(...)`. Datenbanken können im
Arbeitsspeicher oder als Datei geöffnet werden:

```text
database = sql.open(":memory:").
// Für eine Datei: database = sql.open("data.sqlite").
```

`query(...)` ist für lesende Abfragen vorgesehen und gibt eine Liste von
Zeilen zurück. Jede Zeile ist eine Zuordnung von Spaltennamen zu Werten:

```text
database.execute("CREATE TABLE people (name TEXT, age INTEGER)").
database.execute("INSERT INTO people (name, age) VALUES (?, ?)", ["Ada", 36]).
rows = database.query("SELECT name, age FROM people WHERE age > ?", [30]).
print(rows[0]["name"], rows[0].age).
```

Parameter werden als optionale zweite Liste übergeben. Werte nicht durch
Zeichenkettenverkettung in den SQL-Befehl einsetzen; stattdessen `?`-Platzhalter
verwenden.

`execute(...)` führt eine einzelne SQL-Anweisung aus und speichert Änderungen
sofort. Das Ergebnis enthält:

- `changes`: Anzahl der geänderten Zeilen.
- `last_id`: ID des zuletzt eingefügten Eintrags, sofern SQLite eine solche
  bereitstellt.

```text
result = database.execute("UPDATE people SET age = ? WHERE name = ?", [37, "Ada"]).
print(result["changes"]).
database.close().
```

Dateipfade müssen relativ zum Ordner des gestarteten Programms bleiben;
absolute Pfade und Pfade ausserhalb dieses Ordners sind gesperrt. `:memory:`
erstellt eine temporäre Datenbank. Offene Verbindungen werden am Programmende
geschlossen. `query(...)` akzeptiert keine Änderungen. `ATTACH`, `DETACH`,
`PRAGMA` und das Laden von SQLite-Erweiterungen sind gesperrt. PostgreSQL und
MySQL/MariaDB werden nicht unterstützt.

## Grafische Oberflächen

Mit `graphik "Titel" [` wird ein modernes Fenster mit Titelbereich,
abgesetztem Inhaltsbereich und einheitlichen Eingabe- und Aktionsstilen
geöffnet. Das Fenster wird zentriert und passt sich der Anzahl der Elemente an.
Der Block enthält benannte Elemente. Die Namen müssen innerhalb dieses
Fensters eindeutig sein:

```text
graphik "Greeting" [
    text heading = "What is your name?".
    textbox visitor = "".
    button greet = "Say hello".
    text result = "Your greeting will appear here.".
    on greet [
        set(result, "Hello " + read(visitor)).
    ]
]
```

Unterstützte Elemente:

| Deklaration | Verhalten |
| --- | --- |
| `text name = "..." .` | Zeigt einen Text als Label an. |
| `textbox name = "..." .` | Zeigt ein einzeiliges Eingabefeld mit Starttext an. |
| `textarea name = "..." .` | Zeigt ein mehrzeiliges Eingabefeld an. |
| `button name = "..." .` | Zeigt einen Button an. |
| `image name = "..." .` | Zeigt ein Bild aus dem angegebenen Pfad an. |

Die Leerzeichen vor dem Punkt in der Tabelle dienen nur der Lesbarkeit; im
Programm steht der Punkt direkt nach dem Textwert, zum Beispiel
`text heading = "Hello".`.

Die Werte der GUI-Elemente müssen Text sein. Bildpfade sind relativ zum Ordner
der aktuellen `.wo`-Datei oder absolut. Tkinter unterstützt je nach Installation
unter anderem GIF-, PNG-, PGM- und PPM-Bilder.

`on button_name [` legt einen Klick-Ereignisblock für einen Button fest. Der
Name muss auf ein Element vom Typ `button` verweisen. `read(field_name)` liefert
den aktuellen Text aus einem Textfeld; der Feldname kann unquoted als
Variablenname oder als Text angegeben werden:

```text
print(read(visitor)).
print(read("visitor")).
```

Während das Fenster geöffnet ist, können Widgets anhand ihres Namens geändert
werden:

```text
set(message, "Welcome!").
append(visitor, " Words").
clear().
```

- `set(widget_name, value)` ersetzt den Text einer `text`- oder
  `button`-Anzeige. Bei einem `textbox` oder `textarea` wird dessen Inhalt
  ersetzt. Namen können unquoted oder als Text angegeben werden.
- `append(field_name, value)` hängt Text an ein `textbox` oder `textarea` an.
- `clear()` leert alle ein- und mehrzeiligen Eingabefelder im aktuellen Fenster.

Für `set` und `append` wird der Wert in Text umgewandelt. `append` funktioniert
nur mit Textfeldern. Diese Funktionen sind innerhalb eines Button-Ereignisses
verwendbar, solange das zugehörige `graphik`-Fenster offen ist.

Elemente erscheinen untereinander. Mehrzeilige Eingabefelder haben eine
vertikale Bildlaufleiste. Das Programm wartet, bis das Fenster
geschlossen wird. Fehler in einem Button-Ereignis werden als Meldungsfenster
angezeigt. GUIs brauchen Tkinter und eine verfügbare grafische Desktop-Sitzung.
Checkboxen, Menüs, frei wählbare Positionen oder dynamische Erstellung neuer
Widgets nach dem Öffnen werden noch nicht unterstützt.

`read(name)` liest ein `textbox` oder `textarea`. Mehrzeilige Felder behalten
Zeilenumbrüche; `append` hängt den angegebenen Text exakt an.

Die Widgets erscheinen in Deklarationsreihenfolge mit einheitlichen Abständen
in einem hellen Inhaltsbereich. Buttons mit Ereignisblock erhalten eine
hervorgehobene Darstellung. Das Fenster wird zentriert und passt sich dem
Inhalt an. Ein vollständiges Beispiel, das Eingabetext mit
`number(read(input))` umwandelt, ist in [gui_example.wo](./gui_example.wo).

## Dateien und Websites öffnen

`start(...)` öffnet eine vorhandene lokale Datei mit der zugehörigen Anwendung
oder eine Website im Standardbrowser:

```text
start("notes.txt").
start("https://example.com").
```

Relative Dateipfade werden relativ zum Ordner der aktuellen `.wo`-Datei
aufgelöst; absolute Pfade sind erlaubt. Die Datei muss bereits vorhanden sein.
Für Webadressen werden nur `http://` und `https://` akzeptiert. Der Pfad wird
dem Betriebssystem übergeben und nicht als Shell-Befehl interpretiert.

## Zufallszahlen

`random.random()` erzeugt eine Zufallszahl mit Pythons Standard-Zufallsgenerator.
Ohne Argumente erhältst du eine Dezimalzahl im Bereich `[0.0, 1.0)`:

```text
fraction = random.random().
```

Mit zwei Grenzen wird eine Zahl zwischen diesen Grenzen erzeugt; beide Grenzen
sind eingeschlossen:

```text
whole = random.random(1, 10).
decimal = random.random(1.5, 2.5).
```

Sind beide Grenzen ganze Zahlen, ist das Ergebnis eine ganze Zahl. Enthält
mindestens eine Grenze eine Dezimalzahl, ist das Ergebnis eine Dezimalzahl.
Die untere Grenze darf nicht grösser als die obere sein. Wahrheitswerte sind
keine gültigen Grenzen.

`random.mous()` hat dieselbe Bereichssyntax, verwendet aber erfasste
Mausbewegungen als Grundlage:

```text
mouse_fraction = random.mous().
mouse_number = random.mous(1, 10).
```

`random.mous()` funktioniert nur unter Windows. Wird die Funktion im Programm
verwendet, erfasst Words während der Programmlaufzeit lokal Cursorbewegungen.
Es bleiben höchstens die letzten 512 Bewegungen im Arbeitsspeicher; die
Erfassung endet mit dem Programm. Wenn noch keine Bewegung erfasst wurde,
wartet die Funktion auf eine Bewegung. Mausdaten werden weder in Dateien
geschrieben noch übertragen. Für sicherheitskritische Zufallswerte ist die
Funktion nicht geeignet.

`random.hot()` ist nicht implementiert.

## Kommentare

Einzeilige Kommentare beginnen mit `//`. Der Rest der Zeile wird ignoriert:

```text
// Das ist ein Kommentar.
score = 10. // Kommentar hinter einer Anweisung.
```

Mehrzeilige Kommentare beginnen mit `/!` und enden mit `//!`:

```text
/!
Dieser Kommentar darf
mehrere Zeilen umfassen.
//!
```

Kommentarzeichen innerhalb eines Textes in Anführungszeichen sind normaler
Text. Blockkommentare können nicht verschachtelt werden. Ein nicht geschlossener
Blockkommentar erzeugt einen Fehler.

## Fehler und Einschränkungen

Fehler der Words-Syntax und der Laufzeit werden normalerweise mit Zeile und
Spalte gemeldet. Beispiele sind unbekannte Variablen, falsche Einrückung,
ungültige Operatoren, fehlende Dateien, ungültige Listenindizes oder
fehlgeschlagene SQL-Befehle.

Der aktuelle Sprachumfang hat folgende Grenzen:

- Keine selbst definierten Funktionen, `for`-Schleifen,
  `break`-/`continue`-Anweisungen oder Benutzereingaben.
- Keine Wörterbücher als allgemeiner Words-Datentyp; SQL-Zeilen und
  SQL-Ergebnisse liefern allerdings namensbasierte Werte.
- Imports laden ausschliesslich begrenzte `.wo`-Dateien, keine Python-Module.
- SQL ist auf SQLite beschränkt. `query(...)` ist lesend; Schreibzugriff erfolgt
  ausdrücklich über `execute(...)`.
- GUI-Elemente sind einfach und vertikal angeordnet. Tkinter sowie eine
  grafische Desktop-Sitzung müssen verfügbar sein.
- `random.mous()` benötigt Windows und reale Mausbewegungen.
- Die REPL bewahrt Variablen nicht zwischen Eingabezeilen auf.

## Dokumentationspflege

Diese Datei ist die Referenz für den jeweils implementierten Words-Umfang. Wenn
neue Schlüsselwörter, Operatoren, Datentypen, eingebaute Funktionen oder
Einschränkungen hinzukommen oder sich ändern, wird der passende Abschnitt mit
Syntax und einem ausführbaren Beispiel aktualisiert.
