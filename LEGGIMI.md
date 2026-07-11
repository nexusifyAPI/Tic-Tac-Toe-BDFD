# Tris per BDFD - Versione Definitiva

Un gioco completo di Tris per **Bot Designer For Discord** (BDFD) con modalita **PvP** e modalita **vs IA** (4 difficolta).

La difficolta **Impossibile** usa un **algoritmo minimax reale** con una tabella precomputata di 19,683 stati, rendendolo matematicamente perfetto — non perde mai.

---

## Caratteristiche

- **4 difficolta IA**: Facile, Normale, Difficile, Impossibile (minimax)
- **Modalita PvP**: Sfida un altro giocatore con pulsanti accetta/rifiuta
- **Assegnazione casuale X/O**: A volte inizi tu, a volte inizia l'IA
- **Sistema di rivincita**: Entrambi i giocatori devono votare per riavviare (PvP)
- **Arrenditi/Riavvia**: Arrenditi in qualsiasi momento, riavvia dopo la fine (vs IA)
- **Evidenziazione linea vincente**: Le 3 celle vincenti diventano verdi
- **Contatore di mosse**: Mostra X/9 mosse effettuate
- **Nessuna variabile del server necessaria**: Lo stato del gioco e memorizzato nel footer dell'embed
- **Partite simultanee illimitate**: Ogni tabellone e indipendente

---

## Configurazione

### 1. Creare i comandi

Devi creare **3 comandi** nel tuo bot BDFD:

| # | Attivatore | Linguaggio | File | Descrizione |
|---|-----------|-----------|------|-------------|
| 1 | `!ttt` | BDScript 2 | `Tris_BDFD.md` | Comando principale (menu IA o sfida PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Gestisce menu, sfide, resa, rivincita, riavvio |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Gestisce mosse + IA minimax |

### 2. Perche due callback `$onInteraction`?

BDFD supporta multiple callback `$onInteraction` — entrambe vengono eseguite ad ogni interazione. La tabella minimax (19,683 caratteri) e troppo grande per stare in una singola callback, quindi il codice e diviso:

- **Callback 1**: Gestisce le route A (menu difficolta), B (accetta sfida), C (rifiuta sfida), E (resa/rivincita/riavvio)
- **Callback 2**: Gestisce la route D (gioca cella) + la tabella minimax

Ogni callback usa `$stop` per non interferire con l'altra.

### 3. Permessi del bot

Il bot ha bisogno di questi permessi:
- Invia messaggi
- Gestisci messaggi (per modificare il tabellone)
- Usa componenti (pulsanti / menu a selezione)

---

## Come funziona

### Archiviazione dello stato

Lo stato del gioco e memorizzato nel **footer dell'embed** (non in variabili del server), usando il formato:

```
ID_giocatoreX|ID_giocatoreO|difficolta|cella0.cella1.cella2.cella3.cella4.cella5.cella6.cella7.cella8|votoRivincitaX|votoRivincitaO
```

- `ID_giocatoreX` / `ID_giocatoreO`: ID utente Discord (o `IA` per modalita IA)
- `difficolta`: `e` (Facile), `n` (Normale), `h` (Difficile), `i` (Impossibile)
- `cella0`-`cella8`: `e` (vuota), `X`, o `O`
- `votoRivincitaX` / `votoRivincitaO`: `y` (votato) o `n` (non votato)

Questo rende ogni partita indipendente — partite simultanee illimitate nello stesso canale.

### Difficolta IA

| Difficolta | Strategia |
|-----------|-----------|
| **Facile** | Mosse casuali |
| **Normale** | Vinci se possibile, blocca se minacciato, altrimenti casuale |
| **Difficile** | Vinci, blocca, crea fork, blocca fork dell'avversario, centro, angolo opposto, angolo, lato |
| **Impossibile** | **Minimax reale** — mossa ottimale precomputata per tutti i 19,683 stati. Non perde mai. |

### Algoritmo minimax

L'IA Impossibile usa una **tabella minimax** precomputata in Python:

1. **Solver Python**: Esegue un minimax esaustivo su tutti i 19,683 stati possibili del tabellone (3^9). Per ogni stato valido dove e il turno dell'IA, calcola la mossa ottimale assumendo gioco perfetto dell'avversario.

2. **Tabella precomputata** (19,683 caratteri): Ogni posizione nella stringa rappresenta uno stato del tabellone (indicizzato da hash base-3). Il carattere in quella posizione e la mossa ottimale (0-8) o `-` se lo stato non e valido.

3. **In BDFD**, per la difficolta Impossibile:
   - Mappa le celle a numeri: `e=0`, `iaSym=1`, `userSym=2`
   - Calcola l'hash: `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`
   - Consulta la tabella: `$splitText[$sum[$var[hash];1]]`
   - Esegue quella mossa ottimale

### Assegnazione casuale X/O

All'avvio di una partita (nuova, riavvio o rivincita), un numero casuale (`$random[0;2]`) determina chi e X e chi e O:

- **50% di probabilita**: L'utente e X (inizia per primo)
- **50% di probabilita**: L'IA e X (inizia per prima, gioca il centro automaticamente)

L'IA e completamente parametrizzata con `$var[iaSym]` e `$var[userSym]` per funzionare correttamente indipendentemente dal fatto che sia X o O.

### Sistema di rivincita (PvP)

Dopo che una partita PvP termina:
1. Il pulsante cambia in "Rivincita (0/2)"
2. Ogni giocatore preme per votare — il pulsante si aggiorna a "Rivincita (1/2)"
3. Quando entrambi votano (2/2), il tabellone si resetta con nuova assegnazione casuale X/O
4. Solo i due giocatori possono votare; altri ricevono un errore

---

## Uso

### Gioca vs IA

```
!ttt
```

Seleziona una difficolta dal menu a discesa. Ti sara assegnato X o O casualmente.

### Gioca vs un altro giocatore

```
!ttt @utente
```

Il giocatore sfidato deve premere "Accetta" per iniziare. X/O e assegnato casualmente.

### Durante la partita

- Clicca su qualsiasi cella vuota (⬜) per posizionare il tuo segno
- Premi "Arrenditi" per arrenderti
- Dopo la fine della partita, premi "Riavvia" (vs IA) o "Rivincita" (PvP) per giocare di nuovo

---

## Crediti

Creato da **Nexusify team**.

Sei libero di usare questo codice, ma per favore non rimuovere i crediti. Ha richiesto 3 mesi di lavoro — rispetta lo sforzo.

---

## Licenza

Uso libero con crediti. Non reclamare il codice come tuo.
