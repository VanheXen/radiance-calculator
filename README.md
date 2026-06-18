# Capturing Radiance Status

A single-file, offline-friendly tool that reads your Genshin Impact wish history
and tells you your current **Capturing Radiance** counter — and whether your next
character-banner 50/50 is a coin flip, boosted, or a guaranteed win.

> The whole app is **`index.html`**. Open it in any browser. No install.

---

## English

### What it does
- Reconstructs the post-v5.0 Capturing Radiance counter (0–3) from your 50/50 history.
- Tells you what your **next 50/50** does: plain flip (0–1), ~54.5% boosted (2), or guaranteed (3).
- Shows your full character-banner 5★ timeline: pity, win / loss / guaranteed, and which wins were radiance rescues.
- Works with **full or partial** history, and pins the counter for certain whenever possible.

### How to use
Open the app, then pick one of three ways to load your history.

#### 1. Wish-history link — fetches fresh data (Windows)
1. Start Genshin on your PC and open **Wishes → History**.
2. Open **PowerShell** (search "PowerShell" in the Start menu).
3. Run this command, then copy its output (your wish link):
   ```powershell
   iwr -useb stardb.gg/wish | iex
   ```
4. Paste that link into the app's box and press **Fetch**. stardb imports your history from Hoyo,
   the app reads it back.
   - The link carries a **read-only authkey** that expires in ~24h, and is sent **only to stardb**.
   - Not on Windows? Use the [stardb exporter](https://github.com/juliuskreutz/stardb-exporter) or
     [stardb.gg/wish](https://stardb.gg/en/genshin/wish-import), then load via UID or JSON.

#### 2. UID — after you've imported at least once
1. Do the link import above once (via this app or stardb.gg/wish). That stores your history on stardb.
2. From then on, just type your **9-digit UID** (shown bottom-right in-game) and press **Fetch**.
   Only the UID is sent — no link, no authkey. Note: it reads whatever was last imported, so re-run
   the link import when you want fresh pulls.

#### 3. Load JSON — fully offline
Export your history as a JSON file from stardb.gg/wish, then **Load JSON**. Your wish data stays on
your machine. If the export holds several accounts, a **UID picker** appears.

### Partial history
If your data starts after v5.0 (2024-08-28), the counter may be ambiguous. The app
resolves it, strongest method first:
1. **Automatic** — if every possible starting counter lands on the same value, it's certain (no input).
2. **Pick the 5★ you got from Capturing Radiance** (the golden animation) — self-checking, pins everything after it.
3. **Enter your counter** at the start date — memory-based fallback.

### Privacy — read before using Link/UID
This app talks only to stardb.gg and genshin-db, and exposes nothing itself. But **stardb's import
makes your wish history public**, so how you load data matters:

- **Link** → pressing Fetch POSTs your link to stardb's import endpoint. stardb then stores your
  history and makes it **publicly readable by UID** ([stardb's own notice](https://stardb.gg/en/genshin/wish-import):
  *"Importing Wishes will make it public to other users."*). The wish-link authkey is read-only (~24h),
  sent only to stardb.
- **UID** → only reads whatever is already public on stardb for that UID. (Any imported UID is
  readable by anyone — that's how this method works.)
- **Load JSON** → sends nothing; your wish data stays on your machine. **Caveat:** a JSON exported
  from the **stardb website** was already imported there (public). A JSON from a **local exporter**
  ([stardb-exporter](https://github.com/juliuskreutz/stardb-exporter), paimon.moe, in-game/UIGF) that
  doesn't upload is **private** — this is the only way to use the tool without exposing anything.
- **Removing data from stardb:** stardb's *private* toggle hides your profile/leaderboard listing but
  does **not** hide the wish API — the data stays readable by UID. To actually remove it, **delete**
  your wishes on stardb.
- **Character names** are fetched from the public [genshin-db](https://github.com/theBowja/genshin-db)
  repo regardless of method — **no personal data is sent** there.

**Want zero exposure?** Use **Load JSON** with a file from a local exporter, or run this app's
`index.html` locally — your data never touches stardb.

### Disclaimer
- **Unofficial fan tool**, not affiliated with or endorsed by HoYoverse.
- The Capturing Radiance counter comes from an **unofficial community model** — HoYoverse has never
  published the exact mechanic. Results are **estimates, not guarantees**; the model can be wrong,
  especially on partial histories.

### The model
Based on the community analysis of ~4M post-v5.0 pulls
([reddit](https://www.reddit.com/r/Genshin_Impact/comments/1hd1sqa/)):
counter starts at 1 at v5.0, floor 0; **only 50/50s move it** (lose +1, win −1);
at 2 the next 50/50 has a small (~54.5%) radiance chance, at 3 it's a guaranteed
radiance win that resets the counter to 1; guaranteed pulls (after a lost 50/50) don't change it.

### Credits & sources
This tool stands on others' work:
- **[genshin-db](https://github.com/theBowja/genshin-db)** by theBowja — character names, rarities, and IDs (the embedded/auto-updated charmap).
- **[stardb.gg](https://stardb.gg)** — wish-history import and the public API the link/UID fetch uses.
- **Capturing Radiance model** — the community analysis of ~4M post-v5.0 pulls by [u/OneBST](https://www.reddit.com/r/Genshin_Impact/comments/1hd1sqa/), refined by u/benjaminhsieh.
- **Soft/hard pity & drop-rate model** — Cgg / [genshin-wishes.com](https://genshin-wishes.com), [HoYoLAB statistical analysis](https://www.hoyolab.com/article/497840).
- **Genshin Impact** © HoYoverse. Game names and data belong to them; this is an unofficial fan tool, not affiliated with or endorsed by HoYoverse.

### Development
```
python run_tests.py     # regenerate fixtures + run all tests
python gen_testdata.py  # just regenerate testdata/ fixtures
node test_model.js      # model + branch coverage vs real app code
node test_ui.js         # interactive UI paths (DOM-stubbed)
```
`index.html` is the only file you ship. Everything else is dev tooling.

---

## Italiano

### Cosa fa
- Ricostruisce il contatore Capturing Radiance post-v5.0 (0–3) dalla storia dei tuoi 50/50.
- Ti dice cosa farà il **prossimo 50/50**: testa o croce (0–1), potenziato ~54.5% (2), o vincita garantita (3).
- Mostra la cronologia completa dei 5★ del banner personaggio: pity, vinto / perso / garantito, e quali vittorie sono state salvataggi radiance.
- Funziona con storia **completa o parziale**, e fissa il contatore con certezza quando possibile.

### Come si usa
Apri l'app, poi scegli uno dei tre modi per caricare la tua cronologia.

#### 1. Link della cronologia wish — scarica dati aggiornati (Windows)
1. Avvia Genshin sul PC e apri **Wish → Cronologia**.
2. Apri **PowerShell** (cerca "PowerShell" nel menu Start).
3. Esegui questo comando, poi copia il suo output (il tuo link wish):
   ```powershell
   iwr -useb stardb.gg/wish | iex
   ```
4. Incolla quel link nella casella dell'app e premi **Fetch**. stardb importa la cronologia da Hoyo e
   l'app la rilegge.
   - Il link contiene un **authkey di sola lettura** che scade in ~24h, ed è inviato **solo a stardb**.
   - Non sei su Windows? Usa lo [stardb exporter](https://github.com/juliuskreutz/stardb-exporter) o
     [stardb.gg/wish](https://stardb.gg/en/genshin/wish-import), poi carica via UID o JSON.

#### 2. UID — dopo averlo importato almeno una volta
1. Esegui l'import via link sopra almeno una volta (con questa app o stardb.gg/wish). Così la
   cronologia viene salvata su stardb.
2. Da quel momento, basta scrivere il tuo **UID a 9 cifre** (in basso a destra nel gioco) e premere
   **Fetch**. Viene inviato solo l'UID — nessun link, nessun authkey. Nota: legge l'ultimo import,
   quindi riesegui l'import via link quando vuoi i pull aggiornati.

#### 3. Load JSON — completamente offline
Esporta la cronologia come file JSON da stardb.gg/wish, poi **Load JSON**. I dati dei wish restano sul
tuo dispositivo. Se l'export contiene più account, compare un **selettore UID**.

### Storia parziale
Se i dati iniziano dopo la v5.0 (28-08-2024), il contatore può essere ambiguo. L'app
lo risolve, dal metodo più affidabile:
1. **Automatico** — se ogni possibile contatore iniziale porta allo stesso valore, è certo (nessun input).
2. **Scegli il 5★ ottenuto da Capturing Radiance** (l'animazione dorata) — si auto-verifica e fissa tutto ciò che viene dopo.
3. **Inserisci il contatore** alla data d'inizio — ripiego basato sulla memoria.

### Privacy — leggi prima di usare Link/UID
Questa app comunica solo con stardb.gg e genshin-db, e di per sé non espone nulla. Ma **l'import di
stardb rende pubblica la tua cronologia**, quindi conta come carichi i dati:

- **Link** → premendo Fetch invii (POST) il link all'endpoint di import di stardb. stardb salva la
  cronologia e la rende **leggibile pubblicamente tramite UID** ([avviso di stardb](https://stardb.gg/en/genshin/wish-import):
  *"Importing Wishes will make it public to other users."*). L'authkey è di sola lettura (~24h), inviato solo a stardb.
- **UID** → legge solo ciò che è già pubblico su stardb per quell'UID. (Qualsiasi UID importato è
  leggibile da chiunque — è così che funziona il metodo.)
- **Load JSON** → non invia nulla; i dati restano sul tuo dispositivo. **Attenzione:** un JSON esportato
  dal **sito stardb** era già stato importato lì (pubblico). Un JSON da un **exporter locale**
  ([stardb-exporter](https://github.com/juliuskreutz/stardb-exporter), paimon.moe, in-game/UIGF) che non
  carica nulla è **privato** — è l'unico modo di usare lo strumento senza esporre niente.
- **Rimuovere i dati da stardb:** il toggle *private* di stardb nasconde profilo/leaderboard ma **non**
  l'API dei wish — i dati restano leggibili tramite UID. Per rimuoverli davvero, **cancella** i tuoi wish su stardb.
- I **nomi dei personaggi** vengono scaricati dal repo pubblico [genshin-db](https://github.com/theBowja/genshin-db)
  in ogni caso — **nessun dato personale** viene inviato lì.

**Vuoi zero esposizione?** Usa **Load JSON** con un file da un exporter locale, oppure esegui
`index.html` in locale — i tuoi dati non toccano mai stardb.

### Avvertenze
- **Strumento fan non ufficiale**, non affiliato né approvato da HoYoverse.
- Il contatore Capturing Radiance deriva da un **modello non ufficiale della community** — HoYoverse non
  ha mai pubblicato il meccanismo esatto. I risultati sono **stime, non garanzie**; il modello può
  sbagliare, soprattutto con cronologie parziali.

### Il modello
Basato sull'analisi della community di ~4M pull post-v5.0
([reddit](https://www.reddit.com/r/Genshin_Impact/comments/1hd1sqa/)):
il contatore parte da 1 alla v5.0, minimo 0; **solo i 50/50 lo muovono** (perso +1, vinto −1);
a 2 il prossimo 50/50 ha una piccola probabilità (~54.5%) di radiance, a 3 è una
vincita radiance garantita che riporta il contatore a 1; i pull garantiti (dopo un 50/50 perso) non lo cambiano.

### Crediti e fonti
Questo strumento si basa sul lavoro di altri:
- **[genshin-db](https://github.com/theBowja/genshin-db)** di theBowja — nomi, rarità e ID dei personaggi (la charmap integrata e auto-aggiornata).
- **[stardb.gg](https://stardb.gg)** — import della cronologia wish e l'API pubblica usata dal fetch via link/UID.
- **Modello Capturing Radiance** — l'analisi della community su ~4M pull post-v5.0 di [u/OneBST](https://www.reddit.com/r/Genshin_Impact/comments/1hd1sqa/), affinata da u/benjaminhsieh.
- **Modello soft/hard pity e drop-rate** — Cgg / [genshin-wishes.com](https://genshin-wishes.com), [analisi statistica HoYoLAB](https://www.hoyolab.com/article/497840).
- **Genshin Impact** © HoYoverse. Nomi e dati di gioco appartengono a loro; questo è uno strumento fan non ufficiale, non affiliato né approvato da HoYoverse.

### Sviluppo
```
python run_tests.py     # rigenera le fixture + esegue tutti i test
python gen_testdata.py  # rigenera solo le fixture in testdata/
node test_model.js      # modello + copertura dei rami sul codice reale
node test_ui.js         # percorsi UI interattivi (DOM simulato)
```
`index.html` è l'unico file da distribuire. Tutto il resto è strumentazione di sviluppo.
