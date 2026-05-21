# SISTER MATCH — Il gioco di Sister Act il Musical

Match-3 game web per la campagna marketing del musical **Sister Act** in scena al Teatro Nazionale di Milano da ottobre.

## 🎮 Gioca

**URL:** [sister-match.vercel.app](https://sister-match.vercel.app)

## 🎯 Cos'è

Un gioco match-3 giocabile in 90 secondi su mobile, con 10 livelli progressivi e un boss finale. Completando tutti i livelli si accede alla schermata premio con biglietti per l'anteprima dell'8 ottobre.

## 📦 Struttura progetto

```
sister_match/
├── index.html          # Gioco completo (vanilla JS, 40KB)
├── audio_loop.mp3      # "Fammi Volare" loop 96kbps mono (1.9MB)
├── sister-match-brief.md  # Brief di sviluppo originale
├── GRAFICHE/           # Mockup di design (non loghi trasparenti)
│   ├── SISTER ACT - GAME_1.jpg
│   └── SISTER ACT - GAME_2.jpg
├── MUSICHE/            # Tracce originali (non ottimizzate)
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

## 🎨 Asset forniti dal committente

| File | Stato | Note |
|------|-------|------|
| `audio_loop.mp3` | ✅ Incluso (versione ottimizzata 96kbps) | Originale in `MUSICHE/` |
| Logo orizzontale PNG | ⏳ Da fornire | Fallback CSS attivo |
| Logo verticale PNG | ⏳ Da fornire | Fallback CSS attivo |

## 🚀 Deploy

```bash
# Netlify Drop: trascina la cartella su app.netlify.com/drop
# Vercel CLI: vercel --prod
```

Dominio target: `gioca.sisteract.it`

## 📋 Funzionalità implementate

- [x] Griglia match-3 (7×8) con 6 simboli emoji
- [x] Swap via swipe + tap-select, gravità, combo
- [x] 10 livelli con obiettivi e bilanciamento progressivo
- [x] Boss livello 10 — Madre Superiora con barra pazienza
- [x] 9 cutscene tra i livelli (tono divertente)
- [x] HUD obiettivi, mosse, stelle (1/2/3)
- [x] Tutorial alla prima partita (pointer-events: none)
- [x] Schermata intro con logo fallback CSS
- [x] Schermata premio finale con coriandoli e CTA
- [x] Social share (navigator.share + fallback WhatsApp)
- [x] Audio loop + mute con persistenza localStorage
- [x] Effetti sonori Web Audio (match blip, win fanfara)
- [x] Persistenza progresso (resume dal livello salvato)
- [x] Meta tag Open Graph
- [x] Favicon emoji
- [x] Combo toast: Amen → Halleluja → Miracolo → Divino
- [x] Shuffle silenzioso se nessuna mossa disponibile

## ⏳ Da fare (Fase 2)

- [ ] Sostituire emoji con asset illustrati dal grafico (🙏 → velo da suora)
- [ ] Inserire loghi PNG trasparenti quando forniti
- [ ] URL riscatto biglietti definitivo (ora placeholder)
- [ ] Versione bilingue IT/EN
- [ ] Audio differenziato per livelli avanzati