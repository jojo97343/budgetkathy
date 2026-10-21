<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="theme-color" content="#F3F5EF">
<title>Budget Courses</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,700&family=Manrope:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<style>
  :root {
    --bg: #F3F5EF;
    --surface: #FFFFFF;
    --line: #EDF1EA;
    --soft: #E3E9E0;
    --ink: #1E2A22;
    --muted: #56635A;
    --accent: #2F6B4F;
    --accent-dark: #1E4A36;
    --lime: #C9E8B4;
    --warn: #8A3D10;
    --warn-bg: #FBEADF;
    --warn-stroke: #B4541B;
    --danger: #A12C1F;
    --danger-bg: #FBE3DF;
    --nav: #1E2A22;
    --nav-muted: #A9B8AD;
    --r-lg: 28px;
    --r-md: 22px;
    --r-sm: 16px;
    --serif: 'Fraunces', Georgia, serif;
    --sans: 'Manrope', system-ui, sans-serif;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  html, body { background: var(--bg); }
  body { font-family: var(--sans); color: var(--ink); min-height: 100dvh; padding-bottom: calc(110px + env(safe-area-inset-bottom)); }
  button { font-family: inherit; color: inherit; cursor: pointer; }
  input, select { font-family: inherit; }
  .sr { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }
  .wrap { max-width: 480px; margin: 0 auto; }

  /* HEADER */
  .top { display: flex; justify-content: space-between; align-items: center; padding: calc(22px + env(safe-area-inset-top)) 20px 6px; }
  .top-sub { font-size: 13px; color: var(--muted); }
  .top-title { font-family: var(--serif); font-size: 28px; font-weight: 700; line-height: 1.15; }
  .sync { display: inline-flex; align-items: center; gap: 6px; font-size: 12px; color: var(--muted); background: var(--surface); padding: 8px 12px; border-radius: 99px; }
  .sync-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--accent); animation: pulse 2s infinite; }
  .sync-dot.off { background: #9AA39C; animation: none; }
  @keyframes pulse { 0%,100% { opacity: 1; } 50% { opacity: .35; } }

  /* TABS */
  .tab { display: none; }
  .tab.active { display: block; animation: fade .2s ease; }
  @keyframes fade { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: none; } }

  .section { margin: 18px 16px 0; }
  .h2 { font-family: var(--serif); font-size: 19px; font-weight: 700; }
  .row-between { display: flex; justify-content: space-between; align-items: baseline; gap: 12px; }
  .link-btn { background: none; border: 0; color: var(--accent); font-weight: 700; font-size: 13px; padding: 8px 0; }

  /* HERO */
  .hero { margin: 8px 16px 0; background: var(--surface); border-radius: var(--r-lg); padding: 20px; display: flex; flex-direction: column; align-items: center; gap: 14px; }
  .ring { position: relative; width: 196px; height: 196px; }
  .ring svg { display: block; }
  .ring circle.prog { transition: stroke-dasharray .6s cubic-bezier(.4,0,.2,1), stroke .3s; }
  .ring-center { position: absolute; inset: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 2px; text-align: center; }
  .ring-small { font-size: 12px; color: var(--muted); }
  .ring-big { font-family: var(--serif); font-size: 40px; font-weight: 700; letter-spacing: -.02em; line-height: 1.1; }
  .ring-big.neg { color: var(--danger); }
  .mini-grid { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 10px; width: 100%; }
  .mini { background: var(--bg); border-radius: var(--r-sm); padding: 10px 12px; display: flex; flex-direction: column; gap: 2px; }
  .mini .k { font-size: 12px; color: var(--muted); }
  .mini .v { font-size: 17px; font-weight: 700; }
  .mini.warn { background: var(--warn-bg); }
  .mini.warn .k, .mini.warn .v { color: var(--warn); }
  .mini.ok { background: #E4F1DA; }
  .mini.ok .k, .mini.ok .v { color: var(--accent-dark); }
  .hero-empty { text-align: center; display: flex; flex-direction: column; gap: 10px; align-items: center; padding: 10px 0; }
  .hero-empty p { color: var(--muted); font-size: 14px; }

  /* QUICK ADD */
  .quick-grid { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 8px; margin-top: 10px; }
  .quick { height: 78px; border: 0; border-radius: 18px; background: var(--surface); display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 6px; font-weight: 600; font-size: 12px; padding: 0 4px; transition: transform .12s; }
  .quick:active { transform: scale(.96); }
  .quick span:last-child { max-width: 100%; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .avatar { border-radius: 50%; display: inline-flex; align-items: center; justify-content: center; font-weight: 800; color: var(--ink); flex-shrink: 0; }
  .av-32 { width: 32px; height: 32px; font-size: 13px; }
  .av-26 { width: 26px; height: 26px; font-size: 11px; }
  .tile { width: 40px; height: 40px; border-radius: 14px; display: flex; align-items: center; justify-content: center; font-weight: 800; font-size: 13px; flex-shrink: 0; }

  /* EXPENSE LIST */
  .exp-list { display: flex; flex-direction: column; margin-top: 4px; }
  .exp { display: flex; align-items: center; gap: 12px; padding: 8px 0; }
  .exp-info { flex: 1; min-width: 0; display: flex; flex-direction: column; }
  .exp-name { font-size: 15px; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .exp-meta { font-size: 12px; color: var(--muted); }
  .exp-amt { font-size: 15px; font-weight: 700; white-space: nowrap; }
  .icon-btn { width: 36px; height: 36px; border-radius: 12px; border: 0; background: transparent; color: var(--muted); display: inline-flex; align-items: center; justify-content: center; flex-shrink: 0; }
  .icon-btn:active { background: var(--soft); }
  .day-label { font-size: 12px; font-weight: 700; color: var(--muted); text-transform: uppercase; letter-spacing: .05em; margin-top: 10px; }

  /* EMPTY */
  .empty { text-align: center; padding: 26px 16px; color: var(--muted); font-size: 14px; background: var(--surface); border-radius: var(--r-md); margin-top: 10px; }

  /* LIST */
  .people { display: flex; align-items: center; }
  .people .avatar { width: 34px; height: 34px; font-size: 12px; margin-left: -8px; border: 2px solid var(--bg); }
  .invite { width: 40px; height: 40px; margin-left: -6px; border-radius: 50%; border: 2px dashed var(--accent); background: var(--surface); color: var(--accent); display: flex; align-items: center; justify-content: center; }
  .add-bar { margin: 12px 16px 0; height: 56px; background: var(--surface); border-radius: 28px; display: flex; align-items: center; padding: 0 6px 0 20px; gap: 8px; }
  .add-bar input { flex: 1; min-width: 0; border: 0; background: transparent; font-size: 15px; font-weight: 500; color: var(--ink); height: 44px; outline: none; }
  .pill-btn { height: 44px; padding: 0 18px; border-radius: 22px; border: 0; background: var(--accent); color: #fff; font-weight: 700; font-size: 14px; flex-shrink: 0; }
  .pill-btn:active { background: var(--accent-dark); }
  .who-bar { margin: 8px 16px 0; display: flex; align-items: center; gap: 8px; font-size: 13px; color: var(--muted); }
  .who-bar input { border: 0; border-bottom: 1.5px solid var(--soft); background: transparent; font-size: 14px; font-weight: 600; color: var(--ink); padding: 4px 2px; width: 140px; outline: none; }
  .who-bar input:focus { border-color: var(--accent); }
  .groups { margin: 18px 16px 0; display: flex; flex-direction: column; gap: 14px; }
  .group { background: var(--surface); border-radius: var(--r-md); padding: 12px 16px 6px; }
  .group-head { display: flex; justify-content: space-between; align-items: center; padding-bottom: 4px; }
  .group-title { font-family: var(--serif); font-size: 16px; font-weight: 700; }
  .group-count { font-size: 12px; color: var(--muted); }
  .item { display: flex; align-items: center; gap: 12px; min-height: 50px; border-top: 1px solid var(--line); }
  .item label { flex: 1; display: flex; align-items: center; gap: 12px; min-height: 50px; cursor: pointer; min-width: 0; }
  .item input[type=checkbox] { width: 22px; height: 22px; accent-color: var(--accent); flex-shrink: 0; }
  .item-name { flex: 1; font-size: 15px; font-weight: 500; overflow-wrap: anywhere; }
  .item.done .item-name { text-decoration: line-through; color: var(--muted); }
  .basket-bar { margin: 14px 16px 0; display: flex; justify-content: space-between; align-items: center; padding: 0 4px; font-size: 13px; color: var(--muted); gap: 8px; }
  .basket-actions { display: flex; gap: 14px; }
  .basket { margin: 6px 16px 0; }

  /* BILAN */
  .stats4 { margin: 8px 16px 0; display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 10px; }
  .stat { background: var(--surface); border-radius: var(--r-sm); padding: 14px; display: flex; flex-direction: column; gap: 4px; }
  .stat .k { font-size: 12px; color: var(--muted); }
  .stat .v { font-family: var(--serif); font-size: 24px; font-weight: 700; }
  .stat.neg .v { color: var(--danger); }
  .card { background: var(--surface); border-radius: var(--r-md); padding: 16px; margin-top: 10px; }
  .bar-row { display: grid; grid-template-columns: minmax(0, 1.1fr) minmax(0, 1.4fr) auto; gap: 12px; align-items: center; padding: 9px 0; border-top: 1px solid var(--line); font-size: 13px; }
  .bar-row:first-child { border-top: 0; }
  .bar-row .lbl { font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; display: flex; align-items: center; gap: 8px; }
  .bar-row .amt { font-weight: 700; white-space: nowrap; }
  .track { height: 8px; background: var(--soft); border-radius: 99px; overflow: hidden; }
  .fill { height: 100%; border-radius: 99px; background: var(--accent); }
  .fill.warn { background: #D58A3A; }
  .fill.over { background: var(--danger); }
  .dot { width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0; }
  .cmp-grid { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 10px; margin-bottom: 12px; }
  .cmp { background: var(--bg); border-radius: var(--r-sm); padding: 12px; }
  .cmp .k { font-size: 12px; color: var(--muted); }
  .cmp .v { font-size: 19px; font-weight: 800; margin-top: 2px; }
  .cmp .s { font-size: 12px; color: var(--muted); margin-top: 2px; }
  .up { color: var(--danger); } .down { color: var(--accent); }

  /* SETTINGS */
  .set-row { display: flex; align-items: center; justify-content: space-between; gap: 12px; padding: 12px 0; border-top: 1px solid var(--line); }
  .set-row:first-child { border-top: 0; padding-top: 0; }
  .set-label { font-size: 15px; font-weight: 600; }
  .set-sub { font-size: 12px; color: var(--muted); margin-top: 2px; }
  .field { height: 44px; border: 1.5px solid var(--soft); border-radius: 14px; background: var(--bg); padding: 0 12px; font-size: 15px; color: var(--ink); outline: none; width: 130px; -webkit-appearance: none; }
  .field:focus { border-color: var(--accent); }
  .btn-full { width: 100%; height: 52px; border-radius: 26px; border: 0; background: var(--accent); color: #fff; font-weight: 700; font-size: 15px; margin-top: 14px; }
  .btn-full:active { background: var(--accent-dark); }
  .btn-full.ghost { background: var(--bg); color: var(--ink); }
  .btn-full.danger { background: var(--danger-bg); color: var(--danger); }
  .code { font-family: var(--serif); font-size: 32px; font-weight: 700; letter-spacing: .14em; color: var(--accent); text-align: center; padding: 14px; background: var(--bg); border-radius: var(--r-sm); margin-top: 10px; }
  .p-muted { font-size: 13px; color: var(--muted); line-height: 1.5; }
  .arch { display: flex; justify-content: space-between; align-items: center; padding: 12px 0; border-top: 1px solid var(--line); }
  .arch:first-child { border-top: 0; padding-top: 0; }

  /* NAV */
  .nav-wrap { position: fixed; left: 0; right: 0; bottom: 0; padding: 0 16px calc(16px + env(safe-area-inset-bottom)); z-index: 100; pointer-events: none; }
  nav { pointer-events: auto; max-width: 448px; margin: 0 auto; height: 64px; background: var(--nav); border-radius: 32px; display: grid; grid-template-columns: repeat(5, minmax(0, 1fr)); align-items: center; box-shadow: 0 10px 30px rgba(30,42,34,.18); }
  nav.famille { grid-template-columns: 1fr; }
  .nav-btn { height: 64px; border: 0; background: none; color: var(--nav-muted); display: flex; align-items: center; justify-content: center; position: relative; }
  .nav-btn.active { color: #fff; }
  .nav-btn .badge { position: absolute; top: 12px; left: calc(50% + 6px); min-width: 18px; height: 18px; padding: 0 5px; border-radius: 9px; background: var(--lime); color: var(--ink); font-size: 11px; font-weight: 800; display: none; align-items: center; justify-content: center; }
  .nav-btn .badge.on { display: inline-flex; }
  .fab { width: 56px; height: 56px; border-radius: 28px; border: 0; background: var(--lime); color: var(--ink); display: flex; align-items: center; justify-content: center; margin: 0 auto; transition: transform .12s; }
  .fab:active { transform: scale(.94); }

  /* SHEET / MODAL */
  .overlay { position: fixed; inset: 0; background: rgba(20,28,23,.45); z-index: 200; display: none; align-items: flex-end; justify-content: center; }
  .overlay.open { display: flex; }
  .sheet { background: var(--surface); width: 100%; max-width: 480px; border-radius: 28px 28px 0 0; padding: 10px 20px calc(24px + env(safe-area-inset-bottom)); animation: up .28s cubic-bezier(.4,0,.2,1); max-height: 92dvh; overflow-y: auto; }
  @keyframes up { from { transform: translateY(100%); } to { transform: none; } }
  .grab { width: 40px; height: 5px; border-radius: 3px; background: var(--soft); margin: 0 auto 14px; }
  .sheet-title { font-family: var(--serif); font-size: 22px; font-weight: 700; margin-bottom: 14px; }
  .amount-input { width: 100%; border: 0; outline: none; background: transparent; text-align: center; font-family: var(--serif); font-size: 48px; font-weight: 700; color: var(--ink); padding: 6px 0; }
  .amount-input::placeholder { color: #B9C3BB; }
  .amount-wrap { background: var(--bg); border-radius: var(--r-md); padding: 10px 16px 14px; text-align: center; }
  .amount-wrap .lbl { font-size: 12px; color: var(--muted); }
  .text-field { width: 100%; height: 52px; border: 1.5px solid var(--soft); border-radius: var(--r-sm); background: var(--bg); padding: 0 16px; font-size: 16px; color: var(--ink); outline: none; margin-top: 12px; }
  .text-field:focus { border-color: var(--accent); }
  .chips { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 14px; }
  .chip { height: 38px; padding: 0 14px; border-radius: 19px; border: 1.5px solid var(--soft); background: var(--surface); font-size: 13px; font-weight: 600; display: inline-flex; align-items: center; gap: 8px; }
  .chip.on { background: var(--ink); border-color: var(--ink); color: #fff; }
  .sheet-actions { display: flex; gap: 10px; margin-top: 18px; }
  .sheet-actions button { flex: 1; height: 52px; border-radius: 26px; border: 0; font-weight: 700; font-size: 15px; }
  .btn-cancel { background: var(--bg); }
  .btn-ok { background: var(--accent); color: #fff; }
  .share-url { font-size: 13px; color: var(--muted); word-break: break-all; background: var(--bg); padding: 14px; border-radius: var(--r-sm); text-align: center; }

  /* BOUTON ROND */
  .round-btn { width: 44px; height: 44px; border-radius: 22px; border: 0; background: var(--surface); display: inline-flex; align-items: center; justify-content: center; flex-shrink: 0; }
  .round-btn:active { background: var(--soft); }

  /* MENUS */
  .week-nav { display: flex; gap: 8px; }
  .menu-note { margin: 10px 16px 0; background: var(--warn-bg); color: var(--warn); border-radius: var(--r-sm); padding: 12px 14px; font-size: 13px; line-height: 1.45; }
  .menu-actions { margin: 12px 16px 0; display: flex; align-items: center; justify-content: space-between; gap: 12px; }
  .days { margin: 14px 16px 0; display: flex; flex-direction: column; gap: 10px; }
  .day { background: var(--surface); border-radius: var(--r-md); padding: 12px 14px 4px; }
  .day.today { box-shadow: inset 0 0 0 2px var(--accent); }
  .day-head { display: flex; justify-content: space-between; align-items: baseline; padding-bottom: 4px; }
  .day-name { font-family: var(--serif); font-size: 17px; font-weight: 700; text-transform: capitalize; }
  .day-date { font-size: 12px; color: var(--muted); }
  .day-date.now { color: var(--accent); font-weight: 700; }
  .meal { width: 100%; display: flex; align-items: center; gap: 12px; min-height: 52px; border: 0; border-top: 1px solid var(--line); background: none; text-align: left; padding: 6px 0; }
  .meal:disabled { cursor: default; }
  .meal-slot { width: 42px; font-size: 11px; font-weight: 800; text-transform: uppercase; letter-spacing: .05em; color: var(--muted); flex-shrink: 0; }
  .meal-body { flex: 1; min-width: 0; display: flex; flex-direction: column; }
  .meal-dish { font-size: 15px; font-weight: 600; overflow-wrap: anywhere; }
  .meal-ingr { font-size: 12px; color: var(--muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .meal-empty { font-size: 14px; color: #8C978F; }
  .meal-chev { color: #8C978F; flex-shrink: 0; }
  .today-menu { margin: 12px 16px 0; width: calc(100% - 32px); border: 0; background: var(--surface); border-radius: var(--r-md); padding: 14px 16px; display: flex; align-items: center; gap: 12px; text-align: left; }
  .today-menu .k { font-size: 12px; color: var(--muted); }
  .today-menu .v { font-family: var(--serif); font-size: 17px; font-weight: 700; }
  .field-lbl { display: block; font-size: 13px; font-weight: 700; margin: 14px 0 6px; }
  .field-lbl span { font-weight: 500; color: var(--muted); }
  .text-area { width: 100%; border: 1.5px solid var(--soft); border-radius: var(--r-sm); background: var(--bg); padding: 12px 16px; font: 500 15px/1.5 var(--sans); color: var(--ink); outline: none; resize: vertical; }
  .text-area:focus { border-color: var(--accent); }
  .check-row { display: flex; align-items: center; gap: 10px; margin-top: 14px; font-size: 14px; font-weight: 600; min-height: 44px; }
  .check-row input { width: 22px; height: 22px; accent-color: var(--accent); }
  .dish-chips { display: flex; gap: 8px; overflow-x: auto; margin-top: 10px; padding-bottom: 2px; scrollbar-width: none; }
  .dish-chips::-webkit-scrollbar { display: none; }
  .dish-chips .chip { flex-shrink: 0; }
  .fam-seg { margin: calc(14px + env(safe-area-inset-top)) 16px 0; height: 48px; background: var(--surface); border-radius: 24px; padding: 4px; display: grid; grid-template-columns: 1fr 1fr; gap: 4px; }
  .fam-seg[hidden] { display: none; }
  .fam-seg button { border: 0; border-radius: 20px; background: transparent; font-weight: 700; font-size: 14px; color: var(--muted); }
  .fam-seg button.on { background: var(--ink); color: #fff; }
  body.famille .top { padding-top: 14px; }
  body.famille.on-menus .add-bar { display: none; }

  /* TIRAGE AU SORT */
  .suggest-cta { margin: 14px 16px 0; width: calc(100% - 32px); border: 0; background: var(--surface); border-radius: var(--r-md); padding: 14px 16px; display: flex; align-items: center; gap: 12px; text-align: left; }
  .suggest-cta[hidden] { display: none; }
  .suggest-cta:active { background: var(--soft); }
  .suggest-cta .v { font-family: var(--serif); font-size: 17px; font-weight: 700; }
  .suggest-cta .k { font-size: 12px; color: var(--muted); }
  .sugg-card { margin-top: 16px; background: var(--bg); border-radius: var(--r-md); padding: 22px 18px; min-height: 200px; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; gap: 10px; }
  .sugg-card.roll { animation: roll .32s cubic-bezier(.3,.7,.4,1); }
  @keyframes roll { 0% { opacity: 0; transform: translateY(10px) scale(.98); } 100% { opacity: 1; transform: none; } }
  .sugg-dish { font-family: var(--serif); font-size: 28px; font-weight: 700; line-height: 1.15; }
  .sugg-tags { display: flex; flex-wrap: wrap; justify-content: center; gap: 6px; }
  .sugg-tag { font-size: 12px; font-weight: 700; padding: 4px 10px; border-radius: 99px; background: var(--surface); color: var(--ink); }
  .sugg-tag.mine { background: var(--lime); }
  .sugg-ingr { font-size: 13px; color: var(--muted); line-height: 1.5; max-width: 320px; }

  /* RECETTE & FRIGO */
  .recipe-link { display: inline-flex; align-items: center; gap: 6px; font-size: 13px; font-weight: 700; color: var(--accent); text-decoration: none; min-height: 36px; padding: 0 2px; }
  .recipe-link[hidden] { display: none; }
  .meal-row { display: flex; align-items: center; gap: 6px; border-top: 1px solid var(--line); }
  .meal-row .meal { border-top: 0; flex: 1; min-width: 0; }
  .meal-row .recipe-link { flex-shrink: 0; font-size: 12px; padding: 0 4px; }
  .fridge-results { display: flex; flex-direction: column; gap: 8px; margin-top: 14px; }
  .fr-item { background: var(--bg); border-radius: var(--r-sm); padding: 12px 14px; display: flex; flex-direction: column; gap: 6px; }
  .fr-head { display: flex; justify-content: space-between; align-items: baseline; gap: 10px; }
  .fr-dish { font-family: var(--serif); font-size: 17px; font-weight: 700; }
  .fr-score { font-size: 12px; font-weight: 800; color: var(--accent); white-space: nowrap; }
  .fr-ingr { font-size: 13px; line-height: 1.5; color: var(--muted); }
  .fr-ingr b { color: var(--accent-dark); font-weight: 700; }
  .fr-actions { display: flex; align-items: center; justify-content: space-between; gap: 10px; }
  .fr-go { height: 38px; padding: 0 16px; border-radius: 19px; border: 0; background: var(--accent); color: #fff; font-weight: 700; font-size: 13px; }

  /* IDÉES DE DÎNERS */
  .ideas { margin: 14px 16px 0; }
  .ideas:empty { display: none; }
  .ideas-card { background: #FFF8EC; border: 1.5px solid #F1DFC0; border-radius: var(--r-md); padding: 14px 16px 6px; }
  .ideas-head { display: flex; justify-content: space-between; align-items: baseline; gap: 10px; }
  .ideas-title { font-family: var(--serif); font-size: 17px; font-weight: 700; }
  .ideas-sub { font-size: 12px; color: var(--muted); }
  .idea-form { display: flex; gap: 8px; margin-top: 10px; }
  .idea-form input { flex: 1; min-width: 0; height: 46px; border: 1.5px solid #F1DFC0; border-radius: 23px; background: var(--surface); padding: 0 16px; font: 500 15px var(--sans); color: var(--ink); outline: none; }
  .idea-form input:focus { border-color: var(--accent); }
  .idea { display: flex; align-items: center; gap: 10px; min-height: 52px; border-top: 1px solid #F1DFC0; padding: 4px 0; }
  .idea:first-of-type { margin-top: 8px; }
  .idea-body { flex: 1; min-width: 0; display: flex; flex-direction: column; }
  .idea-dish { font-size: 15px; font-weight: 600; overflow-wrap: anywhere; }
  .idea-by { font-size: 12px; color: var(--muted); }
  .idea-plan { height: 38px; padding: 0 14px; border-radius: 19px; border: 0; background: var(--accent); color: #fff; font-weight: 700; font-size: 13px; flex-shrink: 0; }
  .ideas-empty { font-size: 13px; color: var(--muted); padding: 10px 0 8px; }
  .nav-btn .dot-badge { position: absolute; top: 16px; left: calc(50% + 7px); width: 9px; height: 9px; border-radius: 50%; background: #F0B35A; display: none; }
  .nav-btn .dot-badge.on { display: block; }
  .pick-week { display: flex; align-items: center; justify-content: space-between; gap: 10px; margin-bottom: 10px; }
  .pick-week .round-btn { background: var(--bg); }
  .pick-days { display: flex; flex-direction: column; gap: 6px; }
  .pick-day { min-height: 52px; border: 1.5px solid var(--soft); border-radius: var(--r-sm); background: var(--surface); display: flex; align-items: center; gap: 12px; padding: 6px 14px; text-align: left; }
  .pick-day:active { background: var(--bg); }
  .pick-day .n { width: 86px; font-weight: 700; text-transform: capitalize; flex-shrink: 0; }
  .pick-day .w { flex: 1; font-size: 13px; color: var(--muted); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .pick-day .w.free { color: var(--accent); font-weight: 600; }

  /* GARDE-MANGER */
  .list-seg { margin: calc(14px + env(safe-area-inset-top)) 16px 0; height: 46px; background: var(--surface); border-radius: 23px; padding: 4px; display: grid; grid-template-columns: 1fr 1fr; gap: 4px; }
  .list-seg button { border: 0; border-radius: 19px; background: transparent; font-weight: 700; font-size: 14px; color: var(--muted); }
  .list-seg button.on { background: var(--ink); color: #fff; }
  body.famille .list-seg { display: none; }
  #tab-list .top, #tab-pantry .top { padding-top: 14px; }
  body.famille #tab-list .top { padding-top: 14px; }
  .zone-chips { margin-top: 14px; padding: 0 16px; display: flex; gap: 8px; overflow-x: auto; scrollbar-width: none; }
  .zone-chips::-webkit-scrollbar { display: none; }
  .zchip { height: 40px; padding: 0 14px; border-radius: 20px; border: 0; background: var(--surface); font-weight: 700; font-size: 13px; display: inline-flex; align-items: center; gap: 6px; white-space: nowrap; flex-shrink: 0; }
  .zchip .n { font-weight: 600; opacity: .65; }
  .zchip .dot { width: 9px; height: 9px; }
  .zchip.on { background: var(--ink); color: #fff; }
  .zchip.add { background: transparent; border: 2px dashed var(--accent); color: var(--accent); }
  .pantry-alert { margin: 14px 16px 0; background: var(--warn-bg); border-radius: 20px; padding: 12px 14px; display: flex; align-items: center; gap: 12px; }
  .pantry-alert .t { font-size: 13px; font-weight: 800; color: var(--warn); }
  .pantry-alert .s { font-size: 12px; color: var(--warn); }
  .pantry-alert button { height: 38px; padding: 0 14px; border-radius: 19px; border: 0; background: var(--warn); color: #fff; font-weight: 700; font-size: 12px; flex-shrink: 0; }
  .pantry-alert .done { font-size: 12px; font-weight: 800; color: var(--warn); flex-shrink: 0; }
  .pantry-add { margin-top: 12px !important; }
  .pantry-list { margin: 12px 16px 0; background: var(--surface); border-radius: var(--r-md); padding: 4px 14px; }
  .pantry-list:empty { display: none; }
  .pitem { display: flex; align-items: center; gap: 10px; min-height: 54px; border-top: 1px solid var(--line); }
  .pitem:first-child { border-top: 0; }
  .pname { flex: 1; min-width: 0; border: 0; background: none; text-align: left; display: flex; flex-direction: column; padding: 6px 0; }
  .pname .n { font-size: 15px; font-weight: 600; overflow-wrap: anywhere; }
  .pname .s { font-size: 11px; color: var(--muted); }
  .pitem.out .pname .n { color: var(--muted); }
  .ppill { height: 36px; min-width: 100px; padding: 0 12px; border-radius: 18px; border: 0; background: #E4F1DA; color: var(--accent-dark); font-weight: 700; font-size: 12px; display: inline-flex; align-items: center; justify-content: center; gap: 6px; flex-shrink: 0; }
  .ppill::before { content: ''; width: 7px; height: 7px; border-radius: 50%; background: var(--accent); }
  .pitem.out .ppill { background: var(--warn-bg); color: var(--warn); }
  .pitem.out .ppill::before { background: var(--warn-stroke); }
  .pantry-empty { text-align: center; color: var(--muted); font-size: 14px; padding: 22px 6px; }
  .zrow { display: flex; align-items: center; gap: 8px; min-height: 56px; border-bottom: 1px solid var(--line); }
  .zrow .ztile { width: 36px; height: 36px; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: 800; flex-shrink: 0; }
  .zrow .zinfo { flex: 1; min-width: 0; display: flex; flex-direction: column; }
  .zrow .zinfo b { font-size: 15px; overflow-wrap: anywhere; }
  .zrow .zinfo span { font-size: 12px; color: var(--muted); }
  .zrow input { flex: 1; min-width: 0; height: 40px; border: 1.5px solid var(--accent); border-radius: 12px; background: var(--bg); padding: 0 10px; font: 600 15px var(--sans); color: var(--ink); outline: none; }
  .zrow .icon-btn:disabled { opacity: .25; }
  .zrow .icon-btn.del { color: var(--danger); }
  .zconfirm { background: var(--danger-bg); border-radius: 16px; padding: 12px 14px; margin: 8px 0; display: flex; flex-direction: column; gap: 10px; font-size: 14px; color: #7A2117; }
  .zconfirm .acts { display: flex; gap: 8px; }
  .zconfirm button { flex: 1; height: 42px; border-radius: 21px; border: 0; font-weight: 700; font-size: 13px; background: #fff; }
  .zconfirm button.yes { background: var(--danger); color: #fff; }
  .color-dots { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 12px; }
  .color-dots button { width: 36px; height: 36px; border-radius: 18px; border: 1px solid rgba(0,0,0,.08); }
  .color-dots button[aria-checked="true"] { box-shadow: 0 0 0 3px var(--surface), 0 0 0 5px var(--ink); }

  /* MODE FAMILLE */
  body.famille .nav-wrap { display: none; }
  body.famille { padding-bottom: calc(100px + env(safe-area-inset-bottom)); }
  body.famille .add-bar { position: fixed; left: 16px; right: 16px; bottom: calc(16px + env(safe-area-inset-bottom)); margin: 0 auto; max-width: 448px; z-index: 100; box-shadow: 0 10px 30px rgba(30,42,34,.16); }
  .me-chip { height: 40px; padding: 0 6px 0 14px; border-radius: 20px; border: 0; background: var(--surface); display: inline-flex; align-items: center; gap: 8px; font-size: 13px; font-weight: 700; flex-shrink: 0; }
  .welcome { margin: 10px 16px 0; background: #E4F1DA; color: var(--accent-dark); border-radius: var(--r-md); padding: 14px 16px; font-size: 14px; line-height: 1.5; display: flex; gap: 12px; align-items: flex-start; }
  .welcome svg { flex-shrink: 0; margin-top: 2px; }

  /* TOAST */
  .toast { position: fixed; left: 50%; bottom: calc(96px + env(safe-area-inset-bottom)); transform: translate(-50%, 16px); background: var(--ink); color: #fff; padding: 10px 18px; border-radius: 20px; font-size: 13px; font-weight: 600; z-index: 300; opacity: 0; transition: all .25s; pointer-events: none; white-space: nowrap; }
  .toast.show { opacity: 1; transform: translate(-50%, 0); }

  /* LOADING */
  .loading { position: fixed; inset: 0; background: var(--bg); z-index: 400; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 14px; }
  .spinner { width: 36px; height: 36px; border: 3px solid var(--soft); border-top-color: var(--accent); border-radius: 50%; animation: spin .8s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .loading-title { font-family: var(--serif); font-size: 22px; font-weight: 700; }
</style>
</head>
<body>

<div class="loading" id="loading">
  <div class="loading-title">Budget Courses</div>
  <div class="spinner"></div>
</div>

<div class="wrap">

<!-- Onglets du mode famille -->
<div class="fam-seg" id="famSeg" role="tablist" hidden>
  <button type="button" role="tab" data-tab="list" class="on">Liste</button>
  <button type="button" role="tab" data-tab="menus">Dîners</button>
</div>

<!-- ============ ACCUEIL ============ -->
<section class="tab active" id="tab-home">
  <header class="top">
    <div>
      <div class="top-sub" id="hello">Bonjour</div>
      <h1 class="top-title" id="monthTitle">—</h1>
    </div>
    <div style="display:flex;align-items:center;gap:8px">
      <span class="sync"><span class="sync-dot" id="syncDot"></span><span id="syncTxt">en ligne</span></span>
      <button class="round-btn" type="button" data-goto="settings" aria-label="Réglages">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M4 7h10M18 7h2M4 17h4M12 17h8"/><circle cx="16" cy="7" r="2"/><circle cx="10" cy="17" r="2"/></svg>
      </button>
    </div>
  </header>

  <div class="hero" id="hero"></div>

  <button class="today-menu" type="button" data-goto="menus" id="todayMenu" hidden></button>

  <div class="section">
    <div class="h2">Ajout rapide</div>
    <div class="quick-grid" id="quickGrid"></div>
  </div>

  <div class="section">
    <div class="row-between">
      <span class="h2">Récemment</span>
      <button class="link-btn" id="toggleAllBtn" type="button">Tout voir</button>
    </div>
    <div class="exp-list" id="recentList"></div>
  </div>
</section>

<!-- ============ LISTE ============ -->
<section class="tab" id="tab-list">
  <div class="list-seg" role="tablist" aria-label="Courses ou garde-manger">
    <button type="button" role="tab" data-tab="list">Courses</button>
    <button type="button" role="tab" data-tab="pantry">Garde-manger</button>
  </div>
  <header class="top">
    <div>
      <div class="top-sub" id="listSub">—</div>
      <h1 class="top-title" id="listTitle">La liste</h1>
    </div>
    <div class="people" id="people"></div>
  </header>

  <form class="add-bar" id="addItemForm" autocomplete="off">
    <label class="sr" for="itemName">Ajouter un article</label>
    <input id="itemName" type="text" placeholder="Il manque quelque chose ?" enterkeyhint="done">
    <button class="pill-btn" type="submit">Ajouter</button>
  </form>
  <div class="welcome" id="welcome" hidden>
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><circle cx="12" cy="12" r="9"/><path d="M12 11v5M12 8h.01"/></svg>
    <span id="welcomeTxt"></span>
  </div>

  <div class="groups" id="groups"></div>

  <div class="basket-bar" id="basketBar" hidden>
    <span id="basketTxt"></span>
    <span class="basket-actions">
      <button class="link-btn" type="button" id="basketToggle">Afficher</button>
      <button class="link-btn" type="button" id="basketClear" style="color:var(--danger)">Vider</button>
    </span>
  </div>
  <div class="basket" id="basket" hidden></div>
</section>

<!-- ============ BILAN ============ -->
<!-- ============ GARDE-MANGER ============ -->
<section class="tab" id="tab-pantry">
  <div class="list-seg" role="tablist" aria-label="Courses ou garde-manger">
    <button type="button" role="tab" data-tab="list">Courses</button>
    <button type="button" role="tab" data-tab="pantry">Garde-manger</button>
  </div>
  <header class="top">
    <div>
      <div class="top-sub" id="pantrySub">—</div>
      <h1 class="top-title">Chez nous</h1>
    </div>
    <button class="me-chip" type="button" id="zonesBtn" style="padding:0 14px">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="M4 7h10M18 7h2M4 17h4M12 17h8"/><circle cx="16" cy="7" r="2"/><circle cx="10" cy="17" r="2"/></svg>Endroits</button>
  </header>
  <div class="zone-chips" id="zoneChips"></div>
  <div id="pantryAlert"></div>
  <form class="add-bar pantry-add" id="pantryAdd" autocomplete="off">
    <label class="sr" for="pantryName">Ajouter un article</label>
    <input id="pantryName" type="text" placeholder="Ajouter…" enterkeyhint="done">
    <button class="pill-btn" type="submit" aria-label="Ajouter">Ajouter</button>
  </form>
  <div class="pantry-list" id="pantryList"></div>
</section>

<!-- ============ MENUS ============ -->
<section class="tab" id="tab-menus">
  <header class="top">
    <div>
      <div class="top-sub" id="weekLabel">—</div>
      <h1 class="top-title">Dîners</h1>
    </div>
    <div class="week-nav">
      <button class="round-btn" type="button" id="weekPrev" aria-label="Semaine précédente">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m15 6-6 6 6 6"/></svg></button>
      <button class="round-btn" type="button" id="weekNext" aria-label="Semaine suivante">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m9 6 6 6-6 6"/></svg></button>
    </div>
  </header>
  <div class="menu-note" id="menuNote" hidden></div>
  <div class="menu-actions" id="menuActions">
    <button class="pill-btn" type="button" id="weekToList">Tout ajouter à la liste</button>
    <button class="link-btn" type="button" id="weekToday">Cette semaine</button>
  </div>
  <button class="suggest-cta" type="button" id="suggestBtn">
    <span class="tile" style="background:#F6E6C8">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="3" y="3" width="18" height="18" rx="4"/><circle cx="8.5" cy="8.5" r="1.2" fill="currentColor"/><circle cx="15.5" cy="15.5" r="1.2" fill="currentColor"/><circle cx="15.5" cy="8.5" r="1.2" fill="currentColor"/><circle cx="8.5" cy="15.5" r="1.2" fill="currentColor"/></svg></span>
    <span style="flex:1;display:flex;flex-direction:column">
      <span class="v">Je sais pas quoi faire ce soir</span>
      <span class="k" id="suggestSub">Un plat au hasard parmi plus de 150</span></span>
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#8C978F" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m9 6 6 6-6 6"/></svg>
  </button>
  <button class="suggest-cta" type="button" id="fridgeBtn" style="margin-top:10px">
    <span class="tile" style="background:#D4ECD9">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="5" y="2" width="14" height="20" rx="2"/><path d="M5 10h14M9 6v1M9 13v3"/></svg></span>
    <span style="flex:1;display:flex;flex-direction:column">
      <span class="v">Avec ce qu'il me reste</span>
      <span class="k">Des idées à partir de tes ingrédients</span></span>
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#8C978F" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m9 6 6 6-6 6"/></svg>
  </button>
  <div class="ideas" id="ideas"></div>
  <div class="days" id="days"></div>
</section>

<section class="tab" id="tab-bilan">
  <header class="top">
    <div>
      <div class="top-sub">Ce mois-ci</div>
      <h1 class="top-title">Bilan</h1>
    </div>
  </header>
  <div class="stats4">
    <div class="stat"><span class="k">Dépensé</span><span class="v" id="b-spent">0 €</span></div>
    <div class="stat" id="b-reste-card"><span class="k">Restant</span><span class="v" id="b-reste">—</span></div>
    <div class="stat"><span class="k">Achats</span><span class="v" id="b-count">0</span></div>
    <div class="stat"><span class="k">Panier moyen</span><span class="v" id="b-avg">—</span></div>
  </div>
  <div class="section">
    <div class="h2">Par semaine</div>
    <div class="card" id="weekly"></div>
  </div>
  <div class="section">
    <div class="h2">Par catégorie</div>
    <div class="card" id="bycat"></div>
  </div>
  <div class="section">
    <div class="h2">Mois par mois</div>
    <div class="card">
      <div id="compare"></div>
      <canvas id="chart" height="190" style="width:100%;display:none"></canvas>
    </div>
  </div>
</section>

<!-- ============ RÉGLAGES ============ -->
<section class="tab" id="tab-settings">
  <header class="top">
    <div>
      <div class="top-sub">Ton app</div>
      <h1 class="top-title">Réglages</h1>
    </div>
  </header>
  <div class="section" style="margin-top:8px">
    <div class="card" style="margin-top:0">
      <div class="set-row">
        <div><label class="set-label" for="setBudget">Budget mensuel</label><div class="set-sub">Objectif courses par mois</div></div>
        <input class="field" id="setBudget" type="number" inputmode="decimal" min="0" placeholder="400">
      </div>
      <div class="set-row">
        <div><label class="set-label" for="setName">Ton prénom</label><div class="set-sub">Affiché sur la liste partagée</div></div>
        <input class="field" id="setName" type="text" placeholder="Maman">
      </div>
      <button class="btn-full" type="button" id="saveSettings">Enregistrer</button>
    </div>
  </div>
  <div class="section">
    <div class="h2">Code de la liste</div>
    <div class="card">
      <p class="p-muted">Ce code identifie ta liste. Partage le lien pour que la famille puisse ajouter des articles.</p>
      <div class="code" id="myCode">—</div>
      <button class="btn-full ghost" type="button" id="shareBtn2">Partager le lien</button>
    </div>
  </div>
  <div class="section">
    <div class="h2">Nouveau mois</div>
    <div class="card">
      <p class="p-muted">Le mois change tout seul le 1er. Tu peux aussi archiver maintenant : les dépenses partent dans l'historique et la liste est vidée.</p>
      <button class="btn-full danger" type="button" id="resetBtn">Démarrer un nouveau mois</button>
    </div>
  </div>
  <div class="section">
    <div class="h2">Historique</div>
    <div class="card" id="archives"></div>
  </div>
</section>

</div>

<!-- NAV -->
<div class="nav-wrap">
  <nav id="nav" aria-label="Navigation">
    <button class="nav-btn active" data-tab="home" aria-label="Accueil" type="button">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linejoin="round" aria-hidden="true"><path d="M3 11 12 4l9 7v9H3z"/></svg>
    </button>
    <button class="nav-btn" data-tab="list" aria-label="Liste de courses" type="button">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M9 6h11M9 12h11M9 18h11M4 6h.01M4 12h.01M4 18h.01"/></svg>
      <span class="badge" id="listBadge"></span>
    </button>
    <div class="nav-fab"><button class="fab" id="fab" aria-label="Ajouter une dépense" type="button">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" aria-hidden="true"><path d="M12 5v14M5 12h14"/></svg>
    </button></div>
    <button class="nav-btn" data-tab="menus" aria-label="Menus de la semaine" type="button">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M7 3v8M5 3v4a2 2 0 0 0 4 0V3M7 11v10M17 21V3c-2 1.5-3 4-3 7v3h3"/></svg>
      <span class="dot-badge" id="ideasBadge"></span>
    </button>
    <button class="nav-btn" data-tab="bilan" aria-label="Bilan" type="button">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M5 20V11M12 20V5M19 20v-6"/></svg>
    </button>
  </nav>
</div>

<!-- SHEET : AJOUT DÉPENSE -->
<div class="overlay" id="expenseSheet">
  <form class="sheet" id="expenseForm" autocomplete="off">
    <div class="grab"></div>
    <div class="sheet-title">Nouvelle dépense</div>
    <div class="amount-wrap">
      <label class="lbl" for="expAmount">Montant</label>
      <input class="amount-input" id="expAmount" type="text" inputmode="decimal" placeholder="0,00 €">
    </div>
    <label class="sr" for="expName">Magasin</label>
    <input class="text-field" id="expName" type="text" placeholder="Magasin (Lidl, Carrefour…)" list="storeList">
    <datalist id="storeList"></datalist>
    <div class="chips" id="catChips" role="radiogroup" aria-label="Catégorie"></div>
    <div class="sheet-actions">
      <button class="btn-cancel" type="button" data-close>Annuler</button>
      <button class="btn-ok" type="submit">Ajouter</button>
    </div>
  </form>
</div>

<!-- SHEET : PARTAGE -->
<div class="overlay" id="shareSheet">
  <div class="sheet">
    <div class="grab"></div>
    <div class="sheet-title">Partager la liste</div>
    <p class="p-muted" style="margin-bottom:12px">Envoie ce lien à la famille : ils verront la liste et pourront ajouter des articles en temps réel.</p>
    <div class="share-url" id="shareUrl">—</div>
    <div class="sheet-actions">
      <button class="btn-cancel" type="button" data-close>Fermer</button>
      <button class="btn-ok" type="button" id="copyShare">Copier le lien</button>
    </div>
  </div>
</div>

<!-- SHEET : NOUVEAU MOIS -->
<div class="overlay" id="resetSheet">
  <div class="sheet">
    <div class="grab"></div>
    <div class="sheet-title">Nouveau mois ?</div>
    <p class="p-muted">Les dépenses seront archivées dans l'historique et la liste de courses sera vidée.</p>
    <div class="sheet-actions">
      <button class="btn-cancel" type="button" data-close>Annuler</button>
      <button class="btn-ok" type="button" id="confirmReset">Confirmer</button>
    </div>
  </div>
</div>

<!-- SHEET : REPAS -->
<div class="overlay" id="mealSheet">
  <form class="sheet" id="mealForm" autocomplete="off">
    <div class="grab"></div>
    <div class="sheet-title" id="mealTitle">Repas</div>
    <label class="sr" for="mealDish">Plat</label>
    <input class="text-field" id="mealDish" type="text" placeholder="Plat (ex : lasagnes)" list="dishList" style="margin-top:0">
    <datalist id="dishList"></datalist>
    <a class="recipe-link" id="mealRecipe" href="#" target="_blank" rel="noopener" hidden><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 19.5V5a2 2 0 0 1 2-2h13v16H6a2 2 0 0 0-2 2.5zM19 19v3H6"/></svg>Voir une recette</a>
    <div class="dish-chips" id="dishChips"></div>
    <label class="field-lbl" for="mealIngr">Ingrédients <span>un par ligne</span></label>
    <textarea class="text-area" id="mealIngr" rows="5" placeholder="Pâtes à lasagnes&#10;Viande hachée&#10;Sauce tomate"></textarea>
    <label class="check-row"><input type="checkbox" id="mealToList" checked> Ajouter les ingrédients à la liste de courses</label>
    <div class="sheet-actions">
      <button class="btn-cancel" type="button" id="mealDelete" hidden style="color:var(--danger)">Effacer</button>
      <button class="btn-cancel" type="button" data-close>Annuler</button>
      <button class="btn-ok" type="submit">Enregistrer</button>
    </div>
  </form>
</div>

<!-- SHEET : TIRAGE AU SORT -->
<div class="overlay" id="suggestSheet">
  <div class="sheet">
    <div class="grab"></div>
    <div class="sheet-title">Ce soir, on mange…</div>
    <div class="chips" id="suggestFilters" style="margin-top:0" aria-label="Filtres"></div>
    <div class="sugg-card" id="suggestCard" aria-live="polite"></div>
    <div class="sheet-actions">
      <button class="btn-cancel" type="button" id="suggestAgain">Un autre</button>
      <button class="btn-ok" type="button" id="suggestGo">On fait ça</button>
    </div>
    <button class="link-btn" type="button" id="suggestKeep" style="width:100%;margin-top:10px;padding:10px">Garder en idée pour plus tard</button>
  </div>
</div>

<!-- SHEET : AVEC CE QU'IL ME RESTE -->
<div class="overlay" id="fridgeSheet">
  <div class="sheet">
    <div class="grab"></div>
    <div class="sheet-title">Qu'est-ce que je fais avec…</div>
    <label class="sr" for="fridgeInput">Tes ingrédients</label>
    <input class="text-field" id="fridgeInput" type="text" placeholder="courgettes, œufs, fromage" autocomplete="off" enterkeyhint="search" style="margin-top:0">
    <p class="p-muted" style="font-size:12px;margin-top:6px">Sépare les ingrédients par une virgule. Le sel, l'huile, le beurre… sont comptés comme déjà là.</p>
    <div class="fridge-results" id="fridgeResults"></div>
    <div class="sheet-actions"><button class="btn-cancel" type="button" data-close>Fermer</button></div>
  </div>
</div>

<!-- SHEET : MES ENDROITS -->
<div class="overlay" id="zonesSheet">
  <div class="sheet">
    <div class="grab"></div>
    <div class="row-between"><span class="sheet-title" style="margin:0">Mes endroits</span><span class="p-muted" style="font-size:12px">Flèches pour réordonner</span></div>
    <div id="zonesList" style="margin-top:8px"></div>
    <form id="newZoneForm" autocomplete="off" style="margin-top:18px">
      <label class="field-lbl" for="newZoneName" style="margin-top:0">Nouvel endroit</label>
      <input class="text-field" id="newZoneName" type="text" placeholder="Ex : Cave, Cellier, Salle de bain" maxlength="30" style="margin-top:0">
      <div class="color-dots" id="newZoneColors" role="radiogroup" aria-label="Couleur"></div>
      <button class="btn-full" type="submit">Créer l'endroit</button>
    </form>
    <div class="sheet-actions"><button class="btn-cancel" type="button" data-close>Fermer</button></div>
  </div>
</div>

<!-- SHEET : ARTICLE DU GARDE-MANGER -->
<div class="overlay" id="pitemSheet">
  <form class="sheet" id="pitemForm" autocomplete="off">
    <div class="grab"></div>
    <div class="sheet-title">Modifier l'article</div>
    <label class="sr" for="pitemName">Nom</label>
    <input class="text-field" id="pitemName" type="text" style="margin-top:0">
    <div class="field-lbl">Où ?</div>
    <div class="chips" id="pitemZones" style="margin-top:0" role="radiogroup" aria-label="Endroit"></div>
    <div class="sheet-actions">
      <button class="btn-cancel" type="button" id="pitemDelete" style="color:var(--danger)">Supprimer</button>
      <button class="btn-ok" type="submit">Enregistrer</button>
    </div>
  </form>
</div>

<!-- SHEET : CHOISIR UN JOUR POUR UNE IDÉE -->
<div class="overlay" id="pickSheet">
  <div class="sheet">
    <div class="grab"></div>
    <div class="sheet-title" id="pickTitle">Quel soir ?</div>
    <div class="pick-week">
      <button class="round-btn" type="button" id="pickPrev" aria-label="Semaine précédente">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m15 6-6 6 6 6"/></svg></button>
      <span id="pickWeekLbl" class="p-muted"></span>
      <button class="round-btn" type="button" id="pickNext" aria-label="Semaine suivante">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m9 6 6 6-6 6"/></svg></button>
    </div>
    <div class="pick-days" id="pickDays"></div>
    <div class="sheet-actions"><button class="btn-cancel" type="button" data-close>Annuler</button></div>
  </div>
</div>

<!-- SHEET : PRÉNOM (mode famille) -->
<div class="overlay" id="nameSheet">
  <form class="sheet" id="nameForm" autocomplete="off">
    <div class="grab"></div>
    <div class="sheet-title" id="nameTitle">Bienvenue !</div>
    <p class="p-muted">Indique ton prénom : il s'affichera à côté des articles que tu ajoutes, pour que tout le monde sache qui a demandé quoi.</p>
    <label class="sr" for="familyName">Ton prénom</label>
    <input class="text-field" id="familyName" type="text" placeholder="Ton prénom" enterkeyhint="done">
    <div class="sheet-actions">
      <button class="btn-cancel" type="button" data-close>Plus tard</button>
      <button class="btn-ok" type="submit">C'est moi</button>
    </div>
  </form>
</div>

<div class="toast" id="toast" role="status" aria-live="polite"></div>

<script>
// ============================================================
// SUPABASE
// ============================================================
const SUPABASE_URL = 'https://qridhnhidcrfffzejzgt.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InFyaWRobmhpZGNyZmZmemVqemd0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzY2ODk0MzIsImV4cCI6MjA5MjI2NTQzMn0.aRZyOhnFNn-1uUY5fArvqVlmEoGgvLXNTAsJ2zlM1GM';
const sb = supabase.createClient(SUPABASE_URL, SUPABASE_KEY);
const SHARE_BASE = 'https://jojo97343.github.io/budgetcourse/';

// ============================================================
// CONSTANTES
// ============================================================
const MONTHS = ['Janvier','Février','Mars','Avril','Mai','Juin','Juillet','Août','Septembre','Octobre','Novembre','Décembre'];
const MONTHS_S = ['Jan','Fév','Mar','Avr','Mai','Juin','Juil','Août','Sep','Oct','Nov','Déc'];
const CATS = [
  { id: 'Courses', tint: '#DCEFCF' },
  { id: 'Viande/Poisson', tint: '#F5D9D2' },
  { id: 'Légumes/Fruits', tint: '#D4ECD9' },
  { id: 'Produits laitiers', tint: '#DCE6F5' },
  { id: 'Boulangerie', tint: '#F6E6C8' },
  { id: 'Hygiène/Entretien', tint: '#E8DDF2' },
  { id: 'Animaux', tint: '#EFE3D3' },
  { id: 'Boissons', tint: '#D3ECEB' },
  { id: 'Épicerie', tint: '#F2E9C9' },
  { id: 'Autre', tint: '#E3E9E0' }
];
const CAT_COLORS = ['#2F6B4F','#4F7FB8','#C9853A','#8E6BB0','#3E9A97','#B5584B','#7C8B3A','#9A7A55','#5C6B62','#C26F99'];
const PEOPLE_TINTS = ['#DCEFCF','#DCE6F5','#F6E6C8','#E8DDF2','#F5D9D2','#D3ECEB','#F2E9C9'];

// Rayons de la liste (déduits du nom de l'article)
const RAYONS = [
  { name: 'Fruits & légumes', words: ['banane','pomme','poire','orange','citron','salade','tomate','carotte','oignon','ail','patate','courgette','aubergine','poivron','concombre','avocat','fraise','raisin','kiwi','legume','fruit','champignon','epinard','brocoli','haricot','persil','herbe'] },
  { name: 'Frais', words: ['lait','yaourt','yogourt','fromage','beurre','creme','oeuf','jambon','poulet','viande','poisson','steak','saumon','dinde','lardon','saucisse','emmental','mozzarella','skyr','compote','dessert'] },
  { name: 'Boulangerie', words: ['pain','baguette','brioche','croissant','viennoiserie','biscotte','pain de mie'] },
  { name: 'Épicerie', words: ['pate','pates','riz','farine','sucre','huile','sel','poivre','cafe','the','conserve','cereale','biscuit','chocolat','sauce','ketchup','mayonnaise','moutarde','confiture','miel','semoule','lentille','thon','soupe','epice','vinaigre','gateau','chips'] },
  { name: 'Boissons', words: ['eau','jus','soda','biere','vin','coca','sirop','limonade','lait vegetal'] },
  { name: 'Surgelés', words: ['glace','surgele','frites','pizza','nugget','poelee'] },
  { name: 'Maison', words: ['lessive','liquide vaisselle','vaisselle','eponge','sopalin','essuie','papier toilette','papier','sac','poubelle','javel','nettoyant','pastille','adoucissant','ampoule','pile'] },
  { name: 'Hygiène', words: ['shampoing','shampooing','savon','dentifrice','gel douche','deodorant','coton','couche','mouchoir','brosse','rasoir','creme hydratante'] },
  { name: 'Animaux', words: ['croquette','litiere','patee','chat','chien'] }
];

// ============================================================
// ÉTAT
// ============================================================
let local = { budget: 0, userName: 'Maman', listCode: '', expenses: [], archives: [], lastMonth: '', familyName: '' };

const urlParams = new URLSearchParams(window.location.search);
const FAMILLE_CODE = (urlParams.get('liste') || '').trim().toUpperCase() || null;
const IS_FAMILLE = !!FAMILLE_CODE;
const OWNER = (urlParams.get('de') || '').trim().slice(0, 30);

let shoppingItems = [];
let realtimeSub = null;
let showAll = false;
let showBasket = false;
let selectedCat = 'Courses';
let pendingItem = null;
let pendingIdea = null;   // idée tapée avant d'avoir donné son prénom // article tapé avant d'avoir donné son prénom (mode famille)

const $ = (id) => document.getElementById(id);
const listCode = () => IS_FAMILLE ? FAMILLE_CODE : local.listCode;

// ============================================================
// STOCKAGE LOCAL
// ============================================================
function saveLocal() { try { localStorage.setItem('budgetCourses_v2', JSON.stringify(local)); } catch (e) {} }
function loadLocal() {
  try {
    const raw = localStorage.getItem('budgetCourses_v2');
    if (raw) local = { ...local, ...JSON.parse(raw) };
  } catch (e) {}
  if (!local.listCode) {
    local.listCode = Math.random().toString(36).slice(2, 8).toUpperCase();
    saveLocal();
  }
}

// ============================================================
// UTILS
// ============================================================
function esc(s) { return String(s ?? '').replace(/[&<>"']/g, c => ({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[c])); }
function norm(s) { return String(s || '').toLowerCase().normalize('NFD').replace(/[̀-ͯ]/g, '').replace(/œ/g, 'oe'); }
function fmt(n) { return (n || 0).toLocaleString('fr-FR', { minimumFractionDigits: 2, maximumFractionDigits: 2 }) + ' €'; }
function fmt0(n) { return Math.round(n || 0).toLocaleString('fr-FR') + ' €'; }
function uid() { return Math.random().toString(36).slice(2, 10); }
function catLabel(c) { return String(c || 'Autre').replace(/^[^\p{L}]+\s*/u, '') || 'Autre'; }
function catInfo(c) { const l = catLabel(c); return CATS.find(x => x.id === l) || CATS[CATS.length - 1]; }
function initials(name) {
  const w = String(name || '?').trim().split(/\s+/).filter(Boolean);
  if (w.length === 0) return '?';
  if (w.length === 1) return w[0].slice(0, 2).replace(/^./, c => c.toUpperCase());
  return (w[0][0] + w[1][0]).toUpperCase();
}
function hashTint(s) { let h = 0; for (const ch of norm(s)) h = (h * 31 + ch.charCodeAt(0)) >>> 0; return PEOPLE_TINTS[h % PEOPLE_TINTS.length]; }
function rayonOf(name) {
  const n = ' ' + norm(name) + ' ';
  for (const r of RAYONS) if (r.words.some(w => n.includes(w))) return r.name;
  return 'Autres';
}
function dayLabel(iso) {
  const d = new Date(iso); const t = new Date();
  const d0 = new Date(d.getFullYear(), d.getMonth(), d.getDate());
  const t0 = new Date(t.getFullYear(), t.getMonth(), t.getDate());
  const diff = Math.round((t0 - d0) / 86400000);
  if (diff === 0) return "Aujourd'hui";
  if (diff === 1) return 'Hier';
  return d.toLocaleDateString('fr-FR', { weekday: 'long', day: 'numeric', month: 'short' });
}
function parseAmount(v) { return parseFloat(String(v).replace(/[^\d,.-]/g, '').replace(',', '.')); }

let toastTimer;
function toast(msg) {
  const t = $('toast'); t.textContent = msg; t.classList.add('show');
  clearTimeout(toastTimer); toastTimer = setTimeout(() => t.classList.remove('show'), 2000);
}
function setSync(online) {
  $('syncDot').className = 'sync-dot' + (online ? '' : ' off');
  $('syncTxt').textContent = online ? 'en ligne' : 'hors ligne';
}

// ============================================================
// MOIS
// ============================================================
function monthKey(d = new Date()) { return `${d.getFullYear()}-${d.getMonth()}`; }
function checkAutoReset() {
  const cur = monthKey();
  if (local.lastMonth && local.lastMonth !== cur) archiveMonth(local.lastMonth);
  local.lastMonth = cur; saveLocal();
}
function archiveMonth(key) {
  if (local.expenses.length === 0) return;
  let y, m;
  if (key) { [y, m] = key.split('-').map(Number); }
  else { const d = new Date(); y = d.getFullYear(); m = d.getMonth(); }
  const total = local.expenses.reduce((s, e) => s + e.amount, 0);
  local.archives.unshift({ month: `${MONTHS_S[m]} ${y}`, budget: local.budget, total, count: local.expenses.length });
  local.archives = local.archives.slice(0, 12);
  local.expenses = [];
}

// ============================================================
// SUPABASE : LISTE
// ============================================================
async function loadShoppingList() {
  try {
    const { data, error } = await sb.from('shopping_lists').select('*').eq('list_code', listCode()).order('created_at', { ascending: true });
    if (error) throw error;
    shoppingItems = data || [];
    setSync(true);
  } catch (e) { setSync(false); }
}
function subscribeRealtime() {
  const code = listCode();
  if (realtimeSub) realtimeSub.unsubscribe();
  realtimeSub = sb.channel('shopping-' + code)
    .on('postgres_changes', { event: '*', schema: 'public', table: 'shopping_lists', filter: `list_code=eq.${code}` },
      async () => { await loadShoppingList(); renderList(); })
    .subscribe();
}
async function refreshList() { await loadShoppingList(); renderList(); }

async function addListItem(name) {
  let who;
  if (IS_FAMILLE) {
    if (!local.familyName) { pendingItem = name; openNameSheet(); return false; }
    who = local.familyName;
  } else who = local.userName || 'Maman';
  const { error } = await sb.from('shopping_lists').insert({ list_code: listCode(), name, price: 0, checked: false, added_by: who });
  if (error) { toast('Erreur réseau'); return false; }
  await refreshList();
  return true;
}
async function toggleItem(id) {
  const item = shoppingItems.find(i => String(i.id) === String(id));
  if (!item) return;
  item.checked = !item.checked; renderList(); // optimiste
  if (item.checked && !IS_FAMILLE) pantryRestock(item.name);
  const { error } = await sb.from('shopping_lists').update({ checked: item.checked }).eq('id', item.id);
  if (error) toast('Erreur réseau');
  await refreshList();
}
async function deleteListItem(id) {
  shoppingItems = shoppingItems.filter(i => String(i.id) !== String(id)); renderList();
  await sb.from('shopping_lists').delete().eq('id', id);
  await refreshList();
}
async function clearChecked() {
  await sb.from('shopping_lists').delete().eq('list_code', listCode()).eq('checked', true);
  await refreshList();
  toast('Panier vidé');
}

// ============================================================
// DÉPENSES
// ============================================================
function addExpense(name, amount, cat) {
  local.expenses.push({ id: uid(), date: new Date().toISOString(), name, amount, cat });
  saveLocal(); renderAll();
  toast('Dépense ajoutée');
}
function deleteExpense(id) {
  const e = local.expenses.find(x => x.id === id);
  if (!e || !confirm(`Supprimer « ${e.name} » (${fmt(e.amount)}) ?`)) return;
  local.expenses = local.expenses.filter(x => x.id !== id);
  saveLocal(); renderAll();
}
function frequentStores() {
  const count = {};
  local.expenses.forEach(e => { const k = e.name.trim(); count[k] = (count[k] || 0) + 1; });
  return Object.entries(count).sort((a, b) => b[1] - a[1]).map(([n]) => n);
}
function weekTotal() {
  const now = new Date(); const start = new Date(now);
  start.setDate(now.getDate() - ((now.getDay() + 6) % 7)); start.setHours(0, 0, 0, 0);
  return local.expenses.filter(e => new Date(e.date) >= start).reduce((s, e) => s + e.amount, 0);
}

// ============================================================
// RENDU
// ============================================================
function renderAll() {
  if (IS_FAMILLE) { renderList(); return; }
  renderHome(); renderList(); renderBilan(); renderSettings(); renderMenus(); renderTodayMenu();
}

function renderHome() {
  const now = new Date();
  const total = local.expenses.reduce((s, e) => s + e.amount, 0);
  const budget = local.budget;
  $('hello').textContent = 'Bonjour ' + (local.userName || '');
  $('monthTitle').textContent = MONTHS[now.getMonth()];

  const hero = $('hero');
  if (!(budget > 0)) {
    hero.innerHTML = `<div class="hero-empty">
      <div class="ring-big">${fmt(total)}</div>
      <p>dépensés ce mois-ci. Fixe un budget pour suivre ce qu'il te reste.</p>
      <button class="pill-btn" type="button" data-goto="settings">Définir mon budget</button></div>`;
  } else {
    const reste = budget - total;
    const pct = Math.min(total / budget, 1);
    const C = 2 * Math.PI * 80;
    const dayOfMonth = now.getDate();
    const daysInMonth = new Date(now.getFullYear(), now.getMonth() + 1, 0).getDate();
    const elapsed = dayOfMonth / daysInMonth;
    const projected = total > 0 ? (total / dayOfMonth) * daysInMonth : 0;
    const over = projected > budget + 5;
    const stroke = reste < 0 ? 'var(--danger)' : (pct >= .85 ? 'var(--warn-stroke)' : 'var(--accent)');
    const markerOff = -(C * elapsed) + 1;
    hero.innerHTML = `
      <div class="ring">
        <svg width="196" height="196" viewBox="0 0 196 196" role="img" aria-label="${Math.round(pct * 100)} % du budget utilisé">
          <circle cx="98" cy="98" r="80" fill="none" stroke="var(--soft)" stroke-width="16"/>
          <circle class="prog" cx="98" cy="98" r="80" fill="none" stroke="${stroke}" stroke-width="16" stroke-linecap="${pct > 0 ? 'round' : 'butt'}" stroke-dasharray="${(C * pct).toFixed(1)} ${C.toFixed(1)}" transform="rotate(-90 98 98)"/>
          <circle cx="98" cy="98" r="80" fill="none" stroke="var(--ink)" stroke-width="22" stroke-dasharray="2 ${C}" stroke-dashoffset="${markerOff.toFixed(1)}" transform="rotate(-90 98 98)" opacity=".55"/>
        </svg>
        <div class="ring-center">
          <span class="ring-small">${reste < 0 ? 'dépassement' : 'il reste'}</span>
          <span class="ring-big ${reste < 0 ? 'neg' : ''}">${fmt0(Math.abs(reste))}</span>
          <span class="ring-small">sur ${fmt0(budget)}</span>
        </div>
      </div>
      <div class="mini-grid">
        <div class="mini"><span class="k">Cette semaine</span><span class="v">${fmt(weekTotal())}</span></div>
        <div class="mini ${total === 0 ? '' : (over ? 'warn' : 'ok')}"><span class="k">Projection fin de mois</span><span class="v">${total === 0 ? '—' : fmt(projected)}</span></div>
      </div>`;
  }

  // Ajout rapide : magasins les plus fréquents
  const stores = frequentStores().slice(0, 3);
  const defaults = ['Lidl', 'Carrefour', 'Boulangerie'];
  for (const d of defaults) if (stores.length < 3 && !stores.some(s => norm(s) === norm(d))) stores.push(d);
  $('quickGrid').innerHTML = stores.map(s => `
    <button class="quick" type="button" data-quick="${esc(s)}">
      <span class="avatar av-32" style="background:${hashTint(s)}">${esc(initials(s))}</span><span>${esc(s)}</span>
    </button>`).join('') + `
    <button class="quick" type="button" data-quick="">
      <span class="avatar av-32" style="background:var(--soft)">+</span><span>Autre</span>
    </button>`;
  $('storeList').innerHTML = frequentStores().map(s => `<option value="${esc(s)}">`).join('');

  // Récemment
  const sorted = [...local.expenses].sort((a, b) => new Date(b.date) - new Date(a.date));
  const shown = showAll ? sorted : sorted.slice(0, 5);
  $('toggleAllBtn').hidden = sorted.length <= 5;
  $('toggleAllBtn').textContent = showAll ? 'Réduire' : 'Tout voir';
  if (sorted.length === 0) {
    $('recentList').innerHTML = `<div class="empty">Aucune dépense ce mois-ci.<br>Appuie sur + pour commencer.</div>`;
    return;
  }
  let html = '', lastDay = '';
  for (const e of shown) {
    const dl = dayLabel(e.date);
    if (dl !== lastDay) { html += `<div class="day-label">${esc(dl)}</div>`; lastDay = dl; }
    const ci = catInfo(e.cat);
    html += `<div class="exp">
      <span class="tile" style="background:${ci.tint}">${esc(initials(e.name))}</span>
      <span class="exp-info"><span class="exp-name">${esc(e.name)}</span><span class="exp-meta">${esc(catLabel(e.cat))}</span></span>
      <span class="exp-amt">${fmt(e.amount)}</span>
      <button class="icon-btn" type="button" data-del-exp="${esc(e.id)}" aria-label="Supprimer ${esc(e.name)}">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="M6 6l12 12M18 6 6 18"/></svg>
      </button></div>`;
  }
  $('recentList').innerHTML = html;
}

function canDeleteItem(item) {
  if (!IS_FAMILLE) return true;
  return !!local.familyName && norm(item.added_by) === norm(local.familyName);
}
function itemRow(item, canDelete = canDeleteItem(item)) {
  const who = item.added_by || '';
  return `<div class="item ${item.checked ? 'done' : ''}">
    <label>
      <input type="checkbox" data-toggle="${esc(item.id)}" ${item.checked ? 'checked' : ''}>
      <span class="item-name">${esc(item.name)}</span>
    </label>
    ${who ? `<span class="avatar av-26" style="background:${hashTint(who)}" title="Ajouté par ${esc(who)}">${esc(initials(who).slice(0, 1))}</span>` : ''}
    ${canDelete ? `<button class="icon-btn" type="button" data-del-item="${esc(item.id)}" aria-label="Retirer ${esc(item.name)}">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="M6 6l12 12M18 6 6 18"/></svg></button>` : ''}
  </div>`;
}

function renderList() {
  const todo = shoppingItems.filter(i => !i.checked);
  const done = shoppingItems.filter(i => i.checked);
  $('listSub').textContent = todo.length === 0 ? 'Rien à acheter' : `${todo.length} article${todo.length > 1 ? 's' : ''} à prendre`;
  const badge = $('listBadge');
  badge.textContent = todo.length; badge.classList.toggle('on', todo.length > 0 && !IS_FAMILLE);

  if (IS_FAMILLE) {
    // Puce « toi » pour changer de prénom
    const me = local.familyName;
    $('people').innerHTML = `<button class="me-chip" type="button" id="meBtn" aria-label="${me ? 'Changer de prénom' : 'Indiquer ton prénom'}">
      ${me ? esc(me) : 'Qui es-tu ?'}
      <span class="avatar av-26" style="background:${me ? hashTint(me) : 'var(--soft)'}">${me ? esc(initials(me).slice(0, 1)) : '?'}</span></button>`;
  } else {
    const names = [...new Set(shoppingItems.map(i => i.added_by).filter(Boolean))].slice(0, 4);
    $('people').innerHTML = names.map(n => `<span class="avatar" style="background:${hashTint(n)}" title="${esc(n)}">${esc(initials(n).slice(0, 1))}</span>`).join('') +
      `<button class="invite" type="button" id="inviteBtn" aria-label="Partager la liste">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" aria-hidden="true"><path d="M12 5v14M5 12h14"/></svg></button>`;
  }

  // Groupes par rayon
  const order = [...RAYONS.map(r => r.name), 'Autres'];
  const groups = {};
  todo.forEach(i => { const r = rayonOf(i.name); (groups[r] = groups[r] || []).push(i); });
  const keys = order.filter(k => groups[k]);
  $('groups').innerHTML = keys.length === 0
    ? `<div class="empty" style="margin-top:0">${done.length ? 'Tout est dans le panier.' : (IS_FAMILLE ? 'La liste est vide.<br>Ajoute ce qui manque en bas de l\'écran.' : 'La liste est vide.')}</div>`
    : keys.map(k => `<div class="group">
        <div class="group-head"><span class="group-title">${esc(k)}</span><span class="group-count">${groups[k].length}</span></div>
        ${groups[k].sort((a, b) => a.name.localeCompare(b.name, 'fr')).map(i => itemRow(i)).join('')}
      </div>`).join('');

  // Panier
  $('basketBar').hidden = done.length === 0;
  $('basketTxt').textContent = `${done.length} article${done.length > 1 ? 's' : ''} dans le panier`;
  $('basketToggle').textContent = showBasket ? 'Masquer' : 'Afficher';
  $('basket').hidden = !showBasket || done.length === 0;
  $('basket').innerHTML = done.length ? `<div class="group">${done.map(i => itemRow(i)).join('')}</div>` : '';
  $('basketClear').hidden = IS_FAMILLE; // seul le propriétaire vide le panier
}

function renderBilan() {
  const total = local.expenses.reduce((s, e) => s + e.amount, 0);
  const budget = local.budget;
  const reste = budget - total;
  $('b-spent').textContent = fmt(total);
  $('b-count').textContent = local.expenses.length;
  $('b-avg').textContent = local.expenses.length ? fmt(total / local.expenses.length) : '—';
  $('b-reste').textContent = budget > 0 ? (reste < 0 ? '−' + fmt(-reste) : fmt(reste)) : '—';
  $('b-reste-card').classList.toggle('neg', budget > 0 && reste < 0);

  // Par semaine
  const weekBudget = budget > 0 ? budget / 4.33 : 0;
  const weeks = {};
  local.expenses.forEach(e => {
    const d = new Date(e.date); const mon = new Date(d);
    mon.setDate(d.getDate() - ((d.getDay() + 6) % 7)); mon.setHours(0, 0, 0, 0);
    const k = mon.toISOString();
    if (!weeks[k]) {
      const sun = new Date(mon); sun.setDate(mon.getDate() + 6);
      weeks[k] = { label: `${mon.getDate()} – ${sun.getDate()} ${sun.toLocaleString('fr-FR', { month: 'short' })}`, total: 0 };
    }
    weeks[k].total += e.amount;
  });
  const wk = Object.keys(weeks).sort().reverse();
  const maxW = Math.max(weekBudget, ...wk.map(k => weeks[k].total), 1);
  $('weekly').innerHTML = wk.length === 0 ? `<p class="p-muted">Aucune dépense pour l'instant.</p>` :
    wk.map(k => {
      const w = weeks[k];
      const cls = weekBudget > 0 ? (w.total > weekBudget ? 'over' : w.total > weekBudget * .8 ? 'warn' : '') : '';
      return `<div class="bar-row"><span class="lbl">${esc(w.label)}</span>
        <span class="track"><span class="fill ${cls}" style="display:block;width:${(w.total / maxW * 100).toFixed(1)}%"></span></span>
        <span class="amt">${fmt(w.total)}</span></div>`;
    }).join('') + (weekBudget > 0 ? `<p class="p-muted" style="margin-top:8px">Repère : ${fmt(weekBudget)} par semaine</p>` : '');

  // Par catégorie
  const cats = {};
  local.expenses.forEach(e => { const l = catLabel(e.cat); cats[l] = (cats[l] || 0) + e.amount; });
  const sc = Object.entries(cats).sort((a, b) => b[1] - a[1]);
  $('bycat').innerHTML = sc.length === 0 ? `<p class="p-muted">Aucune dépense pour l'instant.</p>` :
    sc.map(([c, v], i) => `<div class="bar-row">
      <span class="lbl"><span class="dot" style="background:${CAT_COLORS[i % CAT_COLORS.length]}"></span>${esc(c)}</span>
      <span class="track"><span class="fill" style="display:block;width:${(v / sc[0][1] * 100).toFixed(1)}%;background:${CAT_COLORS[i % CAT_COLORS.length]}"></span></span>
      <span class="amt">${fmt(v)}</span></div>`).join('');

  renderCompare(total, budget);
}

function renderCompare(totalNow, budget) {
  const el = $('compare'); const canvas = $('chart');
  const now = new Date();
  const all = [...local.archives].reverse().concat([{ month: MONTHS_S[now.getMonth()], total: totalNow, budget, current: true }]);
  if (all.length < 2) {
    el.innerHTML = `<p class="p-muted">L'évolution apparaîtra après ton premier mois archivé.</p>`;
    canvas.style.display = 'none'; return;
  }
  const prev = all[all.length - 2], curr = all[all.length - 1];
  const diff = curr.total - prev.total;
  const avg = all.slice(0, -1).reduce((s, m) => s + m.total, 0) / (all.length - 1);
  const vs = curr.total - avg;
  el.innerHTML = `<div class="cmp-grid">
    <div class="cmp"><div class="k">vs mois dernier</div><div class="v ${diff > 0 ? 'up' : 'down'}">${diff > 0 ? '+' : '−'}${fmt(Math.abs(diff))}</div><div class="s">${esc(prev.month)}</div></div>
    <div class="cmp"><div class="k">vs moyenne</div><div class="v ${vs > 0 ? 'up' : 'down'}">${vs > 0 ? '+' : '−'}${fmt(Math.abs(vs))}</div><div class="s">moyenne ${fmt(avg)}</div></div>
  </div>`;

  canvas.style.display = 'block';
  const dpr = window.devicePixelRatio || 1;
  const W = canvas.clientWidth || 300, H = 190;
  canvas.width = W * dpr; canvas.height = H * dpr;
  const ctx = canvas.getContext('2d'); ctx.scale(dpr, dpr); ctx.clearRect(0, 0, W, H);
  const maxVal = Math.max(...all.map(m => m.total), budget || 0) * 1.15 || 1;
  const n = all.length, chartH = 130, baseY = 160;
  const bw = Math.max(16, Math.min(36, (W - 20) / n - 10));
  const gap = (W - n * bw) / (n + 1);
  if (budget > 0) {
    const by = baseY - budget / maxVal * chartH;
    ctx.setLineDash([4, 4]); ctx.strokeStyle = '#B4541B'; ctx.lineWidth = 1.2;
    ctx.beginPath(); ctx.moveTo(0, by); ctx.lineTo(W, by); ctx.stroke(); ctx.setLineDash([]);
    ctx.fillStyle = '#8A3D10'; ctx.font = '600 10px Manrope, sans-serif'; ctx.textAlign = 'right';
    ctx.fillText('budget', W - 2, by - 4);
  }
  all.forEach((m, i) => {
    const x = gap + i * (bw + gap);
    const h = m.total / maxVal * chartH, y = baseY - h;
    const over = (m.budget || budget) > 0 && m.total > (m.budget || budget);
    ctx.fillStyle = m.current ? (over ? '#A12C1F' : '#2F6B4F') : (over ? '#E7B4AC' : '#C9E8B4');
    ctx.beginPath();
    if (ctx.roundRect) ctx.roundRect(x, y, bw, h, [6, 6, 2, 2]); else ctx.rect(x, y, bw, h);
    ctx.fill();
    ctx.fillStyle = '#1E2A22'; ctx.textAlign = 'center'; ctx.font = '700 10px Manrope, sans-serif';
    ctx.fillText(Math.round(m.total), x + bw / 2, y - 5);
    ctx.fillStyle = '#56635A'; ctx.font = '500 10px Manrope, sans-serif';
    ctx.fillText(m.month.split(' ')[0] + (m.current ? '*' : ''), x + bw / 2, baseY + 15);
  });
  ctx.textAlign = 'left'; ctx.fillStyle = '#56635A'; ctx.font = '500 10px Manrope, sans-serif';
  ctx.fillText('* mois en cours', 0, H - 2);
}

function renderSettings() {
  $('setBudget').value = local.budget || '';
  $('setName').value = local.userName || '';
  $('myCode').textContent = local.listCode || '—';
  const el = $('archives');
  el.innerHTML = local.archives.length === 0 ? `<p class="p-muted">Aucun mois archivé pour l'instant.</p>` :
    local.archives.map(a => {
      const over = a.budget > 0 && a.total > a.budget;
      return `<div class="arch"><div>
        <div style="font-weight:700;font-size:15px">${esc(a.month)}</div>
        <div class="set-sub">${a.count} achats · budget ${a.budget > 0 ? fmt(a.budget) : 'non défini'}</div></div>
        <div style="font-family:var(--serif);font-size:18px;font-weight:700;color:${over ? 'var(--danger)' : 'var(--accent)'}">${fmt(a.total)}</div></div>`;
    }).join('');
}

// ============================================================
// MENUS DE LA SEMAINE
// Table Supabase `weekly_menus` (partagée avec la famille).
// Si elle n'existe pas encore, les menus restent sur ce téléphone.
// ============================================================
const DAYS = ['lundi','mardi','mercredi','jeudi','vendredi','samedi','dimanche'];
const SLOTS = [{ id: 'soir', label: 'Soir' }]; // uniquement le repas du soir
let menus = [];              // repas de la semaine affichée
let menuRemote = true;       // false si la table n'existe pas
let menuSub = null;
let weekOffset = 0;          // 0 = semaine en cours
let editingMeal = null;      // { day, slot }
let thisWeekMenus = [];      // repas de la semaine en cours (carte d'accueil)

function ymd(d) { return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`; }
function weekStart(offset = weekOffset) {
  const d = new Date(); d.setHours(0, 0, 0, 0);
  d.setDate(d.getDate() - ((d.getDay() + 6) % 7) + offset * 7);
  return d;
}
function weekDays() {
  const s = weekStart();
  return DAYS.map((name, i) => { const d = new Date(s); d.setDate(s.getDate() + i); return { name, date: d, key: ymd(d) }; });
}
function splitIngr(t) { return String(t || '').split(/\n|,|;/).map(x => x.trim()).filter(Boolean); }
function mealAt(day, slot) { return menus.find(m => m.day === day && m.slot === slot); }

function localMenus() { try { return JSON.parse(localStorage.getItem('budgetCourses_menus') || '[]'); } catch (e) { return []; } }
function saveLocalMenus(all) { try { localStorage.setItem('budgetCourses_menus', JSON.stringify(all)); } catch (e) {} }

async function loadMenus() {
  const days = weekDays();
  const from = days[0].key, to = days[6].key;
  if (menuRemote) {
    const { data, error } = await sb.from('weekly_menus').select('*').eq('list_code', listCode()).gte('day', from).lte('day', to);
    if (!error) { menus = data || []; if (weekOffset === 0) thisWeekMenus = menus; return; }
    // Table absente → mode local
    if (/weekly_menus|does not exist|schema cache|42P01|PGRST205/i.test((error.code || '') + ' ' + (error.message || ''))) {
      menuRemote = false;
    } else { toast('Menus : erreur réseau'); return; }
  }
  menus = localMenus().filter(m => m.day >= from && m.day <= to);
  if (weekOffset === 0) thisWeekMenus = menus;
}
function subscribeMenus() {
  if (!menuRemote) return;
  const code = listCode();
  if (menuSub) menuSub.unsubscribe();
  menuSub = sb.channel('menus-' + code)
    .on('postgres_changes', { event: '*', schema: 'public', table: 'weekly_menus', filter: `list_code=eq.${code}` },
      async () => { await loadMenus(); renderMenus(); renderTodayMenu(); })
    .subscribe();
}

async function saveMeal(day, slot, dish, ingredients) {
  const row = { list_code: listCode(), day, slot, dish, ingredients };
  if (menuRemote) {
    const { error } = await sb.from('weekly_menus').upsert(row, { onConflict: 'list_code,day,slot' });
    if (error) { toast('Erreur réseau'); return false; }
  } else {
    const all = localMenus().filter(m => !(m.day === day && m.slot === slot));
    all.push({ id: uid(), ...row }); saveLocalMenus(all);
  }
  // Mémoriser le plat pour le réutiliser
  local.dishBook = local.dishBook || {};
  local.dishBook[norm(dish)] = { dish, ingredients, used: Date.now() };
  saveLocal();
  await loadMenus();
  return true;
}
async function deleteMeal(day, slot) {
  if (menuRemote) {
    const { error } = await sb.from('weekly_menus').delete().eq('list_code', listCode()).eq('day', day).eq('slot', slot);
    if (error) { toast('Erreur réseau'); return; }
  } else saveLocalMenus(localMenus().filter(m => !(m.day === day && m.slot === slot)));
  await loadMenus();
}

// Ajoute des ingrédients à la liste de courses, sans doublon
async function ingredientsToList(list) {
  const pending = new Set(shoppingItems.filter(i => !i.checked).map(i => norm(i.name)));
  const fresh = [];
  for (const name of list) { const k = norm(name); if (!pending.has(k)) { pending.add(k); fresh.push(name); } }
  const skipped = list.length - fresh.length;
  if (fresh.length === 0) { toast(list.length ? 'Tout est déjà dans la liste' : 'Aucun ingrédient'); return; }
  const who = IS_FAMILLE ? (local.familyName || 'Famille') : (local.userName || 'Maman');
  const { error } = await sb.from('shopping_lists').insert(fresh.map(name => ({ list_code: listCode(), name, price: 0, checked: false, added_by: who })));
  if (error) { toast('Erreur réseau'); return; }
  await refreshList();
  toast(`${fresh.length} ajouté${fresh.length > 1 ? 's' : ''} à la liste${skipped ? ` · ${skipped} déjà dedans` : ''}`);
}

function renderMenus() {
  const days = weekDays();
  const s = days[0].date, e = days[6].date;
  const sameMonth = s.getMonth() === e.getMonth();
  $('weekLabel').textContent = weekOffset === 0 ? `Cette semaine · ${s.getDate()}${sameMonth ? '' : ' ' + MONTHS_S[s.getMonth()].toLowerCase()} – ${e.getDate()} ${MONTHS_S[e.getMonth()].toLowerCase()}`
    : `Semaine du ${s.getDate()} ${MONTHS_S[s.getMonth()].toLowerCase()} au ${e.getDate()} ${MONTHS_S[e.getMonth()].toLowerCase()}`;
  $('weekToday').hidden = weekOffset === 0;
  $('weekToList').hidden = IS_FAMILLE || !menus.some(m => splitIngr(m.ingredients).length);

  const note = $('menuNote');
  note.hidden = menuRemote || IS_FAMILLE;
  note.textContent = 'Les menus sont pour l\'instant enregistrés sur ce téléphone seulement. Pour les partager avec la famille, il faut créer la table « weekly_menus » dans Supabase.';
  if (IS_FAMILLE && !menuRemote) {
    $('days').innerHTML = `<div class="empty" style="margin-top:0">Les menus ne sont pas encore partagés.</div>`; return;
  }

  const today = ymd(new Date());
  const editable = !IS_FAMILLE;
  $('days').innerHTML = days.map(d => `
    <div class="day ${d.key === today ? 'today' : ''}">
      <div class="day-head"><span class="day-name">${d.name}</span>
        <span class="day-date ${d.key === today ? 'now' : ''}">${d.key === today ? "Aujourd'hui" : d.date.getDate() + ' ' + MONTHS_S[d.date.getMonth()].toLowerCase()}</span></div>
      ${SLOTS.map(sl => {
        const m = mealAt(d.key, sl.id);
        const ing = m ? splitIngr(m.ingredients) : [];
        return `<div class="meal-row"><button class="meal" type="button" ${editable ? `data-meal="${d.key}|${sl.id}"` : 'disabled'} aria-label="Dîner du ${d.name}${m ? ' : ' + esc(m.dish) : ', vide'}">
          <span class="meal-body">${m
            ? `<span class="meal-dish">${esc(m.dish)}</span>${ing.length ? `<span class="meal-ingr">${esc(ing.join(', '))}</span>` : ''}`
            : `<span class="meal-empty">${editable ? 'Ajouter le dîner' : 'Pas encore prévu'}</span>`}</span>
          ${editable && !m ? `<svg class="meal-chev" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="M12 5v14M5 12h14"/></svg>` : ''}
        </button>${m ? recipeLink(m.dish) : ''}</div>`;
      }).join('')}
    </div>`).join('');
}

// Carte « Ce soir » sur l'accueil
function renderTodayMenu() {
  const el = $('todayMenu'); if (!el || IS_FAMILLE) return;
  const today = ymd(new Date());
  const list = thisWeekMenus;
  const m = list.find(x => x.day === today && x.slot === 'soir');
  el.hidden = false;
  if (m) { el.dataset.goto = 'menus'; delete el.dataset.suggest; }
  else { delete el.dataset.goto; el.dataset.suggest = ''; }
  el.innerHTML = `<span class="tile" style="background:var(--lime)">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M7 3v8M5 3v4a2 2 0 0 0 4 0V3M7 11v10M17 21V3c-2 1.5-3 4-3 7v3h3"/></svg></span>
    <span style="flex:1;display:flex;flex-direction:column">
      <span class="k">${m ? 'Ce soir' : 'Rien de prévu ce soir'}</span>
      <span class="v">${m ? esc(m.dish) : 'Une idée de plat ?'}</span></span>
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#8C978F" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m9 6 6 6-6 6"/></svg>`;
}

function openMeal(day, slot, prefill) {
  const m = mealAt(day, slot);
  const d = weekDays().find(x => x.key === day);
  editingMeal = { day, slot, ideaId: prefill ? prefill.ideaId : null, have: prefill && prefill.have ? prefill.have : [] };
  $('mealTitle').textContent = `${d ? d.name.replace(/^./, c => c.toUpperCase()) : ''} soir`;
  $('mealDish').value = prefill ? prefill.dish : (m ? m.dish : '');
  $('mealIngr').value = prefill ? splitIngr(prefill.ingredients).join('\n') : (m ? splitIngr(m.ingredients).join('\n') : '');
  if (prefill) fillFromBook(prefill.dish);
  $('mealToList').checked = !m || !!prefill;
  $('mealDelete').hidden = !m;
  updateMealRecipe();
  // Plats déjà faits
  const book = Object.values(local.dishBook || {}).sort((a, b) => b.used - a.used);
  $('dishList').innerHTML = book.map(b => `<option value="${esc(b.dish)}">`).join('');
  $('dishChips').innerHTML = book.slice(0, 10).map(b => `<button type="button" class="chip" data-dish="${esc(b.dish)}">${esc(b.dish)}</button>`).join('');
  openSheet('mealSheet');
  if (prefill) setTimeout(() => $('mealIngr').focus(), 250);
  else if (!m) setTimeout(() => $('mealDish').focus(), 250);
}
function fillFromBook(dish) {
  const b = (local.dishBook || {})[norm(dish)];
  if (!b) return false;
  $('mealDish').value = b.dish;
  if (!$('mealIngr').value.trim()) $('mealIngr').value = splitIngr(b.ingredients).join('\n');
  return true;
}
$('mealDish').addEventListener('change', () => fillFromBook($('mealDish').value));
$('mealForm').addEventListener('submit', async (ev) => {
  ev.preventDefault();
  if (!editingMeal) return;
  const dish = $('mealDish').value.trim();
  if (!dish) { toast('Écris le nom du plat'); $('mealDish').focus(); return; }
  const ingr = splitIngr($('mealIngr').value);
  const ok = await saveMeal(editingMeal.day, editingMeal.slot, dish, ingr.join('\n'));
  if (!ok) return;
  closeSheet('mealSheet');
  if (editingMeal.ideaId) await deleteIdea(editingMeal.ideaId, true);
  renderMenus(); renderTodayMenu();
  const toBuy = ingr.filter(i => !(editingMeal.have || []).some(t => termMatches(t, i)));
  if ($('mealToList').checked && toBuy.length) await ingredientsToList(toBuy);
  else if ($('mealToList').checked && ingr.length) toast('Repas enregistré · tu as déjà tout');
  else toast('Repas enregistré');
});
async function weekToList() {
  const all = menus.flatMap(m => splitIngr(m.ingredients));
  await ingredientsToList(all);
}
// ============================================================
// CATALOGUE DE PLATS & « JE SAIS PAS QUOI FAIRE CE SOIR »
// Format : Nom|tags|saisons|ingrédients
//   tags : r rapide · v végé · p poisson · e pas cher · m mijoté
//   saisons : H hiver · P printemps · E été · A automne (vide = toute l'année)
// ============================================================
const CATALOGUE_RAW = `Hachis parmentier|e|HA|Viande hachée;Pommes de terre;Oignon;Lait;Beurre;Gruyère râpé
Gratin dauphinois et salade|ve|HA|Pommes de terre;Crème fraîche;Lait;Ail;Salade
Blanquette de veau|m|HA|Veau;Carottes;Champignons;Oignon;Crème fraîche;Bouillon;Riz
Croque-monsieur|re||Pain de mie;Jambon;Gruyère râpé;Beurre;Salade
Quiche lorraine|e||Pâte brisée;Lardons;Œufs;Crème fraîche;Lait;Salade
Chili con carne|em|HA|Viande hachée;Haricots rouges;Tomates concassées;Oignon;Poivron;Épices chili;Riz
Poulet basquaise|m|EA|Poulet;Poivrons;Tomates;Oignon;Ail;Riz
Crêpes salées|re||Farine;Œufs;Lait;Jambon;Gruyère râpé;Champignons
Lasagnes|m||Pâtes à lasagnes;Viande hachée;Sauce tomate;Oignon;Lait;Beurre;Farine;Gruyère râpé
Spaghetti bolognaise|e||Spaghetti;Viande hachée;Sauce tomate;Oignon;Carotte;Parmesan
Pâtes carbonara|re||Spaghetti;Lardons;Œufs;Parmesan;Crème fraîche
Bœuf bourguignon|m|HA|Bœuf à braiser;Vin rouge;Carottes;Lardons;Champignons;Oignons;Pommes de terre
Pot-au-feu|m|H|Bœuf pot-au-feu;Os à moelle;Carottes;Poireaux;Navets;Pommes de terre;Céleri
Tartiflette|e|H|Pommes de terre;Reblochon;Lardons;Oignon;Crème fraîche;Salade
Raclette||H|Fromage à raclette;Pommes de terre;Charcuterie;Cornichons;Salade
Fondue savoyarde||H|Comté;Beaufort;Emmental;Vin blanc;Pain;Ail
Choucroute garnie|m|H|Choucroute;Saucisses de Strasbourg;Saucisses de Morteau;Lard fumé;Pommes de terre
Cassoulet|m|H|Haricots blancs;Saucisses de Toulouse;Confit de canard;Tomates;Oignon
Petit salé aux lentilles|em|HA|Petit salé;Lentilles vertes;Carottes;Oignon
Poulet rôti et pommes de terre|e||Poulet entier;Pommes de terre;Ail;Thym;Beurre
Steak frites|r||Steaks;Frites;Salade
Poisson pané et purée|rpe||Poisson pané;Pommes de terre;Lait;Beurre
Saumon, riz et haricots verts|rp||Pavés de saumon;Riz;Citron;Haricots verts
Cabillaud à la provençale|p|E|Cabillaud;Tomates;Courgettes;Olives;Ail;Riz
Moules-frites|rp|EA|Moules;Frites;Échalotes;Vin blanc;Persil;Crème fraîche
Brandade de morue|p|H|Morue;Pommes de terre;Lait;Ail;Huile d'olive;Salade
Gratin de pâtes au jambon|re||Pâtes;Jambon;Crème fraîche;Gruyère râpé
Tortilla de pommes de terre|rve||Œufs;Pommes de terre;Oignon;Salade
Soupe de légumes|vem|HA|Carottes;Poireaux;Pommes de terre;Courgettes;Bouillon;Pain
Velouté de potiron|ve|A|Potiron;Pommes de terre;Oignon;Crème fraîche;Bouillon
Ratatouille et riz|vm|E|Courgettes;Aubergines;Poivrons;Tomates;Oignon;Ail;Riz
Tomates farcies|e|E|Tomates;Chair à saucisse;Oignon;Riz;Persil
Courgettes farcies|e|E|Courgettes;Viande hachée;Oignon;Riz;Gruyère râpé
Endives au jambon|e|H|Endives;Jambon;Lait;Beurre;Farine;Gruyère râpé
Gratin de chou-fleur|ve|HA|Chou-fleur;Lait;Beurre;Farine;Gruyère râpé
Parmentier de canard|m|H|Confit de canard;Pommes de terre;Lait;Beurre;Échalotes
Pizza maison|e||Pâte à pizza;Sauce tomate;Mozzarella;Jambon;Champignons;Olives
Tarte au thon|rpe||Pâte brisée;Thon en boîte;Tomates;Moutarde;Œufs;Crème fraîche
Tarte aux poireaux|ve|HA|Pâte brisée;Poireaux;Œufs;Crème fraîche;Gruyère râpé
Quiche saumon épinards|p||Pâte brisée;Saumon fumé;Épinards;Œufs;Crème fraîche
Salade niçoise|rp|E|Thon en boîte;Œufs;Tomates;Haricots verts;Olives;Pommes de terre;Salade
Salade de pâtes|rve|E|Pâtes;Tomates cerises;Mozzarella;Maïs;Basilic
Taboulé|rve|E|Semoule;Tomates;Concombre;Menthe;Persil;Citron
Hamburgers maison|r||Pains à burger;Steaks hachés;Cheddar;Tomates;Salade;Oignon;Frites
Fajitas au poulet|r||Tortillas;Poulet;Poivrons;Oignon;Crème fraîche;Fromage râpé;Épices fajitas
Curry de poulet|r||Poulet;Lait de coco;Curry;Oignon;Riz
Poulet à la crème et champignons|r||Poulet;Champignons;Crème fraîche;Échalotes;Riz
Escalopes milanaises|r||Escalopes de dinde;Chapelure;Œufs;Farine;Spaghetti;Sauce tomate
Cordon bleu et haricots verts|re||Cordons bleus;Haricots verts
Saucisses purée|re||Saucisses;Pommes de terre;Lait;Beurre
Saucisse de Morteau aux lentilles|e|HA|Saucisses de Morteau;Lentilles vertes;Carottes;Oignon
Rôti de porc et flageolets|e||Rôti de porc;Flageolets;Oignon;Ail
Côtes de porc à la moutarde|re||Côtes de porc;Moutarde;Crème fraîche;Pommes de terre
Filet mignon à la moutarde|r||Filet mignon de porc;Moutarde;Crème fraîche;Tagliatelles
Osso buco|m|H|Jarret de veau;Tomates concassées;Carottes;Oignon;Vin blanc;Riz
Navarin d'agneau|m|P|Épaule d'agneau;Carottes;Navets;Petits pois;Pommes de terre;Oignon
Gigot d'agneau et flageolets|m|P|Gigot d'agneau;Flageolets;Ail;Romarin
Couscous|m|HA|Semoule;Poulet;Merguez;Pois chiches;Carottes;Courgettes;Navets;Épices couscous
Tajine de poulet aux citrons|m||Poulet;Citrons confits;Olives;Oignon;Épices tajine;Semoule
Paella|mp||Riz;Poulet;Crevettes;Moules;Chorizo;Petits pois;Poivron;Safran
Risotto aux champignons|v|A|Riz arborio;Champignons;Oignon;Vin blanc;Parmesan;Bouillon
Riz cantonais|re||Riz;Œufs;Jambon;Petits pois;Crevettes;Sauce soja
Nouilles sautées au poulet|r||Nouilles chinoises;Poulet;Carottes;Poivron;Sauce soja;Oignon
Wok de légumes|rv||Nouilles chinoises;Brocoli;Carottes;Poivron;Champignons;Sauce soja
Poulet teriyaki|r||Poulet;Sauce teriyaki;Riz;Brocoli
Bœuf aux oignons|r||Bœuf émincé;Oignons;Sauce soja;Riz
Porc au caramel|e||Échine de porc;Sucre;Nuoc-mâm;Oignon;Riz
Pâtes au pesto|rve||Pâtes;Pesto;Parmesan;Tomates cerises
Pâtes au saumon|rp||Tagliatelles;Saumon fumé;Crème fraîche;Citron;Aneth
Pâtes au thon et à la tomate|rpe||Pâtes;Thon en boîte;Sauce tomate;Oignon;Olives
Pâtes aux courgettes|rve|E|Pâtes;Courgettes;Ail;Parmesan;Crème fraîche
One pot pasta|rve||Pâtes;Tomates cerises;Oignon;Ail;Basilic;Parmesan
Gnocchis gratinés|rv||Gnocchis;Sauce tomate;Mozzarella;Basilic
Cannellonis ricotta épinards|v||Cannellonis;Ricotta;Épinards;Sauce tomate;Gruyère râpé
Gratin de courgettes|ve|E|Courgettes;Œufs;Crème fraîche;Gruyère râpé;Riz
Gratin d'aubergines à la parmesane|v|E|Aubergines;Sauce tomate;Mozzarella;Parmesan;Basilic
Dahl de lentilles corail|vem||Lentilles corail;Lait de coco;Tomates concassées;Oignon;Curry;Riz
Curry de pois chiches|rve||Pois chiches;Lait de coco;Épinards;Curry;Oignon;Riz
Chili sin carne|vem|HA|Haricots rouges;Maïs;Tomates concassées;Poivron;Oignon;Épices chili;Riz
Buddha bowl|rv||Quinoa;Pois chiches;Avocat;Carottes;Chou rouge;Œufs
Œufs cocotte|rve||Œufs;Crème fraîche;Jambon;Pain
Shakshuka|rve||Œufs;Tomates concassées;Poivrons;Oignon;Cumin;Pain
Soupe à l'oignon gratinée|vem|H|Oignons;Bouillon;Pain;Gruyère râpé;Beurre
Soupe poireaux pommes de terre|ve|H|Poireaux;Pommes de terre;Bouillon;Crème fraîche
Minestrone|vem|A|Courgettes;Carottes;Haricots blancs;Tomates;Petites pâtes;Céleri
Potée auvergnate|m|H|Chou vert;Lard fumé;Saucisses;Carottes;Pommes de terre;Navets
Gratin de poisson aux épinards|p||Cabillaud;Épinards;Crème fraîche;Gruyère râpé;Pommes de terre
Papillotes de saumon|rp|E|Pavés de saumon;Courgettes;Citron;Aneth;Riz
Crevettes à l'ail|rp||Crevettes;Ail;Persil;Riz;Citron
Colin, riz et sauce citron|rp||Filets de colin;Riz;Citron;Crème fraîche;Haricots verts
Sardines grillées|rpe|E|Sardines;Citron;Pommes de terre;Salade
Truite aux amandes|rp|P|Truites;Amandes effilées;Beurre;Citron;Pommes de terre
Fish and chips|p||Cabillaud;Farine;Bière;Frites;Citron
Soupe de poisson|p|H|Soupe de poisson;Croûtons;Rouille;Gruyère râpé
Galettes complètes|re||Farine de sarrasin;Œufs;Jambon;Gruyère râpé;Beurre
Tarte flambée|re||Pâte à pizza;Crème fraîche;Lardons;Oignons
Hot-dogs|re||Pains à hot-dog;Saucisses de Strasbourg;Moutarde;Oignons frits;Frites
Wraps poulet crudités|r|E|Tortillas;Poulet;Salade;Tomates;Sauce blanche
Salade César|r|E|Poulet;Salade romaine;Parmesan;Croûtons;Sauce César
Salade de lentilles|rve||Lentilles en conserve;Échalote;Carottes;Œufs;Moutarde
Salade de riz|rpe|E|Riz;Thon en boîte;Maïs;Tomates;Œufs;Olives
Pan bagnat|rp|E|Pains ronds;Thon en boîte;Tomates;Œufs;Olives;Salade
Gaspacho et tartines|rv|E|Tomates;Concombre;Poivron;Ail;Huile d'olive;Pain
Brochettes de poulet|r|E|Brochettes de poulet;Poivrons;Oignons;Pommes de terre;Salade
Merguez et semoule|re|E|Merguez;Semoule;Tomates;Poivrons
Cuisses de poulet au four|e||Cuisses de poulet;Pommes de terre;Oignons;Thym;Ail
Poulet vallée d'Auge|m|A|Poulet;Champignons;Crème fraîche;Cidre;Pommes;Riz
Coq au vin|m|H|Poulet;Vin rouge;Lardons;Champignons;Oignons;Carottes;Pommes de terre
Lapin à la moutarde|m|A|Lapin;Moutarde;Crème fraîche;Vin blanc;Pommes de terre
Carbonade flamande|m|H|Bœuf à braiser;Bière brune;Oignons;Pain d'épices;Frites
Bœuf carottes|em|HA|Bœuf à braiser;Carottes;Oignons;Vin blanc;Pommes de terre
Rôti de bœuf et haricots verts|r||Rôti de bœuf;Haricots verts;Pommes de terre
Steak haché coquillettes|re||Steaks hachés;Coquillettes;Beurre;Gruyère râpé
Coquillettes jambon fromage|re||Coquillettes;Jambon;Gruyère râpé;Beurre
Boulettes sauce tomate|e||Viande hachée;Œufs;Chapelure;Sauce tomate;Oignon;Spaghetti
Chou farci|em|H|Chou vert;Chair à saucisse;Oignon;Carottes;Riz
Poivrons farcis|e|E|Poivrons;Viande hachée;Riz;Sauce tomate;Oignon
Moussaka|m|E|Aubergines;Viande hachée;Tomates concassées;Oignon;Lait;Farine;Beurre
Gratin de brocolis au jambon|re|HA|Brocolis;Jambon;Crème fraîche;Gruyère râpé
Cake salé jambon olives|e||Farine;Œufs;Lait;Huile;Olives;Jambon;Gruyère râpé;Salade
Bouchées à la reine|r||Vol-au-vent;Poulet;Champignons;Crème fraîche;Riz
Tagliatelles aux champignons|rv|A|Tagliatelles;Champignons;Crème fraîche;Ail;Persil;Parmesan
Légumes rôtis et halloumi|rv|E|Courgettes;Poivrons;Oignons rouges;Halloumi;Semoule
Poulet et patates douces au four|r|A|Poulet;Patates douces;Paprika;Salade
Velouté de carottes au cumin|rve|HA|Carottes;Oignon;Cumin;Bouillon;Crème fraîche
Œufs brouillés et tartines|rve||Œufs;Pain;Beurre;Salade
Tarte à la tomate et moutarde|ve|E|Pâte brisée;Tomates;Moutarde;Gruyère râpé;Herbes de Provence
Croziflette|e|H|Crozets;Reblochon;Lardons;Oignon;Crème fraîche
Aligot saucisses|e|H|Pommes de terre;Tomme fraîche;Ail;Crème fraîche;Saucisses de Toulouse
Poke bowl au saumon|rp|E|Saumon frais;Riz;Avocat;Concombre;Edamame;Sauce soja
Poulet tikka masala|||Poulet;Yaourt;Tomates concassées;Crème fraîche;Épices tikka;Riz;Naans
Gratin de ravioles|r||Ravioles;Crème fraîche;Gruyère râpé;Salade
Tortellinis crème jambon|re||Tortellinis;Jambon;Crème fraîche;Parmesan
Encornets à la provençale|p|E|Encornets;Tomates;Ail;Persil;Riz
Dorade au four|p|E|Dorades;Citron;Tomates;Fenouil;Pommes de terre
Lieu noir et épinards à la crème|rp||Filets de lieu noir;Épinards;Crème fraîche;Riz
Quenelles sauce tomate|re||Quenelles;Sauce tomate;Gruyère râpé;Riz
Pommes de terre au four et fromage blanc|rve||Pommes de terre;Fromage blanc;Ciboulette;Salade
Salade de chèvre chaud|rv||Salade;Bûche de chèvre;Pain;Miel;Noix
Soupe chinoise au poulet|r|H|Nouilles;Bouillon;Poulet;Champignons;Ciboulette;Sauce soja
Penne poulet brocoli|r||Penne;Poulet;Brocoli;Crème fraîche;Parmesan
Rougail saucisses|m||Saucisses fumées;Tomates;Oignons;Gingembre;Piment;Riz
Colombo de poulet|m||Poulet;Poudre de colombo;Pommes de terre;Courgettes;Oignon;Riz
Enchiladas|||Tortillas;Poulet;Haricots rouges;Sauce tomate;Fromage râpé
Gratin de macaronis|ve||Macaronis;Lait;Beurre;Farine;Gruyère râpé
Asperges et œufs mollets|rv|P|Asperges;Œufs;Pain;Beurre
Petits pois carottes et saucisses|re|P|Petits pois;Carottes;Saucisses;Oignon
Risotto aux asperges|v|P|Riz arborio;Asperges;Parmesan;Oignon;Bouillon
Quiche aux légumes|ve||Pâte brisée;Courgettes;Poivrons;Œufs;Crème fraîche
Pommes à l'huile et harengs|rp|HA|Harengs fumés;Pommes de terre;Oignons rouges;Salade
Bruschettas et salade|rv|E|Pain;Tomates;Mozzarella;Basilic;Salade
Velouté de courgettes|rve|E|Courgettes;Vache qui rit;Bouillon;Oignon
Curry de crevettes|rp||Crevettes;Lait de coco;Curry;Poivron;Riz
Pâtes crème et lardons|re||Pâtes;Lardons;Crème fraîche;Oignon
Jambon purée|re||Jambon;Pommes de terre;Lait;Beurre
Bœuf Stroganoff|r||Bœuf émincé;Champignons;Crème fraîche;Paprika;Oignon;Riz
Escalopes de dinde à la crème|re||Escalopes de dinde;Crème fraîche;Champignons;Pâtes
Magret et pommes sautées|r||Magret de canard;Pommes de terre;Salade
Velouté de champignons|rv|A|Champignons;Pommes de terre;Crème fraîche;Bouillon;Oignon
Hachis parmentier de poisson|pe||Cabillaud;Pommes de terre;Lait;Beurre;Gruyère râpé
Poêlée de gnocchis aux légumes|rv||Gnocchis;Courgettes;Tomates cerises;Pesto;Parmesan
Galettes de pommes de terre et salade|ve||Pommes de terre;Œufs;Farine;Oignon;Salade
Saumon à l'oseille|p|P|Pavés de saumon;Oseille;Crème fraîche;Riz`;
const CATALOGUE = CATALOGUE_RAW.split('\n').map(l => {
  const [dish, tags, seasons, ingr] = l.split('|');
  return { dish, tags: tags || '', seasons: seasons || '', ingredients: (ingr || '').split(';').join('\n') };
});
const SUGG_FILTERS = [
  { id: 'r', label: 'Rapide' }, { id: 'v', label: 'Végé' }, { id: 'p', label: 'Poisson' },
  { id: 'e', label: 'Pas cher' }, { id: 'm', label: 'Mijoté' }, { id: 'saison', label: 'De saison' }, { id: 'mine', label: 'Mes plats' }
];
const TAG_LABELS = { r: 'Rapide', v: 'Végé', p: 'Poisson', e: 'Pas cher', m: 'Mijoté' };
let suggFilters = new Set();
let suggSeen = new Set();
let suggCurrent = null;

function currentSeason() { const m = new Date().getMonth(); return m === 11 || m < 2 ? 'H' : m < 5 ? 'P' : m < 8 ? 'E' : 'A'; }
function suggestionPool() {
  const book = local.dishBook || {};
  const byKey = new Map();
  CATALOGUE.forEach(c => byKey.set(norm(c.dish), { ...c, mine: !!book[norm(c.dish)] }));
  // Plats de ta mère absents du catalogue
  Object.values(book).forEach(b => {
    const k = norm(b.dish);
    if (byKey.has(k)) { const c = byKey.get(k); if (splitIngr(b.ingredients).length) c.ingredients = b.ingredients; }
    else byKey.set(k, { dish: b.dish, tags: '', seasons: '', ingredients: b.ingredients, mine: true });
  });
  return [...byKey.values()];
}
function drawSuggestion() {
  const season = currentSeason();
  const tagF = [...suggFilters].filter(f => TAG_LABELS[f]);
  // À éviter : prévu cette semaine / semaine affichée, ou planifié ces 14 derniers jours
  const avoid = new Set([...thisWeekMenus, ...menus].map(m => norm(m.dish)));
  const twoWeeks = Date.now() - 14 * 86400000;
  Object.values(local.dishBook || {}).forEach(b => { if (b.used > twoWeeks) avoid.add(norm(b.dish)); });
  let pool = suggestionPool().filter(d =>
    tagF.every(t => d.tags.includes(t)) &&
    (!suggFilters.has('saison') || d.seasons.includes(season)) &&
    (!suggFilters.has('mine') || d.mine));
  const total = pool.length;
  let fresh = pool.filter(d => !avoid.has(norm(d.dish)) && !suggSeen.has(norm(d.dish)));
  if (fresh.length === 0) { suggSeen.clear(); fresh = pool.filter(d => !avoid.has(norm(d.dish))); }
  if (fresh.length === 0) fresh = pool;
  if (fresh.length === 0) return { dish: null, total };
  // Pondération : de saison ×2, hors saison ×0,15
  const w = fresh.map(d => !d.seasons ? 1 : d.seasons.includes(season) ? 2 : 0.15);
  let r = Math.random() * w.reduce((a, b) => a + b, 0);
  let pick = fresh[fresh.length - 1];
  for (let i = 0; i < fresh.length; i++) { r -= w[i]; if (r <= 0) { pick = fresh[i]; break; } }
  suggSeen.add(norm(pick.dish));
  return { dish: pick, total };
}
function renderSuggestFilters() {
  const hasMine = Object.keys(local.dishBook || {}).length > 0;
  $('suggestFilters').innerHTML = SUGG_FILTERS.filter(f => f.id !== 'mine' || hasMine).map(f =>
    `<button type="button" class="chip ${suggFilters.has(f.id) ? 'on' : ''}" aria-pressed="${suggFilters.has(f.id)}" data-sfilter="${f.id}">${f.label}</button>`).join('');
}
function rollSuggestion() {
  const { dish, total } = drawSuggestion();
  suggCurrent = dish;
  const card = $('suggestCard');
  card.classList.remove('roll'); void card.offsetWidth; card.classList.add('roll');
  $('suggestGo').disabled = !dish; $('suggestAgain').disabled = !dish;
  if (!dish) {
    card.innerHTML = `<div class="p-muted">Aucun plat ne correspond à ces filtres.<br>Essaie d'en retirer un.</div>`;
    return;
  }
  const tags = [...dish.tags].map(t => TAG_LABELS[t]).filter(Boolean);
  if (dish.seasons.includes(currentSeason())) tags.push('De saison');
  const ing = splitIngr(dish.ingredients);
  card.innerHTML = `
    <div class="sugg-dish">${esc(dish.dish)}</div>
    <div class="sugg-tags">${dish.mine ? '<span class="sugg-tag mine">Déjà fait</span>' : ''}${tags.map(t => `<span class="sugg-tag">${t}</span>`).join('')}</div>
    ${ing.length ? `<div class="sugg-ingr">${esc(ing.join(' · '))}</div>` : ''}
    ${recipeLink(dish.dish, 'Voir une recette')}
    <div class="p-muted" style="font-size:12px">${total} plat${total > 1 ? 's' : ''} possible${total > 1 ? 's' : ''}</div>`;
  $('suggestGo').textContent = IS_FAMILLE ? `Proposer à ${OWNER || 'la cheffe'}` : 'On fait ça';
  $('suggestGo').hidden = IS_FAMILLE && !ideasRemote;
  $('suggestKeep').hidden = IS_FAMILLE || !ideasRemote || !dish;
}
function openSuggest() {
  suggSeen.clear();
  renderSuggestFilters();
  rollSuggestion();
  openSheet('suggestSheet');
}
async function acceptSuggestion() {
  const d = suggCurrent; if (!d) return;
  closeSheet('suggestSheet');
  if (IS_FAMILLE) {
    if (!local.familyName) { pendingIdea = d.dish; openNameSheet(); return; }
    await addIdea(d.dish); return;
  }
  pickIdea = { dish: d.dish, ingredients: d.ingredients, id: null };
  pickOffset = weekOffset;
  $('pickTitle').textContent = `Quel soir pour « ${d.dish} » ?`;
  renderPick();
  openSheet('pickSheet');
}

// ============================================================
// RECETTE (Marmiton) & « AVEC CE QU'IL ME RESTE »
// ============================================================
const BOOK_ICON = '<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 19.5V5a2 2 0 0 1 2-2h13v16H6a2 2 0 0 0-2 2.5zM19 19v3H6"/></svg>';
function recipeUrl(dish) {
  const q = String(dish || '').replace(/\s+et\s+(salade|riz|tartines|haricots verts)$/i, '').trim();
  return 'https://www.marmiton.org/recettes/recherche.aspx?aqt=' + encodeURIComponent(q);
}
function recipeLink(dish, label = 'Recette') {
  return `<a class="recipe-link" href="${esc(recipeUrl(dish))}" target="_blank" rel="noopener" aria-label="Voir une recette de ${esc(dish)}">${BOOK_ICON}${label}</a>`;
}
// Ingrédients de base qu'on a presque toujours
const STAPLES = ['sel','poivre','huile','huile d olive','beurre','farine','sucre','ail','thym','bouillon','herbes de provence','moutarde','vinaigrette','persil','epices'];
// Mots qui regroupent plusieurs ingrédients
const FAMILIES = {
  fromage: ['gruyere','emmental','comte','mozzarella','reblochon','parmesan','cheddar','chevre','raclette','beaufort','ricotta','halloumi','tomme','fromage','vache qui rit'],
  pate: ['pate','spaghetti','tagliatelle','penne','coquillette','macaroni','lasagne','cannelloni','tortellini','raviole','gnocchi','nouille','crozet'],
  poisson: ['cabillaud','saumon','colin','lieu','truite','dorade','sardine','morue','thon','hareng','poisson','encornet'],
  viande: ['viande','boeuf','steak','veau','porc','poulet','dinde','agneau','canard','lapin','saucisse','merguez','jambon','lardon','chorizo'],
  legume: ['carotte','courgette','poireau','tomate','poivron','aubergine','champignon','epinard','brocoli','chou','haricot vert','petit pois','oignon','navet','potiron','asperge','endive','fenouil','celeri'],
  creme: ['creme fraiche','creme'],
  patate: ['pomme de terre']
};
function stem(t) { return norm(t).replace(/[^a-z0-9 ]+/g, ' ').split(/\s+/).filter(Boolean).map(w => w.length > 3 ? w.replace(/(s|x)$/, '') : w).join(' '); }
function isStaple(ing) { const s = stem(ing); return STAPLES.some(x => s === stem(x) || s.startsWith(stem(x) + ' ')); }
function termMatches(term, ing) {
  const t = stem(term), i = stem(ing);
  if (!t) return false;
  const has = (w) => (' ' + i + ' ').includes(' ' + w + ' ');
  if (has(t)) return true;
  const fam = FAMILIES[t];
  return !!fam && fam.some(f => has(stem(f)));
}
function fridgeSearch(text) {
  const terms = text.split(/[,;\n]+| et /).map(x => x.trim()).filter(x => stem(x).length >= 2);
  if (!terms.length) return { terms, results: [] };
  const results = [];
  for (const d of suggestionPool()) {
    const ing = splitIngr(d.ingredients);
    if (!ing.length) continue;
    const used = ing.filter(i => terms.some(t => termMatches(t, i)));
    const termsHit = terms.filter(t => ing.some(i => termMatches(t, i))).length;
    if (!termsHit) continue;
    const missing = ing.filter(i => !used.includes(i) && !isStaple(i));
    results.push({ d, used, missing, termsHit, score: termsHit * 10 - missing.length + (d.mine ? 0.5 : 0) });
  }
  results.sort((a, b) => b.score - a.score || a.missing.length - b.missing.length);
  return { terms, results: results.slice(0, 15) };
}
let fridgeLast = [];
let fridgeTerms = [];
function renderFridge() {
  const { terms, results } = fridgeSearch($('fridgeInput').value);
  fridgeLast = results; fridgeTerms = terms;
  const el = $('fridgeResults');
  if (!terms.length) { el.innerHTML = ''; return; }
  if (!results.length) { el.innerHTML = `<div class="empty" style="margin-top:0">Aucun plat trouvé avec ça.<br>Essaie un mot plus simple (ex : « fromage », « pâtes »).</div>`; return; }
  el.innerHTML = results.map((r, idx) => {
    const ing = splitIngr(r.d.ingredients).filter(i => r.used.includes(i) || !isStaple(i));
    const list = ing.map(i => r.used.includes(i) ? `<b>${esc(i)}</b>` : esc(i)).join(' · ');
    const miss = r.missing.length;
    return `<div class="fr-item">
      <div class="fr-head"><span class="fr-dish">${esc(r.d.dish)}</span>
        <span class="fr-score">${miss === 0 ? 'Tout est là' : `il manque ${miss}`}</span></div>
      <div class="fr-ingr">${list}</div>
      <div class="fr-actions">${recipeLink(r.d.dish)}
        ${IS_FAMILLE && !ideasRemote ? '' : `<button class="fr-go" type="button" data-fridge="${idx}">${IS_FAMILLE ? 'Proposer' : 'On fait ça'}</button>`}</div>
    </div>`;
  }).join('');
}
function openFridge() {
  openSheet('fridgeSheet');
  renderFridge();
  setTimeout(() => $('fridgeInput').focus(), 250);
}
let fridgeTimer;
$('fridgeInput').addEventListener('input', () => { clearTimeout(fridgeTimer); fridgeTimer = setTimeout(renderFridge, 150); });
async function acceptFridge(idx) {
  const r = fridgeLast[idx]; if (!r) return;
  closeSheet('fridgeSheet');
  if (IS_FAMILLE) {
    if (!local.familyName) { pendingIdea = r.d.dish; openNameSheet(); return; }
    await addIdea(r.d.dish); return;
  }
  // Les ingrédients qu'elle a déjà ne partiront pas dans la liste de courses
  pickIdea = { dish: r.d.dish, ingredients: r.d.ingredients, id: null, have: [...fridgeTerms] };
  pickOffset = weekOffset;
  $('pickTitle').textContent = `Quel soir pour « ${r.d.dish} » ?`;
  renderPick();
  openSheet('pickSheet');
}
function updateMealRecipe() {
  const d = $('mealDish').value.trim();
  const a = $('mealRecipe');
  a.hidden = !d;
  if (d) a.href = recipeUrl(d);
}
$('mealDish').addEventListener('input', updateMealRecipe);

// ============================================================
// IDÉES DE DÎNERS (proposées par la famille, choisies par la propriétaire)
// Table Supabase `menu_ideas`.
// ============================================================
let ideas = [];
let ideasRemote = true;
let ideasSub = null;
let pickIdea = null;      // idée en cours de planification
let pickOffset = 0;

async function loadIdeas() {
  if (!ideasRemote) return;
  const { data, error } = await sb.from('menu_ideas').select('*').eq('list_code', listCode()).order('created_at', { ascending: true });
  if (!error) { ideas = data || []; return; }
  if (/menu_ideas|does not exist|schema cache|42P01|PGRST205/i.test((error.code || '') + ' ' + (error.message || ''))) ideasRemote = false;
}
function subscribeIdeas() {
  if (!ideasRemote) return;
  const code = listCode();
  if (ideasSub) ideasSub.unsubscribe();
  ideasSub = sb.channel('ideas-' + code)
    .on('postgres_changes', { event: '*', schema: 'public', table: 'menu_ideas', filter: `list_code=eq.${code}` },
      async () => { await loadIdeas(); renderIdeas(); })
    .subscribe();
}
async function addIdea(dish) {
  const by = IS_FAMILLE ? local.familyName : (local.userName || 'Maman');
  if (ideas.some(i => norm(i.dish) === norm(dish))) { toast('Déjà proposé'); return true; }
  const { error } = await sb.from('menu_ideas').insert({ list_code: listCode(), dish, proposed_by: by });
  if (error) { toast('Erreur : proposition non envoyée'); return false; }
  await loadIdeas(); renderIdeas();
  toast(IS_FAMILLE ? `Proposé à ${OWNER || 'la famille'}` : 'Idée ajoutée');
  return true;
}
async function deleteIdea(id, silent) {
  ideas = ideas.filter(i => String(i.id) !== String(id)); renderIdeas();
  const { error } = await sb.from('menu_ideas').delete().eq('id', id);
  if (error) toast('Erreur réseau');
  await loadIdeas(); renderIdeas();
  if (!silent) toast('Idée retirée');
}
function canDeleteIdea(i) {
  if (!IS_FAMILLE) return true;
  return !!local.familyName && norm(i.proposed_by) === norm(local.familyName);
}
function renderIdeas() {
  const el = $('ideas');
  const badge = $('ideasBadge');
  const isMine = (i) => !IS_FAMILLE && norm(i.proposed_by) === norm(local.userName || 'Maman');
  // Point orange : seulement pour les propositions de la famille
  if (badge) badge.classList.toggle('on', !IS_FAMILLE && ideas.some(i => !isMine(i)));
  if (!ideasRemote) { el.innerHTML = ''; return; }
  const fromFamily = ideas.filter(i => !isMine(i)).length;
  const x = '<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="M6 6l12 12M18 6 6 18"/></svg>';
  const rows = ideas.map(i => `<div class="idea">
      <span class="avatar av-26" style="background:${hashTint(i.proposed_by || '?')}">${esc(initials(i.proposed_by || '?').slice(0, 1))}</span>
      <span class="idea-body"><span class="idea-dish">${esc(i.dish)}</span><span class="idea-by">${isMine(i) ? 'ton idée' : `proposé par ${esc(i.proposed_by || 'quelqu\'un')}`}</span></span>
      ${IS_FAMILLE ? '' : `<button class="idea-plan" type="button" data-plan="${esc(i.id)}">Planifier</button>`}
      ${canDeleteIdea(i) ? `<button class="icon-btn" type="button" data-del-idea="${esc(i.id)}" aria-label="Retirer ${esc(i.dish)}">${x}</button>` : ''}
    </div>`).join('');
  el.innerHTML = `<div class="ideas-card">
    <div class="ideas-head">
      <span class="ideas-title">Idées de dîners</span>
      <span class="ideas-sub">${IS_FAMILLE ? `${esc(OWNER || 'La cheffe')} choisit` : (fromFamily ? `${fromFamily} de la famille` : 'pour plus tard')}</span>
    </div>
    <form class="idea-form" id="ideaForm" autocomplete="off">
      <label class="sr" for="ideaInput">${IS_FAMILLE ? 'Proposer un dîner' : 'Noter une idée de dîner'}</label>
      <input id="ideaInput" type="text" placeholder="${IS_FAMILLE ? 'Une envie ? Ex : raclette' : 'Une idée pour plus tard…'}" enterkeyhint="send" maxlength="60">
      <button class="pill-btn" type="submit">${IS_FAMILLE ? 'Proposer' : 'Noter'}</button></form>
    ${rows || `<div class="ideas-empty">${IS_FAMILLE ? "Aucune proposition pour l'instant." : 'Note ici les plats à faire un de ces jours, la famille peut aussi en proposer.'}</div>`}
  </div>`;
}

// Planifier une idée : choisir le soir, puis compléter le repas
function openPick(id) {
  pickIdea = ideas.find(i => String(i.id) === String(id));
  if (!pickIdea) return;
  pickOffset = weekOffset;
  $('pickTitle').textContent = `Quel soir pour « ${pickIdea.dish} » ?`;
  renderPick();
  openSheet('pickSheet');
}
async function renderPick() {
  const s = weekStart(pickOffset);
  const days = DAYS.map((name, i) => { const d = new Date(s); d.setDate(s.getDate() + i); return { name, date: d, key: ymd(d) }; });
  $('pickWeekLbl').textContent = pickOffset === 0 ? 'Cette semaine' : pickOffset === 1 ? 'Semaine prochaine' : `Semaine du ${s.getDate()} ${MONTHS_S[s.getMonth()].toLowerCase()}`;
  let wk = [];
  if (menuRemote) {
    const { data } = await sb.from('weekly_menus').select('*').eq('list_code', listCode()).gte('day', days[0].key).lte('day', days[6].key);
    wk = data || [];
  } else wk = localMenus().filter(m => m.day >= days[0].key && m.day <= days[6].key);
  const today = ymd(new Date());
  $('pickDays').innerHTML = days.filter(d => d.key >= today).map(d => {
    const m = wk.find(x => x.day === d.key && x.slot === 'soir');
    return `<button class="pick-day" type="button" data-pick="${d.key}">
      <span class="n">${d.name}</span><span class="w ${m ? '' : 'free'}">${m ? 'remplace : ' + esc(m.dish) : 'libre'}</span>
      <span class="p-muted">${d.date.getDate()} ${MONTHS_S[d.date.getMonth()].toLowerCase()}</span></button>`;
  }).join('') || `<p class="p-muted">Cette semaine est passée.</p>`;
}
async function choosePickDay(dayKey) {
  closeSheet('pickSheet');
  const idea = pickIdea;
  // Afficher la bonne semaine puis ouvrir le repas pré-rempli
  if (weekOffset !== pickOffset) { weekOffset = pickOffset; await loadMenus(); renderMenus(); }
  openMeal(dayKey, 'soir', { dish: idea.dish, ingredients: idea.ingredients || '', ideaId: idea.id || null, have: idea.have || [] });
}

async function changeWeek(delta) {
  weekOffset = delta === 0 ? 0 : weekOffset + delta;
  await loadMenus(); renderMenus();
}

// ============================================================
// GARDE-MANGER
// Tables Supabase `pantry_zones` et `pantry_items` (réservées à la propriétaire).
// Si elles n'existent pas encore, tout reste sur ce téléphone.
// ============================================================
const ZONE_COLORS = ['#DCE6F5', '#F6E6C8', '#D3ECEB', '#E8DDF2', '#DCEFCF', '#F5D9D2', '#F2E9C9', '#E3E9E0'];
const DEFAULT_ZONES = [['Frigo', '#DCE6F5'], ['Placard', '#F6E6C8'], ['Congélateur', '#D3ECEB']];
let pantry = { zones: [], items: [] };
let pantryRemote = true;
let pantryZoneId = null;
let renamingZone = null;
let confirmZone = null;
let newZoneColor = ZONE_COLORS[4];
let editingPItem = null;
let pitemZone = null;

function pLocalGet() { try { return JSON.parse(localStorage.getItem('budgetCourses_pantry') || 'null') || { zones: [], items: [] }; } catch (e) { return { zones: [], items: [] }; } }
function pLocalSave() { try { localStorage.setItem('budgetCourses_pantry', JSON.stringify(pantry)); } catch (e) {} }
function pIsMissing(err) { return /pantry_|does not exist|schema cache|42P01|PGRST205/i.test(((err && err.code) || '') + ' ' + ((err && err.message) || '')); }

async function pLoad() {
  if (pantryRemote) {
    const [z, i] = await Promise.all([
      sb.from('pantry_zones').select('*').eq('list_code', local.listCode).order('position', { ascending: true }),
      sb.from('pantry_items').select('*').eq('list_code', local.listCode).order('created_at', { ascending: true })
    ]);
    if (!z.error && !i.error) {
      pantry = { zones: (z.data || []).sort((a, b) => (a.position || 0) - (b.position || 0)), items: i.data || [] };
    } else if (pIsMissing(z.error) || pIsMissing(i.error)) { pantryRemote = false; pantry = pLocalGet(); }
    else { toast('Garde-manger : erreur réseau'); return; }
  } else pantry = pLocalGet();
  if (pantry.zones.length === 0 && !local.pantryInit) {
    local.pantryInit = true; saveLocal();
    for (let k = 0; k < DEFAULT_ZONES.length; k++) await pInsert('pantry_zones', { name: DEFAULT_ZONES[k][0], color: DEFAULT_ZONES[k][1], position: k }, true);
    await pLoad(); return;
  }
  if (!pantry.zones.some(z => String(z.id) === String(pantryZoneId))) {
    pantryZoneId = (pantry.zones.find(z => String(z.id) === String(local.pantryZone)) || pantry.zones[0] || {}).id || null;
  }
}
function pArr(table) { return table === 'pantry_zones' ? pantry.zones : pantry.items; }
async function pInsert(table, row, noReload) {
  row = { list_code: local.listCode, ...row };
  if (pantryRemote) {
    const { error } = await sb.from(table).insert(row);
    if (error) { toast('Erreur réseau'); return false; }
  } else { pArr(table).push({ id: uid(), created_at: new Date().toISOString(), ...row }); pLocalSave(); }
  if (!noReload) await pLoad();
  return true;
}
async function pUpdate(table, id, patch) {
  const r = pArr(table).find(x => String(x.id) === String(id));
  if (r) Object.assign(r, patch);
  if (pantryRemote) {
    const { error } = await sb.from(table).update(patch).eq('id', id);
    if (error) { toast('Erreur réseau'); await pLoad(); return false; }
  } else pLocalSave();
  return true;
}
async function pDelete(table, id) {
  if (table === 'pantry_zones') pantry.items = pantry.items.filter(i => String(i.zone_id) !== String(id));
  if (table === 'pantry_zones') pantry.zones = pantry.zones.filter(z => String(z.id) !== String(id));
  else pantry.items = pantry.items.filter(i => String(i.id) !== String(id));
  if (pantryRemote) {
    if (table === 'pantry_zones') await sb.from('pantry_items').delete().eq('zone_id', id);
    const { error } = await sb.from(table).delete().eq('id', id);
    if (error) { toast('Erreur réseau'); await pLoad(); return false; }
  } else pLocalSave();
  return true;
}

// Liens avec la liste de courses
function inShoppingList(name) { const k = norm(name); return shoppingItems.some(i => !i.checked && norm(i.name) === k); }
async function addNamesToList(names) {
  const fresh = names.filter(n => !inShoppingList(n));
  if (!fresh.length) return 0;
  const { error } = await sb.from('shopping_lists').insert(fresh.map(name => ({ list_code: local.listCode, name, price: 0, checked: false, added_by: local.userName || 'Maman' })));
  if (error) { toast('Erreur réseau'); return 0; }
  await refreshList();
  return fresh.length;
}
// Article coché dans la liste de courses → de retour en stock
async function pantryRestock(name) {
  const k = norm(name);
  const hits = pantry.items.filter(i => !i.in_stock && norm(i.name) === k);
  for (const h of hits) await pUpdate('pantry_items', h.id, { in_stock: true });
  if (hits.length && $('tab-pantry').classList.contains('active')) renderPantry();
}

function zoneById(id) { return pantry.zones.find(z => String(z.id) === String(id)); }
function renderPantry() {
  if (IS_FAMILLE) return;
  const out = pantry.items.filter(i => !i.in_stock);
  $('pantrySub').textContent = `${pantry.items.length} article${pantry.items.length > 1 ? 's' : ''}${out.length ? ` · ${out.length} épuisé${out.length > 1 ? 's' : ''}` : ''}${pantryRemote ? '' : ' · sur ce téléphone'}`;
  // Endroits
  $('zoneChips').innerHTML = pantry.zones.map(z => {
    const n = pantry.items.filter(i => String(i.zone_id) === String(z.id)).length;
    const on = String(z.id) === String(pantryZoneId);
    return `<button type="button" class="zchip ${on ? 'on' : ''}" data-zone="${esc(z.id)}" aria-pressed="${on}"><span class="dot" style="background:${esc(z.color || '#E3E9E0')}"></span>${esc(z.name)}<span class="n">${n}</span></button>`;
  }).join('') + `<button type="button" class="zchip add" id="zoneAddChip">+ Endroit</button>`;
  // À racheter
  const alert = $('pantryAlert');
  if (!out.length) alert.innerHTML = '';
  else {
    const missing = out.filter(i => !inShoppingList(i.name));
    const names = out.map(i => i.name);
    alert.innerHTML = `<div class="pantry-alert"><span style="flex:1;display:flex;flex-direction:column;gap:2px">
      <span class="t">${out.length} chose${out.length > 1 ? 's' : ''} à racheter</span>
      <span class="s">${esc(names.slice(0, 4).join(', '))}${names.length > 4 ? '…' : ''}</span></span>
      ${missing.length ? `<button type="button" id="pantryToList">Dans la liste</button>` : `<span class="done">✓ Dans la liste</span>`}</div>`;
  }
  // Ajout
  const zone = zoneById(pantryZoneId);
  $('pantryAdd').hidden = !zone;
  $('pantryName').placeholder = zone ? `Ajouter dans « ${zone.name} »…` : 'Ajouter…';
  // Articles de l'endroit
  const list = $('pantryList');
  if (!zone) { list.innerHTML = `<div class="pantry-empty">Crée ton premier endroit avec « + Endroit ».</div>`; return; }
  const items = pantry.items.filter(i => String(i.zone_id) === String(zone.id))
    .sort((a, b) => (a.in_stock === b.in_stock ? a.name.localeCompare(b.name, 'fr') : a.in_stock ? 1 : -1));
  list.innerHTML = items.length === 0
    ? `<div class="pantry-empty">Rien dans « ${esc(zone.name)} » pour l'instant.<br>Ajoute ce que tu as juste au-dessus.</div>`
    : items.map(i => {
      const listed = !i.in_stock && inShoppingList(i.name);
      return `<div class="pitem ${i.in_stock ? '' : 'out'}">
        <button type="button" class="pname" data-pitem="${esc(i.id)}" aria-label="Modifier ${esc(i.name)}">
          <span class="n">${esc(i.name)}</span>${!i.in_stock ? `<span class="s">${listed ? 'dans la liste de courses' : 'pas dans la liste'}</span>` : ''}</button>
        <button type="button" class="ppill" data-ptoggle="${esc(i.id)}" aria-pressed="${!i.in_stock}" aria-label="${esc(i.name)} : ${i.in_stock ? 'en stock, marquer épuisé' : 'épuisé, marquer en stock'}">${i.in_stock ? 'En stock' : 'Épuisé'}</button>
      </div>`;
    }).join('');
}
async function togglePantryItem(id) {
  const it = pantry.items.find(i => String(i.id) === String(id)); if (!it) return;
  const nowOut = it.in_stock !== false;
  await pUpdate('pantry_items', id, { in_stock: !nowOut });
  renderPantry();
  if (nowOut) {
    const n = await addNamesToList([it.name]);
    toast(n ? `${it.name} : ajouté à la liste` : `${it.name} : épuisé`);
    renderPantry();
  }
}
async function pantryAllToList() {
  const n = await addNamesToList(pantry.items.filter(i => !i.in_stock).map(i => i.name));
  toast(n ? `${n} ajouté${n > 1 ? 's' : ''} à la liste` : 'Tout est déjà dans la liste');
  renderPantry();
}
$('pantryAdd').addEventListener('submit', async (ev) => {
  ev.preventDefault();
  const inp = $('pantryName'); const name = inp.value.trim();
  const zone = zoneById(pantryZoneId);
  if (!name || !zone) return;
  if (pantry.items.some(i => String(i.zone_id) === String(zone.id) && norm(i.name) === norm(name))) { toast('Déjà dans cet endroit'); return; }
  inp.value = '';
  await pInsert('pantry_items', { zone_id: zone.id, name, in_stock: true });
  renderPantry(); inp.focus();
});

// Mes endroits
function renderZonesSheet() {
  const zs = pantry.zones;
  $('zonesList').innerHTML = zs.length === 0 ? `<p class="p-muted">Aucun endroit pour l'instant.</p>` : zs.map((z, idx) => {
    const n = pantry.items.filter(i => String(i.zone_id) === String(z.id)).length;
    const id = esc(z.id);
    const row = String(renamingZone) === String(z.id)
      ? `<div class="zrow"><span class="ztile" style="background:${esc(z.color || '#E3E9E0')}">${esc(initials(z.name))}</span>
          <label class="sr" for="renameZone">Nouveau nom</label><input id="renameZone" value="${esc(z.name)}" maxlength="30">
          <button class="pill-btn" type="button" data-zone-save="${id}" style="height:40px;padding:0 14px">OK</button></div>`
      : `<div class="zrow"><span class="ztile" style="background:${esc(z.color || '#E3E9E0')}">${esc(initials(z.name))}</span>
          <span class="zinfo"><b>${esc(z.name)}</b><span>${n} article${n > 1 ? 's' : ''}</span></span>
          <button class="icon-btn" type="button" data-zone-up="${id}" aria-label="Monter ${esc(z.name)}" ${idx === 0 ? 'disabled' : ''}><svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m6 15 6-6 6 6"/></svg></button>
          <button class="icon-btn" type="button" data-zone-down="${id}" aria-label="Descendre ${esc(z.name)}" ${idx === zs.length - 1 ? 'disabled' : ''}><svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="m6 9 6 6 6-6"/></svg></button>
          <button class="icon-btn" type="button" data-zone-rename="${id}" aria-label="Renommer ${esc(z.name)}"><svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M12 20h9M16.5 3.5a2.1 2.1 0 0 1 3 3L7 19l-4 1 1-4z"/></svg></button>
          <button class="icon-btn del" type="button" data-zone-del="${id}" aria-label="Supprimer ${esc(z.name)}"><svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M3 6h18M8 6V4h8v2M6 6l1 14h10l1-14"/></svg></button></div>`;
    const conf = String(confirmZone) === String(z.id)
      ? `<div class="zconfirm"><span><b>Supprimer « ${esc(z.name)} » ?</b>${n ? ` Ses ${n} article${n > 1 ? 's' : ''} seront aussi supprimés.` : ''}</span>
          <div class="acts"><button type="button" data-zone-cancel="1">Annuler</button><button type="button" class="yes" data-zone-confirm="${id}">Supprimer</button></div></div>` : '';
    return row + conf;
  }).join('');
  $('newZoneColors').innerHTML = ZONE_COLORS.map(c => `<button type="button" role="radio" aria-checked="${c === newZoneColor}" aria-label="Couleur" data-zcolor="${c}" style="background:${c}"></button>`).join('');
  if (renamingZone) setTimeout(() => { const r = $('renameZone'); if (r) { r.focus(); r.select(); } }, 50);
}
function openZones(focusNew) {
  renamingZone = null; confirmZone = null;
  renderZonesSheet(); openSheet('zonesSheet');
  if (focusNew) setTimeout(() => $('newZoneName').focus(), 250);
}
async function moveZone(id, dir) {
  const zs = pantry.zones; const i = zs.findIndex(z => String(z.id) === String(id)); const j = i + dir;
  if (i < 0 || j < 0 || j >= zs.length) return;
  [zs[i], zs[j]] = [zs[j], zs[i]];
  renderZonesSheet(); renderPantry();
  for (let k = 0; k < zs.length; k++) if (zs[k].position !== k) await pUpdate('pantry_zones', zs[k].id, { position: k });
}
async function saveZoneName(id) {
  const v = ($('renameZone') || {}).value; const name = (v || '').trim();
  renamingZone = null;
  if (name) await pUpdate('pantry_zones', id, { name });
  renderZonesSheet(); renderPantry();
}
$('newZoneForm').addEventListener('submit', async (ev) => {
  ev.preventDefault();
  const name = $('newZoneName').value.trim();
  if (!name) { toast("Donne un nom à l'endroit"); return; }
  if (pantry.zones.some(z => norm(z.name) === norm(name))) { toast('Cet endroit existe déjà'); return; }
  $('newZoneName').value = '';
  const ok = await pInsert('pantry_zones', { name, color: newZoneColor, position: pantry.zones.length });
  if (!ok) return;
  const z = pantry.zones.find(x => norm(x.name) === norm(name));
  if (z) { pantryZoneId = z.id; local.pantryZone = z.id; saveLocal(); }
  newZoneColor = ZONE_COLORS[(ZONE_COLORS.indexOf(newZoneColor) + 1) % ZONE_COLORS.length];
  renderZonesSheet(); renderPantry();
  toast(`« ${name} » créé`);
});

// Fiche article
function renderPItemZones() {
  $('pitemZones').innerHTML = pantry.zones.map(z => `<button type="button" role="radio" aria-checked="${String(z.id) === String(pitemZone)}" class="chip ${String(z.id) === String(pitemZone) ? 'on' : ''}" data-pzone="${esc(z.id)}"><span class="dot" style="background:${esc(z.color || '#E3E9E0')}"></span>${esc(z.name)}</button>`).join('');
}
function openPItem(id) {
  const it = pantry.items.find(i => String(i.id) === String(id)); if (!it) return;
  editingPItem = it.id; pitemZone = it.zone_id;
  $('pitemName').value = it.name;
  renderPItemZones(); openSheet('pitemSheet');
}
$('pitemForm').addEventListener('submit', async (ev) => {
  ev.preventDefault();
  const name = $('pitemName').value.trim();
  if (!name) { toast("Écris le nom de l'article"); return; }
  closeSheet('pitemSheet');
  await pUpdate('pantry_items', editingPItem, { name, zone_id: pitemZone });
  renderPantry(); toast('Article modifié');
});

// ============================================================
// NAVIGATION & FEUILLES
// ============================================================
function showTab(name) {
  document.querySelectorAll('.tab').forEach(t => t.classList.toggle('active', t.id === 'tab-' + name));
  const navName = name === 'pantry' ? 'list' : name;
  document.querySelectorAll('.nav-btn').forEach(b => b.classList.toggle('active', b.dataset.tab === navName));
  document.querySelectorAll('.list-seg button').forEach(b => { b.classList.toggle('on', b.dataset.tab === name); b.setAttribute('aria-selected', b.dataset.tab === name); });
  if (!IS_FAMILLE && (name === 'list' || name === 'pantry') && local.lastListView !== name) { local.lastListView = name; saveLocal(); }
  document.querySelectorAll('#famSeg button').forEach(b => { b.classList.toggle('on', b.dataset.tab === name); b.setAttribute('aria-selected', b.dataset.tab === name); });
  document.body.className = document.body.className.replace(/\bon-\w+/g, '').trim() + ' on-' + name;
  window.scrollTo(0, 0);
  if (name === 'bilan') renderBilan();
  if (name === 'menus') renderMenus();
  if (name === 'pantry') { renderPantry(); pLoad().then(renderPantry); }
}
function openSheet(id) { $(id).classList.add('open'); }
function closeSheet(id) { $(id).classList.remove('open'); }

function renderChips() {
  $('catChips').innerHTML = CATS.map(c => `
    <button type="button" role="radio" aria-checked="${c.id === selectedCat}" class="chip ${c.id === selectedCat ? 'on' : ''}" data-cat="${esc(c.id)}">
      <span class="dot" style="background:${c.tint};border:1px solid rgba(0,0,0,.08)"></span>${esc(c.id)}</button>`).join('');
}
function openExpense(store = '') {
  $('expName').value = store;
  $('expAmount').value = '';
  // catégorie : celle utilisée la dernière fois pour ce magasin
  const last = [...local.expenses].reverse().find(e => store && norm(e.name) === norm(store));
  selectedCat = last ? catLabel(last.cat) : (norm(store).includes('boulang') ? 'Boulangerie' : 'Courses');
  renderChips();
  openSheet('expenseSheet');
  setTimeout(() => $('expAmount').focus(), 250);
}
function shareUrl() {
  const p = new URLSearchParams({ liste: local.listCode });
  if (local.userName) p.set('de', local.userName);
  return `${SHARE_BASE}?${p.toString()}`;
}
function shareList() {
  $('shareUrl').textContent = shareUrl();
  openSheet('shareSheet');
}
async function copyShare() {
  const url = shareUrl();
  if (navigator.share) {
    try {
      await navigator.share({ title: 'Liste de courses', text: `Ajoute ce qu'il manque à la liste de courses de ${local.userName || 'la maison'} :`, url });
      closeSheet('shareSheet'); return;
    } catch (e) { if (e.name === 'AbortError') return; }
  }
  try { await navigator.clipboard.writeText(url); }
  catch (e) {
    const ta = document.createElement('textarea'); ta.value = url; document.body.appendChild(ta);
    ta.select(); document.execCommand('copy'); document.body.removeChild(ta);
  }
  toast('Lien copié'); closeSheet('shareSheet');
}

// ============================================================
// ÉVÉNEMENTS
// ============================================================
document.addEventListener('click', (ev) => {
  const t = ev.target.closest('button, [data-goto]');
  if (!t) return;
  if (t.dataset.tab && t.closest('nav') && t.dataset.tab === 'list' && local.lastListView === 'pantry' && !IS_FAMILLE) return showTab('pantry');
  if (t.dataset.tab) return showTab(t.dataset.tab);
  if (t.dataset.zone) { pantryZoneId = t.dataset.zone; local.pantryZone = t.dataset.zone; saveLocal(); return renderPantry(); }
  if (t.id === 'zoneAddChip') return openZones(true);
  if (t.id === 'zonesBtn') return openZones(false);
  if (t.id === 'pantryToList') return pantryAllToList();
  if (t.dataset.ptoggle) return togglePantryItem(t.dataset.ptoggle);
  if (t.dataset.pitem) return openPItem(t.dataset.pitem);
  if (t.dataset.pzone) { pitemZone = t.dataset.pzone; return renderPItemZones(); }
  if (t.id === 'pitemDelete') { const id = editingPItem; closeSheet('pitemSheet'); return pDelete('pantry_items', id).then(() => { renderPantry(); toast('Article supprimé'); }); }
  if (t.dataset.zoneUp) return moveZone(t.dataset.zoneUp, -1);
  if (t.dataset.zoneDown) return moveZone(t.dataset.zoneDown, 1);
  if (t.dataset.zoneRename) { renamingZone = t.dataset.zoneRename; confirmZone = null; return renderZonesSheet(); }
  if (t.dataset.zoneSave) return saveZoneName(t.dataset.zoneSave);
  if (t.dataset.zoneDel) { confirmZone = t.dataset.zoneDel; renamingZone = null; return renderZonesSheet(); }
  if (t.dataset.zoneCancel) { confirmZone = null; return renderZonesSheet(); }
  if (t.dataset.zoneConfirm) {
    const id = t.dataset.zoneConfirm; const z = zoneById(id); confirmZone = null;
    return pDelete('pantry_zones', id).then(() => {
      if (String(pantryZoneId) === String(id)) pantryZoneId = (pantry.zones[0] || {}).id || null;
      renderZonesSheet(); renderPantry(); toast(`« ${z ? z.name : 'Endroit'} » supprimé`);
    });
  }
  if (t.dataset.zcolor) { newZoneColor = t.dataset.zcolor; return renderZonesSheet(); }
  if (t.dataset.suggest !== undefined) return openSuggest();
  if (t.dataset.goto) return showTab(t.dataset.goto);
  if (t.id === 'fab') return openExpense('');
  if (t.dataset.quick !== undefined) return openExpense(t.dataset.quick);
  if (t.dataset.delExp) return deleteExpense(t.dataset.delExp);
  if (t.dataset.delItem) return deleteListItem(t.dataset.delItem);
  if (t.dataset.cat) { selectedCat = t.dataset.cat; return renderChips(); }
  if (t.hasAttribute('data-close')) return t.closest('.overlay').classList.remove('open');
  if (t.id === 'toggleAllBtn') { showAll = !showAll; return renderHome(); }
  if (t.id === 'basketToggle') { showBasket = !showBasket; return renderList(); }
  if (t.id === 'basketClear') return clearChecked();
  if (t.id === 'inviteBtn' || t.id === 'shareBtn2') return shareList();
  if (t.id === 'meBtn') return openNameSheet();
  if (t.dataset.meal) { const [d, s] = t.dataset.meal.split('|'); return openMeal(d, s); }
  if (t.dataset.dish) { fillFromBook(t.dataset.dish); return; }
  if (t.id === 'mealDelete') {
    if (!editingMeal) return;
    const { day, slot } = editingMeal;
    closeSheet('mealSheet');
    return deleteMeal(day, slot).then(() => { renderMenus(); renderTodayMenu(); toast('Repas effacé'); });
  }
  if (t.id === 'weekPrev') return changeWeek(-1);
  if (t.id === 'weekNext') return changeWeek(1);
  if (t.id === 'weekToday') return changeWeek(0);
  if (t.id === 'weekToList') return weekToList();
  if (t.dataset.plan) return openPick(t.dataset.plan);
  if (t.id === 'suggestBtn' || t.dataset.suggest !== undefined) return openSuggest();
  if (t.id === 'suggestAgain') return rollSuggestion();
  if (t.id === 'fridgeBtn') return openFridge();
  if (t.dataset.fridge !== undefined) return acceptFridge(Number(t.dataset.fridge));
  if (t.id === 'suggestGo') return acceptSuggestion();
  if (t.id === 'suggestKeep') { const d = suggCurrent; if (d) { closeSheet('suggestSheet'); return addIdea(d.dish); } return; }
  if (t.dataset.sfilter) {
    const f = t.dataset.sfilter;
    suggFilters.has(f) ? suggFilters.delete(f) : suggFilters.add(f);
    suggSeen.clear(); renderSuggestFilters(); return rollSuggestion();
  }
  if (t.dataset.delIdea) return deleteIdea(t.dataset.delIdea);
  if (t.dataset.pick) return choosePickDay(t.dataset.pick);
  if (t.id === 'pickPrev') { pickOffset = Math.max(0, pickOffset - 1); return renderPick(); }
  if (t.id === 'pickNext') { pickOffset += 1; return renderPick(); }
  if (t.id === 'copyShare') return copyShare();
  if (t.id === 'resetBtn') return openSheet('resetSheet');
  if (t.id === 'confirmReset') return doReset();
  if (t.id === 'saveSettings') return saveSettings();
});
document.addEventListener('submit', async (ev) => {
  if (ev.target.id !== 'ideaForm') return;
  ev.preventDefault();
  const inp = $('ideaInput'); const dish = inp.value.trim();
  if (!dish) { toast('Écris une idée de plat'); return; }
  if (IS_FAMILLE && !local.familyName) { pendingIdea = dish; openNameSheet(); return; }
  inp.value = '';
  if (!(await addIdea(dish))) { const i2 = $('ideaInput'); if (i2) i2.value = dish; }
});
document.addEventListener('keydown', (ev) => {
  if (ev.key === 'Enter' && ev.target && ev.target.id === 'renameZone') { ev.preventDefault(); saveZoneName(renamingZone); }
});
document.addEventListener('change', (ev) => {
  const id = ev.target.dataset && ev.target.dataset.toggle;
  if (id) toggleItem(id);
});
document.querySelectorAll('.overlay').forEach(o => o.addEventListener('click', e => { if (e.target === o) o.classList.remove('open'); }));

$('expenseForm').addEventListener('submit', (ev) => {
  ev.preventDefault();
  const amount = parseAmount($('expAmount').value);
  const name = $('expName').value.trim() || 'Courses';
  if (isNaN(amount) || amount <= 0) { toast('Indique un montant'); $('expAmount').focus(); return; }
  addExpense(name, Math.round(amount * 100) / 100, selectedCat);
  closeSheet('expenseSheet');
});
$('addItemForm').addEventListener('submit', async (ev) => {
  ev.preventDefault();
  const inp = $('itemName'); const name = inp.value.trim();
  if (!name) { toast('Écris un article'); return; }
  inp.value = '';
  const ok = await addListItem(name);
  if (!ok && !pendingItem) inp.value = name; else if (ok && IS_FAMILLE) toast('Article ajouté');
  inp.focus();
});

function openNameSheet() {
  $('nameTitle').textContent = local.familyName ? 'Changer de prénom' : 'Bienvenue !';
  $('familyName').value = local.familyName || '';
  openSheet('nameSheet');
  setTimeout(() => $('familyName').focus(), 250);
}
$('nameForm').addEventListener('submit', async (ev) => {
  ev.preventDefault();
  const n = $('familyName').value.trim().slice(0, 30);
  if (!n) { toast('Écris ton prénom'); return; }
  local.familyName = n; saveLocal();
  closeSheet('nameSheet');
  renderList();
  renderIdeas();
  if (pendingItem) {
    const item = pendingItem; pendingItem = null;
    if (await addListItem(item)) toast('Article ajouté');
  } else if (pendingIdea) {
    const idea = pendingIdea; pendingIdea = null;
    await addIdea(idea);
  } else toast(`Salut ${n} !`);
});

function saveSettings() {
  local.budget = parseAmount($('setBudget').value) || 0;
  local.userName = $('setName').value.trim() || 'Maman';
  saveLocal(); renderAll();
  toast('Réglages enregistrés');
}
async function doReset() {
  closeSheet('resetSheet');
  archiveMonth();
  local.lastMonth = monthKey(); saveLocal();
  await sb.from('shopping_lists').delete().eq('list_code', local.listCode);
  shoppingItems = [];
  renderAll();
  toast('Nouveau mois démarré');
}

// ============================================================
// INIT
// ============================================================
async function init() {
  loadLocal();
  if (IS_FAMILLE) {
    // Mode famille : uniquement la liste
    // Mode famille : uniquement la liste, sans navigation
    document.body.classList.add('famille');
    const owner = OWNER || 'la famille';
    $('listTitle').textContent = OWNER ? `Liste de ${OWNER}` : 'Liste partagée';
    document.title = `Liste de ${owner}`;
    $('itemName').placeholder = 'Ajouter un article…';
    $('welcome').hidden = false;
    $('welcomeTxt').textContent = `Ajoute ce qui manque, ${OWNER || 'la personne qui fait les courses'} le verra tout de suite. Tu peux cocher ce qui est déjà pris et retirer ce que tu as ajouté.`;
    $('famSeg').hidden = false;
    $('fridgeBtn').hidden = true; // « Avec ce qu'il me reste » réservé à la propriétaire
    showTab('list');
    if (!local.familyName) setTimeout(openNameSheet, 400);
  } else {
    checkAutoReset();
  }
  await Promise.all([loadShoppingList(), loadMenus(), loadIdeas(), IS_FAMILLE ? null : pLoad()]);
  subscribeRealtime();
  subscribeMenus();
  subscribeIdeas();
  renderIdeas();
  if (IS_FAMILLE) renderMenus();
  renderAll();
  $('loading').style.display = 'none';
}
window.addEventListener('resize', () => { if ($('tab-bilan').classList.contains('active')) renderBilan(); });
init();
</script>
</body>
</html>
