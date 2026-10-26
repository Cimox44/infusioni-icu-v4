# ICU Cardio · Calcolatore Infusioni

Calcolatore di infusione continua per Terapia Intensiva Cardiochirurgica.
Applicazione web installabile (PWA), **completamente offline**, nessun dato trasmesso a server esterni.

---

## 1. Contenuto della cartella

| File | Ruolo | Obbligatorio |
|---|---|---|
| `index.html` | Applicazione completa (HTML + CSS + JS in un unico file) | Sì |
| `manifest.json` | Metadati PWA: nome, icone, colori, modalità standalone | Sì per l'installazione |
| `sw.js` | Service worker: mette in cache i file e garantisce il funzionamento offline | Sì per l'uso offline |
| `icon-192.png` | Icona 192×192 (Android, scorciatoia) | Sì |
| `icon-512.png` | Icona 512×512 (splash screen) | Sì |
| `icon-512-maskable.png` | Icona adattiva Android (purpose `maskable`) | Consigliata |
| `README.md` | Questo documento | No |

> **Regola d'oro:** i cinque file devono stare **nella stessa cartella**, allo stesso livello.
> Il file della pagina iniziale deve chiamarsi esattamente `index.html` (minuscolo).

---

## 2. Pubblicazione su GitHub Pages — percorso A (interfaccia web, senza installare nulla)

### Passo 1 — Account
Vai su <https://github.com> → **Sign up** (o **Sign in** se hai già l'account `Cimox44`).

### Passo 2 — Crea il repository
1. In alto a destra: **+** → **New repository**.
2. **Repository name**: `calcolatore-infusioni-icu` (solo minuscole, trattini al posto degli spazi).
3. **Description** (facoltativa): `Calcolatore infusioni ICU cardiochirurgica`.
4. Seleziona **Public** — *GitHub Pages sui repository privati richiede un piano a pagamento*.
5. **Non** spuntare "Add a README file" (lo carichi tu).
6. **Create repository**.

### Passo 3 — Carica i file
1. Nella pagina del repository vuoto: **uploading an existing file**
   (oppure **Add file** → **Upload files**).
2. Trascina **tutti e sei i file insieme** — non le cartelle, i singoli file.
3. In basso, nel campo **Commit changes**, scrivi `Prima versione del calcolatore`.
4. **Commit changes**.

### Passo 4 — Attiva GitHub Pages
1. Tab **Settings** (in alto nel repository).
2. Menu di sinistra → **Pages**.
3. **Source**: `Deploy from a branch`.
4. **Branch**: `main` · Cartella: **`/ (root)`** ← passaggio critico, non `/docs`.
5. **Save**.

### Passo 5 — Attendi e apri
Dopo 1–3 minuti, ricaricando la pagina *Settings → Pages* compare il banner verde con l'indirizzo:

```
https://cimox44.github.io/calcolatore-infusioni-icu/
```

Lo stato della pubblicazione è visibile anche nel tab **Actions** (segno di spunta verde = pubblicato).

---

## 3. Pubblicazione — percorso B (Git da terminale)

Utile se prevedi aggiornamenti frequenti.

```bash
# una sola volta, per configurare l'identità
git config --global user.name  "Cimox"
git config --global user.email "tua@email.it"

# dentro la cartella che contiene i file
cd percorso/della/cartella
git init
git add .
git commit -m "Prima versione del calcolatore"
git branch -M main
git remote add origin https://github.com/Cimox44/calcolatore-infusioni-icu.git
git push -u origin main
```

Poi esegui il **Passo 4** della sezione precedente (Settings → Pages).

Per gli aggiornamenti successivi:

```bash
git add .
git commit -m "Aggiornati i range di dosaggio"
git push
```

---

## 4. Installazione come app sul dispositivo

**Android (Chrome)** — apri il link → menu ⋮ → **Installa app** / **Aggiungi a schermata Home**.
L'app si apre a schermo intero, senza barra del browser, e funziona senza rete.

**iPhone / iPad (Safari — obbligatorio, non funziona da Chrome iOS)** — apri il link → icona
**Condividi** → **Aggiungi a Home**.

**Windows / macOS (Chrome o Edge)** — icona di installazione nella barra degli indirizzi
(un monitor con freccia verso il basso) → **Installa**.

> Apri l'app collegato alla rete **almeno una volta**: è il momento in cui il service worker
> scarica e memorizza i file. Da quel momento funziona anche in modalità aereo.

---

## 5. Aggiornare l'app dopo una modifica

Il service worker serve i file dalla cache: se modifichi `index.html` senza altro,
i dispositivi già installati continuano a vedere la versione vecchia.

**Procedura corretta a ogni modifica:**

1. Apri `sw.js` e incrementa il numero di versione:
   ```js
   const CACHE = 'icu-infusioni-v1.3';   // era v1.2
   ```
2. Carica su GitHub `index.html` **e** `sw.js`.
3. Sul dispositivo: chiudi completamente l'app e riaprila (a volte servono due aperture —
   la prima scarica la nuova versione, la seconda la mostra).

---

## 6. Risoluzione dei problemi

| Sintomo | Causa più probabile | Soluzione |
|---|---|---|
| **404 – There isn't a GitHub Pages site here** | Pages non attivato, oppure branch/cartella errati | Settings → Pages → Branch `main`, cartella `/ (root)` → Save; attendi 2–3 min |
| **404 nonostante Pages attivo** | Il file si chiama `Index.html`, `index.htm` o è dentro una sottocartella | Rinominalo `index.html` e spostalo nella radice del repository |
| **Pagina bianca** | Un file mancante o rinominato | Controlla che `manifest.json` e `sw.js` siano presenti con quel nome esatto |
| **Non compare "Installa app"** | Sito non in HTTPS, o `manifest.json` non raggiungibile | GitHub Pages è già in HTTPS: verifica il manifest aprendo `…/manifest.json` nel browser |
| **Le modifiche non si vedono** | Cache del service worker | Incrementa `CACHE` in `sw.js` (sezione 5) |
| **Repository privato** | Pages non disponibile nel piano gratuito | Settings → General → Change visibility → Public |
| **Il calcolo restituisce «—»** | Unità non convertibili (es. dose in mEq con concentrazione in mg) | Allinea le unità: massa con massa, U con U, mEq con mEq, mmol con mmol |

---

## 7. Come si usa il calcolatore

1. **Peso** in alto a destra: aggiorna istantaneamente tutti i calcoli peso-dipendenti.
2. **Prontuario** a sinistra: filtra per categoria o cerca per nome.
3. **Diluizione**: scegli una preparazione standard oppure inserisci quantità, unità e volume tuoi.
4. **Calcolo bidirezionale**: scrivi nella casella **Dose** e ottieni i **ml/h**; scrivi nella casella
   **Velocità** e ottieni la dose. Il campo con il bordo oro è quello che sta guidando il calcolo.
5. **Righello di titolazione**: mostra la posizione della dose rispetto a minima, mantenimento e massima,
   con avviso esplicito al superamento del massimo di riferimento.
6. **Equivalenze**: la stessa dose espressa in tutte le unità della stessa famiglia.
7. **Farmaco personalizzato**: nome, unità (tra 22 disponibili), range e diluizione definiti da te.
8. **Tabella sinottica**: quadro completo di tutti i farmaci, stampabile o esportabile in PDF.

---

## 7b. Punteggi vasoattivi (VIS · NEE · VDI · VEI)

Pulsante **Score vasoattivi**. Inserisci le dosi dei farmaci in corso — oppure invia direttamente la dose
dal calcolatore con **Invia questa dose ai punteggi** — e aggiungi la MAP misurata nello stesso momento.

**VIS** — Vasoactive-Inotropic Score (dosi in mcg/kg/min salvo indicazione):

```
VIS = dopamina + dobutamina + enoximone
    + 10  × milrinone
    + 10  × fenilefrina
    + 50  × levosimendan
    + 100 × (adrenalina + noradrenalina)
    + 10.000 × vasopressina (U/kg/min)
    + 20  × blu di metilene (mg/kg/h)
    + 10  × terlipressina (mcg/min)
    + 0,25 × angiotensina II (ng/kg/min)
```

**NEE / NED** — dose equivalente di noradrenalina, formulazione aggiornata (Kotani et al., *Critical Care* 2023):

```
NEE = noradrenalina + adrenalina
    + 0,01   × dopamina
    + 0,06   × fenilefrina
    + 0,125  × metaraminolo   (1/8 — correzione Kotani 07/03/2025)
    + 2,5    × vasopressina (U/min)
    + 0,0025 × angiotensina II (ng/kg/min)
    + 10     × terlipressina (mcg/kg/min)
    + 0,2    × blu di metilene (mg/kg/h)
```

**VDI** = VIS / MAP · **VEI** = MAP / VIS

| Punteggio | Fasce di lettura |
|---|---|
| VIS | <10 basso · 10–20 moderato · 20–30 elevato · >30 molto elevato |
| NEE | <0,1 dose bassa · 0,1–0,3 intermedia · 0,3–1 elevata · >1 shock refrattario |
| VDI | più alto = maggiore dipendenza dal supporto farmacologico a parità di pressione |
| VEI | più alto = maggiore efficienza (più pressione per unità di supporto) |

**Tre avvertenze da tenere presenti.**
I coefficienti sono attribuiti per convenzione e non rappresentano una reale equipotenza farmacologica.
Il valore isolato conta poco: è il **trend** nel tempo l'informazione clinicamente utile.
La denominazione «vasopressor dependency index» in letteratura compare sia come VIS/MAP sia, in alcuni
protocolli, come MAP/VIS: nell'app sono riportati entrambi in modo esplicito, senza ambiguità.
Infine, noradrenalina **base** (1 mg/ml) e **tartrato** (2 mg/ml) differiscono di un fattore 2 e cambiano
il punteggio: va sempre dichiarato quale formulazione si utilizza.

---

## 7c. Modalità pediatrica

Selettore **Adulto / Pediatrico** in alto. Quando è attiva:

- i **range di riferimento diventano pediatrici** per 33 farmaci (badge oro «Range pediatrico» accanto al nome);
  per i farmaci privi di range pediatrico dedicato compare un avviso rosso esplicito;
- compare il pannello **carico idrico**: ml/kg/h, ml/kg/24 h e **quota percentuale del fabbisogno di
  mantenimento** calcolato con la formula di Holliday-Segar (100 ml/kg per i primi 10 kg, 50 ml/kg per i
  successivi 10, 20 ml/kg oltre i 20 kg);
- sono disponibili le **regole di diluizione peso-dipendenti**:

| Regola | Preparazione | Corrispondenza |
|---|---|---|
| Regola del 6 | 6 mg × peso in 100 ml | 1 ml/h = 1 mcg/kg/min |
| Regola dello 0,6 | 0,6 mg × peso in 100 ml | 1 ml/h = 0,1 mcg/kg/min |
| Regola del 3 | 3 mg × peso in 50 ml | 1 ml/h = 1 mcg/kg/min |

Le regole si applicano ai farmaci dosati in mcg/kg/min (catecolamine, vasodilatatori); per gli altri
resta valida la diluizione libera. Nel neonato e nel lattante il volume dell'infusione è spesso il vincolo
principale: verifica sempre il carico idrico complessivo prima di scegliere la concentrazione.

---

## 8. Avvertenza

Strumento di supporto al calcolo destinato a personale sanitario formato. I range riportati sono
valori orientativi per il paziente adulto in terapia intensiva cardiochirurgica e non sostituiscono
la prescrizione medica, il protocollo di reparto, la scheda tecnica del farmaco (RCP) né il giudizio
clinico. Verificare sempre concentrazione, diluente, compatibilità, via di somministrazione e
adeguamento della dose in insufficienza renale ed epatica, ipotermia, CEC, ECMO e CRRT.
**Doppio controllo indipendente per tutti i farmaci ad alto rischio.**

---

## 9. Registro delle versioni

### v1.2 — 26/10/2026 (revisione di sicurezza)

**Motore di calcolo**
- Cambio dell'unità di dose: ora il valore viene convertito mantenendo la stessa dose fisica (in v1.1 il numero restava invariato: 0,1 mcg/kg/min → «0,1 mcg/min», velocità 70 volte inferiore).
- Peso vuoto o non valido: nessun ripiego silenzioso a 70 kg; i calcoli per kg mostrano «—» con avviso (range ammesso 0,3–350 kg).
- Modalità pediatrica: dose e unità iniziali presi dal range pediatrico (in v1.1 si partiva dalla dose adulta, es. KCl 10 mEq/h a 10 kg).
- Conversioni g ⇄ mEq ⇄ mmol per gli elettroliti (MgSO₄, CaCl₂, Ca gluconato, KCl, NaHCO₃, NaCl 3%): le unità offerte ora funzionano.
- Autonomia arrotondata al minuto (eliminato «2:60»); valori negativi rifiutati; avvisi per velocità <0,1 ml/h o >200 ml/h e peso incoerente con la modalità.
- Formattazione a 3 cifre significative per le dosi molto piccole; confronti con il range con tolleranza numerica.
- Regole del 6/0,6/3 attive solo per i farmaci titolati in mcg/kg/min.

**Punteggi**
- NEE: fattore del metaraminolo 1/8 (era 8, sovrastima ×64) secondo la correzione pubblicata il 07/03/2025.
- Nota sulle trascrizioni discordanti dei coefficienti del VIS esteso (Belletti 2021).

**Contenuti clinici**
- Terlipressina 2→12 mg/24 h (EMA 2022) con avvertenza di insufficienza respiratoria; enoximone 5–20 mcg/kg/min, solo plastica, ≤48 h;
  cisatracurio 3 (0,5–10,2) mcg/kg/min; nitroglicerina massimo di riferimento 5 mcg/kg/min; eparina massimo 25 U/kg/h;
  amiodarone secondo RCP (≤1200 mg/24 h, ≥0,6 mg/ml, set senza DEHP); piperacillina/tazobactam espressa come piperacillina;
  noradrenalina → vasopressina a 0,25–0,5 mcg/kg/min (SSC 2021); dexmedetomidina e propofol aggiornati a PADIS 2025 e DECADE;
  propofol: non indicato per la sedazione in ICU ≤16 anni (avviso in modalità pediatrica).

**PWA**
- Service worker: pagina «rete prima» (gli aggiornamenti arrivano senza svuotare la cache), installazione tollerante ai file mancanti, CACHE `icu-infusioni-v1.2`.

### v1.1 — 22/07/2026
Punteggi vasoattivi (VIS, NEE, VDI, VEI) e modalità pediatrica.

### v1.0 — 22/07/2026
Prima pubblicazione: 81 farmaci, calcolo bidirezionale, tabella sinottica.
