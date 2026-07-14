# Tic-Tac-Toe für BDFD

Ein Tic-Tac-Toe-Spiel für Bot Designer For Discord (BDFD) mit PvP-Modus und einem KI-Modus mit 4 Schwierigkeitsstufen. Die Schwierigkeit Unmöglich verwendet Minimax mit einer vorausberechneten Nachschlagetabelle für alle 19.683 Spielfeldzustände, sodass es nie verliert.

## Was es enthält

- 4 KI-Schwierigkeiten: Einfach, Normal, Schwer, Unmöglich (Minimax)
- PvP-Modus mit Annehmen/Ablehnen-Buttons für Herausforderungen
- Zufällige X/O-Zuweisung in jeder Partie
- Revanche-System: Beide Spieler müssen zum Neustart abstimmen
- Aufgeben und Neustart im KI-Modus
- Gewinnlinie wird grün hervorgehoben
- Zug-Zähler (X/9)
- Keine Server-Variablen nötig, Spielstatus lebt im Embed-Footer
- Unbegrenzte gleichzeitige Spiele, jedes Spielfeld ist unabhängig

## Einrichtung

Du benötigst 3 Befehle in deinem Bot:

| # | Auslöser | Sprache | Datei | Was es macht |
|---|---------|----------|------|---------------|
| 1 | `!ttt` | BDScript 2 | `Tic-Tac-Toe_BDFD.md` | Hauptbefehl (KI-Menü oder PvP-Herausforderung) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Menü, Herausforderungen, Aufgeben, Revanche, Neustart |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Zellklicks + Minimax-KI |

**Warum zwei `$onInteraction`-Callbacks?** BDFD erlaubt mehrere, und führt alle bei jeder Interaktion aus. Die Minimax-Nachschlagetabelle ist 19.683 Zeichen lang und passt nicht zusammen mit dem Rest des Codes in einen einzelnen Callback, daher wurde sie in zwei aufgeteilt: der erste behandelt das Schwierigkeitsmenü, Annehmen/Ablehnen und Aufgeben/Revanche/Neustart; der zweite behandelt Zellklicks und die Minimax-Tabelle. Jeder verwendet `$stop`, damit sie sich nicht gegenseitig stören.

Bot-Berechtigungen benötigt: Nachrichten senden, Nachrichten verwalten (um das Spielfeld zu bearbeiten) und Komponenten verwenden (Buttons/Auswahlmenüs).

## Wie es funktioniert

### Spielstatus

Gespeichert im Embed-Footer in diesem Format:

```
SpielerX_ID|SpielerO_ID|Schwierigkeit|Zelle0.Zelle1.Zelle2.Zelle3.Zelle4.Zelle5.Zelle6.Zelle7.Zelle8|RevancheStimmeX|RevancheStimmeO
```

- `SpielerX_ID` / `SpielerO_ID`: Discord-Benutzer-ID (oder `KI`)
- `Schwierigkeit`: `e` Einfach, `n` Normal, `h` Schwer, `i` Unmöglich
- `Zelle0`–`Zelle8`: `e` leer, `X` oder `O`
- `RevancheStimmeX` / `RevancheStimmeO`: `y` oder `n`

Da es nicht auf Server-Variablen angewiesen ist, ist jedes Spiel in sich geschlossen und man kann mehrere gleichzeitig im selben Kanal ausführen.

### KI-Schwierigkeiten

| Schwierigkeit | Strategie |
|-----------|----------|
| Einfach | Zufällige Züge |
| Normal | Gewinnt wenn möglich, blockiert Bedrohung, sonst zufällig |
| Schwer | Gewinnen, blockieren, Gabeln aufbauen, gegnerische Gabeln blockieren, Zentrum, gegenüberliegende Ecke, Ecke, Seite |
| Unmöglich | Minimax mit vorausberechneter Tabelle für alle 19.683 Zustände. Verliert nie. |

### Der Minimax-Algorithmus

Für die Schwierigkeit Unmöglich:

1. `minimax_solver.py` führt einen erschöpfenden Minimax über alle 3⁹ = 19.683 möglichen Spielfeldzustände aus und berechnet für jeden, in dem die KI am Zug ist, den optimalen Zug unter der Annahme perfekten gegnerischen Spiels.
2. Das wird als 19.683-Zeichen-Tabelle gespeichert: Jede Position entspricht einem Zustand (indiziert durch einen Basis-3-Hash), und das Zeichen dort ist der optimale Zug (0-8) oder `-` wenn der Zustand ungültig ist.
3. In BDFD: Zellen werden Zahlen zugeordnet (`e=0`, `iaSym=1`, `userSym=2`), der Hash wird berechnet als `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`, nachgeschlagen mit `$splitText[$sum[$var[hash];1]]`, und dieser Zug wird gespielt.

### Zufällige X/O-Zuweisung

Beim Start einer Partie (neu, Neustart oder Revanche) entscheidet `$random[0;2]` wer X ist: die Hälfte der Zeit startest du, die Hälfte startet die KI (und spielt automatisch das Zentrum). Die KI ist mit `$var[iaSym]` und `$var[userSym]` parametrisiert, sodass sie gleich funktioniert egal ob X oder O.

### Revanche (PvP)

Wenn eine PvP-Partie endet, ändert sich der Button zu "Revanche (0/2)". Jeder Spieler stimmt durch Drücken ab, und wenn es 2/2 erreicht wird das Spielfeld mit einer neuen zufälligen X/O-Zuweisung zurückgesetzt. Nur die beiden ursprünglichen Spieler können abstimmen; alle anderen erhalten einen Fehler.

## Repo-Struktur

```
Repository/
│
├── README.md                          (Englische Version)
├── LIES-MICH.md                       (Deutsche Version)
│
├── English/
│   ├── Tic-Tac-Toe_Code.md
│   ├── Callback 1 ($onInteraction).md
│   └── Callback 2 ($onInteraction).md
│
└── Deutsch/
    ├── Tic-Tac-Toe_BDFD.md
    ├── Callback 1 ($onInteraction).md
    └── Callback 2 ($onInteraction).md
```

## Verwendung

Gegen KI: Tippe `!ttt` und wähle eine Schwierigkeit aus dem Dropdown. Dir wird zufällig X oder O zugewiesen.

Gegen einen anderen Spieler: `!ttt @benutzer`. Der herausgeforderte Spieler nimmt mit dem Button an, und X/O wird ebenfalls zufällig zugewiesen.

Während des Spiels: Klicke eine leere Zelle (⬜) um zu spielen, "Aufgeben" um aufzugeben, und wenn es vorbei ist "Neustart" (vs KI) oder "Revanche" (PvP) um nochmal zu spielen.

## Technische Details

- Sprache: BDScript 2 (verwendet `$eval`, `$var`, `$try`, `$async`)
- Jeder Callback bleibt unter dem 65.500-Zeichen-Limit von BDFD
- Keine externen Abhängigkeiten, alles lebt in den 3 Befehlen
- Keine Server-Variablen, Status lebt in den Embed-Footern

## Credits

Erstellt vom Nexusify Team. Du darfst den Code frei verwenden, behalte nur die Credits.

## Lizenz

Freie Nutzung mit Credits. Gib ihn nicht als deinen aus.
