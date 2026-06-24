# SISTER MATCH — Progress.md

Ultimo aggiornamento: 24 giugno 2026

## ✅ Completato

### Fase 1 — Sviluppo gioco (21-22 maggio 2026)
- [x] Gioco match-3 completo single-file (`index.html`, vanilla JS)
- [x] Griglia 7×8, 6 simboli, swap swipe+tap, gravità, combo
- [x] 10 livelli progressivi con obiettivi
- [x] Boss livello 10 (Madre Superiora, barra pazienza)
- [x] 9 cutscene tra i livelli
- [x] HUD, stelle, tutorial, persistenza localStorage
- [x] Schermata premio con coriandoli + CTA
- [x] Audio loop + Web Audio SFX + mute
- [x] Social share (navigator.share + fallback WhatsApp)
- [x] Open Graph meta tag, favicon emoji
- [x] Layout desktop centrato 480px con ombre
- [x] Footer overlay visibile solo in gioco
- [x] Logo topbar trasparente

### Fase 2 — Asset grafici (26 maggio 2026)
- [x] Sostituita icona boss poliziotta → suora (`nun.jpg`)
- [x] Sostituita emoji 📿 → fiore mandala (`rosary.jpg`)
- [x] Fix logo prize overlay troppo grande
- [x] Fix apostrofo "dall'8 ottobre"

### Fase 3 — Sostituzione simboli griglia (24 giugno 2026)
- [x] Simbolo 2: emoji 🙏 → `church.jpg` → `star.png` (PNG trasparente)
- [x] Simbolo 4: emoji 📿 → `rosary.jpg` → `ball.jpg` → `nun2.png` (PNG trasparente)
- [x] Simbolo 6: emoji 💄 → `shoe.png` (PNG trasparente)
- [x] Tutte le immagini convertite da JPEG a PNG con sfondo trasparente (ImageMagick `-fuzz 15% -transparent white`)
- [x] Aggiornati livelli, cutscene, boss, HUD con i nuovi simboli

### Fase 4 — Integrazione Supabase (24 giugno 2026)
- [x] Aggiunto CDN Supabase JS v2 nell'`<head>`
- [x] Inizializzato client con URL + anon key
- [x] Sostituito TODO con chiamata reale a tabella `app_leads`
- [x] Salvataggio: app, email, privacy_accepted, marketing_opt_in, score
- [x] Gestione errori: email duplicata (23505), errore generico
- [x] Test browser: insert OK, RLS blocca SELECT anonimo, unique constraint funziona

## 📍 Stato attuale

- **Repo:** `/Users/roberta/projects/teatro/SISTER_MATCH`
- **Branch:** `main` — pulito, allineato con origin
- **Deploy:** sister-match.vercel.app (auto-deploy da GitHub push)
- **Supabase:** `https://csqzpitlohkaegqbbcvy.supabase.co` — tabella `app_leads` attiva

## ⏳ Da fare

- [ ] Sostituire emoji rimanenti (🎤, 🕯️, 💿) con asset illustrati
- [ ] Loghi PNG trasparenti definitivi
- [ ] Versione bilingue IT/EN
- [ ] Audio differenziato per livelli avanzati
- [ ] Dominio target: `gioca.sisteract.it`