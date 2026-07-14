# Tris per BDFD

Un gioco di Tris per Bot Designer For Discord (BDFD), con modalità PvP e modalità contro IA con 4 difficoltà. La difficoltà Impossibile usa minimax con una tabella precomputata di tutti i 19.683 stati del tabellone, quindi non perde mai.

## Cosa include

- 4 difficoltà IA: Facile, Normale, Difficile, Impossibile (minimax)
- Modalità PvP con pulsanti accetta/rifiuta per le sfide
- Assegnazione casuale di X/O ad ogni partita
- Sistema di rivincita: entrambi i giocatori devono votare per riavviare
- Arrendersi e riavviare in modalità IA
- La linea vincente viene evidenziata in verde
- Contatore di mosse (X/9)
- Non servono variabili del server, lo stato del gioco vive nel footer dell'embed
- Partite simultanee illimitate, ogni tabellone è indipendente

## Configurazione

Ti servono 3 comandi nel tuo bot:

| # | Attivatore | Linguaggio | File | Cosa fa |
|---|-----------|----------|------|---------|
| 1 | `!ttt` | BDScript 2 | `Tris_BDFD.md` | Comando principale (menu IA o sfida PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Menu, sfide, resa, rivincita, riavvio |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Mosse + IA minimax |

**Perché due callback `$onInteraction`?** BDFD permette di averne più di uno, e li esegue tutti ad ogni interazione. La tabella minimax occupa 19.683 caratteri e non entra insieme al resto del codice in un singolo callback, quindi è divisa in due: il primo gestisce il menu di difficoltà, accetta/rifiuta e resa/rivincita/riavvio; il secondo gestisce i clic sulle celle e la tabella minimax. Ognuno usa `$stop` per non interferire con l'altro.

Permessi che servono al bot: inviare messaggi, gestire messaggi (per modificare il tabellone) e usare componenti (pulsanti/menu a selezione).

## Come funziona

### Stato del gioco

Salvato nel footer dell'embed, con questo formato:

```
ID_giocatoreX|ID_giocatoreO|difficoltà|cella0.cella1.cella2.cella3.cella4.cella5.cella6.cella7.cella8|votoRivincitaX|votoRivincitaO
```

- `ID_giocatoreX` / `ID_giocatoreO`: ID utente Discord (o `IA`)
- `difficoltà`: `e` Facile, `n` Normale, `h` Difficile, `i` Impossibile
- `cella0`–`cella8`: `e` vuota, `X` o `O`
- `votoRivincitaX` / `votoRivincitaO`: `y` o `n`

Siccome non dipende da variabili del server, ogni partita è autonoma e puoi averne diverse in corso nello stesso canale.

### Difficoltà IA

| Difficoltà | Strategia |
|-----------|----------|
| Facile | Mosse casuali |
| Normale | Vince se può, blocca se c'è minaccia, altrimenti a caso |
| Difficile | Vince, blocca, crea fork, blocca fork dell'avversario, centro, angolo opposto, angolo, lato |
| Impossibile | Minimax con tabella precomputata per tutti i 19.683 stati. Non perde mai. |

### L'algoritmo minimax

Per la difficoltà Impossibile:

1. `minimax_solver.py` esegue un minimax esaustivo su tutti i 3⁹ = 19.683 stati possibili del tabellone, e per ognuno in cui tocca alla IA calcola la mossa ottimale assumendo che l'avversario giochi perfettamente.
2. Quello viene salvato come tabella di 19.683 caratteri: ogni posizione corrisponde a uno stato (indicizzato da un hash in base 3), e il carattere lì è la mossa ottimale (0-8), o `-` se quello stato non è valido.
3. In BDFD: le celle vengono mappate a numeri (`e=0`, `iaSym=1`, `userSym=2`), si calcola l'hash come `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`, lo si cerca con `$splitText[$sum[$var[hash];1]]`, e quella mossa viene giocata.

### Assegnazione casuale di X/O

All'inizio di una partita (nuova, riavvio o rivincita), `$random[0;2]` decide chi è X: metà delle volte inizi tu, metà inizia la IA (e gioca il centro automaticamente). La IA è parametrizzata con `$var[iaSym]` e `$var[userSym]`, quindi funziona uguale sia che sia X o O.

### Rivincita (PvP)

Quando finisce una partita PvP, il pulsante cambia in "Rivincita (0/2)". Ogni giocatore vota premendolo, e quando arriva a 2/2 il tabellone si resetta con una nuova assegnazione casuale di X/O. Solo i due giocatori originali possono votare; se un altro ci prova, riceve un errore.

## Struttura del repo

```
Repository/
│
├── README.md                          (versione inglese)
├── LEGGIMI.md                         (questo file)
│
├── English/
│   ├── Tic-Tac-Toe_Code.md
│   ├── Callback 1 ($onInteraction).md
│   └── Callback 2 ($onInteraction).md
│
└── Italiano/
    ├── Tris_BDFD.md
    ├── Callback 1 ($onInteraction).md
    └── Callback 2 ($onInteraction).md
```

## Uso

Contro la IA: scrivi `!ttt` e scegli una difficoltà dal menu a tendina. Ti verrà assegnato X o O a caso.

Contro un altro giocatore: `!ttt @utente`. Il giocatore sfidato accetta con il pulsante, e X/O viene assegnato a caso.

Durante la partita: clicca una cella vuota (⬜) per giocare, "Arrenditi" per abbandonare, e quando finisce "Riavvia" (vs IA) o "Rivincita" (PvP) per ripartire.

## Dettagli tecnici

- Linguaggio: BDScript 2 (usa `$eval`, `$var`, `$try`, `$async`)
- Ogni callback resta sotto il limite di 65.500 caratteri di BDFD
- Nessuna dipendenza esterna, tutto vive nei 3 comandi
- Nessuna variabile del server, lo stato va nei footer degli embed

## Crediti

Fatto dal Nexusify team. Sentiti libero di usare il codice, mantieni solo i crediti.

## Licenza

Uso libero con crediti. Non presentarlo come tuo.
