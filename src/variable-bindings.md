# Variablen-Bindungen

Die Grundlage von praktisch jedem Programm ist die Fähigkeit Daten zu speichern und zu verändern. Rust Programme sind da nicht anders.
Lass uns mit einem kurzen Beispiel anfangen.

## Die Grundlagen von Variablen-Bindungen

Zuerst werden wir ein neues Projekt mit Cargo erzeugen. Öffne ein Terminal und navigiere in das Verzeichnis wo du deine Projekte aufbewahren möchtest.
Lass uns jetzt hier ein neues Projekt erzeugen:

```bash
$ cargo new --bin bindings
$ cd bindings
```

Dies erstellt ein neues Projekt namens „bindings“ und erzeugt unsere `Cargo.toml` und `src/main.rs` Dateien. Wie wir in „Hello, World!“ sahen, wird Cargo diese Dateien generieren und ein kleines ‘hello world’ Programm für uns erzugen:

```rust
fn main() {
    println!("Hello, world!");
}
```

Lass uns das Programm durch folgendes ersetzen:

```rust
fn main() {
    let x = 5;

    println!("The value of x is: {}", x);
}
```

Und nun führe es aus:

```bash
$ cargo run
   Compiling bindings v0.1.0 (file:///projects/bindings)
     Running `target/debug/bindings`
The value of x is: 5
```

Wenn du stattdessen einen Fehler bekommst, dann stelle sicher, dass du das Programm exakt so wie hier dargestellt kopiert hast.
Lass es uns Zeile für Zeile durchgehen.

```rust,ignore
fn main() {
```

Die `main()` Funktion ist der Einsprungspunkt eines jeden Rust Programmes. Wir werden im nächsten Abschnitt mehr über Funktionen reden, aber zunächst ist alles was wir wissen müssen, dass unser Programm dort beginnt. Die öffnende geschweifte Klammer, `{`, markiert den Anfang des Funktionskörpers.

```rust,ignore
    let x = 5;
```

Dies ist unsere erste „Variablen-Bindung“, welche wir mit einer „`let` Anweisung“ erzeugen.

Diese `let` Anweisung hat folgende Form:

```text
let NAME = AUSDRUCK;
```

Eine `let` Anweisung wertet zuerst den `AUSDRUCK` aus und bindet den sich ergebenden Wert an `NAME`, sodass dieser später im Programm wiederverwendet werden kann. In unserem einfachen Beispiel war der Ausdruck bereits ein Wert, nämlich `5`, aber wir könnten hiermit den selben Effekt erreichen:

```rust
let x = 2 + 3;
```

Im Allgemeinen arbeiten `let` Ausdrücke mit Mustern; ein Name ist eine besonders einfache Form eines Musters. Muster sind ein großer Teil von Rust, wir werden nach und nach noch komplexere und mächtigere Muster sehen.

Doch lass uns zuerst noch dieses Beispiel untersuchen. Hier ist die nächste Zeile:

```rust,ignore
    println!("The value of x is: {}", x);
```

Das `println!` Makro gibt Text auf dem Bildschirm aus. Wir wissen durch das `!`, dass es sich um ein Makro handelt. Wir werden in diesem Buch erst sehr spät lernen wie man Makros schreibt, aber wir werden durchgehend Makros verwenden, welche von der Standardbibliothek bereitgestellt werden. Jedes mal, wenn du ein `!` siehst, denke daran, dass es ein Makro kennzeichnet. Makros können der Sprache neue Syntax hinzufügen und das `!` dient als Erinnerung daran, dass diese Sachen manchmal etwas ungewöhnlich aussehen können.

`println!`, im Detail, hat einen benötigten Parameter, den ‘format string’, und keine oder mehr optionale Parameter. Der format String kan den speziellen Text `{}` enthalten. Jedes Vorkommen von `{}` entspricht einem zusätzlichen Parameter.
Hier ein Beispiel:

```rust
let x = 2 + 3;
let y = x + 5;
println!("The value of x is {}, and the value of y is {}", x, y);
```

Du kannst dir die `{}` wir kleine Krabbenscheren vorstellen, die den Wert festhalten. Dieser Platzhalter hat eine Reihe erweiterter Formatierungsoptionen, welche wir später diskutieren.

```rust,ignore
}
```

Zum Schluss eine schließende geschweifte Klammer, welche zu der öffnenden geschweiften Klammer der `main()` Funktionsdeklaration passt und ihr Ende festlegt.

Das erklärt unsere Ausgabe:

```text
The value of x is: 5
```

Wir weisen `5` einer Bindung `x` zu und geben sie dann mit `println!` auf dem Bildschirm aus. We assign `5` to a binding, `x`, and then print it to the screen with `println!`.

## Mehrere Bindungen

Lass uns nun ein komplexeres Muster ausprobieren.
Ändere unser Beispielprogramm so ab:

```rust
fn main() {
    let (x, y) = (5, 6);

    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

Und führe es mit `cargo run` aus:

```text
$ cargo run
   Compiling bindings v0.1.0 (file:///projects/bindings)
     Running `target/debug/bindings`
The value of x is: 5
The value of y is: 6
```

Wir haven zwei Bindungen mit einem `let` erzeugt! Hier ist unser Muster:

```text
(x, y)
```

Und hier ist der Wert:

```text
(5, 6)
```

Wie du sehen kannst sind diese beiden optisch sehr ähnlich, so bindet `let` `5` an `x` und `6` an `y`. Wir hätten auch zwei `let` Anweisungen verwenden können:

```rust
fn main() {
    let x = 5;
    let y = 6;
}
```

In einfachen Fällen wie diesen sind zwei `let`s klarer, aber in anderen Fällen ist es praktisch mehrere Bindungen auf einmal zu erstellen. Während wir Rust immer besser beherrschen, werden wir herausfinden welcher Stil besser ist, aber meistens ist es eine Frage des Ermessens.

## Typ-Annotationen

Du hast vielleicht bemerkt, dass wir den Typ von `x` oder `y` in unseren vorherigen Beispielen nicht deklariert haben. Rust ist eine *statische typisierte* Sprache, was bedeutet, dass wir die Typen aller Bindungen zur Übersetzungszeit kennen müssen. Aber jede einzelne Bindung mit einem Typ zu annotieren fühlt sich manchmal nach einer sinnlosen Beschäftigung an und kann den Code unleserlich machen. Um das zu lösen verwendet Rust Typableitung, was bedeutet dass der Compiler versucht die Typen deiner Bindungen abzuleiten.

Das Ableiten des Types erfolgt hauptsächlich dadurch, indem geschaut wird wie er verwendet wird.
Schauen wir uns das Beispiel nochmal an:

```rust
fn main() {
    let x = 5;
}
```

Wenn wir `x` an `5` binden, dann weiß der Compiler, dass `x` ein numerischer Type sein sollte. Ohne irgendwelche weiteren Information wird standardmäßig `i32` angenommen, ein 32-bit ganzzahliger Typ.
Wir werden mehr über Rusts Grundlegende Typen in Abschnitt 3.3 erzählen.

So sieht eine `let` Anweisung mit einer ‘Typ-Annotation’ aus:

```rust
fn main() {
    let x: i32 = 5;
}
```

Wir können ein Doppelpunkt, gefolgt von dem Typnamen, hinzufügen. Hier ist die Struktur einer `let` Anweisung mit Typ-Aannotation:

```text
let MUSTER: TYP = WERT;
```

Beachte, dass der Doppelpunkt und der `TYP` _nach_ dem `MUSTER` erscheinen, jedoch nicht im Muster selbst.

```rust
fn main() {
    let (x, y): (i32, i32) = (5, 6);
}
```

Genauso wie `WERT` und `MUSTER` strukturell zueinander passen, so passt auch der `TYP` strukturell zu dem `MUSTER`.

## Verzögerte Initialisierung

Wir müssen Bindungen keinen anfänglichen Wert geben und können später einen zuweisen.
Probier dieses Programm aus:

```rust
fn main() {
    let x;

    x = 5;

    println!("The value of x is: {}", x);
}
```

Und starte es mit `cargo run`:

```text
$ cargo run
   Compiling bindings v0.1.0 (file:///projects/bindings)
     Running `target/debug/bindings`
The value of x is: 5
```

Alles klar. Dies bringt allerdings eine Frage auf: Was passiert, wenn wir versuchen `x` auszugeben bevor wir einen Wert zuweisen? Hier ein Programm, was das mal ausprobiert:

```rust,ignore
fn main() {
    let x;

    println!("The value of x is: {}", x);

    x = 5;
}
```

Wir bekommen unsere Antwort mit `cargo run`:

```text
   Compiling bindings v0.1.0 (file:///projects/bindings)
src/main.rs:4:39: 4:40 error: use of possibly uninitialized variable: `x` [E0381]
src/main.rs:4     println!(“The value of x is: {}”, x);
                                                    ^
<std macros>:2:25: 2:56 note: in this expansion of format_args!
<std macros>:3:1: 3:54 note: in this expansion of print! (defined in <std macros>)
src/main.rs:4:5: 4:42 note: in this expansion of println! (defined in <std macros>)
src/main.rs:4:39: 4:40 help: run `rustc --explain E0381` to see a detailed explanation
error: aborting due to previous error
Could not compile `bindings`.

To learn more, run the command again with --verbose.
```

Ein Fehler! Der Compiler lässt uns ein solches Programm nicht schreiben. Dies ist unser erstes Beispiel wo der Compiler uns hilft einen Fehler in unserem Programm zu finden. Verschiedene Programmiersprachen haben verschiedene Herangehensweisen für dieses Problem. Manche Sprachen initialisieren immer mit einem Standardwert. Andere Sprachen lassen den Wert uninitialisiert und machen keine Versprechungen darüber was passiert, wenn man ihn vor seiner Initialisierung verwendet. Rust wählt einen anderen Weg: Ein Fehler wird angezeigt und der Programmier wird gezwungen zu erklären was er tun will. Wir müssen irgendeine Art von Initialisierung vornehmen bevor wir `x` benutzen können.

### Detaillierte Fehler Erklärungen

Es gibt noch etwas interessantes bei dieser Fehlermeldung: 

```text
src/main.rs:4:39: 4:40 help: run `rustc --explain E0381` to see a detailed explanation
```

Wir können eine detaillierte Erklärung sehen indem wir den `--explain` Schalter an `rustc` übergeben. Nicht jeder Fehler hat eine längere Erklärung, aber viele haben eine. Diese detaillierten Erklärungen versuchen häufig auftretende Problemsituationen und Lösungswege zu zeigen.
Hier ist `E0381`:

```bash
$ rustc --explain E0381
It is not allowed to use or capture an uninitialized variable. For example:

fn main() {
    let x: i32;
    let y = x; // error, use of possibly uninitialized variable

To fix this, ensure that any declared variables are initialized before being
used.
```

Diese Erklärungen können wirklich hilfreich sein, wenn du bei einem Problem feststeckst. Der Compiler ist dein Freund und da um zu helfen.

## Veränderbare Bindungen

Was ist, wenn wir den Wert einer Bindung ändern wollen?
Hier ein weiteres Beispielprogramm, welches diese Frage aufwirft:

```rust,ignore
fn main() {
    let x = 5;

    x = 6;

    println!("The value of x is: {}", x);
}
```

`cargo run` hat die Antwort für uns:

```bash
$ cargo run
   Compiling bindings v0.1.0 (file:///projects/bindings)
src/main.rs:4:5: 4:10 error: re-assignment of immutable variable `x` [E0384]
src/main.rs:4     x = 6;
                  ^~~~~
src/main.rs:4:5: 4:10 help: run `rustc --explain E0384` to see a detailed explanation
src/main.rs:2:9: 2:10 note: prior assignment occurs here
src/main.rs:2     let x = 5;
                      ^
```

Der Fehler erwähnt `re-assigment of immutable variable`, also „Neuzuweisung einer unveränderbaren Variable“. Ganz recht: Bindungen sind unveränderbar. Aber sie sind nur standardmäßig unveränderbar. Wenn wir in einem Muster einen neuen Namen erzeugen, dann können wir `mut` davor schreiben um die Bindung veränderbar zu machen. Hier ein Beispiel:

```rust
fn main() {
    let mut x = 5;

    println!("The value of x is: {}", x);

    x = 6;

    println!("The value of x is: {}", x);
}
```

Wenn wir das ausführen erhalten wir:

```bash
$ cargo run
   Compiling bindings v0.1.0 (file:///projects/bindings)
     Running `target/debug/bindings`
The value of x is: 5
The value of x is: 6
```

Wir können nun den Wert der an `x` gebunden wird verändern. Beachte, dass `let mut` keine spezielle Syntax, sondern einfach eine `let` Anweisung mit einem Muster, welches `mut` verwendet. Dies wird deutlicher durch unser `()` Muster:

```rust,ignore
fn main() {
    let (mut x, y) = (5, 6);

    x = 7;
    y = 8;
}
```

Der Compiler wird sich über dieses Programm beschweren:

```bash
$ cargo build
   Compiling bindings v0.1.0 (file:///projects/bindings)
src/main.rs:5:5: 5:10 error: re-assignment of immutable variable `y` [E0384]
src/main.rs:5     y = 8;
                  ^~~~~
src/main.rs:5:5: 5:10 help: run `rustc --explain E0384` to see a detailed explanation
src/main.rs:2:17: 2:18 note: prior assignment occurs here
src/main.rs:2     let (mut x, y) = (5, 6);
                              ^
```

Es ist in Ordnung `x` erneut zuzuweisen, aber nicht `y`. Das `mut` wird nur auf dem ihm nachfolgendem Namen angewendet, nicht auf das gesamte Muster.

### Neuzuweisung, nicht Veränderung

Es gibt eine Feinheit, die wir noch nicht behandelt haben: `mut` erlaubt einem _die Bindung_ zu verändern, jedoch nicht _das, woran die Variable bindet_. In andere Worten:

```rust
fn main() {
    let mut x = 5;

    x = 6;
}
```

Dies ändert nicht den Wert, der an `x` gebunden ist, sondern erzeugt einen neuen Wert, `6`, und ändert die Bindung, sodass an den neuen Wert gebunden wird. Das ist ein feiner, aber wichtiger Unterschied. Nun, im Moment macht es noch keinen so großen Unterschied, aber, wenn unser Programm komplexer wird, dann wird es das. Besonders das übergeben von Parametern an Funktionen wird den Unterschied verdeutlichen. Wir werden darüber im nächsten Abschnitt, wo es dann um Funktionen geht, sprechen.

## Geltungsbereich

Variablenbindungen haben einen Geltungsbereich in dem sie gültig sind. Der Geltungsbereich beginnt an dem Punkt, wo die Bindung deklariert ist, und endet am Ende des nächsten Code-Blocks. Wir können nur auf Bindungen zugreifen, welche sich im Geltungsbereich befinden. Wir können nicht nicht auf sie zugreifen bevor sie in den Geltungsbereich gelangen oder nachdem sie den Geltungsbereich verlassen.
Hier ist ein Beispiel:

```rust
fn main() {
    println!("x is not yet in scope");

    let x = 5;
    println!("x is now in scope");

    println!("In real code, we’d now do a bunch of work.");

    println!("x will go out of scope now! The next curly brace is ending the main function.");
}
```

Wir können beliebige Geltunsbereiche mithilfe von `{` und `}` erstellen:

```rust
fn main() {
    println!("x is not yet in scope");

    let x = 5;
    println!("x is now in scope");

    println!("Let’s start a new scope!");

    {
        let y = 5;
        println!("y is now in scope");
        println!("x is also still in scope");

        println!("y will go out of scope now!");
        println!("The next curly brace is ending the scope we started.");
    }

    println!("x is still in scope, but y is now out of scope and is not usable");

    println!("x will go out of scope now! The next curly brace is ending the main function.");
}
```

Welche Bindungen im oder nicht im Geltungsbereich sind wird später noch viel wichtiger sein, sobald wir über „Referenzen“ und „Traits“ erfahren haben.

## Überdeckung

Noch eine letzte Sache über Bindungen: Sie können vorherige Bindungen mit dem selben Namen „überdecken“. Hier ein Beispielprogramm:

```rust
fn main() {
    let x = 5;
    let x = 6;

    println!("The value of x is: {}", x);
}
```

Wenn wir es ausführen, können wir das Überdecken in Aktion sehen:

```text
src/main.rs:2:9: 2:10 warning: unused variable: `x`, #[warn(unused_variables)] on by default
src/main.rs:2     let x = 5;
                      ^
     Running `target/debug/bindings`
The value of x is: 6
```

Es gibt zwei interessante Dinge in dieser Ausgabe. Zunächst einmal wird Rust das Programm kompilieren und ausführen, kein Problem. Und wie wir sehen können, ist der Wert von `x` gleich `6`. Aber wir haben `x` nicht als veränderbar deklariert. Stattdessen haben eine _neue_ Bindung deklariert, welche _ebenfalls_ `x` heißt, und haven dieser einen neuen Wert gegeben. Auf den älten Wert, den wir an `x` gebunden haben, kann nicht mehr zugegriffen werden sobald das neue `x` deklariert wurde. Dies kann nützlich sein, falls du vorhast ein paar Transformationen auf einen Wert anzuwenden und ihn unveränderbar zu lassen. Zum Beispiel:

```rust
fn main() {
    let x = 5;
    let x = x + 1;
    let x = x * 2;

    println!("The value of x is: {}", x);
}
```

Der Code gibt folgendes aus:

```bash
   Compiling bindings v0.1.0 (file:///projects/bindings)
     Running `target/debug/bindings`
The value of x is: 12
```

Dies erlaubt uns `x` zu modifizieren ohne den Wert tatsächlich zu verändern. Das ist super, weil wir wissen, dass der Compiler es uns wissen lässt, wenn wir den Wert später versuchen zu verändern. Nehmen wir mal an, dass wir `x` nicht mehr verändern wollen, nachdem wir `12` ausgerechnet haben. Wenn wir einen „veränderbaren Stil“, wie folgt verwenden würden:

```rust
fn main() {
    let mut x = 5;
    x = x + 1;
    x = x * 2;

    println!("The value of x is: {}", x);

    x = 15;

    println!("The value of x is: {}", x);
}
```

Dann ließe uns Rust den Wert ohne Widerworte auf `15` setzen. Ein ähnliches Programm in unserem „unveränderbaren Stil“ setzt uns stattdessen über diese unbeabsichtigte Veränderung in Kenntnis:

```rust,ignore
fn main() {
    let x = 5;
    let x = x + 1;
    let x = x * 2;

    println!("The value of x is: {}", x);

    x = 15;

    println!("The value of x is: {}", x);
}
```

Wenn wir versuchen das zu kompilieren erhalten wir nämlich einen Fehler:

```bash
$ cargo build
   Compiling bindings v0.1.0 (file:///projects/bindings)
src/main.rs:8:5: 8:11 error: re-assignment of immutable variable `x` [E0384]
src/main.rs:8     x = 15;
                  ^~~~~~
src/main.rs:8:5: 8:11 help: run `rustc --explain E0384` to see a detailed explanation
src/main.rs:4:9: 4:10 note: prior assignment occurs here
src/main.rs:4     let x = x * 2;
                      ^
error: aborting due to previous error
Could not compile `bindings`.
```

Genau was wir wollten.

Es kann etwas dauern bis man sich an Überdeckung gewöhnt hat, aber sie ist sehr mächtig und arbeitet gut mit Unveränderbarkeit zusammen.

Es gab noch einen weiteren Teil in der Compiler-Ausgabe unseres ursprünglichen Programme über den wir sprechen sollten:

```text
src/main.rs:2:9: 2:10 warning: unused variable: `x`, #[warn(unused_variables)] on by default
```

Hier sind nochmal die beiden relevanten Code-Zeilen:

```rust
let x = 5;
let x = 6;
```

Rust weiß, dass wir `x` überdecken, aber den ursprünglichen Wert niemals verwendet haben. Das ist genau genommen nicht _falsch_, es könnte jedoch sein, dass das so nicht von uns gewollt war. In diesem Fall gibt der Compiler eine „Warnung“ aus, aber kompiliert trotzdem unser Programm.
Die `#[warn(unused_variables)]` Syntax wird „Attribut“ genannt und werden in einem späteren Abschnitt behandelt. Genauer gesagt wird eine solche Warnung „lint“ („Fussel”) genannt, was ein alter englischer Begriff für einen Teil Schafwolle ist, den man nicht in seine Kleidung stecken wollen würde. Gleichermaßen sagt uns dieses Lint, dass wir ein Stück extra Code haben, den wir wir nicht brauchen. Unser Programm würde auch genauso gut ohne ihn funktionieren. Es lohnt sich auf diese Warnungen zu hören und die Probleme, auf die sie hinweisen, zu beheben. Sie können ein Zeichen eines größeren Problemes sein. In diesem Fall könnten wir vielleicht garnicht realisiert haben, dass wir `x` überdeckt haben.

### Überdeckung und Geltungsbereiche

Wie jede Bindung, verschwindet eine Bindung, die eine andere überdeckt, am Ende eines Geltungsbereiches. Hier ein Beispielprogramm:

```rust
fn main() {
    let x = 5;

    println!("Before shadowing, x is: {}", x);

    {
        let x = 6;

        println!("Now that x is shadowed, x is: {}", x);
    }

    println!("After shadowing, x is: {}", x);
}
```

Wenn wir dieses Beispiel ausführen, können wir die Überdeckung erscheinen und verschwinden sehen:

```bash
$ cargo run
   Compiling bindings v0.1.0 (file:///projects/bindings)
     Running `target/debug/bindings`
Before shadowing, x is: 5
Now that x is shadowed, x is: 6
After shadowing, x is: 5
```
