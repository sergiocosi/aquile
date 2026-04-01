# 🦅 Aquile CC — Ottimizzatore

Web app per la gestione settimanale del Campionato per Club (CC) in Athletics Championship.

**Link:** `https://sergiocosi.github.io/aquile`

---

## Cosa fa

- Suggerisce le **discipline libere ottimali** per ogni giocatore, massimizzando i punti stimati del club
- Compone le **6 griglie staffetta** (4×100 e 4×400) con metodo serpentino, top-heavy o bilanciato ottimale
- Genera il **messaggio pronto** da copiare su WhatsApp/Telegram con staffette e libere
- Mostra la **copertura discipline** e il ranking completo della squadra

---

## Aggiornamento settimanale

Il venerdì, dopo aver ricevuto i nuovi dati di attrezzatura, aggiorna il file `index.html`.

Apri il file e cerca il blocco tra questi due commenti:

```
// ============================================================
// DATA — aggiorna questo blocco ogni settimana
// ============================================================
```

```
// ============================================================
// END DATA
// ============================================================
```

### Cosa aggiornare

| Variabile | Cosa contiene | Quando cambia |
|-----------|--------------|---------------|
| `WEEK_LABEL` | Etichetta settimana (es. "CC Diamond AA — Settimana 1") | Ogni settimana |
| `LEAGUE_LABEL` | Nome lega per il badge header (es. "DIAMOND AA") | Solo se cambia lega |
| `RANKS` | Rank CC per ogni giocatore in ogni disciplina | Ogni settimana |
| `PB` | Trofei e PB 100m/400m di ogni giocatore | Quando cambia |
| `OBL` | Obbligatorie per ogni giorno (1=Lun, 2=Mar, 3=Mer, 4=Gio) | Ogni settimana |
| `TOP_NAMES` | Set dei giocatori "TOP" (ricevono indicazioni personalizzate) | Raramente |

### Come caricare il file aggiornato su GitHub

1. Apri la repo su github.com/[tuonome]/aquile
2. Clicca su `index.html`
3. Clicca l'icona matita (Edit) in alto a destra
4. Oppure: trascina il nuovo file sulla repo — GitHub chiede se sovrascrivere, conferma
5. Scrivi un messaggio di commit tipo "Aggiornamento settimana X" e clicca **Commit changes**
6. Dopo circa 1 minuto il sito è aggiornato

---

## Struttura dei dati

### RANKS
Oggetto `{ NomeGiocatore: [rank1, rank2, ..., rank19] }` — un valore per ognuna delle 19 discipline nell'ordine:
`100m, 200m, 400m, 800m, 1500m, 5000m, 10000m, Marathon, 110H, 400H, 3000S, LJ, TJ, HJ, PV, SP, DT, HT, JT`

Il rank è la posizione nella classifica CC di quella settimana (1 = primo). `null` = giocatore non classificato.

### OBL
Oggetto con 4 chiavi (1, 2, 3, 4 per i giorni). Ogni giorno è `{ NomeGiocatore: ["disc1", "disc2"] }`.

I nomi dei giocatori devono corrispondere esattamente a quelli in `RANKS`.

### TOP_NAMES
Set JavaScript con i nomi dei giocatori che ricevono indicazioni personalizzate sulle libere. Di solito i 6-8 giocatori con rank mediamente più alto.

---

## Logica dell'algoritmo

Le libere vengono assegnate con un algoritmo **greedy a guadagno marginale**: ad ogni passo assegna la combinazione (giocatore, disciplina) che porta il maggior incremento di punti stimati rispetto alla situazione corrente. Il sistema di punteggio usato è quello reale del CC: 120 punti al 1°, 116 al 2°, 112 al 3°, poi -4 per ogni posizione.

---

## Aggiornamento tramite Claude

Ogni settimana puoi caricare i file Excel aggiornati direttamente su Claude con il messaggio:

> **"Aquile CC, settimana nuova"** + allega i tre file Excel

Claude genera automaticamente il nuovo `index.html` con tutti i dati aggiornati.

File necessari:
- `CC_potenziale_[...].xlsx` — rank CC attrezzatura
- `ClubAquile.xlsx` — PB e trofei
- `obbligatorieCC_tot.xlsx` — obbligatorie della settimana

---

## Note

- **Gianluca**: rank stimato (media Giuseppe + Cosmo) finché non comunica i dati attrezzatura
- **Nesli**: uscito dal club, non presente nei dati
- Il file funziona completamente **offline** una volta caricato, ma richiede HTTPS per funzionare su iOS Safari (quindi usare sempre il link GitHub Pages, non il file locale)
