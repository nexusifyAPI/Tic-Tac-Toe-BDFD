# Tic-Tac-Toe fur BDFD - Definitive Version

Ein vollstandiges Tic-Tac-Toe-Spiel fur **Bot Designer For Discord** (BDFD) mit **PvP**-Modus und **Gegen KI**-Modus (4 Schwierigkeitsstufen).

Die Schwierigkeit **Unmoglich** verwendet einen **echten Minimax-Algorithmus** mit einer vorausberechneten Nachschlagetabelle von 19,683 Zustaenden, was ihn mathematisch perfekt macht — er verliert nie.

---

## Funktionen

- **4 KI-Schwierigkeitsstufen**: Einfach, Normal, Schwer, Unmoglich (Minimax)
- **PvP-Modus**: Fordere einen anderen Spieler mit Annehmen/Ablehnen-Buttons heraus
- **Zufallige X/O-Zuweisung**: Manchmal startest du, manchmal startet die KI
- **Rematch-System**: Beide Spieler mussen zum Neustart abstimmen (PvP)
- **Aufgeben/Neustart**: Gib jederzeit auf, starte nach Spielende neu (vs KI)
- **Hervorhebung der Gewinnlinie**: Die 3 Gewinnzellen werden grun
- **Zug-Zahler**: Zeigt X/9 gemachte Zuge an
- **Keine Server-Variablen notig**: Spielstatus wird im Embed-Footer gespeichert
- **Unbegrenzte gleichzeitige Spiele**: Jedes Spielfeld ist unabhangig

---

## Einrichtung

### 1. Befehle erstellen

Du musst **3 Befehle** in deinem BDFD-Bot erstellen:

| # | Ausloser | Sprache | Datei | Beschreibung |
|---|---------|--------|-------|-------------|
| 1 | `!ttt` | BDScript 2 | `Tic-Tac-Toe_BDFD.md` | Hauptbefehl (KI-Menü oder PvP-Herausforderung) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Verwaltet Menu, Herausforderungen, Aufgeben, Rematch, Neustart |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Verwaltet Zellklicks + Minimax-KI |

### 2. Warum zwei `$onInteraction`-Callbacks?

BDFD unterstutzt mehrere `$onInteraction`-Callbacks — beide werden bei jeder Interaktion ausgefuhrt. Die Minimax-Nachschlagetabelle (19,683 Zeichen) ist zu gross fur einen einzelnen Callback, daher wird der Code aufgeteilt:

- **Callback 1**: Verwaltet Routen A (Schwierigkeitsmenu), B (Herausforderung annehmen), C (Herausforderung ablehnen), E (Aufgeben/Rematch/Neustart)
- **Callback 2**: Verwaltet Route D (Zelle spielen) + die Minimax-Nachschlagetabelle

Jeder Callback verwendet `$stop`, um sich nicht mit dem anderen zu storen.

### 3. Bot-Berechtigungen

Der Bot benotigt diese Berechtigungen:
- Nachrichten senden
- Nachrichten verwalten (um das Spielfeld zu bearbeiten)
- Komponenten verwenden (Buttons / Auswahlmenus)

---

## Wie es funktioniert

### Spielspeicherung

Der Spielstatus wird im **Embed-Footer** gespeichert (nicht in Server-Variablen), im Format:

```
SpielerX_ID|SpielerO_ID|Schwierigkeit|Zelle0.Zelle1.Zelle2.Zelle3.Zelle4.Zelle5.Zelle6.Zelle7.Zelle8|RematchStimmeX|RematchStimmeO
```

- `SpielerX_ID` / `SpielerO_ID`: Discord-Benutzer-IDs (oder `KI` fur KI-Modus)
- `Schwierigkeit`: `e` (Einfach), `n` (Normal), `h` (Schwer), `i` (Unmoglich)
- `Zelle0`-`Zelle8`: `e` (leer), `X` oder `O`
- `RematchStimmeX` / `RematchStimmeO`: `y` (abgestimmt) oder `n` (nicht abgestimmt)

Dadurch ist jedes Spiel unabhangig — unbegrenzte gleichzeitige Spiele im selben Kanal.

### KI-Schwierigkeitsstufen

| Schwierigkeit | Strategie |
|--------------|-----------|
| **Einfach** | Zufallige Zuge |
| **Normal** | Gewinnen wenn moglich, blockieren wenn bedroht, sonst zufallig |
| **Schwer** | Gewinnen, blockieren, Gabeln erstellen, gegnerische Gabeln blockieren, Zentrum, gegenuberliegende Ecke, Ecke, Seite |
| **Unmoglich** | **Echtes Minimax** — vorausberechneter optimaler Zug fur alle 19,683 Zustande. Verliert nie. |

### Minimax-Algorithmus

Die Unmoglich-KI verwendet eine **in Python vorausberechnete Minimax-Tabelle**:

1. **Python-Solver**: Fuhrt einen erschopfenden Minimax uber alle 19,683 moglichen Spielfeldzustande (3^9) aus. Fur jeden gultigen Zustand, in dem die KI am Zug ist, berechnet es den optimalen Zug unter der Annahme perfekten gegnerischen Spiels.

2. **Nachschlagetabelle** (19,683 Zeichen): Jede Position in der Zeichenkette reprasentiert einen Spielfeldzustand (indiziert durch Basis-3-Hash). Das Zeichen an dieser Position ist der optimale Zug (0-8) oder `-` wenn der Zustand ungultig ist.

3. **In BDFD**, fur die Schwierigkeit Unmoglich:
   - Ordnet Zellen Zahlen zu: `e=0`, `iaSym=1`, `userSym=2`
   - Berechnet den Hash: `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`
   - Schlagt in der Tabelle nach: `$splitText[$sum[$var[hash];1]]`
   - Spielt diesen optimalen Zug

### Zufallige X/O-Zuweisung

Beim Start eines Spiels (neu, Neustart oder Rematch) bestimmt eine Zufallszahl (`$random[0;2]`), wer X und wer O ist:

- **50% Chance**: Benutzer ist X (startet zuerst)
- **50% Chance**: KI ist X (startet zuerst, spielt automatisch Zentrum)

Die KI ist vollstandig parametrisiert mit `$var[iaSym]` und `$var[userSym]`, sodass sie korrekt funktioniert unabhangig davon, ob sie X oder O ist.

### Rematch-System (PvP)

Nach Ende eines PvP-Spiels:
1. Der Button wechselt zu "Rematch (0/2)"
2. Jeder Spielerdruckt zum Abstimmen — Button aktualisiert zu "Rematch (1/2)"
3. Wenn beide abstimmen (2/2), wird das Spielfeld mit neuer zufalliger X/O-Zuweisung zuruckgesetzt
4. Nur die zwei Spieler konnen abstimmen; andere erhalten einen Fehler

---

## Verwendung

### Gegen KI spielen

```
!ttt
```

Wahle eine Schwierigkeit aus dem Dropdown-Menu. Dir wird zufallig X oder O zugewiesen.

### Gegen einen anderen Spieler spielen

```
!ttt @benutzer
```

Der herausgeforderte Spieler muss "Annehmen" drucken um zu starten. X/O wird zufallig zugewiesen.

### Wahrend des Spiels

- Klicke auf ein leeres Feld (⬜) um dein Zeichen zu setzen
- Drucke "Aufgeben" um aufzugeben
- Nach Spielende drucke "Neustart" (vs KI) oder "Rematch" (PvP) um wieder zu spielen

---

## Credits

Erstellt vom **Nexusify team**.

Du darfst diesen Code frei verwenden, aber bitte entferne nicht die Credits. Es hat 3 Monate Arbeit gekostet — respektiere die Muehe.

---

## Lizenz

Freie Nutzung mit Namensnennung. Beanspruche den Code nicht als deinen.
