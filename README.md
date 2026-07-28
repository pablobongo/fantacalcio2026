# FantaIgor2026

PWA desktop personale per preparare e tracciare l'asta del fantacalcio della lega a 6 (stagione 2026/27), e seguire la squadra FC Albaredo durante il campionato.

Gira interamente nel browser, senza backend: i dati vivono in locale (IndexedDB) più un backup su file scelto da te. L'asta reale si svolge sull'app Leghe Fantacalcio — questa la traccia in parallelo.

> **Stato attuale:** funzionante con un **catalogo di test** di 65 giocatori. Il catalogo reale nasce quando esce il listone ufficiale 2026/27 (vedi *Aggiornare il catalogo*).

---

## Requisiti

- **Chrome o Edge desktop.** Il backup automatico su file usa la File System Access API, che esiste solo su questi browser. Su Firefox/Safari l'app si apre ma il backup su file non funziona.
- Connessione internet solo per aprire la pagina la prima volta. Una volta aperta la scheda, l'asta gira anche se la connessione cade: i dati restano in locale.

Non è un'app da installare: si tiene aperta come scheda del browser.

---

## File del progetto

| File | Ruolo |
|---|---|
| `index.html` | tutta l'app: interfaccia, logica, persistenza. File unico. |
| `icon.svg` | icona/favicon (riconoscibile tra le schede). Da rifinire in CorelDraw se vuoi. |
| `README.md` | questo file |

Il catalogo giocatori è per ora incluso dentro `index.html` come dati di test. Quando ci sarà il listone vero, verrà spostato in un file `data/catalogo.json` separato (vedi sotto).

---

## Deploy su GitHub Pages

1. Crea un repository (pubblico, per avere GitHub Pages gratis).
2. Carica `index.html`, `icon.svg`, `README.md`.
3. Settings → Pages → Branch `main` → Save.
4. Dopo 1-2 minuti la pagina è online su `https://<tuo-utente>.github.io/<repository>/`.
5. Apri quell'indirizzo in Chrome e aggiungilo ai preferiti.

Per aggiornare l'app in futuro: modifichi `index.html`, fai push, e GitHub Pages si aggiorna da solo in un paio di minuti. Ricaricando la scheda (F5) vedi la versione nuova.

---

## Primo utilizzo — impostare il backup

Alla prima apertura, entra e vai in **Impostazioni → Backup → Scegli file**. Scegli dove salvare `fantaigor2026_backup.json` (una cartella che ricordi). Da quel momento l'app riscrive quel file ogni 30 secondi durante l'asta, sovrascrivendolo — non ne crea centinaia.

Se il browser dovesse chiudere la scheda a metà asta, riaprendola trovi tutto: alla splash comparirà "Riprendi asta".

---

## Aggiornare il catalogo (quando esce il listone 2026/27)

Al momento il catalogo è di test, dentro `index.html`. Quando fantacalcio.it pubblica il listone ufficiale e il file statistiche:

1. Si scaricano i due file da fantacalcio.it (Quotazioni e Statistiche).
2. Con lo script `estrai_voti_fantacalcio.js` si estraggono gli indicatori dai voti della stagione precedente (continuità, distribuzione dei voti).
3. Si uniscono i tre sulla chiave `Id` e si produce `data/catalogo.json`.
4. Si aggancia il JSON all'app al posto del catalogo di test.

Questo passaggio si fa insieme, nella chat del progetto: è la **Fase 1** prevista dal documento di progetto. Le rose e gli acquisti già registrati non si toccano — vivono in IndexedDB e si agganciano al catalogo tramite `Id`.

Nota: le note per-giocatore e i valori di titolarità che inserisci a mano sono legati all'`Id` del giocatore, quindi restano attaccati ai giocatori giusti anche dopo aver caricato un listone aggiornato.

---

## Cosa fare se qualcosa non va durante l'asta

1. **Non chiudere la scheda.** Prima esporta un backup manuale da Impostazioni → Backup → Esporta ora.
2. Se l'interfaccia sembra bloccata ma i dati ci sono: ricarica (F5). Lo stato si ricarica da IndexedDB.
3. Se dopo il refresh mancano dati: Impostazioni → Importa backup, punta all'ultimo file salvato.
4. Se in alto compare un avviso "backup in pausa" o "permesso da riattivare": clicca il pulsante dell'avviso e riscegli il file.

---

## Note tecniche

- **Nessun dato lascia il tuo computer.** Tutto resta nel browser e nel file di backup locale.
- **Un solo dispositivo.** Niente sincronizzazione: usa sempre lo stesso Chrome sullo stesso PC durante l'asta.
- Le statistiche dei giocatori vengono dalla stagione precedente: cambi di squadra e allenatore le rendono indicative. L'algoritmo di suggerimento aiuta a orientarsi, non decide al posto tuo.
- L'asta di riparazione di gennaio è predisposta nel modello dati (sessioni) ma non ancora implementata: si aggiungerà quando servirà.
