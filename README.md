# SISTER MATCH — Il gioco di Sister Act il Musical

Match-3 game web per la campagna marketing del musical **Sister Act** in scena al Teatro Nazionale di Milano dall'8 ottobre.

## 🎮 Gioca

**URL:** [sister-match.vercel.app](https://sister-match.vercel.app)

## 🎯 Cos'è

Un gioco match-3 giocabile in 90 secondi su mobile, con 10 livelli progressivi e un boss finale. Completando tutti i livelli si accede alla schermata premio con form email per codice sconto esclusivo.

## 📦 Struttura progetto

```
SISTER_MATCH/
├── index.html                # Gioco completo (vanilla JS + CSS inline)
├── sister-act-logo-oriz.png  # Logo trasparente topbar (52px)
├── sister-match-brief.md     # Brief di sviluppo originale
├── assets/                   # Asset aggiuntivi (non serviti)
├── GRAFICHE/                 # Mockup di design + foto originali
│   ├── SISTER ACT - GAME_1.jpg
│   ├── SISTER ACT - GAME_2.jpg
│   ├── footer-banner.jpg
│   └── intro-vertical.jpg
├── MUSICHE/                  # Tracce originali (non ottimizzate)
│   ├── 01 - FAMMI VOLARE NIGHTCLUB  , sezione lenta a 118 2.mp3
│   └── 17 - SISTER ACT.mp3
└── README.md
```

## 🔧 Tecnologie

- **Vanilla JS** — nessun framework
- **Pointer Events API** — input unificato mouse + touch
- **Web Audio API** — effetti sonori sintetici (match, win)
- **Google Fonts** — Lobster, Bowlby One SC, Manrope
- **localStorage** — persistenza progresso, tutorial, mute

## 🎨 Asset grafici

- `intro-photo.jpg` — Foto verticale promozionale, schermata intro (`max-height:75vh`, `object-fit:cover`)
- `footer-banner.jpg` — Banner logo Sister Act, footer overlay gioco (solo schermata di gioco)
- `sister-act-logo-oriz.png` — Logo trasparente "Sister Act Match Mania", topbar gioco (52px)
- `audio_loop.mp3` — Loop musicale "Fammi Volare", 96kbps mono (~1.9MB)

## 🚀 Deploy

```bash
# Vercel CLI: vercel --prod
# Netlify Drop: trascina la cartella su app.netlify.com/drop
```

**Deploy live:** sister-match.vercel.app
**Repo:** github.com/robertafrezzato-svg/SISTER_MATCH
**Dominio target:** `gioca.sisteract.it`

## 📋 Funzionalità implementate

### Gioco
- [x] Griglia match-3 (7×8) con 6 simboli emoji
- [x] Swap via swipe + tap-select, gravità, combo
- [x] 10 livelli con obiettivi e bilanciamento progressivo
- [x] Boss livello 10 — Madre Superiora con barra pazienza
- [x] 9 cutscene tra i livelli (testi aggiornati dal committente)
- [x] HUD obiettivi, mosse, stelle (1/2/3)
- [x] Tutorial alla prima partita (pointer-events: none, auto-scompare 5 sec)
- [x] Combo toast: Amen → Halleluja → Miracolo → Divino
- [x] Shuffle silenzioso se nessuna mossa disponibile

### Livelli — Bilanciamento aggiornato

| # | Nome | Mosse | Obiettivi |
|---|------|-------|-----------|
| 1 | Coro alle Prime Armi | 25 | 🎤 ×12, 🙏 ×10 |
| 2 | La Prima Lezione | 24 | 🎤 ×14, 🕯️ ×10 |
| 3 | Hallelujah! | 22 | 🎤 ×15, 🕯️ ×12, 📿 ×8 |
| 4 | Le Voci si Alzano | 22 | 🙏 ×15, 📿 ×12, 💿 ×8 |
| 5 | Che Favola! | 20 | 💿 ×18, 💄 ×12, 🎤 ×10 |
| 6 | La Chiesa Si Riempie | 20 | 🕯️ ×16, 🙏 ×14, 📿 ×12 |
| 7 | La Stampa Ne Parla | 19 | 🎤 ×18, 💿 ×15, 💄 ×10 |
| 8 | Verso il Palcoscenico | 18 | 🎤 ×20, 🙏 ×16, 🕯️ ×14 |
| 9 | La Notte Prima del Debutto | **25** | tutti i 6 simboli ×10 ciascuno |
| 10 | **Madre Superiora** (BOSS) | 20 | 🎤 ×10, 🙏 ×12, 🕯️ ×8 |

### Cutscene (testi aggiornati)

- ATTO 2: "Suor Patrizia è entusiasta di te. Suor Lazzara è un osso duro. Continua a provare per convincerla!"
- ATTO 3: "Le preghiere si trasformano in canzoni. Il convento non ha mai sentito nulla del genere."
- ATTO 4: "Sento che stai prendendo ritmo! Anche la Madre Superiora ha smesso di tapparsi le orecchie. È un buon segno, fidati."
- ATTO 5: "Il tuo sound sta crescendo, non fermarti adesso!"
- ATTO 6: "La messa è sold out e il Monsignore balla. Tutto merito tuo. Ma il meglio deve ancora venire — continua!"
- ATTO 7: "Sei finita sul giornale insieme al coro. Ora non puoi più fermarti — tutti ti stanno guardando!"
- ATTO 8: "Il debutto si avvicina e abbiamo bisogno di te al massimo. Allenati ancora!"
- ATTO 9: "Domani è il grande giorno. Un'ultima prova e poi si va in scena."
- ATTO 10: "Questo è il momento della verità. La Madre Superiora non si convince facilmente. Ma tu sei arrivata fin qui. Adesso vai e conquistala!"

### Grafica
- [x] Schermata intro con foto promozionale verticale + simboli obiettivo + bottone (nessun logo/tagline)
- [x] Intro photo: `max-height:75vh` con `object-fit:cover` (visibile senza scroll su desktop)
- [x] Overlay (intro, cutscene, end, prize, tutorial) centrati a 480px su desktop (non a tutto schermo)
- [x] Footer overlay con banner Sister Act — visibile **solo nella schermata di gioco** (nascosto in intro, fade-in all'avvio)
- [x] Logo trasparente "Sister Act Match Mania" nella topbar (52px, PNG con trasparenza)
- [x] Effetto lampo dorato sullo sfondo ad ogni match (radial-gradient ellisse 80%×50%, posizione top 20%, durata 0.8s)
- [x] Layout desktop centrato: app + overlay a larghezza fissa 480px con sfondo nero e ombra laterale

### Audio
- [x] Audio loop + mute con persistenza localStorage
- [x] Effetti sonori Web Audio (match blip, win fanfara)

### Schermata premio (post-livello 10)
- [x] Overlay celebrativo con logo, coriandoli dorati CSS
- [x] Titolo "COMPLIMENTI!" + "HAI SUPERATO TUTTI I LIVELLI"
- [x] Testo: "Inserisci la tua mail per ricevere un codice sconto esclusivo! Ti aspettiamo a teatro per vedere Sister Act il Musical in scena dall'8 ottobre"
- [x] Form email con validazione
- [x] Checkbox privacy obbligatorio con link informativa
- [x] Checkbox marketing opzionale
- [x] Bottone "RICEVI IL CODICE SCONTO" → per ora log in console, Supabase in TODO
- [x] Messaggio conferma "✉️ Codice inviato! Controlla la tua email."
- [x] Bottone condivisione social (navigator.share + fallback WhatsApp)

### Messaggio sconfitta
- [x] "Sister, riprova! Noi crediamo in te!" (mosse esaurite o boss)

### Condivisione social
- [x] Messaggio aggiornato: "Ho fatto [N] punti a Sister Match! Riesci a battermi? Sister Act Il Musical in scena al Teatro Nazionale di Milano dall'8 ottobre #sisteractilmusical"
- [x] Meta tag Open Graph

### Persistenza
- [x] Progresso salvato (resume dal livello salvato)
- [x] Stato mute salvato
- [x] Tutorial visto salvato

### Altro
- [x] Favicon emoji 🎤
- [x] Griglia a pieno schermo con footer overlay fisso (solo in gioco)

## ⏳ Da fare (Fase 2)

- [ ] Salvataggio email premio su Supabase (tabella: email, marketing_opt_in, score, timestamp)
- [ ] Sostituire emoji con asset illustrati dal grafico (🙏 → velo da suora)
- [ ] Inserire loghi PNG trasparenti quando forniti
- [ ] Versione bilingue IT/EN
- [ ] Audio differenziato per livelli avanzati