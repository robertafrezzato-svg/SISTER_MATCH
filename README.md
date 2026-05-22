# SISTER MATCH — Il gioco di Sister Act il Musical

Match-3 game web per la campagna marketing del musical **Sister Act** in scena al Teatro Nazionale di Milano da ottobre.

## 🎮 Gioca

**URL:** [sister-match.vercel.app](https://sister-match.vercel.app)

## 🎯 Cos'è

Un gioco match-3 giocabile in 90 secondi su mobile, con 10 livelli progressivi e un boss finale. Completando tutti i livelli si accede alla schermata premio con biglietti per l'anteprima dell'8 ottobre.

## 📦 Struttura progetto

```
SISTER_MATCH/
├── index.html                # Gioco completo (vanilla JS)
├── intro-photo.jpg           # Foto promozionale verticale (intro screen)
├── footer-banner.jpg         # Banner logo Sister Act (footer gioco)
├── sister-act-logo-oriz.png  # Logo orizzontale (topbar)
├── audio_loop.mp3            # "Fammi Volare" loop 96kbps mono (1.9MB)
├── sister-match-brief.md     # Brief di sviluppo originale
├── assets/                   # Asset aggiuntivi
├── GRAFICHE/                 # Mockup di design + foto originali
│   ├── SISTER ACT - GAME_1.jpg
│   ├── SISTER ACT - GAME_2.jpg
│   ├── footer-banner.jpg     # Originale banner footer
│   └── intro-vertical.jpg   # Originale foto intro
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

| File | Stato | Uso |
|------|-------|-----|
| `intro-photo.jpg` | ✅ Incluso | Foto verticale promozionale, schermata intro |
| `footer-banner.jpg` | ✅ Incluso | Banner logo Sister Act, footer overlay gioco |
| `sister-act-logo-oriz.png` | ✅ Incluso | Logo orizzontale, topbar gioco |
| `audio_loop.mp3` | ✅ Incluso (versione ottimizzata 96kbps) | Loop musicale, originale in `MUSICHE/` |

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
- [x] 9 cutscene tra i livelli (tono divertente)
- [x] HUD obiettivi, mosse, stelle (1/2/3)
- [x] Tutorial alla prima partita (pointer-events: none, auto-scompare 5 sec)
- [x] Combo toast: Amen → Halleluja → Miracolo → Divino
- [x] Shuffle silenzioso se nessuna mossa disponibile

### Grafica
- [x] Schermata intro con foto promozionale verticale + simboli obiettivo + bottone
- [x] Footer overlay con banner Sister Act a larghezza piena
- [x] Logo orizzontale nella topbar (52px) con fallback CSS
- [x] Effetto lampo dorato sullo sfondo ad ogni match (radial-gradient, 0.8s)

### Audio
- [x] Audio loop + mute con persistenza localStorage
- [x] Effetti sonori Web Audio (match blip, win fanfara)

### Premio e condivisione
- [x] Schermata premio finale con coriandoli e CTA
- [x] Social share (navigator.share + fallback WhatsApp)
- [x] Meta tag Open Graph

### Persistenza
- [x] Progresso salvato (resume dal livello salvato)
- [x] Stato mute salvato
- [x] Tutorial visto salvato

### Altro
- [x] Favicon emoji 🎤
- [x] Griglia a pieno schermo con footer overlay fisso

## ⏳ Da fare (Fase 2)

- [ ] Sostituire emoji con asset illustrati dal grafico (🙏 → velo da suora)
- [ ] Inserire loghi PNG trasparenti quando forniti
- [ ] URL riscatto biglietti definitivo (ora placeholder `sisteractmilano.it/premio`)
- [ ] Versione bilingue IT/EN
- [ ] Audio differenziato per livelli avanzati