# Sister Match — Brief di sviluppo per LLM

> **Documento di lavoro** per la realizzazione completa del gioco web "Sister Match", parte della campagna marketing del musical *Sister Act* in scena al Teatro Nazionale di Milano da ottobre.
> 
> **Destinatario:** un LLM con capacità di scrittura codice (Claude, GPT-5, Gemini o equivalente).
> 
> **Output atteso:** singolo pacchetto deploy-ready (`index.html` + `audio_loop.mp3` + asset logo), con tutti i 10 livelli, boss finale, schermata premio, pronto per drag & drop su Netlify Drop o Vercel.

---

## 0. Come usare questo documento

Questo brief è scritto perché tu (LLM esecutore) possa svilupparlo **senza dover indovinare nulla**. Se trovi un'ambiguità, **non inventare**: solleva la questione al committente prima di procedere.

**Lavora in 3 fasi:**
1. **Studio** del documento intero prima di scrivere una riga di codice
2. **Esecuzione** seguendo l'ordine dei capitoli
3. **Auto-verifica** finale contro la checklist in §14

**Principi non negoziabili:**
- Singolo file HTML autonomo (`index.html`), eccetto `audio_loop.mp3` e file logo
- Vanilla JS, niente framework (no React, no Vue, no librerie esterne a parte font Google)
- Mobile-first: testato su iPhone Safari e Android Chrome
- Peso totale &lt; 3 MB (audio incluso)
- Caricamento &lt; 5 secondi su 4G
- Performance: 60 FPS sulle animazioni, niente lag tattile

---

## 1. Contesto del progetto

### 1.1 Cliente e prodotto
Il committente produce il musical **Sister Act** che debutta al **Teatro Nazionale di Milano a ottobre**, con un'anteprima esclusiva l'**8 ottobre**.

### 1.2 Obiettivo della campagna
Marketing virale pre-debutto. Il gioco deve:
- Incuriosire il pubblico (awareness)
- Essere condiviso su WhatsApp, Instagram, TikTok, Facebook
- Generare lead qualificati attraverso il premio finale (biglietti per l'anteprima)
- Funzionare da "calling card" della produzione

### 1.3 Pubblico
Target trasversale: nostalgici del film (40-60 anni), appassionati di musical, giovani/famiglie. Il gioco deve essere giocabile in metropolitana, in 90 secondi, senza istruzioni complicate.

### 1.4 Diritti
- ✅ Logo ufficiale Sister Act (fornito)
- ✅ Audio "Fammi Volare" (fornito, già ottimizzato)
- ❌ NESSUN uso di asset del film Disney/Touchstone
- ❌ NESSUN uso dell'immagine di Whoopi Goldberg
- ❌ NESSUN frame, foto, audio originali del film

Se l'LLM è tentato di "arricchire" con riferimenti al film, **fermarsi**: usare solo asset forniti o emoji/CSS originali.

---

## 2. Asset forniti dal committente

Il committente fornirà i seguenti file da posizionare nella stessa cartella di `index.html`:

| File | Descrizione | Uso |
|------|-------------|-----|
| `audio_loop.mp3` | Traccia "Fammi Volare (Nightclub, sezione lenta 118 BPM)", ottimizzata a 96 kbps, ~1,9 MB, durata 2'46" | Loop di sottofondo del gioco |
| `intro-photo.jpg` | Foto promozionale verticale Sister Act | Schermata intro (a larghezza piena sopra bottoni) |
| `footer-banner.jpg` | Banner orizzontale logo Sister Act | Footer overlay gioco (larghezza piena, fisso in basso, **visibile solo in schermata di gioco**) |
| `sister-act-logo-oriz.png` | Logo ufficiale orizzontale, rosso script con outline cream su sfondo trasparente | Topbar gioco |
| `sister-act-logo-vert.png` | Logo ufficiale verticale, stessa identità | Schermata finale (premio) |

**Comportamento di fallback se i file non sono presenti:** l'LLM implementa una versione tipografica del logo usando CSS (vedi §4.3 fallback logo), così il gioco resta funzionante anche senza i file PNG mentre si attendono dal committente.

---

## 3. Identità visiva

### 3.1 Palette colori (CSS variables obbligatorie)

```css
:root {
  --red:        #d8232a;  /* rosso del logo */
  --red-deep:   #8a0e14;  /* ombra rossa */
  --gold:       #f4c842;  /* accento gospel */
  --gold-deep:  #b8891f;
  --cream:      #fff5e1;  /* bianco caldo */
  --ink:        #0a0a0a;  /* nero profondo */
  --ink-soft:   #1c1c1c;
  --halo:       rgba(244, 200, 66, 0.4);
}
```

**Non aggiungere** colori "alla moda" tipo viola/teal/gradient pastello. L'estetica è **gospel-pop anni '90**, non SaaS moderno.

### 3.2 Tipografia

Caricare via Google Fonts:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Lobster&family=Bowlby+One+SC&family=Manrope:wght@400;600;800&display=swap" rel="stylesheet">
```

| Variabile | Font | Uso |
|---|---|---|
| `--font-display` | Lobster | Titoli grandi, logo fallback, scritte celebrative |
| `--font-impact` | Bowlby One SC | Etichette HUD, bottoni, scritte impact |
| `--font-body` | Manrope (400/600/800) | Testi corpo, descrizioni |

### 3.3 Tono delle scritte
**Gospel-italiano, caloroso, mai ironico cattivo.** Esempi del registro:
- "Halleluja!" "Amen!" "Miracolo!" "Sister, riprova!" "Che gioia!" "Cantiamo!"
- Mai cinico, mai sarcastico, mai cool-detached
- L'utente è una "Sister" — verso di lei sempre incoraggiante, mai punitivo

---

## 4. Struttura dell'app

### 4.1 Layout generale

App single-page, layout mobile portrait, `max-width: 480px` centrato. Su desktop l'app è centrata con sfondo nero ai lati e ombra laterale (`box-shadow`). Struttura verticale:

```
┌──────────────────────────┐
│  TOPBAR (livello, logo, mute/restart) │
├──────────────────────────┤
│  HUD (obiettivi + mosse residue)      │
├──────────────────────────┤
│                                       │
│  GRIGLIA DI GIOCO (7 colonne × 8 righe)│
│  (a pieno schermo, footer è overlay)   │
├──────────────────────────┤
│  FOOTER OVERLAY (banner, fixed bottom) │
│  ⚠️ Visibile SOLO nella schermata di  │
│     gioco, nascosto nella intro        │
└──────────────────────────┘
```

### 4.2 Overlay (full-screen, z-index alto)
- `#introOverlay` — schermata iniziale con foto promozionale e bottone (NESSUN logo o tagline separati)
- `#cutsceneOverlay` — narrazione tra un livello e l'altro (vedi §6)
- `#endOverlay` — fine livello (win/lose)
- `#prizeOverlay` — schermata premio finale (post-livello 10) ⭐ §11
- `#tutorialOverlay` — manina animata alla prima partita

### 4.3 Logo (intro screen e fallback)

**Versione con file PNG:**
```html
<img src="sister-act-logo-vert.png" alt="Sister Act" class="logo-vert">
```

**Versione fallback CSS** (se PNG non presente o per refusione tipografica):
```html
<div class="logo-fallback">Sister<br>Act</div>
```
```css
.logo-fallback {
  font-family: var(--font-display);
  font-size: clamp(58px, 18vw, 110px);
  color: var(--red);
  text-shadow: 4px 4px 0 var(--cream), 8px 8px 0 var(--ink);
  transform: rotate(-5deg);
  line-height: 0.85;
}
```

L'LLM esecutore implementa **entrambi** con fallback automatico (img onerror → mostra div fallback).

---

## 5. Game design — Meccanica match-3

### 5.1 Griglia
- **7 colonne × 8 righe** = 56 celle
- Celle quadrate, dimensione auto-fit per riempire l'altezza disponibile
- Aspect ratio mantenuto

### 5.2 Simboli (le "icone" del match-3)

```js
const SYMBOLS = ['🎤', '🙏', '🕯️', '📿', '💿', '💄'];
```

Ogni simbolo ha un significato narrativo (NON da spiegare all'utente, ma da rispettare nella scelta degli obiettivi):

| Emoji | Significato | "Mondo" narrativo |
|---|---|---|
| 🎤 | microfono | canto / coro / palcoscenico |
| 🙏 | mani giunte | preghiera / suora (segnaposto temporaneo del **velo da suora**, in attesa di asset definitivo) |
| 🕯️ | candela | chiesa / sacralità |
| 📿 | rosario | religiosità |
| 💿 | disco vinile | musica anni '90 / Deloris artista |
| 💄 | rossetto | vita "civile" / glamour di Deloris |

**Nota importante per Fase 2:** quando arriveranno gli asset illustrati, il `🙏` verrà sostituito con un'illustrazione di **velo da suora**. Tutti gli emoji diventeranno `<img>` con SVG/PNG dedicati. Il codice deve essere strutturato per consentire questa sostituzione facilmente (es. funzione `renderSymbol(sym)` centralizzata, non emoji hardcoded sparsi nel DOM).

### 5.3 Regole base
- L'utente scambia un simbolo con uno **adiacente** (su/giù/sx/dx, mai diagonale) trascinando
- Se lo scambio crea un allineamento di **3+ simboli uguali** in orizzontale o verticale, è valido
- Se non crea match, lo swap viene annullato (animazione "indietro")
- I match raccolti spariscono, i simboli sopra scendono per gravità, nuovi simboli appaiono dall'alto
- Le cascate generano **combo** (vedi §5.5)

### 5.4 Generazione griglia iniziale
- Mai generare match-3 al caricamento (la griglia iniziale è "neutra")
- Garantire sempre almeno una mossa valida possibile
- Se durante il gioco non ci sono più mosse → **shuffle silenzioso** della griglia (senza notificare l'utente)

### 5.5 Combo (cascate)
Quando un match scatena una caduta che crea un altro match, è una combo. Mostrare un toast centrale con la scritta:
- Combo 2: "**Amen!**"
- Combo 3: "**Halleluja!**"
- Combo 4: "**Miracolo!**"
- Combo 5+: "**Divino!**"

Toast con animazione di apparizione (scale 0 → 1.2 → 1, rotazione lieve), durata 1 secondo, font display rosso/oro.

**Effetto lampo:** ad ogni match (incluso il primo di una sequenza combo), un lampo dorato (`radial-gradient` con centro bianco-oro) illumina la parte alta dello schermo per 0.8 secondi. L'effetto è un div `#matchFlash` con `position:fixed` e classe `.flash` aggiunta/rimossa ad ogni match. Il lampo rende il momento del match più soddisfacente e visivamente impattante sullo sfondo nero.

### 5.6 Punteggio
- Match base: 10 punti × moltiplicatore combo
- Bonus fine livello: 100 × (mosse residue) + 500 × (stelle ottenute)
- Le stelle sono basate sulle mosse residue: 3⭐ se ≥40% residue, 2⭐ se ≥15%, 1⭐ se &lt; 15%

### 5.7 Input
**Pointer Events API** (unificato mouse + touch). Soglia trascinamento: 18px. Click+click (cella1 → cella2 adiacente) come fallback.

Critico: `touch-action: none` sulla griglia per impedire scroll durante swipe.

---

## 6. I 10 livelli — Bilanciamento

### 6.1 Filosofia di bilanciamento

- **Livelli 1-3: onboarding morbido.** L'utente impara senza frustrarsi. Win rate atteso al primo tentativo: ≥80%.
- **Livelli 4-7: sfida progressiva.** Difficoltà crescente, obiettivi più diversificati. Win rate atteso al primo tentativo: 50-70%.
- **Livelli 8-9: vero impegno.** Combinazioni di obiettivi che richiedono strategia. Win rate atteso: 30-50%.
- **Livello 10 — boss Madre Superiora:** meccanica speciale (vedi §7). Difficile ma fattibile in 2-3 tentativi.

### 6.2 Tabella dei 10 livelli

Per ogni livello: nome narrativo, mosse disponibili, obiettivi (simbolo + quantità da raccogliere).

| # | Nome | Mosse | Obiettivi |
|---|---|---|---|
| 1 | Coro alle Prime Armi | 25 | 🎤 ×12, 🙏 ×10 |
| 2 | La Prima Lezione | 24 | 🎤 ×14, 🕯️ ×10 |
| 3 | Hallelujah! | 22 | 🎤 ×15, 🕯️ ×12, 📿 ×8 |
| 4 | Le Voci si Alzano | 22 | 🙏 ×15, 📿 ×12, 💿 ×8 |
| 5 | Disco Inferno | 20 | 💿 ×18, 💄 ×12, 🎤 ×10 |
| 6 | La Chiesa Si Riempie | 20 | 🕯️ ×16, 🙏 ×14, 📿 ×12 |
| 7 | La Stampa Ne Parla | 19 | 🎤 ×18, 💿 ×15, 💄 ×10 |
| 8 | Verso il Palcoscenico | 18 | 🎤 ×20, 🙏 ×16, 🕯️ ×14 |
| 9 | La Notte Prima del Debutto | 17 | tutti i 6 simboli ×10 ciascuno |
| 10 | **Madre Superiora** (BOSS) | 20 | vedi §7 |

### 6.3 Cutscenes tra un livello e l'altro

Tra ogni livello superato e il successivo, mostrare una **cutscene testuale breve** (5-6 secondi auto-skip + bottone "Avanti"). Schema:

```
[ATTO X DI 10]
[icona emoji decorativa]
[Titolo del livello successivo]
[1 frase di contesto narrativo, max 25 parole]
[bottone "Avanti"]
```

**Tono storyline** (richiesta del committente): variante tra **"divertente"**, **"epica"** e **"misteriosa"**. L'LLM è libero di sviluppare le frasi di contesto scegliendo una di queste varianti coerentemente per tutta la sequenza. Es. di tono "divertente":
- Liv 2: "*Sister Mary Patrick è entusiasta. Sister Mary Lazarus, un po' meno. Tu ci credi!*"
- Liv 5: "*Le suore stanno scoprendo gli anni '90. Ora vogliono tutte un rossetto.*"

Es. tono "epico":
- Liv 2: "*Il primo passo è stato fatto. La voce si alza. Il convento, da oggi, non è più lo stesso.*"

L'LLM **sceglie una variante di tono e la mantiene fino al livello 10**.

---

## 7. Boss livello 10 — Madre Superiora

### 7.1 Concept
La Madre Superiora è scettica sul coro. L'utente deve "convincerla" raccogliendo simboli specifici secondo una sequenza imposta.

### 7.2 Meccanica
- 20 mosse a disposizione
- Sulla griglia, un riquadro speciale in cima con icona **Madre Superiora** (👩🏾‍✈️ come segnaposto) e una **barra di pazienza** dorata
- La barra parte piena. **Diminuisce di 1 unità a ogni mossa**, indipendentemente da match o no
- Per riempire la barra, l'utente deve raccogliere **simboli "preghiera" 🙏** (ogni 🙏 raccolto = +2 unità di pazienza)
- Obiettivo finale: raccogliere **30 simboli totali misti** (🎤 ×10, 🙏 ×12, 🕯️ ×8) **prima che la pazienza arrivi a zero**
- Se la pazienza arriva a zero → game over (la Madre Superiora ha "perso fiducia")
- Se l'utente completa gli obiettivi → vince → schermata premio §11

### 7.3 Identità visiva del boss
- Sfondo della griglia leggermente "scurito" (overlay viola scuro al 20%)
- Pulsazione del bordo della griglia in rosso quando pazienza &lt; 30%
- Toast speciali: "*La Madre Superiora ti scruta...*" ogni 5 mosse

### 7.4 Cutscene di intro al boss
```
[ATTO 10 — IL BOSS FINALE]
[icona 👩🏾‍✈️]
[Titolo: "Madre Superiora"]
[Testo: "La Madre Superiora non è convinta. Devi conquistare la sua fiducia con la preghiera e il canto. Non perderla di vista: la sua pazienza è limitata!"]
[bottone "Inizia la sfida"]
```

---

## 8. HUD e indicatori

### 8.1 Top bar
- Pulsante "Atto X" (a sinistra, pill dorata)
- Logo trasparente "Sister Act Match Mania" al centro (PNG `logo-match-mania.png`, altezza 52px, con fallback CSS)
- Bottoni icona a destra: 🔊/🔇 mute, ↻ restart

### 8.2 HUD principale (sotto la topbar)
- A sinistra: lista degli obiettivi in corso, ognuno con simbolo + "X/Y" (es. 🎤 5/12)
- A destra: contatore mosse rimanenti (font impact dorato grande)
- Quando un obiettivo è completato: la scritta diventa dorata + animazione pulse

### 8.3 Boss HUD speciale (solo livello 10)
- Sopra l'HUD normale, riga extra con icona Madre Superiora + barra pazienza dorata orizzontale
- La barra usa `transition: width 0.3s ease` per ogni cambiamento

---

## 9. Audio

### 9.1 Specifica
- File: `audio_loop.mp3`
- Tag HTML: `<audio id="bgAudio" src="audio_loop.mp3" loop preload="auto"></audio>`
- Volume default: **0.45**
- Loop infinito
- Parte **solo al primo tap utente** (regola autoplay browser). Trigger: bottone "Inizia a giocare" su `#introOverlay`

### 9.2 Mute toggle
- Bottone 🔊 / 🔇 sempre visibile in topbar
- Click → alterna lo stato muted dell'audio
- Stato visivo: opacità 50% e bordo opaco quando muted
- **Lo stato viene salvato in `localStorage`** sotto chiave `sm_muted` (in modo che riaprendo il gioco lo trovi come l'aveva lasciato)

### 9.3 Effetti sonori (opzionali, da implementare se possibile)
Se l'LLM riesce a generare effetti sonori di base via Web Audio API (sintesi pura, no file esterni), aggiungere:
- Tin/bling al match (frequenza alta breve)
- Whoosh alla cascata
- Fanfara breve al completamento livello

Se la generazione audio via Web Audio appare rischiosa, **omettere completamente** gli effetti sonori. Solo il loop musicale è obbligatorio.

---

## 10. Tutorial e prima esperienza

### 10.1 Tutorial alla prima partita
- Alla prima apertura del livello 1, overlay semi-trasparente sulla griglia con:
  - Manina animata 👆 che oscilla (animazione CSS keyframes)
  - Testo: "**Trascina un simbolo verso quello vicino**"
- Sparisce al primo swap riuscito
- Lo stato "tutorial visto" salvato in `localStorage` (chiave `sm_tutorial_done`) per non rimostrarlo

### 10.2 Schermata intro
- Foto promozionale verticale Sister Act in alto (a larghezza piena, scrollabile se serve, **nessun max-height che tagli l'immagine**)
- Sotto la foto: anteprima dei simboli obiettivo del livello 1
- Bottone "Inizia a giocare" (gold)
- Opzione "Riprendi dal livello X" se progresso salvato
- NESSUN logo, tagline o sub-tagline separati (la foto contiene già il branding)
- Footer banner **nascosto** in questa schermata, appare con fade-in solo quando si chiude l'overlay intro

---

## 11. ⭐ SCHERMATA PREMIO FINALE (post-livello 10)

Quando l'utente completa il livello 10 (boss), mostrare un overlay celebrativo dedicato. **Questa è la schermata strategicamente più importante del gioco** — è la conversion del progetto.

### 11.1 Contenuto
```
[Logo verticale Sister Act in alto]

[Titolo gigante font Lobster rosso/cream:]
"COMPLIMENTI!"

[Sottotitolo gold:]
"HAI SUPERATO TUTTI I LIVELLI"

[Testo principale, font body 16-18px, centrato:]
"Ti regaliamo i biglietti per l'Anteprima esclusiva
di Sister Act il Musical
in scena l'8 ottobre"

[CTA principale grande gold:]
"RISCATTA I BIGLIETTI"
→ apre link a pagina dedicata (placeholder: https://sisteractmilano.it/premio
   il committente sostituirà con l'URL reale del modulo di riscatto)

[CTA secondaria minore:]
"📲 Condividi con gli amici"
→ apre share WhatsApp con testo:
   "Ho appena vinto i biglietti per l'anteprima di Sister Act!
    Provaci anche tu: [URL_DEL_GIOCO]"
```

### 11.2 Animazioni
- Apparizione del logo con bounce
- Coriandoli dorati che cadono (CSS keyframes, no librerie)
- Bottone CTA con pulse leggero
- Audio: opzionale stacco musicale di celebrazione (se non disponibile, alza volume del loop principale a 0.7 in questa schermata)

### 11.3 Riscatto biglietti — chiarimento importante
**Il gioco NON gestisce direttamente il riscatto.** La pagina di destinazione del bottone "Riscatta i biglietti" (`https://sisteractmilano.it/premio`) sarà sviluppata separatamente dal committente e gestirà:
- Form email + dati per contatto
- Consenso GDPR
- Limite massimo biglietti (logica anti-frode)
- Conferma riscatto via email

Il gioco si limita a **passare l'utente** a quella pagina con un parametro UTM tracking, es.:
```
https://sisteractmilano.it/premio?source=sister-match&score=12345
```
Dove `score` è il punteggio totale accumulato dall'utente nei 10 livelli (può servire per leaderboard, gamification, segmentazione).

### 11.4 Cosa succede se l'utente perde il livello 10
La schermata premio **non appare**. L'utente vede la schermata di game-over standard con incoraggiamento a riprovare ("La Madre Superiora ti dà un'altra chance!").

---

## 12. Condivisione social

### 12.1 Bottone condividi (presente a fine ogni livello)
Usare `navigator.share()` se disponibile, altrimenti fallback WhatsApp Web:

```js
const text = `Ho fatto ${score} punti a Sister Match! 🎤✨ Sister Act il Musical, al Teatro Nazionale di Milano da ottobre. #SisterActMilano`;
const url = location.href;

if (navigator.share) {
  navigator.share({ title: 'Sister Match', text, url });
} else {
  window.open(`https://wa.me/?text=${encodeURIComponent(text + ' ' + url)}`, '_blank');
}
```

### 12.2 Meta tag Open Graph
Includere in `<head>` per ottimizzare condivisioni:

```html
<meta property="og:title" content="Sister Match — Il gioco di Sister Act il Musical">
<meta property="og:description" content="Allinea, canta, vinci i biglietti per l'anteprima dell'8 ottobre!">
<meta property="og:image" content="sister-act-logo-oriz.png">
<meta property="og:type" content="website">
<meta name="twitter:card" content="summary_large_image">
```

---

## 13. Persistenza dati (localStorage)

Salvare:
- `sm_muted` — stato audio mute (string '0' o '1')
- `sm_tutorial_done` — tutorial visto ('1')
- `sm_max_level` — massimo livello sbloccato dall'utente (per consentirgli di riprendere dal livello dove era arrivato)
- `sm_best_score` — miglior punteggio totale

### 13.1 Comportamento riapertura
Se l'utente riapre il gioco e ha già `sm_max_level > 0`:
- Mostrare un piccolo prompt nella schermata intro: "*Riprendi dal livello X*" / "*Ricomincia da capo*"
- Il "Riprendi" carica direttamente il livello salvato

---

## 14. Checklist di accettazione

Prima di consegnare, l'LLM esecutore deve **verificare ciascun punto**:

### 14.1 Funzionalità di gioco
- [ ] Tutti i 10 livelli sono giocabili in sequenza
- [ ] Il boss livello 10 ha meccanica pazienza funzionante
- [ ] Le cascate combo mostrano i toast "Amen/Halleluja/Miracolo/Divino"
- [ ] Lo shuffle silenzioso si attiva se non ci sono mosse
- [ ] Il sistema stelle (1/2/3) funziona in base alle mosse residue

### 14.2 UI/UX
- [ ] Layout mobile-first testato fino a 360px di larghezza
- [ ] Touch swipe funziona fluido su iPhone Safari + Android Chrome
- [ ] Mouse drag funziona su desktop
- [ ] Tutorial appare alla prima partita, non più dopo
- [ ] Cutscenes tra livelli funzionano con un solo tono coerente
- [ ] Logo (PNG o fallback CSS) appare nella intro

### 14.3 Audio
- [ ] Audio parte al primo tap di "Inizia a giocare"
- [ ] Bottone mute funziona e persiste in localStorage
- [ ] Volume al 45% di default
- [ ] Loop continuo senza buchi audibili

### 14.4 Schermata premio
- [ ] Appare SOLO dopo aver vinto il livello 10
- [ ] Testo conforme: "Complimenti! Hai superato tutti i livelli. Ti regaliamo i biglietti per l'Anteprima esclusiva di Sister Act il Musical in scena l'8 ottobre"
- [ ] Bottone "Riscatta i biglietti" punta a URL placeholder configurabile
- [ ] Bottone "Condividi" funzionante (WhatsApp/Share API)

### 14.5 Performance
- [ ] Caricamento iniziale &lt; 5 secondi su 4G
- [ ] Peso totale &lt; 3 MB
- [ ] 60 FPS sulle animazioni di match e cascata
- [ ] Nessun warning in console
- [ ] Nessuna dipendenza esterna a runtime tranne font Google

### 14.6 Codice
- [ ] HTML semantico (heading, button, img con alt)
- [ ] CSS organizzato con variabili e commenti per sezione
- [ ] JS commentato in italiano, funzioni nominate, nessun codice morto
- [ ] Nessun uso di `eval`, `innerHTML` con dati utente, o pattern insicuri
- [ ] Sintassi JS valida (verificare con `node --check`)

---

## 15. Anti-pattern (cose da NON fare)

L'LLM deve evitare attivamente questi errori comuni:

1. **Non usare framework.** Niente React, Vue, jQuery. Vanilla JS puro.
2. **Non usare gradient viola/teal.** L'estetica è rossa/dorata/cream, non SaaS.
3. **Non aggiungere riferimenti al film Disney.** Né nei testi, né visivamente.
4. **Non sovra-decorare.** Niente emoji a caso nei testi (es. non scrivere "Vinci 🏆 i biglietti 🎟️ adesso 🎁"). Le emoji sono nei simboli di gioco, non nei testi UI.
5. **Non mettere bottoni minuscoli.** Tutti i target tap minimi 44×44 px.
6. **Non bloccare lo scroll della pagina** a meno che non sia un overlay (uso `touch-action: none` solo sulla griglia).
7. **Non usare `alert()` o `prompt()`.** Solo overlay custom.
8. **Non chiedere permessi** (geolocation, notifications, ecc.) — il gioco non ne ha bisogno.
9. **Non hardcodare URL inglesi/internazionali.** I link di condivisione e premio sono italiani.
10. **Non rompere la coerenza narrativa** mescolando toni (es. "epico" al livello 1 e "divertente" al livello 5).

---

## 16. Deploy

### 16.1 Struttura cartella finale
```
sister-match/
├── index.html
├── public/
│   ├── audio_loop.mp3
│   ├── intro-photo.jpg
│   ├── footer-banner.jpg
│   ├── logo-match-mania.png
│   └── sister-act-logo-vert.png
└── README.md (opzionale, istruzioni deploy)
```

### 16.2 Deploy raccomandato
1. **Netlify Drop** (test): https://app.netlify.com/drop, drag&drop cartella
2. **Vercel** (produzione): `vercel` da terminale dentro la cartella

### 16.3 Dominio
Il committente ha già un account Vercel. URL finale atteso: `https://gioca.sisteract.it` (dominio custom, da configurare dopo il deploy).

---

## 17. Contatto e iterazioni

Se in fase di sviluppo emergono:
- **Ambiguità** in questo brief → sollevare la questione invece di indovinare
- **Migliorie tecniche** che si discostano dal brief → proporre prima di implementare
- **Limitazioni tecniche** (es. audio che non parte su un device specifico) → documentare nel README finale

Il committente preferisce **decisioni concrete con motivazione** rispetto a liste di opzioni vaghe.

---

## 18. Roadmap successiva (fuori scope di questo brief)

Dopo la consegna di questa versione, il committente pianificherà:
- Sostituzione delle emoji con asset illustrati dal grafico (in particolare 🙏 → velo da suora)
- Eventuale audio differenziato per livelli (es. tracks più intense ai livelli alti)
- Versione bilingue IT/EN per pubblico turistico
- Filtri Instagram/TikTok AR a tema
- Sticker pack WhatsApp ufficiali

Lo sviluppo di queste estensioni è fuori dal perimetro di questo documento.

---

**Fine documento. Buon lavoro.**

> Se l'LLM esecutore si accorge a metà sviluppo che il timing è troppo stretto per implementare tutto, **prioritizzare nell'ordine:** (1) Gameplay completo 10 livelli + boss, (2) Schermata premio finale, (3) Audio, (4) Persistenza localStorage, (5) Effetti sonori opzionali. Consegnare meno ma funzionante, non più ma rotto.
