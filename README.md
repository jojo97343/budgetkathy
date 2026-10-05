<!DOCTYPE html>
<html lang="fr" data-theme="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <title>Mon Coach Finance</title>
    <link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --main: #5b5ef4; --main-light: #818cf8; --main-glow: rgba(91,94,244,0.18);
            --bg: #f4f6fb; --bg2: #e8ecf5; --card: #ffffff; --card-border: rgba(0,0,0,0.07);
            --text: #1a1d2e; --text-muted: #6b7280; --text-hint: #9ca3af;
            --danger: #ef4444; --success: #10b981; --warning: #f59e0b;
            --shadow: 0 1px 3px rgba(0,0,0,0.07), 0 4px 16px rgba(0,0,0,0.04);
            --shadow-hover: 0 4px 24px rgba(91,94,244,0.12);
            --radius: 16px; --radius-sm: 10px; --transition: 0.22s cubic-bezier(.4,0,.2,1);
            --nav-h: 68px; --header-h: 64px;
        }
        [data-theme="dark"] {
            --bg: #111218; --bg2: #1c1e2b; --card: #1c1e2b; --card-border: rgba(255,255,255,0.07);
            --text: #e8eaf2; --text-muted: #9ca3af; --text-hint: #6b7280;
            --shadow: 0 1px 3px rgba(0,0,0,0.3), 0 4px 16px rgba(0,0,0,0.2);
            --shadow-hover: 0 4px 24px rgba(91,94,244,0.2);
        }
        * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
        html, body { font-family: 'Manrope', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; overscroll-behavior: none; }
        .mobile-nav { display: none; position: fixed; bottom: 0; left: 0; right: 0; background: var(--card); border-top: 1px solid var(--card-border); z-index: 100; padding: 0 4px; padding-bottom: env(safe-area-inset-bottom); }
        .mobile-nav-inner { display: flex; height: 60px; align-items: stretch; }
        .nav-btn { flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 3px; background: none; border: none; cursor: pointer; padding: 6px 4px; border-radius: 12px; transition: color var(--transition); color: var(--text-muted); font-family: 'Manrope', sans-serif; }
        .nav-btn.active { color: var(--main); }
        .nav-btn svg { width: 22px; height: 22px; }
        .nav-btn span { font-size: 9px; font-weight: 500; letter-spacing: 0.01em; white-space: nowrap; }
        .nav-btn svg  { width: 20px; height: 20px; }

        /* ── COUPLE ── */
        .couple-budget-bar { background:var(--bg2); border-radius:12px; height:14px; overflow:hidden; margin:10px 0 4px; }
        .couple-budget-fill { height:100%; border-radius:12px; background:var(--main); transition:width 0.7s cubic-bezier(.4,0,.2,1); }
        .couple-budget-fill.over { background:var(--danger); }
        .couple-cat-grid { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-bottom:16px; }
        .couple-cat-box { background:var(--bg); border:1px solid var(--bg2); border-radius:12px; padding:12px; }
        .couple-cat-name { font-size:0.78rem; font-weight:700; color:var(--text-muted); text-transform:uppercase; letter-spacing:0.06em; margin-bottom:6px; }
        .couple-cat-amounts { display:flex; justify-content:space-between; font-size:0.85rem; margin-bottom:6px; }
        .couple-cat-bar { background:var(--bg2); border-radius:6px; height:6px; overflow:hidden; }
        .couple-cat-fill { height:100%; border-radius:6px; background:var(--main); }
        .couple-cat-fill.over { background:var(--danger); }
        .couple-dep-row { display:flex; justify-content:space-between; align-items:center; padding:10px 0; border-bottom:1px solid var(--bg2); font-size:0.84rem; }
        .couple-dep-row:last-child { border-bottom:none; }
        .couple-dep-who { font-size:0.7rem; font-weight:700; padding:2px 8px; border-radius:20px; }
        .who-moi  { background:rgba(91,94,244,0.12); color:var(--main); }
        .who-elle { background:rgba(236,72,153,0.12); color:#db2777; }
        .couple-session-badge { display:inline-flex; align-items:center; gap:6px; background:var(--bg2); border-radius:20px; padding:4px 12px; font-size:0.78rem; font-weight:600; color:var(--text-muted); cursor:pointer; }
        .sync-dot { width:8px; height:8px; border-radius:50%; background:#10b981; animation:pulse 1.5s infinite; }
        @keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:0.4;} }
        .sync-dot.off { background:#ef4444; animation:none; }
        /* ── ARCHIVES COUPLE ── */
        .couple-arc-card { background:var(--bg); border:1px solid var(--bg2); border-radius:12px; padding:14px; margin-bottom:10px; }
        .couple-arc-card:last-child { margin-bottom:0; }
        .couple-arc-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:8px; }
        .couple-arc-nom { font-weight:700; font-size:0.9rem; }
        .couple-arc-date { font-size:0.7rem; color:var(--text-muted); }
        .couple-arc-stats { display:grid; grid-template-columns:1fr 1fr 1fr; gap:8px; margin-top:8px; }
        .couple-arc-stat { text-align:center; background:var(--card); border:1px solid var(--bg2); border-radius:8px; padding:8px; }
        .couple-arc-stat .lbl { font-size:0.65rem; color:var(--text-muted); text-transform:uppercase; letter-spacing:0.06em; }
        .couple-arc-stat .val { font-size:0.95rem; font-weight:700; margin-top:3px; }
        .mobile-header { display: none; position: sticky; top: 0; height: var(--header-h); background: var(--card); border-bottom: 1px solid var(--card-border); z-index: 90; align-items: center; justify-content: space-between; padding: 0 20px; padding-top: env(safe-area-inset-top); }
        .mobile-header-title { font-family: 'Manrope', serif; font-size: 1.1rem; color: var(--text); }
        .mobile-header-actions { display: flex; gap: 8px; align-items: center; }
        .desktop-header { display: flex; justify-content: space-between; align-items: center; padding: 28px 32px 0; margin-bottom: 28px; gap: 16px; flex-wrap: wrap; }
        .header-title h1 { font-family: 'Manrope', serif; font-size: 1.9rem; color: var(--text); letter-spacing: -0.01em; }
        .header-title h1 em { font-style: normal; color: var(--main); }
        .header-title p { color: var(--text-muted); font-size: 0.83rem; margin-top: 3px; }
        .header-actions { display: flex; gap: 10px; align-items: center; }
        .container { max-width: 1200px; margin: auto; padding: 0 24px 40px; }
        .page { display: block; }
        .card { background: var(--card); padding: 20px; border-radius: var(--radius); border: 1px solid var(--card-border); box-shadow: var(--shadow); margin-bottom: 16px; }
        .card-title { font-size: 0.75rem; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; color: var(--text-muted); margin-bottom: 14px; display: flex; align-items: center; gap: 8px; }
        .card-title .badge { width: 20px; height: 20px; background: var(--main); color: white; border-radius: 50%; font-size: 0.68rem; display: inline-flex; align-items: center; justify-content: center; }
        .stat-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 20px; }
        .stat-box { background: var(--card); border: 1px solid var(--card-border); border-radius: var(--radius); box-shadow: var(--shadow); padding: 16px; text-align: center; }
        .stat-label { font-size: 0.7rem; color: var(--text-muted); font-weight: 600; text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 6px; }
        .stat-value { font-size: 1.9rem; font-weight: 700; line-height: 1; }
        .stat-value small { font-size: 1.1rem; font-weight: 600; }
        .stat-box-gold { border-color: rgba(245,158,11,0.3); }
        .stat-box-gold .stat-value { color: #b45309; }
        .stat-box-gold .stat-label { color: #d97706; }
        .reset-coffre { font-size: 0.68rem; color: #b45309; background: none; border: 1px solid #d97706; border-radius: 20px; padding: 2px 10px; margin-top: 6px; cursor: pointer; font-family: 'Manrope', sans-serif; width: auto; display: inline-block; transition: all var(--transition); }
        .reset-coffre:hover { background: #fef3c7; }
        .grid-top { display: grid; grid-template-columns: 1fr 2fr; gap: 16px; margin-bottom: 16px; }
        label { font-size: 0.8rem; font-weight: 500; color: var(--text-muted); display: block; margin-bottom: 4px; }
        input[type="text"], input[type="number"], select, textarea { width: 100%; padding: 11px 14px; margin-bottom: 12px; border-radius: var(--radius-sm); border: 1.5px solid var(--bg2); background: var(--bg); color: var(--text); font-family: 'Manrope', sans-serif; font-size: 0.95rem; transition: border-color var(--transition), box-shadow var(--transition); outline: none; -webkit-appearance: none; appearance: none; min-height: 44px; }
        input:focus, select:focus { border-color: var(--main); box-shadow: 0 0 0 3px var(--main-glow); }
        [data-theme="dark"] input, [data-theme="dark"] select { background: var(--bg2); border-color: rgba(255,255,255,0.1); }
        .btn { display: inline-flex; align-items: center; justify-content: center; gap: 6px; padding: 12px 18px; border-radius: var(--radius-sm); border: none; font-family: 'Manrope', sans-serif; font-weight: 600; font-size: 0.9rem; cursor: pointer; transition: all var(--transition); min-height: 44px; width: 100%; }
        .btn-primary { background: var(--main); color: white; }
        .btn-primary:hover { background: #4a4de0; transform: translateY(-1px); }
        .btn-primary:active { transform: scale(0.98); }
        .btn-secondary { background: var(--bg2); color: var(--text-muted); }
        .btn-icon-sm { width: 36px; height: 36px; padding: 0; border-radius: 8px; border: none; background: rgba(239,68,68,0.1); color: var(--danger); font-size: 0.8rem; cursor: pointer; transition: all var(--transition); display: inline-flex; align-items: center; justify-content: center; flex-shrink: 0; }
        .btn-icon-sm:hover { background: var(--danger); color: white; }
        .btn-delete { background: none; border: none; color: var(--danger); cursor: pointer; padding: 6px; font-size: 1rem; transition: transform var(--transition); }
        .btn-delete:hover { transform: scale(1.2); }
        .budget-row { display: flex; gap: 8px; align-items: center; margin-bottom: 8px; }
        .budget-row .cat-label { flex: 1; font-size: 0.88rem; font-weight: 500; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .budget-row input { width: 100px; margin: 0; flex-shrink: 0; }
        .add-cat-box { display: flex; gap: 8px; margin-top: 14px; padding-top: 14px; border-top: 1px dashed var(--bg2); }
        .add-cat-box input { margin: 0; flex: 1; }
        .btn-add-cat { width: 44px; height: 44px; padding: 0; flex-shrink: 0; border-radius: var(--radius-sm); border: none; background: var(--main); color: white; font-size: 1.4rem; cursor: pointer; display: flex; align-items: center; justify-content: center; }
        .plan-summary { margin-top: 14px; padding: 14px; border-radius: 12px; background: var(--bg); border: 1px solid var(--bg2); font-size: 0.85rem; }
        .plan-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 5px; }
        .plan-row:last-child { margin-bottom: 0; }
        .text-danger { color: var(--danger); font-weight: 700; }
        .text-success { color: var(--success); font-weight: 700; }
        .expense-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .expense-grid .field-full { grid-column: span 2; }
        .recurring-row { display: flex; align-items: center; gap: 10px; font-size: 0.85rem; color: var(--text-muted); margin-bottom: 14px; }
        .recurring-row input[type="checkbox"] { width: 18px; height: 18px; accent-color: var(--main); cursor: pointer; margin: 0; flex-shrink: 0; }
        .table-scroll { overflow-x: auto; border-radius: var(--radius-sm); border: 1px solid var(--bg2); -webkit-overflow-scrolling: touch; }
        .table-scroll table { width: 100%; border-collapse: collapse; min-width: 400px; }
        thead th { text-align: left; font-size: 0.72rem; color: var(--text-muted); padding: 10px 14px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; border-bottom: 1.5px solid var(--bg2); background: var(--bg); position: sticky; top: 0; z-index: 2; white-space: nowrap; }
        tbody td { padding: 11px 14px; border-top: 1px solid var(--bg2); font-size: 0.86rem; vertical-align: middle; }
        tbody tr { transition: background var(--transition); }
        tbody tr:hover { background: var(--bg); }
        [data-theme="dark"] thead th { background: var(--bg2); }
        .expense-list-scroll { max-height: 55vh; overflow-y: auto; -webkit-overflow-scrolling: touch; border-radius: var(--radius-sm); border: 1px solid var(--bg2); }
        .expense-list-scroll::-webkit-scrollbar { width: 4px; }
        .expense-list-scroll::-webkit-scrollbar-track { background: transparent; }
        .expense-list-scroll::-webkit-scrollbar-thumb { background: var(--main-light); border-radius: 2px; }
        .expense-list-scroll table { width: 100%; border-collapse: collapse; min-width: 300px; }
        .cat-pill { background: var(--bg2); padding: 2px 10px; border-radius: 20px; font-size: 0.75rem; white-space: nowrap; }
        .recurring-tag { font-size: 0.65rem; color: var(--main); font-weight: 700; margin-left: 4px; background: var(--main-glow); padding: 1px 6px; border-radius: 20px; }
        .status-ok { background: rgba(16,185,129,0.1); color: #059669; border: 1px solid rgba(16,185,129,0.25); padding: 2px 8px; border-radius: 20px; font-size: 0.68rem; font-weight: 700; white-space: nowrap; }
        .status-over { background: rgba(239,68,68,0.1); color: #dc2626; border: 1px solid rgba(239,68,68,0.2); padding: 2px 8px; border-radius: 20px; font-size: 0.68rem; font-weight: 700; white-space: nowrap; }
        .search-wrap { margin-bottom: 10px; }
        .search-wrap input { margin: 0; }
        .progress-wrap { display: flex; align-items: center; gap: 8px; min-width: 100px; }
        .progress-bg { flex: 1; height: 6px; border-radius: 3px; background: var(--bg2); overflow: hidden; }
        .progress-fill { height: 100%; border-radius: 3px; background: var(--main); transition: width 0.7s cubic-bezier(.4,0,.2,1); }
        .progress-fill.over { background: var(--danger); }
        .progress-pct { font-size: 0.72rem; color: var(--text-muted); min-width: 30px; text-align: right; }
        .bilan-layout { display: flex; gap: 24px; align-items: flex-start; flex-wrap: wrap; }
        .bilan-table-side { flex: 2; min-width: 280px; overflow: hidden; }
        .bilan-chart-side { flex: 0 0 260px; }
        .epargne-total { display: flex; justify-content: space-between; align-items: center; margin-top: 12px; padding: 14px 16px; background: rgba(245,158,11,0.06); border: 1px solid rgba(245,158,11,0.25); border-radius: 12px; }
        .epargne-total .label { font-size: 0.85rem; color: #b45309; font-weight: 600; }
        .epargne-total .value { font-size: 1.25rem; font-weight: 700; color: #b45309; }
        .top3-item { display: flex; align-items: center; gap: 12px; padding: 12px 14px; border-radius: 12px; margin-bottom: 8px; border: 1px solid var(--bg2); background: var(--bg); }
        .top3-item:last-child { margin-bottom: 0; }
        .top3-rank { width: 28px; height: 28px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 0.8rem; font-weight: 700; flex-shrink: 0; }
        .top3-rank.r1 { background: rgba(239,68,68,0.15); color: #dc2626; }
        .top3-rank.r2 { background: rgba(245,158,11,0.15); color: #d97706; }
        .top3-rank.r3 { background: rgba(99,102,241,0.15); color: #4f46e5; }
        .top3-info { flex: 1; min-width: 0; }
        .top3-name { font-size: 0.88rem; font-weight: 600; color: var(--text); }
        .top3-detail { font-size: 0.75rem; color: var(--text-muted); margin-top: 2px; }
        .top3-bar-wrap { width: 70px; flex-shrink: 0; }
        .top3-bar-bg { height: 6px; border-radius: 3px; background: var(--bg2); overflow: hidden; }
        .top3-bar-fill { height: 100%; border-radius: 3px; background: var(--danger); }
        .top3-pct { font-size: 0.72rem; color: var(--text-muted); text-align: right; margin-top: 3px; }
        .tendance-box { display: flex; align-items: center; gap: 10px; padding: 11px 14px; border-radius: 11px; background: var(--bg); border: 1px solid var(--bg2); margin-bottom: 8px; }
        .tendance-box:last-child { margin-bottom: 0; }
        .tendance-arrow { font-size: 1.1rem; flex-shrink: 0; }
        .tendance-info { flex: 1; min-width: 0; }
        .tendance-label { font-size: 0.86rem; font-weight: 600; color: var(--text); }
        .tendance-detail { font-size: 0.74rem; color: var(--text-muted); margin-top: 2px; }
        .tendance-badge { font-size: 0.72rem; font-weight: 700; padding: 3px 9px; border-radius: 20px; flex-shrink: 0; white-space: nowrap; }
        .tendance-up   { background: rgba(239,68,68,0.1);  color: #dc2626; }
        .tendance-down { background: rgba(16,185,129,0.1); color: #059669; }
        .tendance-flat { background: rgba(107,114,128,0.1); color: #6b7280; }
        .archives-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 14px; }
        .archive-card { background: var(--bg); border: 1px solid var(--bg2); border-radius: 14px; padding: 16px; }
        .arc-name { font-weight: 700; font-size: 0.9rem; margin-bottom: 2px; }
        .arc-date { font-size: 0.7rem; color: var(--text-muted); margin-bottom: 12px; }
        .arc-chart-wrap { display: flex; justify-content: center; margin-bottom: 12px; }
        .arc-stats { font-size: 0.78rem; color: var(--text-muted); }
        .arc-stat-row { display: flex; justify-content: space-between; align-items: center; padding: 4px 0; border-bottom: 1px solid var(--bg2); }
        .arc-stat-row:last-child { border-bottom: none; }
        .arc-stat-row strong { color: var(--text); font-weight: 600; }
        .arc-actions { display: flex; gap: 8px; margin-top: 12px; }
        .arc-actions .btn { flex: 1; padding: 8px 10px; font-size: 0.78rem; border-radius: 8px; min-height: 36px; }
        .btn-pdf { background: var(--main); color: white; }
        .btn-pdf:hover { background: #4a4de0; }
        .btn-del { background: var(--bg2); color: var(--text-muted); }
        .btn-del:hover { background: rgba(239,68,68,0.1); color: var(--danger); }
        .empty-state { text-align: center; padding: 48px 20px; color: var(--text-muted); }
        .empty-icon { font-size: 2.5rem; margin-bottom: 10px; }
        .empty-state p { font-size: 0.85rem; line-height: 1.6; }
        .chart-title { font-size: 0.72rem; font-weight: 700; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 12px; }
        .evolution-wrap { position: relative; height: 220px; }
        .theme-toggle { width: 52px; height: 28px; background: var(--bg2); border-radius: 999px; border: 2px solid var(--card-border); cursor: pointer; position: relative; transition: background var(--transition); flex-shrink: 0; }
        .theme-toggle::after { content: '☀️'; position: absolute; top: 1px; left: 2px; width: 22px; height: 22px; background: white; border-radius: 50%; font-size: 12px; display: flex; align-items: center; justify-content: center; transition: transform var(--transition); text-align: center; line-height: 22px; }
        [data-theme="dark"] .theme-toggle::after { content: '🌙'; transform: translateX(24px); }
        .btn-cloture { background: var(--bg2); color: var(--text-muted); padding: 10px 18px; border-radius: var(--radius-sm); border: none; font-family: 'Manrope', sans-serif; font-weight: 600; font-size: 0.85rem; cursor: pointer; transition: all var(--transition); width: auto; min-height: 44px; white-space: nowrap; }
        .btn-cloture:hover { background: var(--main); color: white; }
        .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); backdrop-filter: blur(6px); z-index: 200; display: flex; align-items: center; justify-content: center; padding: 20px; }
        .modal-box { background: var(--card); border: 1px solid var(--card-border); border-radius: var(--radius); padding: 28px; width: 100%; max-width: 420px; box-shadow: 0 24px 64px rgba(0,0,0,0.25); }
        .modal-box h3 { font-family: 'Manrope', serif; font-size: 1.3rem; margin-bottom: 8px; }
        .modal-box p { color: var(--text-muted); font-size: 0.85rem; margin-bottom: 20px; line-height: 1.6; }
        .modal-actions { display: flex; gap: 10px; margin-top: 4px; }
        .modal-actions .btn { flex: 1; margin: 0; }
        .btn-cancel { background: var(--bg2); color: var(--text-muted); }
        #toast-container { position: fixed; top: 16px; right: 16px; z-index: 9999; display: flex; flex-direction: column; gap: 8px; pointer-events: none; max-width: calc(100vw - 32px); }
        .toast { padding: 12px 18px; border-radius: 12px; font-size: 0.85rem; font-weight: 600; color: white; box-shadow: 0 8px 24px rgba(0,0,0,0.2); display: flex; align-items: center; gap: 8px; pointer-events: auto; max-width: 360px; animation: toastIn 0.3s ease; }
        .toast.success { background: #10b981; } .toast.danger { background: #ef4444; } .toast.info { background: var(--main); } .toast.warning { background: #f59e0b; }
        @keyframes toastIn  { from { opacity:0; transform:translateX(40px); } to { opacity:1; transform:translateX(0); } }
        @keyframes toastOut { from { opacity:1; } to { opacity:0; transform:translateX(40px); } }
        .mobile-fab { display: none; position: fixed; bottom: calc(var(--nav-h) + env(safe-area-inset-bottom) + 12px); right: 16px; width: 52px; height: 52px; background: var(--main); color: white; border: none; border-radius: 50%; font-size: 1.4rem; cursor: pointer; z-index: 80; align-items: center; justify-content: center; box-shadow: 0 4px 20px var(--main-glow); transition: all var(--transition); }
        .mobile-fab:active { transform: scale(0.95); }
        .mobile-stat-row { display: none; gap: 10px; margin-bottom: 14px; }
        .mobile-stat-row .stat-box { flex: 1; padding: 12px 10px; }
        .mobile-stat-row .stat-value { font-size: 1.4rem; }

        @media (max-width: 768px) {
            :root { --nav-h: 60px; --header-h: 54px; }
            .desktop-header { display: none !important; }
            .mobile-header { display: flex; }
            .mobile-nav { display: block; }
            .mobile-fab { display: flex; }
            html { overflow-x: clip; }
            body { padding-bottom: calc(var(--nav-h) + env(safe-area-inset-bottom)); }
            .container { padding: 10px 12px calc(var(--nav-h) + env(safe-area-inset-bottom) + 16px); max-width: 100%; }
            #page-bilan .card { padding: 14px 0 14px 0; overflow: hidden; }
            #page-bilan .card .card-title { padding: 0 14px; }
            #page-bilan .bilan-layout { flex-direction: column; gap: 0; }
            #page-bilan .bilan-table-side { width: 100%; min-width: 0; }
            #page-bilan .table-scroll { overflow-x: auto; overflow-y: visible; -webkit-overflow-scrolling: touch; border-radius: 0; border-left: none; border-right: none; margin: 0; width: 100%; }
            #page-bilan .table-scroll table { min-width: 520px; width: max-content; }
            #page-bilan thead th { padding: 9px 14px; font-size: 0.7rem; }
            #page-bilan tbody td { padding: 10px 14px; font-size: 0.84rem; }
            #page-bilan .bilan-chart-side { width: 100%; flex: none; display: flex; justify-content: center; padding: 14px 14px 0; }
            #page-bilan .bilan-chart-side canvas { max-width: 180px !important; max-height: 180px !important; }
            .page { display: none; }
            .page.active { display: block; }
            .stat-grid { display: none; }
            .mobile-stat-row { display: flex; gap: 8px; }
            .mobile-stat-row .stat-box { padding: 10px 8px; border-radius: 12px; min-width: 0; }
            .mobile-stat-row .stat-value { font-size: 1.25rem; }
            .mobile-stat-row .stat-label { font-size: 0.6rem; }
            .card { border-radius: 14px; padding: 14px; margin-bottom: 12px; }
            .card-title { font-size: 0.7rem; margin-bottom: 12px; }
            .grid-top { grid-template-columns: 1fr; gap: 12px; }
            .expense-grid { grid-template-columns: 1fr; gap: 8px; }
            .expense-grid .field-full { grid-column: span 1; }
            .archives-grid { grid-template-columns: 1fr; gap: 12px; }
            .bilan-layout { flex-direction: column; }
            .bilan-table-side { width: 100%; min-width: 0; }
            .budget-row { gap: 6px; }
            .budget-row input { width: 75px; min-height: 38px; padding: 8px 8px; font-size: 0.88rem; }
            .budget-row .cat-label { font-size: 0.82rem; }
            .btn-icon-sm { width: 32px; height: 32px; font-size: 0.72rem; }
            .table-scroll { margin: 0 -12px; border-radius: 0; border-left: none; border-right: none; -webkit-overflow-scrolling: touch; overflow-x: auto; }
            .table-scroll table { min-width: 340px; }
            thead th { padding: 8px 10px; font-size: 0.68rem; }
            tbody td { padding: 9px 10px; font-size: 0.82rem; }
            input[type="text"], input[type="number"], select { font-size: 16px; padding: 10px 12px; min-height: 44px; }
            .modal-overlay { align-items: flex-end; padding: 0; }
            .modal-box { border-radius: 20px 20px 0 0; position: relative; width: 100%; max-width: 100%; padding: 24px 16px; padding-bottom: calc(20px + env(safe-area-inset-bottom)); }
            #toast-container { top: auto; bottom: calc(var(--nav-h) + env(safe-area-inset-bottom) + 8px); right: 10px; left: 10px; }
            .toast { max-width: 100%; font-size: 0.82rem; padding: 10px 14px; }
            .mobile-fab { bottom: calc(var(--nav-h) + env(safe-area-inset-bottom) + 10px); right: 14px; width: 48px; height: 48px; font-size: 1.2rem; }
            .plan-summary { padding: 12px; font-size: 0.82rem; }
            .epargne-total { padding: 12px 14px; }
            .epargne-total .value { font-size: 1.1rem; }
            .recurring-row { font-size: 0.82rem; gap: 8px; }
            .expense-list-scroll { max-height: 50vh; }
        }
        @media (max-width: 375px) {
            :root { --nav-h: 58px; }
            .container { padding: 8px 10px calc(var(--nav-h) + env(safe-area-inset-bottom) + 12px); }
            .card { padding: 12px; }
            .mobile-stat-row .stat-value { font-size: 1.1rem; }
            .mobile-stat-row .stat-label { font-size: 0.58rem; }
            .nav-btn span { font-size: 9px; }
            .nav-btn svg { width: 20px; height: 20px; }
            .budget-row input { width: 68px; font-size: 0.85rem; }
            thead th { font-size: 0.64rem; padding: 7px 8px; }
            tbody td { padding: 8px; font-size: 0.8rem; }
            .table-scroll table { min-width: 300px; }
        }
        @media (min-width: 769px) {
            .page { display: block !important; }
            .desktop-header { display: flex; }
        }
        /* ── SUGGESTIONS ── */
        .suggestion-item { display:flex; flex-direction:column; gap:5px; padding:14px 14px 14px 18px; border-radius:14px; margin-bottom:8px; background:var(--card); border:1px solid var(--bg2); position:relative; overflow:hidden; }
        .suggestion-item:last-child { margin-bottom:0; }
        .suggestion-item::before { content:''; position:absolute; left:0; top:0; bottom:0; width:4px; border-radius:4px 0 0 4px; }
        .suggestion-item.s-over::before { background:var(--danger); }
        .suggestion-item.s-warn::before { background:var(--warning); }
        .suggestion-item.s-ok::before { background:var(--success); }
        .suggestion-item.s-info::before { background:var(--main); }
        .suggestion-header { display:flex; justify-content:space-between; align-items:center; gap:8px; }
        .suggestion-cat { font-size:0.88rem; font-weight:700; color:var(--text); }
        .suggestion-badge { font-size:0.68rem; font-weight:700; padding:3px 10px; border-radius:20px; white-space:nowrap; flex-shrink:0; }
        .s-over .suggestion-badge { background:rgba(239,68,68,0.1); color:#dc2626; }
        .s-warn .suggestion-badge { background:rgba(245,158,11,0.1); color:#d97706; }
        .s-ok .suggestion-badge { background:rgba(16,185,129,0.1); color:#059669; }
        .s-info .suggestion-badge { background:rgba(91,94,244,0.1); color:var(--main); }
        .suggestion-msg { font-size:0.82rem; color:var(--text-muted); line-height:1.55; }
        .suggestion-msg strong { color:var(--text); font-weight:600; }

        /* ═════════════════════════════════════════
           REFONTE DA — style bancaire épuré
        ═════════════════════════════════════════ */
        :root {
            --main: #4f46e5; --main-light: #818cf8; --main-glow: rgba(79,70,229,0.16);
            --bg: #f2f3f6; --bg2: #e7e9ef; --card: #ffffff; --card-border: rgba(17,19,26,0.08);
            --line: rgba(17,19,26,0.08); --track: rgba(17,19,26,0.07);
            --text: #11131a; --text-muted: #5d6270; --text-hint: #8a8f9b;
            --danger: #c2410c; --success: #0b7a53; --warning: #b45309;
            --shadow: none; --shadow-hover: none; --radius: 20px; --radius-sm: 14px; --nav-h: 68px;
            --soft: color-mix(in srgb, var(--main) 11%, var(--card));
            --soft-text: var(--main);
        }
        [data-theme="dark"] {
            --bg: #0e0f13; --bg2: #24252c; --card: #1a1b21; --card-border: rgba(255,255,255,0.08);
            --line: rgba(255,255,255,0.08); --track: rgba(255,255,255,0.10);
            --text: #eceef3; --text-muted: #9ea3ae; --text-hint: #7a7f8a;
            --danger: #f87171; --success: #34d399; --warning: #fbbf24;
            --soft: color-mix(in srgb, var(--main) 24%, var(--card));
            --soft-text: color-mix(in srgb, var(--main) 50%, #ffffff);
        }
        html, body { font-family: 'Manrope', system-ui, sans-serif; font-variant-numeric: tabular-nums; -webkit-font-smoothing: antialiased; }
        button, input, select, textarea { font-family: 'Manrope', system-ui, sans-serif; }

        /* Cartes & titres */
        .card { border: 1px solid var(--line); box-shadow: none; border-radius: var(--radius); padding: 18px; }
        .card-title { font-size: 15px; font-weight: 800; text-transform: none; letter-spacing: -0.01em; color: var(--text); margin-bottom: 14px; }
        .card-title .badge { background: var(--soft); color: var(--soft-text); width: 24px; height: 24px; font-size: 12px; font-weight: 800; }
        .chart-title { color: var(--text); font-size: 13px; text-transform: none; letter-spacing: 0; font-weight: 800; }
        .stat-box { border: 1px solid var(--line); box-shadow: none; }
        .stat-label { text-transform: none; letter-spacing: 0; font-size: 12px; }
        .stat-value { font-weight: 800; }
        .stat-box-gold { border-color: var(--line); }
        .stat-box-gold .stat-value, .stat-box-gold .stat-label { color: inherit; }
        .reset-coffre { border-color: var(--line); color: var(--text-muted); font-family: 'Manrope', sans-serif; }
        .reset-coffre:hover { background: var(--track); }

        /* Formulaires */
        label { font-size: 13px; font-weight: 700; color: var(--text); margin-bottom: 6px; }
        input[type="text"], input[type="number"], select, textarea { border-radius: 14px; border: 1px solid var(--line); background-color: var(--card); min-height: 50px; font-weight: 600; }
        [data-theme="dark"] input, [data-theme="dark"] select { background-color: var(--card); border-color: var(--line); }
        input:focus, select:focus { border-color: var(--main); box-shadow: 0 0 0 3px var(--main-glow); }
        select { background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 24 24' fill='none' stroke='%238a8f9b' stroke-width='2.5' stroke-linecap='round'%3E%3Cpath d='M6 9l6 6 6-6'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 16px center; padding-right: 40px; }
        #add_mt, #add_mt_d, #edit-mt { font-size: 26px; font-weight: 800; min-height: 62px; }
        .recurring-row { background: var(--bg); border: 1px solid var(--line); border-radius: 14px; padding: 12px 14px; color: var(--text); }
        .recurring-row label { font-weight: 600; }

        /* Boutons */
        .btn { border-radius: 16px; font-weight: 700; min-height: 52px; }
        .btn-primary:hover { background: var(--main); filter: brightness(0.94); transform: none; }
        .btn-secondary, .btn-cancel { background: var(--track); color: var(--text); }
        .btn-add-cat { border-radius: 14px; width: 50px; height: 50px; }
        .btn-icon-sm { border-radius: 10px; background: var(--track); color: var(--text-muted); }
        .btn-icon-sm:hover { background: var(--danger); color: #fff; }
        .btn-delete { color: var(--text-hint); font-size: 0.85rem; }
        .btn-delete:hover { color: var(--danger); transform: none; }
        .btn-cloture { background: var(--card); border: 1px solid var(--line); color: var(--text); font-family: 'Manrope', sans-serif; }
        .btn-pdf { background: var(--main); }
        .btn-pdf:hover { background: var(--main); filter: brightness(0.94); }
        .btn-del { background: var(--track); color: var(--text-muted); }
        .link-btn { background: none; border: none; color: var(--soft-text); font-weight: 800; font-size: 13px; cursor: pointer; padding: 4px 0; }

        /* Blocs secondaires */
        .plan-summary, .epargne-total, .top3-item, .tendance-box, .archive-card, .couple-cat-box, .couple-arc-card { background: var(--bg); border: 1px solid var(--line); border-radius: 16px; }
        .epargne-total .label, .epargne-total .value { color: var(--text); }
        .couple-arc-stat { background: var(--card); border-color: var(--line); }
        .couple-cat-name, .couple-arc-stat .lbl { text-transform: none; letter-spacing: 0; }
        .couple-session-badge { background: var(--card); border: 1px solid var(--line); }
        .suggestion-item { border-color: var(--line); border-radius: 16px; }
        .empty-icon { display: none; }

        /* Tableaux */
        .table-scroll, .expense-list-scroll { border: 1px solid var(--line); border-radius: 16px; }
        thead th, [data-theme="dark"] thead th { background: var(--card); font-size: 11px; letter-spacing: 0.04em; border-bottom: 1px solid var(--line); }
        tbody td { border-top: 1px solid var(--line); }
        tbody tr:hover { background: var(--bg); }

        /* Pastilles & barres */
        .cat-pill, .recurring-tag { background: var(--soft); color: var(--soft-text); font-weight: 700; }
        .status-ok { background: color-mix(in srgb, var(--success) 12%, transparent); color: var(--success); border: none; }
        .status-over { background: color-mix(in srgb, var(--danger) 12%, transparent); color: var(--danger); border: none; }
        .progress-bg, .couple-budget-bar, .couple-cat-bar, .top3-bar-bg { background: var(--track); }
        .tendance-up { color: var(--danger); } .tendance-down { color: var(--success); }

        /* Modales & notifications */
        .modal-overlay { background: rgba(10,11,15,0.45); }
        .modal-box { border-radius: 24px; border: 1px solid var(--line); }
        .modal-box h3 { font-family: 'Manrope', sans-serif; font-weight: 800; letter-spacing: -0.01em; }
        .header-title h1 { font-family: 'Manrope', sans-serif; font-weight: 800; letter-spacing: -0.02em; }
        .toast { border-radius: 14px; }
        .toast.success { background: #0b7a53; } .toast.danger { background: #c2410c; } .toast.warning { background: #b45309; }
        .sync-dot { background: #0b7a53; }

        /* En-tête mobile */
        .mobile-header { background: var(--bg); border-bottom: none; height: auto; padding: 12px 16px 8px; padding-top: calc(12px + env(safe-area-inset-top)); }
        .mh-left { display: flex; align-items: center; gap: 12px; min-width: 0; }
        .mh-avatar { width: 42px; height: 42px; border-radius: 50%; background: var(--soft); color: var(--soft-text); display: flex; align-items: center; justify-content: center; font-weight: 800; font-size: 16px; flex-shrink: 0; }
        .mh-text { display: flex; flex-direction: column; gap: 1px; min-width: 0; }
        .mh-sub { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        .mobile-header-title { font-family: 'Manrope', sans-serif; font-size: 17px; font-weight: 800; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .mh-actions { display: flex; align-items: center; gap: 10px; flex-shrink: 0; }
        .mh-btn { width: 44px; height: 44px; border-radius: 50%; border: 1px solid var(--line); background: var(--card); color: var(--text); display: flex; align-items: center; justify-content: center; cursor: pointer; flex-shrink: 0; }

        /* Barre de navigation */
        .mobile-nav { background: var(--card); border-top: 1px solid var(--line); padding: 0 8px; padding-bottom: env(safe-area-inset-bottom); }
        .mobile-nav-inner { height: 68px; align-items: center; }
        .nav-btn { color: var(--text-muted); gap: 4px; height: 100%; }
        .nav-btn span { font-size: 11px; font-weight: 600; }
        .nav-btn svg { width: 22px; height: 22px; }
        .nav-btn.active { color: var(--soft-text); }
        .nav-btn.active span { font-weight: 800; }
        .nav-fab { flex: 1; display: flex; align-items: center; justify-content: center; background: none; border: none; cursor: pointer; padding: 0; }
        .nav-fab > span { width: 54px; height: 54px; border-radius: 20px; background: var(--main); color: #fff; display: flex; align-items: center; justify-content: center; transition: transform var(--transition); }
        .nav-fab:active > span { transform: scale(0.95); }
        .nav-fab svg { width: 26px; height: 26px; }

        /* Menu « Plus » */
        .sheet-overlay { position: fixed; inset: 0; background: rgba(10,11,15,0.45); z-index: 150; display: flex; align-items: flex-end; justify-content: center; }
        .sheet { background: var(--card); width: 100%; max-width: 520px; border-radius: 24px 24px 0 0; padding: 10px 16px calc(20px + env(safe-area-inset-bottom)); animation: sheetIn .25s ease; }
        @keyframes sheetIn { from { transform: translateY(40px); opacity: 0; } to { transform: none; opacity: 1; } }
        .sheet-handle { width: 40px; height: 5px; border-radius: 999px; background: var(--track); margin: 0 auto 14px; }
        .sheet-title { font-size: 17px; font-weight: 800; margin: 0 4px 14px; }
        .sheet-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 10px; }
        .sheet-item { display: flex; flex-direction: column; align-items: center; gap: 8px; padding: 16px 6px; border-radius: 18px; background: var(--bg); border: 1px solid var(--line); color: var(--text); font-size: 12px; font-weight: 700; cursor: pointer; text-align: center; }
        .sheet-item svg { width: 22px; height: 22px; color: var(--soft-text); }
        .sheet-item.active { border-color: var(--main); }

        /* Accueil : carte solde */
        .hero { background: var(--main); color: #fff; border-radius: 24px; padding: 22px; margin-bottom: 18px; display: flex; flex-direction: column; gap: 6px; }
        .hero-top { display: flex; justify-content: space-between; align-items: center; gap: 8px; }
        .hero-label { font-size: 13px; font-weight: 600; color: rgba(255,255,255,0.88); }
        .hero-badge { font-size: 11px; font-weight: 800; background: rgba(255,255,255,0.22); padding: 4px 10px; border-radius: 999px; }
        .hero-amount { font-size: 40px; font-weight: 800; letter-spacing: -0.02em; line-height: 1.05; }
        .hero-sub { font-size: 13px; color: rgba(255,255,255,0.88); }
        .hero-stats { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 8px; margin-top: 12px; }
        .hero-stat { display: flex; flex-direction: column; gap: 3px; padding: 10px 12px; border-radius: 14px; background: rgba(255,255,255,0.14); min-width: 0; }
        .hs-l { font-size: 11px; font-weight: 600; color: rgba(255,255,255,0.9); }
        .hs-v { font-size: 14px; font-weight: 800; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .hero-reset { align-self: flex-start; margin-top: 4px; background: rgba(255,255,255,0.18); border: none; color: #fff; font-size: 10px; font-weight: 700; padding: 3px 8px; border-radius: 999px; cursor: pointer; }

        /* Accueil : actions rapides */
        .qa-row { display: flex; gap: 8px; margin-bottom: 18px; }
        .qa { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 8px; background: none; border: none; color: var(--text); font-size: 12px; font-weight: 700; cursor: pointer; padding: 0; min-width: 0; }
        .qa-ico { width: 54px; height: 54px; border-radius: 18px; background: var(--card); border: 1px solid var(--line); color: var(--soft-text); display: flex; align-items: center; justify-content: center; }
        .qa-ico svg { width: 22px; height: 22px; }

        /* Accueil : blocs */
        .row-between { display: flex; justify-content: space-between; align-items: baseline; gap: 8px; }
        .sec-title { font-size: 15px; font-weight: 800; }
        .sec-head { display: flex; justify-content: space-between; align-items: baseline; margin: 22px 4px 10px; }
        .muted-sm { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        .bar { height: 8px; border-radius: 999px; background: var(--track); overflow: hidden; margin: 12px 0 8px; }
        .bar-fill { height: 100%; border-radius: 999px; background: var(--main); transition: width .7s cubic-bezier(.4,0,.2,1); }
        .proj-grid { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); border: 1px solid var(--line); border-radius: 20px; background: var(--card); overflow: hidden; margin-bottom: 12px; }
        .proj-cell { padding: 14px 16px; display: flex; flex-direction: column; gap: 4px; }
        .proj-cell:nth-child(odd) { border-right: 1px solid var(--line); }
        .proj-cell:nth-child(-n+2) { border-bottom: 1px solid var(--line); }
        .pl { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        .pv { font-size: 18px; font-weight: 800; }
        .ep-mini { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 8px; margin-bottom: 10px; }
        .ep-mini > div { background: var(--bg); border: 1px solid var(--line); border-radius: 14px; padding: 10px 12px; }

        /* Liste d'opérations */
        .op-row { display: flex; align-items: center; gap: 12px; padding: 11px 0; border-bottom: 1px solid var(--line); cursor: pointer; }
        .op-row:last-child { border-bottom: none; }
        .op-ico { width: 40px; height: 40px; border-radius: 14px; background: var(--soft); color: var(--soft-text); display: flex; align-items: center; justify-content: center; font-weight: 800; font-size: 14px; flex-shrink: 0; }
        .op-main { flex: 1; min-width: 0; }
        .op-name { font-size: 14px; font-weight: 700; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .op-meta { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
        .op-note { font-size: 12px; color: var(--text-muted); margin-top: 3px; font-style: italic; }
        .op-right { display: flex; align-items: center; gap: 4px; flex-shrink: 0; }
        .op-amt { font-size: 14px; font-weight: 800; white-space: nowrap; }
        .op-amt.pos { color: var(--success); }
        .op-del { width: 30px; height: 30px; border-radius: 10px; border: none; background: transparent; color: var(--text-hint); cursor: pointer; font-size: 12px; }
        .op-del:hover { background: var(--track); color: var(--danger); }


        .mobile-fab { bottom: calc(var(--nav-h) + env(safe-area-inset-bottom) + 14px); right: 16px; }
        .qa.active { color: var(--soft-text); }
        /* Dépense : catégories en boutons */
        .cat-chips { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 14px; }
        .cat-chip { min-height: 40px; padding: 0 16px; border-radius: 999px; border: 1px solid var(--line); background: var(--card); color: var(--text); font-size: 13px; font-weight: 700; cursor: pointer; transition: background var(--transition), color var(--transition), border-color var(--transition); }
        .cat-chip.on { background: var(--main); border-color: var(--main); color: #fff; }

        /* Partenaire */
        :root { --partner: #be185d; }
        .pt-head { display: flex; align-items: center; justify-content: space-between; gap: 12px; margin: 4px 4px 14px; }
        .pt-avatar { background: color-mix(in srgb, var(--partner) 12%, var(--card)); color: var(--partner); }
        [data-theme="dark"] .pt-avatar { color: color-mix(in srgb, var(--partner) 50%, #ffffff); }
        .pt-title { font-size: 17px; font-weight: 800; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .pt-sync { display: flex; align-items: center; gap: 6px; padding: 6px 12px; border-radius: 999px; background: var(--card); border: 1px solid var(--line); font-size: 12px; font-weight: 700; color: var(--text-muted); white-space: nowrap; flex-shrink: 0; }
        .pt-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--text-hint); display: inline-block; }
        .pt-hero { background: var(--partner); }
        .pt-cats { padding-top: 4px; padding-bottom: 4px; }
        .pt-row { padding: 12px 0; display: flex; flex-direction: column; gap: 8px; border-bottom: 1px solid var(--line); }
        .pt-row:last-child { border-bottom: none; }
        .pt-row-top { display: flex; justify-content: space-between; align-items: baseline; gap: 8px; }
        .pt-row-top .n { font-size: 14px; font-weight: 700; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .pt-row-top .v { font-size: 12px; color: var(--text-muted); font-weight: 600; white-space: nowrap; }
        .pt-row-top .v strong { color: var(--text); font-weight: 800; }
        .pt-row-top .v strong.over { color: var(--danger); }

        /* Paramètres (page plein écran) */
        .st-page { position: fixed; inset: 0; z-index: 200; background: var(--bg); overflow-y: auto; -webkit-overflow-scrolling: touch; }
        .st-inner { width: 100%; box-sizing: border-box; max-width: 560px; margin: 0 auto; padding: 16px 16px calc(32px + env(safe-area-inset-bottom)); padding-top: calc(16px + env(safe-area-inset-top)); display: flex; flex-direction: column; }
        .st-inner { align-self: flex-start; flex-shrink: 0; }
        .st-inner > * { flex-shrink: 0; }
        .st-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 18px; }
        .st-title { font-size: 16px; font-weight: 800; }
        .st-profile { display: flex; align-items: center; gap: 14px; padding: 16px; border-radius: 20px; background: var(--card); border: 1px solid var(--line); margin-bottom: 22px; }
        .st-avatar { width: 56px; height: 56px; border-radius: 50%; background: var(--soft); color: var(--soft-text); display: flex; align-items: center; justify-content: center; font-size: 22px; font-weight: 800; flex-shrink: 0; }
        .st-pinfo { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 2px; }
        .st-pname { font-size: 17px; font-weight: 800; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .st-psub { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        #set_widget_code { letter-spacing: 0.08em; color: var(--text); }
        .st-pill { height: 36px; padding: 0 14px; border-radius: 999px; border: 1px solid var(--line); background: var(--bg); color: var(--text); font-size: 12px; font-weight: 700; cursor: pointer; flex-shrink: 0; }
        .st-sec { font-size: 12px; font-weight: 700; color: var(--text-muted); letter-spacing: 0.04em; margin: 0 6px 8px; }
        .st-group { border-radius: 20px; background: var(--card); border: 1px solid var(--line); margin-bottom: 22px; overflow: hidden; }
        .st-row { width: 100%; display: flex; align-items: center; gap: 12px; padding: 10px 14px; min-height: 58px; box-sizing: border-box; border: none; border-bottom: 1px solid var(--line); background: transparent; color: var(--text); text-align: left; cursor: pointer; margin: 0; font-size: inherit; }
        .st-group > :last-child { border-bottom: none; }
        .st-ico { width: 34px; height: 34px; border-radius: 10px; background: var(--soft); color: var(--soft-text); display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
        .st-ico svg { width: 18px; height: 18px; }
        .st-ico.pink { background: color-mix(in srgb, #be185d 12%, var(--card)); color: #be185d; }
        .st-ico.green { background: color-mix(in srgb, #047857 12%, var(--card)); color: #047857; }
        .st-ico.grey { background: var(--track); color: var(--text); }
        [data-theme="dark"] .st-ico.pink { color: #f472b6; } [data-theme="dark"] .st-ico.green { color: #34d399; }
        .st-lbl { flex: 1; font-size: 14px; font-weight: 600; color: var(--text); display: flex; flex-direction: column; gap: 1px; min-width: 0; }
        .st-lbl small { font-size: 12px; font-weight: 500; color: var(--text-muted); }
        .st-row input.st-val, .st-row select.st-val { flex: 0 1 48%; width: auto; min-width: 0; min-height: 0; height: 38px; margin: 0; padding: 0 4px; border: none; background-color: transparent; box-shadow: none; color: var(--text-muted); text-align: right; font-size: 14px; font-weight: 600; }
        .st-row select.st-val { padding-right: 28px; background-position: right 4px center; text-align-last: right; }
        .st-row input.st-val:focus, .st-row select.st-val:focus { box-shadow: none; color: var(--text); }
        .st-code { letter-spacing: 0.08em; text-transform: uppercase; }
        .st-txt { font-size: 14px; color: var(--text-muted); font-weight: 600; }
        .st-chev { width: 16px; height: 16px; color: var(--text-hint); transition: transform var(--transition); flex-shrink: 0; }
        .st-chev.open { transform: rotate(90deg); }
        .st-block { padding: 14px; border-bottom: 1px solid var(--line); display: flex; flex-direction: column; gap: 12px; }
        .st-block > .st-lbl { flex: none; }
        .st-mini { font-size: 12px; font-weight: 700; color: var(--text-muted); }
        .st-seg { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 4px; padding: 4px; border-radius: 14px; background: var(--track); }
        .st-seg button { height: 40px; border-radius: 10px; border: none; background: transparent; color: var(--text); font-size: 13px; font-weight: 700; cursor: pointer; }
        .st-seg button.on { background: var(--card); box-shadow: 0 1px 3px rgba(0,0,0,0.08); }
        .st-swatches { display: grid; grid-template-columns: repeat(8, minmax(0, 1fr)); gap: 8px; }
        .st-swatches > div { width: auto !important; height: auto !important; aspect-ratio: 1; }
        .st-bgs { display: flex; flex-wrap: wrap; gap: 10px; }
        .st-switch { position: relative; display: inline-block; width: 52px; height: 30px; margin: 0; flex-shrink: 0; cursor: pointer; }
        .st-switch input { opacity: 0; width: 0; height: 0; position: absolute; }
        .st-track { position: absolute; inset: 0; border-radius: 999px; background: var(--track); transition: background .25s; }
        .st-thumb { position: absolute; width: 24px; height: 24px; left: 3px; top: 3px; border-radius: 50%; background: #fff; box-shadow: 0 1px 3px rgba(0,0,0,0.25); transition: transform .25s; }
        .st-danger { height: 52px; border-radius: 16px; border: 1px solid color-mix(in srgb, var(--danger) 30%, transparent); background: var(--card); color: var(--danger); font-size: 14px; font-weight: 700; cursor: pointer; }
        .st-foot { text-align: center; font-size: 12px; color: var(--text-muted); margin-top: 16px; }

        /* Accueil : rythme fin de mois */
        .pace { display: flex; flex-direction: column; gap: 4px; }
        .pace-main { font-size: 32px; font-weight: 800; letter-spacing: -0.02em; line-height: 1.1; }
        .pace-main small { font-size: 15px; font-weight: 700; color: var(--text-muted); letter-spacing: 0; }
        .pace-row { display: flex; justify-content: space-between; align-items: center; gap: 8px; margin-top: 12px; padding-top: 12px; border-top: 1px solid var(--line); }
        .pace-rythme { font-size: 15px; font-weight: 800; white-space: nowrap; }

        /* Bilan : total dépensé vs budget */
        .bl-sum { display: flex; flex-direction: column; }
        #page-bilan .card.bl-card { padding: 16px; }
        .bl-top { display: flex; justify-content: space-between; align-items: flex-start; gap: 12px; }
        .bl-amt { font-size: 28px; font-weight: 800; letter-spacing: -0.02em; line-height: 1.15; margin-top: 2px; }
        .bl-amt small { font-size: 14px; font-weight: 700; color: var(--text-muted); letter-spacing: 0; }

        /* Dépense */
        .edit-del { width: 100%; margin-top: 10px; height: 46px; border-radius: 14px; border: 1px solid color-mix(in srgb, var(--danger) 30%, transparent); background: transparent; color: var(--danger); font-size: 14px; font-weight: 700; cursor: pointer; }
        #log_list_tableau .dp-op:last-child { border-bottom: none; }
        .dp-scroll-wrap { position: relative; }
        #expense-scroll { max-height: 460px; overflow-y: auto; -webkit-overflow-scrolling: touch; overscroll-behavior: contain; margin: 0 -4px; padding: 0 4px; }
        #expense-scroll .dp-op:last-child { border-bottom: none; }
        .dp-scroll-btn { position: absolute; right: 4px; bottom: 8px; width: 38px; height: 38px; border-radius: 50%; border: none; background: var(--main); color: #fff; font-size: 17px; font-weight: 800; cursor: pointer; display: flex; align-items: center; justify-content: center; box-shadow: 0 4px 14px var(--main-glow); opacity: 0; pointer-events: none; transition: opacity .2s; z-index: 2; }
        .dp-form { display: flex; flex-direction: column; gap: 14px; }
        .dp-amount { display: flex; flex-direction: column; align-items: center; gap: 4px; padding: 14px 0 18px; border-radius: 18px; background: var(--soft); cursor: text; margin: 0; }
        .dp-amount-l { font-size: 12px; font-weight: 700; color: var(--soft-text); }
        .dp-amount-row { display: flex; align-items: baseline; justify-content: center; gap: 6px; }
        .dp-amount-row input { width: 170px; min-height: 0; height: 60px; margin: 0; padding: 0; border: none !important; background: transparent !important; box-shadow: none !important; text-align: right; font-size: 46px !important; font-weight: 800; letter-spacing: -0.02em; -moz-appearance: textfield; appearance: textfield; }
        .dp-amount-row input::-webkit-outer-spin-button, .dp-amount-row input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
        .dp-amount-row b { font-size: 28px; font-weight: 800; color: var(--soft-text); }
        .dp-field { display: flex; flex-direction: column; gap: 6px; margin: 0; }
        .dp-field > span:first-child { font-size: 13px; font-weight: 700; color: var(--text); }
        .dp-field em { font-style: normal; font-weight: 500; color: var(--text-muted); }
        .dp-field input, .dp-field select { margin: 0 !important; background-color: var(--bg); }
        .dp-select { position: relative; display: block; }
        .dp-select i { position: absolute; left: 14px; top: 50%; width: 10px; height: 10px; margin-top: -5px; border-radius: 50%; background: var(--text-hint); pointer-events: none; z-index: 1; }
        .dp-select select { padding-left: 34px; width: 100%; }
        .dp-rec { display: flex; align-items: center; justify-content: space-between; gap: 12px; padding: 12px 14px; border-radius: 14px; background: var(--bg); border: 1px solid var(--line); cursor: pointer; margin: 0; }
        .dp-rec-txt { display: flex; flex-direction: column; gap: 1px; }
        .dp-rec-txt strong { font-size: 14px; font-weight: 700; }
        .dp-rec-txt small { font-size: 12px; color: var(--text-muted); }
        .dp-rec .st-switch input:checked ~ .st-track { background: var(--main); }
        .dp-rec .st-switch input:checked ~ .st-thumb { transform: translateX(22px); }
        .dp-submit { margin: 0; min-height: 54px; font-size: 15px; font-weight: 800; }
        .dp-hist { padding-bottom: 6px; }
        .dp-search { display: flex; align-items: center; gap: 10px; height: 46px; padding: 0 14px; border-radius: 14px; background: var(--bg); border: 1px solid var(--line); color: var(--text-muted); cursor: text; margin: 0 0 6px; }
        .dp-search input { flex: 1; min-width: 0; min-height: 0; height: 40px; margin: 0; padding: 0; border: none !important; background: transparent !important; box-shadow: none !important; font-size: 14px; font-weight: 500; }
        .dp-day { display: flex; justify-content: space-between; padding: 12px 0 4px; font-size: 12px; font-weight: 700; color: var(--text-muted); }
        .dp-day:first-letter { text-transform: uppercase; }
        .dp-op { display: flex; align-items: center; gap: 12px; padding: 11px 0; border-bottom: 1px solid var(--line); cursor: pointer; }
        .dp-ico { width: 40px; height: 40px; border-radius: 14px; background: color-mix(in srgb, var(--c) 11%, var(--card)); color: var(--c); display: flex; align-items: center; justify-content: center; font-size: 14px; font-weight: 800; flex-shrink: 0; }
        .dp-ico.emoji { font-size: 18px; }
        [data-theme="dark"] .dp-ico { background: color-mix(in srgb, var(--c) 22%, var(--card)); color: color-mix(in srgb, var(--c) 55%, #ffffff); }
        .dp-main { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 2px; }
        .dp-name { font-size: 14px; font-weight: 700; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .dp-meta { font-size: 12px; color: var(--text-muted); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .dp-amt { font-size: 14px; font-weight: 800; white-space: nowrap; }
        .dp-amt.pos { color: var(--success); }
        .dp-hint { text-align: center; font-size: 12px; color: var(--text-muted); margin: 12px 0 8px; }

        /* Budget */
        #page-budget input[type=number]::-webkit-outer-spin-button, #page-budget input[type=number]::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
        #page-budget input[type=number] { -moz-appearance: textfield; appearance: textfield; }
        .bg-hero { gap: 4px; }
        .bg-rev { grid-template-columns: repeat(2, minmax(0, 1fr)); }
        .bg-field { cursor: text; margin: 0; }
        .bg-in { display: flex; align-items: center; gap: 6px; }
        .bg-in input { width: 100%; min-width: 0; min-height: 0; height: 26px; margin: 0; padding: 0; border: none !important; background: transparent !important; box-shadow: none !important; color: #fff; font-size: 17px; font-weight: 800; }
        .bg-in input::placeholder { color: rgba(255,255,255,0.6); }
        .bg-in b { font-size: 14px; font-weight: 700; }
        .bg-rep { display: flex; flex-direction: column; gap: 12px; }
        .bg-rep-top { display: flex; justify-content: space-between; align-items: flex-start; gap: 10px; }
        .bg-reste { font-size: 28px; font-weight: 800; letter-spacing: -0.02em; color: var(--success); line-height: 1.2; }
        .bg-reste.neg { color: var(--danger); }
        .bg-plan { font-size: 16px; font-weight: 800; margin-top: 2px; }
        .bg-stack { display: flex; height: 14px; border-radius: 999px; overflow: hidden; gap: 2px; background: var(--track); }
        .bg-stack > div { height: 100%; transition: width .6s cubic-bezier(.4,0,.2,1); }
        .bg-list { padding-top: 4px; padding-bottom: 14px; }
        .bg-row { display: flex; align-items: center; gap: 12px; padding: 12px 0; border-bottom: 1px solid var(--line); }
        .bg-ico { width: 42px; height: 42px; border-radius: 50%; border: 2px solid var(--c); background: color-mix(in srgb, var(--c) 10%, var(--card)); color: var(--c); box-sizing: border-box; display: flex; align-items: center; justify-content: center; font-size: 15px; font-weight: 800; flex-shrink: 0; }
        .bg-ico.emoji { font-size: 18px; }
        .bg-main { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 2px; }
        .bg-name { font-size: 14px; font-weight: 800; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .bg-sub { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        .bg-amt { display: flex; align-items: center; gap: 4px; height: 40px; padding: 0 12px; border-radius: 12px; background: var(--bg); border: 1px solid var(--line); cursor: text; margin: 0; flex-shrink: 0; }
        .bg-amt input.prev-input { width: 56px; min-height: 0; height: 36px; margin: 0; padding: 0; border: none !important; background: transparent !important; box-shadow: none !important; text-align: right; font-size: 15px; font-weight: 800; }
        .bg-amt b { font-size: 13px; font-weight: 700; color: var(--text-muted); }
        .bg-del { width: 32px; height: 32px; border-radius: 10px; border: none; background: transparent; color: var(--text-hint); display: flex; align-items: center; justify-content: center; cursor: pointer; flex-shrink: 0; }
        .bg-del:hover { color: var(--danger); background: var(--track); }
        .bg-add { display: flex; align-items: center; gap: 10px; padding-top: 12px; }
        .bg-add input { flex: 1; min-width: 0; min-height: 0; height: 44px; margin: 0; border: 1px dashed var(--text-hint) !important; background: transparent !important; }
        .bg-plus { width: 44px; height: 44px; border-radius: 12px; border: none; background: var(--main); color: #fff; display: flex; align-items: center; justify-content: center; cursor: pointer; flex-shrink: 0; }
        .bg-prev { display: flex; align-items: center; gap: 12px; padding: 12px 0; border-bottom: 1px solid var(--line); }
        .bg-dot { width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0; }
        .bg-ok, .bg-x { width: 34px; height: 34px; border-radius: 10px; border: none; display: flex; align-items: center; justify-content: center; cursor: pointer; flex-shrink: 0; }
        .bg-ok { background: color-mix(in srgb, var(--success) 12%, var(--card)); color: var(--success); }
        .bg-x { background: var(--track); color: var(--text-muted); }
        .bg-empty { text-align: center; padding: 16px 0 4px; color: var(--text-muted); font-size: 13px; }
        .bg-form { display: flex; flex-direction: column; gap: 8px; padding-top: 14px; }
        .bg-form-row { display: grid; grid-template-columns: minmax(0, 1.6fr) minmax(0, 1fr); gap: 8px; }
        .bg-form-row2 { display: grid; grid-template-columns: minmax(0, 1fr) auto; gap: 8px; }
        .bg-form input, .bg-form select { margin: 0 !important; min-height: 0; height: 46px; background-color: var(--bg); }
        .bg-addbtn { width: auto; min-height: 0; height: 46px; padding: 0 18px; margin: 0; }
        .bg-transfer { height: 46px; border-radius: 12px; border: 1px solid var(--line); background: var(--card); color: var(--soft-text); font-size: 14px; font-weight: 700; cursor: pointer; margin-top: 4px; }

        /* Archives */
        .ar-card { display: flex; flex-direction: column; gap: 14px; }
        .ar-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; }
        .ar-legend span { display: flex; align-items: center; gap: 6px; font-size: 12px; font-weight: 700; color: var(--text-muted); }
        .ar-legend i { display: inline-block; width: 10px; height: 10px; border-radius: 50%; }
        .ar-chart { position: relative; }
        .ar-tiles { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 8px; }
        .ar-tile { padding: 10px 12px; border-radius: 14px; background: var(--bg); display: flex; flex-direction: column; gap: 3px; min-width: 0; }
        .ar-tile span { font-size: 11px; color: var(--text-muted); font-weight: 600; }
        .ar-tile strong { font-size: 14px; font-weight: 800; white-space: nowrap; }
        .ar-tile.pos { background: color-mix(in srgb, var(--success) 11%, var(--card)); } .ar-tile.pos span, .ar-tile.pos strong { color: var(--success); }
        .ar-tile.neg { background: color-mix(in srgb, var(--danger) 11%, var(--card)); } .ar-tile.neg span, .ar-tile.neg strong { color: var(--danger); }
        .ar-tend { display: flex; flex-direction: column; }
        .ar-tend-h { font-size: 12px; font-weight: 700; color: var(--text-muted); margin: 2px 0 4px; }
        .ar-tend-row { display: flex; align-items: center; gap: 12px; padding: 10px 0; border-bottom: 1px solid var(--line); }
        .ar-tend-row:last-child { border-bottom: none; padding-bottom: 0; }
        .ar-tend-ico { width: 32px; height: 32px; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 15px; font-weight: 800; flex-shrink: 0; background: var(--track); color: var(--text-muted); }
        .ar-tend-ico.up { background: color-mix(in srgb, var(--danger) 12%, var(--card)); color: var(--danger); }
        .ar-tend-ico.down { background: color-mix(in srgb, var(--success) 12%, var(--card)); color: var(--success); }
        .ar-tend-main { flex: 1; min-width: 0; }
        .ar-tend-name { font-size: 14px; font-weight: 700; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .ar-tend-det { font-size: 12px; color: var(--text-muted); margin-top: 1px; }
        .ar-tend-val { font-size: 14px; font-weight: 800; white-space: nowrap; }
        .ar-tend-val.up { color: var(--danger); } .ar-tend-val.down { color: var(--success); } .ar-tend-val.flat { color: var(--text-muted); }
        #archives-container .archives-grid, #archives-container-d .archives-grid { display: flex; flex-direction: column; gap: 12px; }
        #archives-container-d .archives-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); }
        .am-card { background: var(--card); border: 1px solid var(--line); border-radius: 20px; padding: 16px; display: flex; flex-direction: column; gap: 14px; }
        .am-top { display: flex; align-items: center; gap: 14px; }
        .am-ring { width: 64px; height: 64px; border-radius: 50%; display: flex; align-items: center; justify-content: center; flex-shrink: 0; background: var(--track); }
        .am-hole { width: 46px; height: 46px; border-radius: 50%; background: var(--card); display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: 800; }
        .am-info { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 3px; }
        .am-name { font-size: 16px; font-weight: 800; line-height: 1.25; }
        .am-pill { display: flex; margin-top: 3px; }
        .am-sub { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        .am-stats { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 6px; }
        .am-stat { padding: 9px 8px; border-radius: 12px; background: var(--bg); display: flex; flex-direction: column; gap: 2px; min-width: 0; }
        .am-stat span { font-size: 10px; color: var(--text-muted); font-weight: 600; }
        .am-stat strong { font-size: 13px; font-weight: 800; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .am-actions { display: flex; gap: 8px; }
        .am-pdf { flex: 1; height: 42px; border-radius: 12px; border: none; background: var(--soft); color: var(--soft-text); font-size: 13px; font-weight: 700; cursor: pointer; }
        .am-del { width: 42px; height: 42px; border-radius: 12px; border: none; background: var(--track); color: var(--text-muted); display: flex; align-items: center; justify-content: center; cursor: pointer; flex-shrink: 0; }
        .am-del:hover { color: var(--danger); }

        /* Bilan mobile : grand anneau + lignes avec cercles et barres */
        .bd-wrap { display: flex; flex-direction: column; align-items: center; gap: 12px; padding: 6px 0 16px; border-bottom: 1px solid var(--line); margin-bottom: 4px; }
        .bd-donut { position: relative; width: 248px; height: 248px; }
        .bd-donut canvas { position: absolute; inset: 0; width: 100% !important; height: 100% !important; }
        .bd-hole { position: absolute; inset: 0; margin: auto; width: 168px; height: 168px; border-radius: 50%; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 2px; text-align: center; pointer-events: none; }
        .bd-lbl { max-width: 140px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .bd-sub { white-space: pre-line; line-height: 1.35; }
        .bd-amt { font-size: 28px; font-weight: 800; letter-spacing: -0.02em; line-height: 1.15; }
        .bd-hole .bc-pill { margin-top: 6px; }
        .bd-reste { text-align: center; font-weight: 700; }
        .bm-row { display: flex; gap: 12px; align-items: center; padding: 14px 0; border-bottom: 1px solid var(--line); }
        .bm-row:last-child { border-bottom: none; padding-bottom: 2px; }
        .bm-ring { width: 56px; height: 56px; border-radius: 50%; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
        .bm-ico { width: 44px; height: 44px; border-radius: 50%; background: var(--card); display: flex; align-items: center; justify-content: center; font-size: 17px; font-weight: 800; color: var(--c); line-height: 1; }
        .bm-ico.emoji { font-size: 22px; }
        .bm-main { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 8px; }
        .bm-top, .bm-bot { display: flex; justify-content: space-between; align-items: center; gap: 8px; }
        .bm-name { font-size: 14px; font-weight: 800; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .bm-bar { height: 12px; border-radius: 999px; background: color-mix(in srgb, var(--c) 14%, var(--card)); overflow: hidden; }
        .bm-fill { height: 100%; border-radius: 999px; background: var(--c); transition: width .7s cubic-bezier(.4,0,.2,1); }
        .bm-bot { font-size: 12px; font-weight: 600; color: var(--text-muted); }
        .bm-bot strong { color: var(--text); font-weight: 800; }
        .bm-reste { font-weight: 700; color: var(--success); white-space: nowrap; }
        .bm-reste.over { color: var(--danger); } .bm-reste.done { color: var(--text-muted); }
        .bc-pill.done { background: var(--track); color: var(--text-muted); }

        /* Bilan mobile : une carte par catégorie */
        #page-bilan #bilan-card-m { padding: 16px; }
        #page-bilan #bilan-card-m .card-title { padding: 0; }
        .bilan-pie { display: flex; justify-content: center; padding: 4px 0 14px; border-bottom: 1px solid var(--line); margin-bottom: 2px; }
        .bc-row { padding: 14px 0; display: flex; flex-direction: column; gap: 9px; border-bottom: 1px solid var(--line); }
        .bc-row:last-child { border-bottom: none; padding-bottom: 2px; }
        .bc-top, .bc-bot { display: flex; justify-content: space-between; align-items: center; gap: 8px; }
        .bc-name { font-size: 14px; font-weight: 700; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
        .bc-pill { font-size: 11px; font-weight: 700; padding: 4px 10px; border-radius: 999px; white-space: nowrap; flex-shrink: 0; }
        .bc-pill.ok { background: color-mix(in srgb, var(--success) 12%, transparent); color: var(--success); }
        .bc-pill.warn { background: color-mix(in srgb, var(--warning) 14%, transparent); color: var(--warning); }
        .bc-pill.over { background: color-mix(in srgb, var(--danger) 12%, transparent); color: var(--danger); }
        .bc-bar { height: 6px; border-radius: 999px; background: var(--track); overflow: hidden; }
        .bc-fill { height: 100%; border-radius: 999px; background: var(--main); transition: width .7s cubic-bezier(.4,0,.2,1); }
        .bc-fill.warn { background: #d97706; } .bc-fill.over { background: var(--danger); }
        .bc-bot { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        .bc-bot strong { color: var(--text); font-weight: 800; }
        .bc-ecart { font-weight: 800; }
        .bc-ecart.pos { color: var(--success); } .bc-ecart.neg { color: var(--danger); }
        @media (max-width: 768px) {
            :root { --nav-h: 68px; }
            .container { padding: 6px 16px calc(var(--nav-h) + env(safe-area-inset-bottom) + 20px); }
            .card { padding: 16px; border-radius: 20px; }
            .modal-box { border-radius: 24px 24px 0 0; }
        }
    </style>
</head>
<body>

<div id="toast-container"></div>

<div id="modal-edit-depense" class="modal-overlay" style="display:none;">
    <div class="modal-box">
        <h3>✏️ Modifier la dépense</h3>
        <input type="hidden" id="edit-id">
        <label>Objet</label>
        <input type="text" id="edit-desc" placeholder="Nom de la dépense">
        <label>Montant (€)</label>
        <input type="number" inputmode="decimal" id="edit-mt" placeholder="0.00">
        <label>Catégorie</label>
        <select id="edit-cat"></select>
        <label style="margin-top:4px;">Note <span style="font-weight:400;color:var(--text-hint);">(optionnel)</span></label>
        <input type="text" id="edit-note" placeholder="Ex: remboursement prévu...">
        <div class="recurring-row" style="margin-top:4px;">
            <input type="checkbox" id="edit-recurring">
            <label for="edit-recurring" style="margin:0;cursor:pointer;">Dépense récurrente</label>
        </div>
        <div class="modal-actions">
            <button class="btn btn-cancel" onclick="fermerEditDepense()">Annuler</button>
            <button class="btn btn-primary" onclick="sauvegarderEditDepense()">💾 Enregistrer</button>
        </div>
        <button class="edit-del" onclick="supprimerDepuisEdit()">Supprimer cette dépense</button>
    </div>
</div>

<!-- ════════ PAGE PARAMÈTRES ════════ -->
<div id="modal-settings" class="st-page" style="display:none;">
    <div class="st-inner">
        <div class="st-top">
            <button class="mh-btn" aria-label="Retour" onclick="fermerSettings()">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 18l-6-6 6-6"/></svg>
            </button>
            <span class="st-title">Paramètres</span>
            <span style="width:44px;"></span>
        </div>

        <div class="st-profile">
            <div class="st-avatar" id="st-avatar">M</div>
            <div class="st-pinfo">
                <span class="st-pname" id="st-pname">Mon Coach Finance</span>
                <span class="st-psub">Code widget · <span id="set_widget_code">—</span></span>
            </div>
            <button class="st-pill" onclick="copierCodeWidget()">Copier</button>
        </div>

        <div class="st-sec">PROFIL</div>
        <div class="st-group">
            <label class="st-row" for="set_prenom">
                <span class="st-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 21v-1a7 7 0 0 1 16 0v1"/></svg></span>
                <span class="st-lbl">Prénom</span>
                <input class="st-val" type="text" id="set_prenom" placeholder="Ex : Enzo" onchange="autoSaveSettings()">
            </label>
            <label class="st-row" for="set_nom_moi">
                <span class="st-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg></span>
                <span class="st-lbl">Ton nom (Couple)</span>
                <input class="st-val" type="text" id="set_nom_moi" placeholder="Moi" onchange="autoSaveSettings()">
            </label>
            <label class="st-row" for="set_nom_partner">
                <span class="st-ico pink"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg></span>
                <span class="st-lbl">Partenaire</span>
                <input class="st-val" type="text" id="set_nom_partner" placeholder="Son prénom" onchange="autoSaveSettings()">
            </label>
            <label class="st-row" for="set_code_partner">
                <span class="st-ico pink"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="8" cy="15" r="4"/><path d="M10.85 12.15 19 4"/><path d="m18 5 2 2"/><path d="m15 8 2 2"/></svg></span>
                <span class="st-lbl">Code de sa synchro</span>
                <input class="st-val st-code" type="text" id="set_code_partner" placeholder="Aucun" oninput="this.value=this.value.toUpperCase()" onchange="autoSaveSettings()">
            </label>
        </div>

        <div class="st-sec">BUDGET</div>
        <div class="st-group">
            <label class="st-row" for="set_jour_debut">
                <span class="st-ico green"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="17" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg></span>
                <span class="st-lbl">Début du cycle</span>
                <select class="st-val" id="set_jour_debut" onchange="autoSaveSettings()"></select>
            </label>
            <label class="st-row" for="set_devise">
                <span class="st-ico green"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 7a7 7 0 1 0 0 10"/><path d="M4 10h9M4 14h9"/></svg></span>
                <span class="st-lbl">Devise</span>
                <select class="st-val" id="set_devise" onchange="autoSaveSettings()">
                    <option value="€">Euro (€)</option>
                    <option value="$">Dollar ($)</option>
                    <option value="£">Livre (£)</option>
                    <option value="CHF">Franc suisse (CHF)</option>
                    <option value="MAD">Dirham (MAD)</option>
                    <option value="XOF">Franc CFA (XOF)</option>
                </select>
            </label>
        </div>

        <div class="st-sec">APPARENCE</div>
        <div class="st-group">
            <div class="st-block">
                <span class="st-lbl">Mode</span>
                <div class="st-seg">
                    <button id="btn_theme_light" onclick="appliquerModeTheme('light')">Clair</button>
                    <button id="btn_theme_dark" onclick="appliquerModeTheme('dark')">Sombre</button>
                </div>
            </div>
            <div class="st-block">
                <span class="st-lbl">Couleur principale</span>
                <div id="theme-swatches" class="st-swatches"></div>
            </div>
            <button class="st-row" onclick="toggleFondsSettings()">
                <span class="st-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="3"/><path d="M3 15l5-5 4 4 3-3 6 6"/></svg></span>
                <span class="st-lbl">Fond</span>
                <span class="st-txt" id="st-fond-val">—</span>
                <svg class="st-chev" id="st-fond-chev" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18l6-6-6-6"/></svg>
            </button>
            <div class="st-block" id="st-fonds" style="display:none;">
                <span class="st-mini">Mode clair</span>
                <div id="bg-swatches-light" class="st-bgs"></div>
                <span class="st-mini">Mode sombre</span>
                <div id="bg-swatches-dark" class="st-bgs"></div>
            </div>
        </div>

        <div class="st-sec">ONGLETS</div>
        <div class="st-group">
            <div class="st-row">
                <span class="st-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg></span>
                <span class="st-lbl">Couple<small>Budget commun synchronisé</small></span>
                <label class="st-switch" aria-label="Afficher l'onglet Couple">
                    <input type="checkbox" id="set_mode_couple" onchange="previewCoupleToggle(this.checked);autoSaveSettings()">
                    <span id="couple-toggle-track" class="st-track"></span>
                    <span id="couple-toggle-thumb" class="st-thumb"></span>
                </label>
            </div>
            <div class="st-row">
                <span class="st-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg></span>
                <span class="st-lbl">Vacances<small>Séjour avec conversion de devises</small></span>
                <label class="st-switch" aria-label="Activer le mode Vacances">
                    <input type="checkbox" id="set_mode_vacances" onchange="previewVacancesToggle(this.checked);autoSaveSettings()">
                    <span id="vac-toggle-track" class="st-track"></span>
                    <span id="vac-toggle-thumb" class="st-thumb"></span>
                </label>
            </div>
        </div>

        <div class="st-sec">DONNÉES</div>
        <div class="st-group">
            <button class="st-row" onclick="exporterDonnees()">
                <span class="st-ico grey"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 15V3"/><path d="M7 8l5-5 5 5"/><path d="M5 21h14"/></svg></span>
                <span class="st-lbl">Exporter mes données</span>
                <span class="st-txt">JSON</span>
            </button>
            <label class="st-row" for="import_file">
                <span class="st-ico grey"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3v12"/><path d="M7 10l5 5 5-5"/><path d="M5 21h14"/></svg></span>
                <span class="st-lbl">Importer des données</span>
                <span class="st-txt">JSON</span>
                <input type="file" id="import_file" accept=".json" onchange="importerDonnees(event)" style="display:none;">
            </label>
        </div>

        <button class="st-danger" onclick="reinitialiserApp()">Réinitialiser l'application</button>
        <p class="st-foot">Mon Coach Finance · les modifications sont enregistrées automatiquement</p>
    </div>
</div>


<div id="modal-cloture" class="modal-overlay" style="display:none;">
    <div class="modal-box">
        <h3>📁 Clôturer le mois</h3>
        <p>Donne un nom à ce mois pour l'archiver avec son graphique. Tu pourras le télécharger en PDF et comparer les mois.</p>
        <label>Nom du mois</label>
        <input type="text" id="modal-mois-nom" placeholder="Ex : Janvier 2025...">
        <div class="modal-actions">
            <button class="btn btn-cancel" onclick="fermerModal()">Annuler</button>
            <button class="btn btn-primary" onclick="confirmerCloture()">✅ Archiver</button>
        </div>
    </div>
</div>

<div class="mobile-header">
    <div class="mh-left">
        <div class="mh-avatar" id="mh-avatar">M</div>
        <div class="mh-text">
            <span class="mh-sub" id="mh-sub">Mon budget</span>
            <span class="mobile-header-title" id="mh-title">Mon Coach Finance</span>
        </div>
    </div>
    <div class="mh-actions">
        <button class="theme-toggle" aria-label="Basculer clair / sombre" onclick="toggleTheme()"></button>
        <button class="mh-btn" aria-label="Paramètres" onclick="ouvrirSettings()">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 1 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.6 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 1 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 1 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-1.51V3a2 2 0 1 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 1 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
    </button>
    </div>
</div>

<nav class="mobile-nav">
    <div class="mobile-nav-inner">
        <button class="nav-btn active" id="nav-tableau" onclick="goTo('tableau')">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10.5 12 3l9 7.5V20a1 1 0 0 1-1 1h-5v-6h-6v6H4a1 1 0 0 1-1-1z"/></svg>
            <span>Accueil</span>
        </button>
        <button class="nav-btn" id="nav-budget" onclick="goTo('budget')">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="6" width="18" height="13" rx="2"/><path d="M3 10h18"/><path d="M16 14.5h2"/></svg>
            <span>Budget</span>
        </button>
        <button class="nav-fab" id="nav-depenses" aria-label="Ajouter une dépense" onclick="goTo('depenses')">
            <span><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg></span>
        </button>
        <button class="nav-btn" id="nav-bilan" onclick="goTo('bilan')">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><path d="M4 20V10"/><path d="M10 20V4"/><path d="M16 20v-7"/><path d="M22 20H2"/></svg>
            <span>Bilan</span>
        </button>
        <button class="nav-btn" id="nav-archives" onclick="goTo('archives')">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><polyline points="21 8 21 21 3 21 3 8"/><rect x="1" y="3" width="22" height="5" rx="1"/><line x1="10" y1="12" x2="14" y2="12"/></svg>
            <span>Archives</span>
        </button>
    </div>
</nav>


<button class="mobile-fab" onclick="ouvrirModal()" aria-label="Clôturer le mois">📁</button>

<div class="desktop-header">
    <div class="header-title">
        <h1>Mon Coach <em>Finance</em></h1>
        <p>Prévu vs Réel — suivi de budget mensuel</p>
    </div>
    <div class="header-actions">
        <button class="theme-toggle" onclick="toggleTheme()"></button>
        <button class="btn-cloture" onclick="afficherCodeWidget()" style="background:var(--bg2);color:var(--text-muted);">📱 Code widget</button>
        <button class="btn-cloture" onclick="ouvrirModal()">📁 Clôturer le mois</button>
    </div>
</div>

<div class="container">

    <div class="stat-grid">
        <div class="stat-box"><div class="stat-label">Dépensé</div><div class="stat-value"><span id="view_total_dep">0</span><small> €</small></div></div>
        <div class="stat-box"><div class="stat-label">Épargné</div><div class="stat-value" style="color:var(--success)"><span id="view_total_ep">0</span><small> €</small></div></div>
        <div class="stat-box"><div class="stat-label">Solde</div><div class="stat-value" id="view_solde_wrapper"><span id="view_solde">0</span><small> €</small></div></div>
        <div class="stat-box stat-box-gold"><div class="stat-label">Coffre-fort</div><div class="stat-value"><span id="view_global_ep">0</span><small> €</small></div><button class="reset-coffre" onclick="resetCoffre()">Reset</button></div>
    </div>

    <div id="page-tableau" class="page active">

        <!-- ── SOLDE (carte principale) ── -->
        <div class="hero" id="hero-card">
            <div class="hero-top">
                <span class="hero-label">Solde du mois</span>
                <span class="hero-badge" id="hero-neg" style="display:none;">Solde négatif</span>
            </div>
            <div class="hero-amount" id="m_solde_wrap"><span id="m_solde">0</span> €</div>
            <div class="hero-sub" id="m_revenu_txt">sur 0.00 € de revenus</div>
            <div class="hero-stats">
                <div class="hero-stat"><span class="hs-l">Dépensé</span><span class="hs-v"><span id="m_dep">0</span> €</span></div>
                <div class="hero-stat"><span class="hs-l">Épargné</span><span class="hs-v"><span id="m_ep">0</span> €</span></div>
                <div class="hero-stat"><span class="hs-l">Coffre</span><span class="hs-v"><span id="m_coffre">0</span> €</span><button class="hero-reset" onclick="resetCoffre()">Reset</button></div>
            </div>
        </div>

        <!-- ── ACTIONS RAPIDES ── -->
        <div class="qa-row">
            <button class="qa" onclick="allerPrevisions()">
                <span class="qa-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="17" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg></span>
                <span>Prévoir</span>
            </button>
            <button class="qa" onclick="ouvrirModal()">
                <span class="qa-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg></span>
                <span>Clôturer</span>
            </button>
            <button class="qa" id="nav-couple" onclick="goTo('couple')" style="display:none;">
                <span class="qa-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg></span>
                <span>Couple</span>
            </button>
            <button class="qa" id="nav-partenaire" onclick="goTo('partenaire')" style="display:none;">
                <span class="qa-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><circle cx="9" cy="8" r="4"/><path d="M2 21v-1a6 6 0 0 1 6-6h2a6 6 0 0 1 6 6v1"/><path d="M17 3.5a4 4 0 0 1 0 8"/><path d="M22 21v-1a6 6 0 0 0-4-5.6"/></svg></span>
                <span id="nav-partner-label">Elle</span>
            </button>
            <button class="qa" id="nav-vacances" onclick="goTo('vacances')" style="display:none;">
                <span class="qa-ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg></span>
                <span id="nav-vacances-label">Vacances</span>
            </button>
        </div>

        <!-- ── BUDGET UTILISÉ ── -->
        <div class="card">
            <div class="row-between">
                <span class="sec-title">Budget utilisé</span>
                <span class="muted-sm" id="m_budget_bar_label">0 € / 0 €</span>
            </div>
            <div class="bar"><div id="m_budget_bar" class="bar-fill" style="width:0%;"></div></div>
            <div class="muted-sm" id="m_budget_pct">0 %</div>
        </div>

        <!-- ── RYTHME FIN DE MOIS ── -->
        <div class="sec-head">
            <span class="sec-title">Fin de mois</span>
            <span class="muted-sm"><span id="m_jours_restants">0</span> jours restants</span>
        </div>
        <div class="card pace">
            <span class="pl">Tu peux dépenser</span>
            <div class="pace-main"><span id="m_budget_jour">0</span> € <small>/ jour</small></div>
            <div class="pace-row">
                <span class="pl">Tu dépenses en moyenne</span>
                <span class="pace-rythme" id="m_rythme_wrap"><span id="m_rythme">0</span> € / jour</span>
            </div>
        </div>

        <!-- ── DERNIÈRES OPÉRATIONS ── -->
        <div class="sec-head">
            <span class="sec-title">Dernières opérations</span>
            <button class="link-btn" onclick="goTo('depenses')">Tout voir</button>
        </div>
        <div class="card">
            <label class="dp-search"><svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="11" cy="11" r="7"/><path d="M20 20l-3.5-3.5"/></svg><input type="text" id="search_input_tableau" placeholder="Rechercher une dépense…" oninput="majAffichage()" aria-label="Rechercher"></label>
            <div id="log_list_tableau" style="max-height:380px;overflow-y:auto;-webkit-overflow-scrolling:touch;"></div>
            <table id="log_table_tableau" style="display:none;"><tbody></tbody></table>
        </div>

        <!-- ── ÉPARGNE ── -->
        <div class="sec-head"><span class="sec-title">Épargne</span></div>
        <div class="card">
            <div class="ep-mini">
                <div><span class="pl">Ce mois</span><div class="pv" style="color:var(--success);"><span id="m_ep_epargne">0</span> €</div></div>
                <div><span class="pl">Coffre-fort total</span><div class="pv"><span id="m_coffre_ep">0</span> €</div></div>
            </div>
            <div id="ep_list_tableau"></div>
            <table id="epargne_history_table" style="display:none;"><tbody></tbody></table>
            <span id="local_epargne_total" style="display:none;">0</span>
        </div>

        <!-- IDs desktop conservés pour compat JS -->
        <div id="proj-global-card" style="display:none;">
            <div id="proj-subtitle"></div>
            <div id="proj-bar-actual"></div>
            <div id="proj-bar-extra"></div>
            <span id="proj-bar-label"></span>
            <span id="proj-bar-max"></span>
        </div>
        <div id="proj-cats-card" style="display:none;"><div id="proj-cats-list"></div></div>
    </div>

    <div id="page-budget" class="page">
        <!-- Revenus -->
        <div class="hero bg-hero">
            <span class="hero-label">Revenus du mois</span>
            <div class="hero-amount"><span id="bg_rev_total">0</span> €</div>
            <div class="hero-stats bg-rev">
                <label class="hero-stat bg-field"><span class="hs-l">Salaire</span><span class="bg-in"><input type="number" inputmode="decimal" id="prev_revenu" placeholder="0" onchange="sauvegarder()" aria-label="Salaire"><b>€</b></span></label>
                <label class="hero-stat bg-field"><span class="hs-l">CAF / aides</span><span class="bg-in"><input type="number" inputmode="decimal" id="prev_caf" placeholder="0" onchange="sauvegarder()" aria-label="CAF"><b>€</b></span></label>
            </div>
        </div>

        <!-- Répartition -->
        <div class="card bg-rep">
            <div class="bg-rep-top">
                <div><span class="pl" id="bg_reste_lbl">Reste à répartir</span><div class="bg-reste" id="bg_reste">0 €</div></div>
                <div style="text-align:right;"><span class="pl">Planifié</span><div class="bg-plan" id="total_prevu_val">0 €</div></div>
            </div>
            <div class="bg-stack" id="bg_stack"></div>
            <div class="muted-sm" id="bg_pct"></div>
            <div id="status_plan_container" style="display:none;"></div>
        </div>

        <!-- Limites par catégorie -->
        <div class="sec-head">
            <span class="sec-title">Limites par catégorie</span>
            <span class="muted-sm" id="bg_nb"></span>
        </div>
        <div class="card bg-list">
            <div id="setup_categories"></div>
            <div class="bg-add">
                <input type="text" id="new_cat_name" placeholder="Nouvelle catégorie…" aria-label="Nouvelle catégorie" onkeydown="if(event.key==='Enter') ajouterNouvelleCategorie()">
                <button class="bg-plus" aria-label="Ajouter la catégorie" onclick="ajouterNouvelleCategorie()"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg></button>
            </div>
        </div>

        <!-- Prévu le mois prochain -->
        <div class="sec-head">
            <span class="sec-title">Prévu le mois prochain</span>
            <span class="muted-sm" id="previsions-total" style="display:none;">Total <span id="previsions-total-val">0 €</span></span>
        </div>
        <div class="card bg-list">
            <div id="previsions-list"></div>
            <div class="bg-form">
                <div class="bg-form-row">
                    <input type="text" id="prev_desc" placeholder="Ex : Assurance" aria-label="Objet">
                    <input type="number" inputmode="decimal" id="prev_mt" placeholder="0 €" aria-label="Montant">
                </div>
                <div class="bg-form-row2">
                    <select id="prev_cat_select" aria-label="Catégorie"></select>
                    <button class="btn btn-primary bg-addbtn" onclick="ajouterPrevision()">Ajouter</button>
                </div>
                <button class="bg-transfer" id="bg_transfer" onclick="toutTransferer()" style="display:none;">Tout transférer en dépenses</button>
            </div>
            <p class="muted-sm" style="margin:12px 2px 0;font-weight:500;line-height:1.5;">Ces dépenses sont conservées automatiquement à la clôture du mois.</p>
        </div>
    </div>

    <div id="page-bilan" class="page">
        <div class="card" id="bilan-card-m">
            <div class="bd-wrap">
                <div class="bd-donut" id="bd_donut">
                    <canvas id="bd_canvas" aria-label="Répartition des dépenses par catégorie" role="img"></canvas>
                    <div class="bd-hole">
                        <span class="pl bd-lbl" id="bd_lbl">Dépensé</span>
                        <span class="bd-amt" id="bd_dep">0 €</span>
                        <span class="muted-sm bd-sub" id="bd_budget">sur 0 €</span>
                        <span class="bc-pill ok" id="bd_pill">0 %</span>
                    </div>
                </div>
                <div class="muted-sm bd-reste" id="bd_reste" style="display:none;"></div>
            </div>
            <div id="bilan_cards"></div>
            <table id="bilan_table" style="display:none;"><tbody></tbody></table>
        </div>
        <div class="card" id="suggestions-card" style="display:none;">
            <div class="card-title">Suggestions pour le mois prochain</div>
            <div id="suggestions-list"></div>
        </div>
        <div class="card">
            <div class="card-title">Espace Épargne</div>
            <div class="expense-list-scroll" style="max-height:220px;"><table id="epargne_history_table_bilan"><thead><tr><th>Date</th><th>Nom</th><th>Montant</th><th></th></tr></thead><tbody></tbody></table></div>
            <div class="epargne-total"><span class="label">Total épargné</span><span class="value"><span id="local_epargne_total_bilan">0</span> €</span></div>
        </div>
    </div>

    <div id="page-depenses" class="page">
        <div class="card dp-form">
            <div class="sec-title">Nouvelle dépense</div>
            <label class="dp-amount" for="add_mt">
                <span class="dp-amount-l">Montant</span>
                <span class="dp-amount-row"><input type="number" inputmode="decimal" step="0.01" id="add_mt" placeholder="0,00" aria-label="Montant"><b>€</b></span>
            </label>
            <label class="dp-field"><span>Objet</span><input type="text" id="add_desc" placeholder="Ex : Courses Lidl"></label>
            <label class="dp-field"><span>Catégorie</span>
                <span class="dp-select"><i id="add_cat_dot"></i><select id="add_cat" onchange="majPastilleCat()"></select></span>
            </label>
            <label class="dp-field"><span>Note <em>(optionnel)</em></span><input type="text" id="add_note" placeholder="Ex : remboursement prévu"></label>
            <label class="dp-rec" for="add_recurring">
                <span class="dp-rec-txt"><strong>🔄 Dépense récurrente</strong><small>Reportée automatiquement le mois suivant</small></span>
                <span class="st-switch"><input type="checkbox" id="add_recurring"><span class="st-track"></span><span class="st-thumb"></span></span>
            </label>
            <button class="btn btn-primary dp-submit" onclick="ajouterDepense()">Ajouter la dépense</button>
        </div>

        <div class="sec-head">
            <span class="sec-title">Historique</span>
            <span class="muted-sm" id="dp_count"></span>
        </div>
        <div class="card dp-hist">
            <label class="dp-search">
                <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><circle cx="11" cy="11" r="7"/><path d="M20 20l-3.5-3.5"/></svg>
                <input type="text" id="search_input" placeholder="Rechercher une dépense, une catégorie, une note…" oninput="majAffichage()" aria-label="Rechercher">
            </label>
            <div class="dp-scroll-wrap">
                <div id="expense-scroll"><div id="dp_history"></div></div>
                <button id="scroll-down-btn" class="dp-scroll-btn" onclick="scrollDepenses()" aria-label="Aller en bas de la liste">↓</button>
            </div>
            <table id="log_table" style="display:none;"><tbody></tbody></table>
            <p class="dp-hint" id="dp_hint">Touche une dépense pour la modifier</p>
        </div>
    </div>



    <!-- ════════ PAGE COUPLE ════════ -->
    <div id="page-couple" class="page">

        <!-- Session -->
        <div class="card">
            <div class="card-title">💑 Budget Couple
                <span class="couple-session-badge" onclick="copierSessionCouple()" style="margin-left:auto;">
                    <span class="sync-dot" id="sync-dot"></span>
                    <span id="session-label">Non connecté</span>
                </span>
            </div>
            <div style="display:flex;gap:8px;margin-bottom:12px;">
                <input type="text" id="couple-session-input" placeholder="Code session (ex: ENZO-2026)" style="margin:0;flex:1;text-transform:uppercase;">
                <button class="btn btn-primary" style="width:auto;padding:0 14px;flex-shrink:0;" onclick="rejoindreSession()">Rejoindre</button>
            </div>
            <p style="font-size:0.75rem;color:var(--text-muted);">Entrez le même code sur les deux téléphones pour synchroniser le budget en temps réel.</p>
        </div>

        <!-- Budget global -->
        <div class="card" id="couple-main-card" style="display:none;">
            <div class="card-title">Budget commun du mois</div>
            <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px;">
                <span style="font-size:0.84rem;color:var(--text-muted);">Dépensé</span>
                <span style="font-size:1.1rem;font-weight:700;" id="couple-total-dep">0.00 €</span>
            </div>
            <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px;">
                <span style="font-size:0.84rem;color:var(--text-muted);">Budget total</span>
                <span style="font-size:1.1rem;font-weight:700;color:var(--main);" id="couple-total-budget">0.00 €</span>
            </div>
            <div class="couple-budget-bar"><div class="couple-budget-fill" id="couple-bar" style="width:0%"></div></div>
            <div style="display:flex;justify-content:space-between;font-size:0.78rem;color:var(--text-muted);">
                <span id="couple-pct">0%</span>
                <span id="couple-reste">Reste : 0.00 €</span>
            </div>
        </div>

        <!-- Catégories -->
        <div class="card" id="couple-cats-card" style="display:none;">
            <div class="card-title">Par catégorie</div>
            <div class="couple-cat-grid" id="couple-cat-grid"></div>
            <div style="margin-top:8px;">
                <label style="font-size:0.78rem;">Modifier les budgets par catégorie</label>
                <div id="couple-budget-inputs"></div>
            <div class="add-cat-box" style="margin-top:12px;">
                <input type="text" id="couple-new-cat" placeholder="Nouvelle catégorie..." style="margin:0;flex:1;">
                <button class="btn-add-cat" onclick="ajouterCategorieCouple()">+</button>
            </div>
            </div>
        </div>

        <!-- Ajouter dépense -->
        <div class="card" id="couple-form-card" style="display:none;">
            <div class="card-title">Ajouter une dépense commune</div>
            <label>Objet</label>
            <input type="text" id="couple-desc" placeholder="Ex: Restaurant La Bonne Table">
            <label>Montant (€)</label>
            <input type="number" inputmode="decimal" id="couple-mt" placeholder="0.00">
            <label>Catégorie</label>
            <select id="couple-cat" style="margin-bottom:12px;"></select>
            <label>Payé par</label>
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:14px;">
                <button id="btn-moi" class="btn btn-primary" onclick="selectAuteur('moi')" style="min-height:40px;">👤 <span id="label-moi">Moi</span></button>
                <button id="btn-elle" class="btn btn-secondary" onclick="selectAuteur('elle')" style="min-height:40px;">👤 <span id="label-partner">Ma copine</span></button>
            </div>
            <button class="btn btn-primary" onclick="ajouterDepenseCouple()">+ Ajouter</button>
        </div>

        <!-- Historique -->
        <div class="card" id="couple-history-card" style="display:none;">
            <div class="card-title">Historique commun</div>
            <!-- Filtre par catégorie -->
            <div style="margin-bottom:12px;">
                <select id="couple-filtre-cat" onchange="majAffichageCouple()" style="margin:0;">
                    <option value="">Toutes les catégories</option>
                </select>
            </div>
            <div id="couple-history-list"></div>
        </div>

        <!-- Clôture mensuelle -->
        <div class="card" id="couple-cloture-card" style="display:none;">
            <div class="card-title">Clôture du mois</div>
            <p style="font-size:0.82rem;color:var(--text-muted);margin-bottom:14px;line-height:1.6;">Archive le budget de ce mois et repart à zéro pour le mois suivant. Les dépenses Supabase seront conservées dans l'historique local.</p>
            <button class="btn btn-primary" onclick="cloturerMoisCouple()">📁 Clôturer le mois couple</button>
        </div>

        <!-- Archives couple -->
        <div class="card" id="couple-archives-card" style="display:none;">
            <div class="card-title">Archives couple</div>
            <div id="couple-archives-list"></div>
        </div>

    </div>

    <div id="page-archives" class="page">
        <div class="card ar-card" id="cat-evolution-card" style="display:none;">
            <div class="sec-title">Évolution par catégorie</div>
            <select id="cat-select" onchange="afficherEvolutionCategorie()" style="margin:0;"></select>
            <div class="ar-legend">
                <span><i id="cat-legend-dot" style="width:14px;height:3px;border-radius:2px;background:var(--main);"></i>Réel</span>
                <span><i style="width:14px;height:0;border-top:2px dashed var(--text-hint);border-radius:0;"></i>Prévu</span>
            </div>
            <div class="ar-chart" style="height:190px;"><canvas id="catEvolutionChart"></canvas></div>
            <div class="ar-tiles" id="cat-tiles"></div>
            <div id="cat-tendance"></div>
        </div>
        <div class="card ar-card" id="evolution-section" style="display:none;">
            <div class="sec-title">Évolution globale</div>
            <div class="ar-legend" id="evo-legend"></div>
            <div class="ar-chart" style="height:200px;"><canvas id="evolutionChart"></canvas></div>
        </div>
        <div class="sec-head" style="margin-top:8px;">
            <span class="sec-title">Mois archivés</span>
            <span class="muted-sm" id="ar-count"></span>
        </div>
        <div id="archives-container"></div>
    </div>

    <div id="page-vacances" class="page">
        <!-- Configuration -->
        <div class="card" id="vac-config-card">
            <div style="text-align:center;padding:10px 0 16px;">
                <div style="font-size:2rem;margin-bottom:8px;">🌍</div>
                <div style="font-size:1rem;font-weight:700;color:var(--text);margin-bottom:4px;">Configurer ton séjour</div>
                <div style="font-size:0.8rem;color:var(--text-muted);margin-bottom:16px;">Les dépenses vacances sont séparées de ton budget mensuel</div>
            </div>
            <label>Nom du séjour</label>
            <input type="text" id="vac-nom" placeholder="Ex: Maroc juillet 2026">
            <label>Budget total du séjour (€)</label>
            <input type="number" inputmode="decimal" id="vac-budget" placeholder="Ex: 800">
            <button class="btn btn-primary" onclick="sauvegarderConfigVacances()">🌍 Créer le séjour</button>
        </div>

        <!-- Résumé -->
        <div class="card" id="vac-main-card" style="display:none;">
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
                <div style="font-size:0.68rem;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--text-muted);" id="vac-titre">Séjour</div>
                <button onclick="document.getElementById('vac-config-card').style.display='block';document.getElementById('vac-main-card').style.display='none';" style="background:none;border:none;font-size:0.75rem;color:var(--text-muted);cursor:pointer;">✏️ Modifier</button>
            </div>
            <div style="font-size:2rem;font-weight:700;line-height:1;margin-bottom:12px;color:var(--text);" id="vac-total-dep">0.00 €</div>
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px;">
                <div style="background:var(--bg);border-radius:10px;padding:8px 10px;">
                    <div style="font-size:0.68rem;color:var(--text-muted);margin-bottom:2px;">Budget total</div>
                    <div style="font-size:0.95rem;font-weight:700;color:var(--main);" id="vac-total-budget">0.00 €</div>
                </div>
                <div style="background:var(--bg);border-radius:10px;padding:8px 10px;">
                    <div style="font-size:0.68rem;color:var(--text-muted);margin-bottom:2px;">Reste</div>
                    <div style="font-size:0.95rem;font-weight:700;color:var(--success);" id="vac-reste">0.00 €</div>
                </div>
            </div>
            <div style="background:var(--bg2);border-radius:999px;height:6px;overflow:hidden;margin-bottom:4px;">
                <div id="vac-bar" style="height:100%;border-radius:999px;background:var(--main);transition:width 0.7s;width:0%;"></div>
            </div>
            <div style="font-size:0.7rem;color:var(--text-muted);text-align:right;" id="vac-pct">0%</div>
        </div>

        <!-- Formulaire ajout dépense -->
        <div class="card" id="vac-form-card" style="display:none;">
            <div style="font-size:0.68rem;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--text-muted);margin-bottom:12px;">➕ Ajouter une dépense</div>
            <label>Objet</label>
            <input type="text" id="vac-desc" placeholder="Ex: Restaurant La Mamounia">
            <label>Montant</label>
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px;">
                <input type="number" inputmode="decimal" id="vac-mt" placeholder="0.00" style="margin:0;">
                <select id="vac-devise" style="margin:0;"></select>
            </div>
            <label>Catégorie</label>
            <select id="vac-cat" style="margin-bottom:14px;"></select>
            <button class="btn btn-primary" id="vac-btn-ajouter" onclick="ajouterDepenseVacances()">+ Ajouter</button>
        </div>

        <!-- Répartition par catégorie -->
        <div class="card" id="vac-hist-card" style="display:none;">
            <div style="font-size:0.68rem;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--text-muted);margin-bottom:10px;">📊 Par catégorie</div>
            <div id="vac-cats-list" style="margin-bottom:16px;"></div>
            <div style="font-size:0.68rem;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--text-muted);margin-bottom:10px;">🧾 Historique</div>
            <div id="vac-hist-list" style="max-height:340px;overflow-y:auto;-webkit-overflow-scrolling:touch;"></div>
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:14px;">
                <button onclick="reinitialiserVacances()" style="background:rgba(239,68,68,0.06);border:1px solid rgba(239,68,68,0.2);color:var(--danger);border-radius:10px;padding:10px;font-family:'Manrope',sans-serif;font-size:0.82rem;font-weight:600;cursor:pointer;">🗑️ Réinitialiser</button>
                <button onclick="cloturerVoyage()" style="background:var(--main);border:none;color:white;border-radius:10px;padding:10px;font-family:'Manrope',sans-serif;font-size:0.82rem;font-weight:600;cursor:pointer;">📁 Clôturer le voyage</button>
            </div>
            <!-- Archives voyages -->
            <div id="vac-archives-section" style="display:none;margin-top:20px;">
                <div style="font-size:0.68rem;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--text-muted);margin-bottom:10px;">🗂️ Voyages archivés</div>
                <div id="vac-archives-list"></div>
            </div>
        </div>
    </div>

        <div id="page-partenaire" class="page">
        <div class="card" id="partner-no-code-card" style="display:none;">
            <div class="empty-state" style="padding:24px 8px;"><p><strong style="color:var(--text);font-size:0.95rem;">Aucun code partenaire</strong><br>Ajoute le code widget de ton·ta partenaire dans les paramètres pour voir son budget ici.</p>
            <button class="btn btn-primary" style="margin-top:14px;" onclick="ouvrirSettings()">Ouvrir les paramètres</button></div>
        </div>
        <div id="partner-budget-card">
            <div class="pt-head">
                <div class="mh-left">
                    <div class="mh-avatar pt-avatar" id="partner-avatar">P</div>
                    <div class="mh-text">
                        <span class="mh-sub">Lecture seule</span>
                        <span class="pt-title" id="partner-card-title">Budget partenaire</span>
                    </div>
                </div>
                <span class="pt-sync"><span id="partner-sync-dot" class="pt-dot"></span><span id="partner-last-sync">—</span></span>
            </div>

            <div class="hero pt-hero">
                <span class="hero-label">Son solde du mois</span>
                <div class="hero-amount" id="partner-solde-wrap">— €</div>
                <div class="hero-sub" id="partner-revenu-txt">—</div>
                <div class="hero-stats">
                    <div class="hero-stat"><span class="hs-l">Dépensé</span><span class="hs-v" id="partner-dep">—</span></div>
                    <div class="hero-stat"><span class="hs-l">Épargné</span><span class="hs-v" id="partner-ep">—</span></div>
                    <div class="hero-stat"><span class="hs-l">Coffre</span><span class="hs-v" id="partner-coffre">—</span></div>
                </div>
            </div>

            <div class="card">
                <div class="row-between">
                    <span class="sec-title">Budget utilisé</span>
                    <span class="muted-sm" id="partner-bar-label">—</span>
                </div>
                <div class="bar"><div id="partner-bar" class="bar-fill" style="width:0%;background:var(--partner);"></div></div>
                <div class="muted-sm" id="partner-bar-pct">—</div>
            </div>

            <div class="sec-head">
                <span class="sec-title">Fin de mois</span>
                <span class="muted-sm"><span id="partner-jours">0</span> jours restants</span>
            </div>
            <div class="card pace">
                <span class="pl" id="partner-peut-lbl">Peut dépenser</span>
                <div class="pace-main"><span id="partner-peut">—</span> <small>/ jour</small></div>
                <div class="pace-row">
                    <span class="pl" id="partner-rythme-lbl">Dépense en moyenne</span>
                    <span class="pace-rythme" id="partner-rythme">—</span>
                </div>
            </div>

            <div class="sec-head"><span class="sec-title">Par catégorie</span></div>
            <div class="card pt-cats" id="partner-cats"></div>
        </div>
    </div>

    <div class="grid-top" style="margin-top:0;">
        <div class="card desktop-budget">
            <div class="card-title"><span class="badge">1</span> Je prévois mon mois</div>
            <label>Salaire / Revenus (€)</label>
            <input type="number" inputmode="decimal" id="prev_revenu_d" placeholder="Ex: 1800" onchange="sauvegarder()">
            <label>CAF (€)</label>
            <input type="number" inputmode="decimal" id="prev_caf_d" placeholder="Ex: 200" onchange="sauvegarder()">
            <p style="font-size:0.78rem;color:var(--text-muted);margin-bottom:10px;font-weight:500;">Limites par catégorie :</p>
            <div id="setup_categories_d"></div>
            <div class="add-cat-box">
                <input type="text" id="new_cat_name_d" placeholder="Nouvelle catégorie...">
                <button class="btn-add-cat" onclick="ajouterNouvelleCategorie()">+</button>
            </div>
            <div class="plan-summary">
                <div class="plan-row"><span>Total planifié</span><strong id="total_prevu_val_d">0 €</strong></div>
                <div class="plan-row" id="status_plan_container_d"><span>Reste à répartir</span><span class="text-success">0 €</span></div>
            </div>
            <button class="btn btn-primary" onclick="sauvegarder()" style="margin-top:14px;">Mettre à jour</button>
        </div>
        <div class="card desktop-depenses">
            <div class="card-title"><span class="badge">2</span> J'ajoute mes dépenses</div>
            <div class="expense-grid">
                <div><label>Objet</label><input type="text" id="add_desc_d" placeholder="Ex: Courses Lidl"></div>
                <div><label>Montant (€)</label><input type="number" inputmode="decimal" id="add_mt_d" placeholder="0.00"></div>
                <div class="field-full"><label>Catégorie</label><select id="add_cat_d"></select></div>
                <div class="field-full"><label>Note <span style="font-weight:400;color:var(--text-hint);">(optionnel)</span></label><input type="text" id="add_note_d" placeholder="Ex: remboursement prévu, achat pour la maison..."></div>
            </div>
            <div class="recurring-row">
                <input type="checkbox" id="add_recurring_d">
                <label for="add_recurring_d" style="margin:0;cursor:pointer;">Dépense récurrente (conservée en fin de mois)</label>
            </div>
            <button class="btn btn-primary" onclick="ajouterDepenseDesktop()">+ Ajouter la dépense</button>
            <div style="margin-top:16px;">
                <div class="card-title" style="margin-bottom:10px;">🔎 Historique</div>
                <div class="search-wrap"><input type="text" id="search_input_d" placeholder="Rechercher..." oninput="majAffichage()"></div>
                <div class="table-scroll" style="max-height:220px;overflow-y:auto;"><table id="log_table_d"><thead><tr><th>Date</th><th>Nom</th><th>Cat.</th><th>Prix</th><th></th></tr></thead><tbody></tbody></table></div>
            </div>
        </div>
    </div>

    <div class="card desktop-bilan">
        <div class="card-title"><span class="badge">3</span> Bilan — Ai-je respecté mes engagements ?</div>
        <div class="bl-sum">
            <div class="bl-top">
                <div>
                    <span class="pl">Dépensé ce mois</span>
                    <div class="bl-amt"><span id="bl_dep_d">0</span> € <small>sur <span id="bl_budget_d">0</span> €</small></div>
                </div>
                <span class="bc-pill ok" id="bl_pill_d">0 %</span>
            </div>
            <div class="bar"><div class="bar-fill" id="bl_bar_d" style="width:0%;"></div></div>
            <div class="muted-sm" id="bl_reste_d">—</div>
        </div>
        <div style="height:18px;"></div>
        <div class="bilan-layout">
            <div class="bilan-table-side"><div class="table-scroll"><table id="bilan_table_d"><thead><tr><th>Catégorie</th><th>Prévu</th><th>Réel</th><th>Écart</th><th>%</th><th>Statut</th></tr></thead><tbody></tbody></table></div></div>
            <div class="bilan-chart-side"><canvas id="budgetChart_d" style="max-width:260px;max-height:260px;"></canvas></div>
        </div>
    </div>

    <div class="card desktop-epargne">
        <div class="card-title">Espace Épargne (Suivi détaillé)</div>
        <div class="table-scroll" style="max-height:220px;overflow-y:auto;"><table id="epargne_history_table_d"><thead><tr><th>Date</th><th>Nom</th><th>Montant</th><th></th></tr></thead><tbody></tbody></table></div>
        <div class="epargne-total"><span class="label">Total de cet espace</span><span class="value"><span id="local_epargne_total_d">0</span> €</span></div>
    </div>

    <div class="card desktop-archives">
        <div class="card-title">Historique des mois archivés</div>
        <div id="evolution-section-d" style="display:none;margin-bottom:24px;"><div class="chart-title">📈 Évolution mois par mois</div><div class="evolution-wrap"><canvas id="evolutionChartD"></canvas></div></div>
        <div id="archives-container-d"></div>
    </div>

</div>

<style>
    @media (min-width: 769px) {
        #page-tableau, #page-budget, #page-depenses, #page-bilan, #page-couple, #page-archives, #page-partenaire, #page-vacances { display: none !important; }
        .desktop-budget, .desktop-depenses, .desktop-bilan, .desktop-epargne, .desktop-archives { display: block !important; }
    }
    @media (max-width: 768px) {
        .desktop-budget, .desktop-depenses, .desktop-bilan, .desktop-epargne, .desktop-archives, .grid-top { display: none !important; }
    }
</style>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script>
    let db = JSON.parse(localStorage.getItem('budget_vGestion')) || {
        revenu: 0,
        caf: 0,
        categories: [
            { id:"Fixe", label:"Loyers/Charges" }, { id:"Courses", label:"Alimentation" },
            { id:"Loisirs", label:"Sorties/Plaisirs" }, { id:"Epargne", label:"Épargne" }, { id:"Autres", label:"Divers" }
        ],
        previsions: { Fixe:0, Courses:0, Loisirs:0, Epargne:0, Autres:0 },
        depenses: [], historiqueEpargne: []
    };
    let globalSavings = parseFloat(localStorage.getItem('globalSavings')) || 0;
    let archives = JSON.parse(localStorage.getItem('budget_archives')) || [];
    let chartInstance = null, chartInstanceD = null, evolutionChartInstance = null, evolutionChartInstanceD = null, catChartInstance = null;
    const colors = ['#4f46e5','#0b7a53','#d97706','#c2410c','#64748b','#0e7490','#9333ea','#be185d'];

    document.documentElement.setAttribute('data-theme', localStorage.getItem('theme') || 'light');
    function toggleTheme() {
        const next = document.documentElement.getAttribute('data-theme') === 'light' ? 'dark' : 'light';
        document.documentElement.setAttribute('data-theme', next);
        localStorage.setItem('theme', next);
        appliquerFond();
        majAffichage(); afficherEvolution();
    }

    const PAGES = ['tableau','budget','depenses','bilan','couple','archives','partenaire','vacances'];
    function goTo(name) {
        PAGES.forEach(p => { const el=document.getElementById('page-'+p), btn=document.getElementById('nav-'+p); if(el) el.classList.remove('active'); if(btn) btn.classList.remove('active'); });
        const el=document.getElementById('page-'+name), btn=document.getElementById('nav-'+name);
        if(el) el.classList.add('active'); if(btn) btn.classList.add('active');
        fermerPlus();
        window.scrollTo(0,0);
        if(name==='partenaire') { majAffichagePartenaire(partnerData); chargerBudgetPartenaire(); }
        if(name==='vacances') majAffichageVacances();
        if(name==='bilan') majAffichage();
        if(name==='depenses') setTimeout(()=>{ if(typeof updateScrollBtn==='function') updateScrollBtn(); },60);
    }

    function showToast(msg, type='success', duration=3000) {
        const icons={success:'✅',danger:'❌',info:'ℹ️',warning:'⚠️'};
        const t=document.createElement('div'); t.className='toast '+type;
        t.innerHTML=`<span>${icons[type]||'📢'}</span> ${msg}`;
        document.getElementById('toast-container').appendChild(t);
        setTimeout(()=>{ t.style.animation='toastOut 0.3s ease both'; setTimeout(()=>t.remove(),300); },duration);
    }

    function ouvrirModal() {
        const now=new Date(); const mois=['Janvier','Février','Mars','Avril','Mai','Juin','Juillet','Août','Septembre','Octobre','Novembre','Décembre'];
        document.getElementById('modal-mois-nom').value=`${mois[now.getMonth()]} ${now.getFullYear()}`;
        document.getElementById('modal-cloture').style.display='flex';
        setTimeout(()=>document.getElementById('modal-mois-nom').focus(),100);
    }
    function fermerModal() { document.getElementById('modal-cloture').style.display='none'; }
    document.getElementById('modal-cloture').addEventListener('click', e=>{ if(e.target===e.currentTarget) fermerModal(); });
    document.getElementById('modal-mois-nom').addEventListener('keydown', e=>{ if(e.key==='Enter') confirmerCloture(); if(e.key==='Escape') fermerModal(); });

    function confirmerCloture() {
        const nom=document.getElementById('modal-mois-nom').value.trim()||'Mois sans nom';
        fermerModal();
        let totaux={}; db.categories.forEach(c=>{ totaux[c.id]=0; });
        let totalDep=0, totalEp=0;
        db.depenses.forEach(d=>{ if(d.ct in totaux) totaux[d.ct]+=d.mt; const cat=db.categories.find(c=>c.id===d.ct); if(cat&&cat.label.toLowerCase().includes("épargne")) totalEp+=d.mt; else totalDep+=d.mt; });
        const archive={ id:Date.now(), nom, date:new Date().toLocaleDateString('fr-FR'), revenu:(db.revenu||0)+(db.caf||0), totalDep, totalEp, solde:((db.revenu||0)+(db.caf||0))-totalDep-totalEp, labels:db.categories.map(c=>c.label), data:db.categories.map(c=>totaux[c.id]||0), previsions:{...db.previsions}, catIds:db.categories.map(c=>c.id), depenses:db.depenses.map(d=>{ const cat=db.categories.find(c=>c.id===d.ct); return{...d,catLabel:cat?cat.label:'N/A'}; }) };
        archives.push(archive); localStorage.setItem('budget_archives',JSON.stringify(archives));
        let em=0; db.depenses.forEach(d=>{ const cat=db.categories.find(c=>c.id===d.ct); if(cat&&cat.label.toLowerCase().includes("épargne")) em+=d.mt; });
        globalSavings+=em; localStorage.setItem('globalSavings',globalSavings);
        db.depenses=db.depenses.filter(d=>d.recurring===true);
        sauvegarder(); afficherArchives(); afficherEvolution();
        const nbPrev = previsionsMoisProchain.length;
        const prevMsg = nbPrev > 0 ? ` · ${nbPrev} prévision${nbPrev > 1 ? 's' : ''} conservée${nbPrev > 1 ? 's' : ''} 📋` : '';
        showToast(`"${nom}" archivé ! +${em}€ au coffre 🎉${prevMsg}`,'success',5000);
    }

    function ajouterNouvelleCategorie() {
        const inpM=document.getElementById('new_cat_name'), inpD=document.getElementById('new_cat_name_d');
        const inp=(inpM&&inpM.value.trim())?inpM:(inpD&&inpD.value.trim())?inpD:inpM;
        const name=inp?inp.value.trim():'';
        if(!name){ showToast('Écris le nom de la catégorie','warning'); return; }
        const newId='cat_'+Date.now();
        db.categories.push({id:newId,label:name,color:nouvelleCouleurCategorie()}); db.previsions[newId]=0;
        if(inpM) inpM.value=''; if(inpD) inpD.value='';
        localStorage.setItem('budget_vGestion',JSON.stringify(db)); majAffichage();
        showToast(`Catégorie "${name}" ajoutée`,'info');
    }
    function supprimerCategorie(id) {
        const cat=db.categories.find(c=>c.id===id);
        if(confirm(`Supprimer "${cat?cat.label:''}" ?`)){ db.categories=db.categories.filter(c=>c.id!==id); delete db.previsions[id]; sauvegarder(); showToast('Catégorie supprimée','warning'); }
    }

    function _ajouter(descId,mtId,catId,recurId,noteId) {
        const descEl=document.getElementById(descId),mtEl=document.getElementById(mtId),catEl=document.getElementById(catId),recurEl=document.getElementById(recurId),noteEl=document.getElementById(noteId);
        if(!descEl||!mtEl||!catEl){ showToast('Formulaire introuvable','danger'); return; }
        const desc=descEl.value.trim(),mt=parseFloat(mtEl.value),ctId=catEl.value,isRec=recurEl?recurEl.checked:false,note=noteEl?noteEl.value.trim():'';
        if(!desc||isNaN(mt)||mt<=0){ showToast('Remplis tous les champs correctement','warning'); return; }
        if(!ctId){ showToast('Sélectionne une catégorie','warning'); return; }
        const expense={id:Date.now(),date:new Date().toLocaleDateString('fr-FR'),desc,mt,ct:ctId,recurring:isRec,note:note||''};
        db.depenses.push(expense);
        const catObj=db.categories.find(c=>c.id===ctId);
        if(catObj&&catObj.label.toLowerCase().includes("épargne")) db.historiqueEpargne.push(expense);
        descEl.value=''; mtEl.value=''; if(recurEl) recurEl.checked=false; if(noteEl) noteEl.value='';
        localStorage.setItem('budget_vGestion',JSON.stringify(db)); majAffichage();
        showToast(`${desc} — ${mt}€ ajouté${isRec?' 🔄':''}`,'success');
    }
    function ajouterDepense()        { _ajouter('add_desc','add_mt','add_cat','add_recurring','add_note'); }
    function ajouterDepenseDesktop() { _ajouter('add_desc_d','add_mt_d','add_cat_d','add_recurring_d','add_note_d'); }

    function supprimer(id) {
        const idx=db.historiqueEpargne.findIndex(d=>d.id===id);
        if(idx!==-1){ globalSavings=Math.max(0,globalSavings-db.historiqueEpargne[idx].mt); localStorage.setItem('globalSavings',globalSavings); db.historiqueEpargne.splice(idx,1); }
        db.depenses=db.depenses.filter(d=>d.id!==id);
        localStorage.setItem('budget_vGestion',JSON.stringify(db)); majAffichage(); showToast('Dépense supprimée','danger');
    }
    function resetCoffre() {
        if(confirm("Vider le coffre-fort ET l'historique d'épargne ?")){ globalSavings=0; db.historiqueEpargne=[]; localStorage.setItem('globalSavings',0); sauvegarder(); showToast('Coffre-fort vidé','warning'); }
    }

    function sauvegarder() {
        ['prev_revenu','prev_revenu_d'].forEach(id=>{ const el=document.getElementById(id); if(el&&el.value!==''&&el.offsetParent!==null) db.revenu=parseFloat(el.value)||0; });
        ['prev_caf','prev_caf_d'].forEach(id=>{ const el=document.getElementById(id); if(el&&el.value!==''&&el.offsetParent!==null) db.caf=parseFloat(el.value)||0; });
        document.querySelectorAll('.prev-input').forEach(inp=>{ if(inp.offsetParent!==null) db.previsions[inp.dataset.cat]=parseFloat(inp.value)||0; });
        localStorage.setItem('budget_vGestion',JSON.stringify(db)); majAffichage();
    }

    function majAffichage() {
        assurerCouleursCategories();
        let totaux={}; db.categories.forEach(c=>{ totaux[c.id]=0; });
        let totalGeneral=0, totalEpargne=0;
        db.depenses.forEach(d=>{ if(d.ct in totaux) totaux[d.ct]+=d.mt; const cat=db.categories.find(c=>c.id===d.ct); if(cat&&cat.label.toLowerCase().includes("épargne")) totalEpargne+=d.mt; else totalGeneral+=d.mt; });
        const revenuTotal = (db.revenu||0) + (db.caf||0);
        const solde=revenuTotal-totalGeneral-totalEpargne;
        const setN=(id,v)=>{ const el=document.getElementById(id); if(el) el.innerText=parseFloat(v).toFixed(2); };
        setN('view_total_dep',totalGeneral); setN('view_total_ep',totalEpargne); setN('view_solde',solde); setN('view_global_ep',globalSavings);
        setN('m_dep',totalGeneral); setN('m_ep',totalEpargne); setN('m_solde',solde); setN('m_coffre',globalSavings);
        const sc=solde<0?'var(--danger)':'var(--success)';
        const vsw=document.getElementById('view_solde_wrapper'); if(vsw) vsw.style.color=sc;
        const heroNeg=document.getElementById('hero-neg'); if(heroNeg) heroNeg.style.display=solde<0?'inline-block':'none';
        const revTxt=document.getElementById('m_revenu_txt'); if(revTxt) revTxt.innerText='sur '+revenuTotal.toFixed(2)+' € de revenus';

        // Barre de budget
        const totalPrevBudget=db.categories.reduce((s,c)=>s+(db.previsions[c.id]||0),0);
        const budgetBarRef=totalPrevBudget>0?totalPrevBudget:revenuTotal;
        const budgetBarPct=budgetBarRef>0?Math.min(Math.round((totalGeneral/budgetBarRef)*100),100):0;
        const barEl=document.getElementById('m_budget_bar');
        if(barEl){barEl.style.width=budgetBarPct+'%';barEl.style.background=budgetBarPct>=100?'var(--danger)':budgetBarPct>=85?'var(--warning)':'var(--main)';}
        const barLbl=document.getElementById('m_budget_bar_label');
        if(barLbl)barLbl.innerText=totalGeneral.toFixed(0)+' € / '+budgetBarRef.toFixed(0)+' €';
        const barPct=document.getElementById('m_budget_pct');
        if(barPct)barPct.innerText=budgetBarPct+' % du budget prévu';

        // Épargne résumé
        const epEpEl=document.getElementById('m_ep_epargne');
        if(epEpEl)epEpEl.innerText=totalEpargne.toFixed(2);
        const coffreEpEl=document.getElementById('m_coffre_ep');
        if(coffreEpEl)coffreEpEl.innerText=globalSavings.toFixed(2);
        ['prev_revenu','prev_revenu_d'].forEach(id=>{ const el=document.getElementById(id); if(el&&document.activeElement!==el) el.value=db.revenu||''; });
        ['prev_caf','prev_caf_d'].forEach(id=>{ const el=document.getElementById(id); if(el&&document.activeElement!==el) el.value=db.caf||''; });

        const renderSetup=(cId,tId,sId,w)=>{ const div=document.getElementById(cId); if(!div) return; div.innerHTML=''; let total=0; db.categories.forEach(cat=>{ const val=db.previsions[cat.id]||0; total+=val; div.innerHTML+=`<div class="budget-row"><button class="btn-icon-sm" onclick="supprimerCategorie('${cat.id}')">✕</button><span class="cat-label">${cat.label}</span><input type="number" inputmode="decimal" class="prev-input" data-cat="${cat.id}" value="${val}" onchange="sauvegarder()" style="width:${w};margin:0;min-height:36px;"></div>`; }); const tEl=document.getElementById(tId); if(tEl) tEl.innerText=total.toFixed(0)+' €'; const reste=revenuTotal-total; const sEl=document.getElementById(sId); if(sEl) sEl.innerHTML=reste<0?`<span>Dépassement</span><span class="text-danger">${Math.abs(reste).toFixed(0)} €</span>`:`<span>Reste à répartir</span><span class="text-success">${reste.toFixed(0)} €</span>`; };
        renderBudgetMobile(revenuTotal);
        renderSetup('setup_categories_d','total_prevu_val_d','status_plan_container_d','90px');

        ['add_cat','add_cat_d'].forEach(id=>{ const sel=document.getElementById(id); if(!sel) return; const cur=sel.value; sel.innerHTML=''; db.categories.forEach(cat=>{ sel.innerHTML+=`<option value="${cat.id}">${cat.label}</option>`; }); if(cur) sel.value=cur; });
        renderCatChips();

        const sm=(document.getElementById('search_input')||{}).value||'', st=(document.getElementById('search_input_tableau')||{}).value||'', sd=(document.getElementById('search_input_d')||{}).value||'';
        const searchTerm=(sm||st||sd).toLowerCase();

        const renderLog=(tbodyId)=>{ const tbody=document.querySelector('#'+tbodyId+' tbody'); if(!tbody) return; tbody.innerHTML=''; [...db.depenses].reverse().forEach(d=>{ const cat=db.categories.find(c=>c.id===d.ct),catLabel=cat?cat.label.toLowerCase():''; if(!searchTerm||d.desc.toLowerCase().includes(searchTerm)||catLabel.includes(searchTerm)||(d.note&&d.note.toLowerCase().includes(searchTerm))){ const noteHtml=d.note?`<div style="font-size:0.72rem;color:var(--text-muted);margin-top:2px;font-style:italic;padding-left:2px;border-left:2px solid var(--main);padding-left:6px;">${d.note}</div>`:''; tbody.innerHTML+=`<tr style="cursor:pointer;" onclick="ouvrirEditDepense(${d.id})"><td style="color:var(--text-muted);font-size:0.76rem;white-space:nowrap">${d.date}</td><td>${d.desc}${d.recurring?'<span class="recurring-tag">🔄</span>':''}${noteHtml}</td><td><span class="cat-pill">${cat?cat.label:'N/A'}</span></td><td><strong>${parseFloat(d.mt).toFixed(2)}€</strong></td><td><button class="btn-delete" onclick="event.stopPropagation();supprimer(${d.id})">✕</button></td></tr>`; } }); };
        renderLog('log_table'); renderLog('log_table_d'); renderLog('log_table_tableau');
        renderHistoriqueGroupe(searchTerm);
        majPastilleCat();
        renderLogList('log_list_tableau');

        db.historiqueEpargne.sort((a,b)=>a.desc.toLowerCase().localeCompare(b.desc.toLowerCase()));
        const renderEp=(tbodyId,totalId)=>{ const tbody=document.querySelector('#'+tbodyId+' tbody'); if(!tbody) return; tbody.innerHTML=''; let total=0; db.historiqueEpargne.forEach(d=>{ tbody.innerHTML+=`<tr><td style="color:var(--text-muted);font-size:0.76rem;white-space:nowrap">${d.date}</td><td>${d.desc}${d.recurring?'<span class="recurring-tag">🔄</span>':''}</td><td><strong>${parseFloat(d.mt).toFixed(2)}€</strong></td><td><button class="btn-delete" onclick="supprimer(${d.id})">✕</button></td></tr>`; total+=d.mt; }); const el=document.getElementById(totalId); if(el) el.innerText=total.toFixed(2); };
        renderEp('epargne_history_table','local_epargne_total');
        renderEp('epargne_history_table_d','local_epargne_total_d');
        renderEp('epargne_history_table_bilan','local_epargne_total_bilan');
        renderEpList('ep_list_tableau');

        const renderBilan=(tbodyId,chartId,oldChart)=>{ const tbody=document.querySelector('#'+tbodyId+' tbody'); if(!tbody) return oldChart; tbody.innerHTML=''; let labels=[],data=[]; db.categories.forEach(cat=>{ const prev=db.previsions[cat.id]||0,reel=totaux[cat.id]||0,pct=prev>0?Math.min((reel/prev)*100,100):0,over=reel>prev,ecart=prev-reel; tbody.innerHTML+=`<tr style="${over?'background:rgba(239,68,68,0.03)':''}"><td style="font-weight:600;white-space:nowrap">${cat.label}</td><td style="color:var(--text-muted);white-space:nowrap">${prev.toFixed(0)} €</td><td style="font-weight:700;white-space:nowrap">${reel.toFixed(2)} €</td><td style="color:${ecart<0?'var(--danger)':'var(--success)'};font-weight:700;white-space:nowrap">${ecart>=0?'+':''}${ecart.toFixed(0)} €</td><td><div class="progress-wrap"><div class="progress-bg"><div class="progress-fill ${over?'over':''}" style="width:${pct}%"></div></div><span class="progress-pct">${pct.toFixed(0)}%</span></div></td><td>${over?'<span class="status-over">⚠ Dépassé</span>':'<span class="status-ok">✓ OK</span>'}</td></tr>`; labels.push(cat.label); data.push(reel); }); if(oldChart){ oldChart.destroy(); oldChart=null; } const isDark=document.documentElement.getAttribute('data-theme')==='dark'; const ctx=document.getElementById(chartId); if(!ctx) return null; return new Chart(ctx,{type:'pie',data:{labels,datasets:[{data,backgroundColor:db.categories.map(c=>c.color||'#64748b'),borderWidth:3,borderColor:isDark?'#1a1b21':'#fff',hoverOffset:5}]},options:{responsive:true,maintainAspectRatio:true,aspectRatio:1,plugins:{legend:{position:'bottom',labels:{color:isDark?'#9ca3af':'#6b7280',font:{family:'Manrope',size:10},padding:8,usePointStyle:true,pointStyleWidth:6}},tooltip:{callbacks:{label:c=>` ${c.label}: ${c.parsed}€`}}},animation:{animateRotate:true,duration:600}}}); };
        chartInstance  = renderBilan('bilan_table',  'budgetChart',   chartInstance);
        renderBilanCartes(totaux);
        renderBilanResume(totalGeneral);
        chartInstanceD = renderBilan('bilan_table_d','budgetChart_d', chartInstanceD);

        const top3Card=document.getElementById('top3-card'), top3List=document.getElementById('top3-list');
        if(top3Card&&top3List){ const scored=db.categories.map(cat=>{ const prev=db.previsions[cat.id]||0,reel=totaux[cat.id]||0,pct=prev>0?(reel/prev)*100:(reel>0?999:0); return{label:cat.label,prev,reel,pct}; }).filter(c=>c.reel>0).sort((a,b)=>b.pct-a.pct).slice(0,3); if(scored.length>0){ top3Card.style.display='block'; const rankClass=['r1','r2','r3'],rankEmoji=['','',''],conseils=(c)=>{ if(c.pct>=100) return`Dépassement de ${Math.round(c.reel-c.prev)}€ — à réduire le mois prochain`; if(c.pct>=80) return`${Math.round(c.pct)}% du budget utilisé — attention`; return`${Math.round(c.pct)}% utilisé — surveille cette catégorie`; }; top3List.innerHTML=scored.map((c,i)=>`<div class="top3-item"><div class="top3-rank ${rankClass[i]}">${i+1}</div><div class="top3-info"><div class="top3-name">${rankEmoji[i]} ${c.label}</div><div class="top3-detail">${conseils(c)}</div></div><div class="top3-bar-wrap"><div class="top3-bar-bg"><div class="top3-bar-fill" style="width:${Math.min(c.pct,100)}%;background:${c.pct>=100?'#ef4444':c.pct>=80?'#f59e0b':'#5b5ef4'}"></div></div><div class="top3-pct">${c.prev>0?Math.round(c.pct)+'%':c.reel+'€'}</div></div></div>`).join(''); } else { top3Card.style.display='none'; } }
        majSuggestions(totaux);
        if (typeof syncSoloBudget === 'function') { clearTimeout(window._soloSyncTimer); window._soloSyncTimer = setTimeout(syncSoloBudget, 800); }
        majProjection(totaux, totalGeneral);
        if (typeof majPrevisionsCatSelect === 'function') majPrevisionsCatSelect();
    }

    // Couleur d'une catégorie archivée : celle de la catégorie actuelle si elle existe encore
    function couleurCatArchive(arc, i) {
        const id = arc.catIds ? arc.catIds[i] : null;
        const cat = db.categories.find(c => c.id === id) || db.categories.find(c => c.label === arc.labels[i]);
        const pal = ['#4f46e5', '#0e7490', '#d97706', '#be185d', '#047857', '#9333ea', '#64748b', '#c2410c'];
        return (cat && cat.color) || pal[i % pal.length];
    }
    function budgetArchive(arc) {
        if (!arc.previsions || !arc.catIds) return 0;
        return arc.catIds.reduce((s, id, i) => (arc.labels[i] || '').toLowerCase().includes('épargne') ? s : s + (parseFloat(arc.previsions[id]) || 0), 0);
    }
    function buildArchiveCards(containerId) {
        const container = document.getElementById(containerId); if (!container) return;
        const cnt = document.getElementById('ar-count'); if (cnt && containerId === 'archives-container') cnt.innerText = archives.length ? archives.length + ' mois' : '';
        if (archives.length === 0) { container.innerHTML = `<div class="card"><div class="empty-state"><p>Aucun mois archivé.<br>Clôture ton premier mois pour le voir ici.</p></div></div>`; return; }
        const fmt = n => (Math.round(parseFloat(n) * 100) / 100).toFixed(parseFloat(n) % 1 ? 2 : 0) + ' €';
        container.innerHTML = '<div class="archives-grid"></div>'; const grid = container.querySelector('.archives-grid');
        archives.slice().reverse().forEach(arc => {
            const dep = parseFloat(arc.totalDep) || 0, ep = parseFloat(arc.totalEp) || 0, solde = parseFloat(arc.solde) || 0, rev = parseFloat(arc.revenu) || 0;
            const budget = budgetArchive(arc), over = budget > 0 && dep > budget, pct = budget > 0 ? Math.round(dep / budget * 100) : null;
            // anneau de répartition (couleurs des catégories)
            const parts = (arc.data || []).map((v, i) => ({ v: parseFloat(v) || 0, c: couleurCatArchive(arc, i) })).filter(p => p.v > 0);
            const tot = parts.reduce((s, p) => s + p.v, 0); let acc = 0; const st = [];
            parts.forEach(p => { const L = p.v / tot * 100, g = parts.length > 1 ? 1 : 0; st.push(`${p.c} ${acc.toFixed(2)}% ${(acc + L - g).toFixed(2)}%`); if (g) st.push(`var(--card) ${(acc + L - g).toFixed(2)}% ${(acc + L).toFixed(2)}%`); acc += L; });
            const ring = tot > 0 ? `conic-gradient(${st.join(', ')})` : 'var(--track)';
            const pill = budget > 0 ? `<span class="bc-pill ${over ? 'over' : 'ok'}">${over ? 'Dépassé' : 'Dans le budget'}</span>` : '';
            const card = document.createElement('div'); card.className = 'am-card';
            card.innerHTML = `<div class="am-top">
                    <div class="am-ring" style="background:${ring}"><div class="am-hole" style="color:${over ? 'var(--danger)' : 'var(--text)'}">${pct !== null ? pct + '%' : ''}</div></div>
                    <div class="am-info"><span class="am-name">${arc.nom}</span><span class="am-sub">${budget > 0 ? fmt(dep) + ' sur ' + fmt(budget) : 'Archivé le ' + arc.date}</span>${pill ? '<span class="am-pill">' + pill + '</span>' : ''}</div>
                </div>
                <div class="am-stats">
                    <div class="am-stat"><span>Revenus</span><strong>${fmt(rev)}</strong></div>
                    <div class="am-stat"><span>Dépensé</span><strong>${fmt(dep)}</strong></div>
                    <div class="am-stat"><span>Épargné</span><strong style="color:var(--success)">${fmt(ep)}</strong></div>
                    <div class="am-stat"><span>Solde</span><strong style="color:${solde < 0 ? 'var(--danger)' : 'var(--success)'}">${solde >= 0 ? '+' : '−'}${fmt(Math.abs(solde))}</strong></div>
                </div>
                <div class="am-actions">
                    <button class="am-pdf" onclick="telechargerPDF(${arc.id})">Exporter en PDF</button>
                    <button class="am-del" aria-label="Supprimer ce mois" onclick="supprimerArchive(${arc.id})"><svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M8 6V4h8v2"/><path d="M19 6l-1 14H6L5 6"/></svg></button>
                </div>
                <canvas id="arc-chart-${arc.id}-${containerId}" width="240" height="240" style="display:none;"></canvas>`;
            grid.appendChild(card);
            // camembert caché, utilisé pour l'export PDF
            setTimeout(() => { const ctx = document.getElementById(`arc-chart-${arc.id}-${containerId}`); if (!ctx || ctx._done) return; ctx._done = true; new Chart(ctx, { type: 'pie', data: { labels: arc.labels, datasets: [{ data: arc.data, backgroundColor: arc.labels.map((l, i) => couleurCatArchive(arc, i)), borderWidth: 2, borderColor: '#fff' }] }, options: { responsive: false, animation: false, plugins: { legend: { display: false }, tooltip: { enabled: false } } } }); }, 60);
        });
    }
    function afficherArchives() { buildArchiveCards('archives-container'); buildArchiveCards('archives-container-d'); }

    function buildEvolution(canvasId,sectionId) {
        const section=document.getElementById(sectionId); if(!section) return;
        if(archives.length<1){ section.style.display='none'; return; } section.style.display=sectionId==='evolution-section'?'flex':'block';
        const sorted=[...archives].sort((a,b)=>a.id-b.id); const isDark=document.documentElement.getAttribute('data-theme')==='dark'; const gc=isDark?'rgba(255,255,255,0.06)':'rgba(0,0,0,0.05)',tc=isDark?'#9ca3af':'#6b7280';
        const main=getComputedStyle(document.documentElement).getPropertyValue('--main').trim()||'#4f46e5';
        const succ=isDark?'#34d399':'#0b7a53';
        const series=[['Revenus',sorted.map(a=>a.revenu),'#64748b',false],['Dépenses',sorted.map(a=>a.totalDep),main,false],['Épargne',sorted.map(a=>a.totalEp),succ,false],['Solde',sorted.map(a=>a.solde),'#d97706',true]];
        const leg=document.getElementById(sectionId==='evolution-section'?'evo-legend':'evo-legend-d');
        if(leg) leg.innerHTML=series.map(s=>`<span><i style="background:${s[2]}"></i>${s[0]}</span>`).join('');
        const ctx=document.getElementById(canvasId); if(!ctx) return; const inst=canvasId==='evolutionChart'?evolutionChartInstance:evolutionChartInstanceD; if(inst) inst.destroy();
        const court=n=>{ const m=String(n).split(' ')[0]; return m.length>5?m.slice(0,4)+'.':m; };
        const newInst=new Chart(ctx,{type:'line',data:{labels:sorted.map(a=>court(a.nom)),datasets:series.map(s=>({label:s[0],data:s[1],borderColor:s[2],backgroundColor:s[2],borderWidth:2.5,borderDash:s[3]?[5,4]:[],pointRadius:3.5,pointBackgroundColor:isDark?'#1a1b21':'#fff',pointBorderWidth:2.5,fill:false,tension:0.35}))},options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},plugins:{legend:{display:false},tooltip:{backgroundColor:isDark?'#1a1b21':'#fff',titleColor:isDark?'#e8eaf2':'#1a1d2e',bodyColor:tc,borderColor:isDark?'rgba(255,255,255,0.1)':'#e5e7eb',borderWidth:1,padding:10,callbacks:{title:items=>sorted[items[0].dataIndex].nom,label:c=>` ${c.dataset.label} : ${parseFloat(c.parsed.y).toFixed(2)} €`}}},scales:{x:{ticks:{color:tc,font:{family:'Manrope',size:11,weight:'600'}},grid:{display:false},border:{display:false}},y:{ticks:{color:tc,font:{family:'Manrope',size:10},callback:v=>v+' €',maxTicksLimit:5},grid:{color:gc},border:{display:false}}},animation:{duration:700}}});
        if(canvasId==='evolutionChart') evolutionChartInstance=newInst; else evolutionChartInstanceD=newInst;
    }
    function afficherEvolution() { buildEvolution('evolutionChart','evolution-section'); buildEvolution('evolutionChartD','evolution-section-d'); afficherEvolutionCategorie(); }

    function afficherEvolutionCategorie() {
        const card=document.getElementById('cat-evolution-card'); if(!card) return;
        if(archives.length<2){ card.style.display='none'; return; } card.style.display='flex';
        const sel=document.getElementById('cat-select');
        if(sel){ const cur=sel.value; sel.innerHTML=''; db.categories.forEach(cat=>{ sel.innerHTML+=`<option value="${cat.id}">${cat.label}</option>`; }); if(cur&&db.categories.find(c=>c.id===cur)) sel.value=cur; }
        const selectedId=sel?sel.value:(db.categories[0]?db.categories[0].id:null); if(!selectedId) return;
        const selectedLabel=db.categories.find(c=>c.id===selectedId)?.label||'';
        const sorted=[...archives].sort((a,b)=>a.id-b.id);
        const labels=sorted.map(a=>a.nom);
        const data=sorted.map(arc=>{ let idx=arc.catIds?arc.catIds.indexOf(selectedId):-1; if(idx===-1) idx=arc.labels.indexOf(selectedLabel); return idx!==-1?(arc.data[idx]||0):0; });
        const prevData=sorted.map(arc=>{ let idx=arc.catIds?arc.catIds.indexOf(selectedId):-1; return idx!==-1&&arc.previsions?(arc.previsions[selectedId]||0):0; });
        const isDark=document.documentElement.getAttribute('data-theme')==='dark'; const gc=isDark?'rgba(255,255,255,0.06)':'rgba(0,0,0,0.05)',tc=isDark?'#9ca3af':'#6b7280';
        const cat=db.categories.find(c=>c.id===selectedId); const col=(cat&&cat.color)||'#4f46e5';
        const danger=isDark?'#f87171':'#c2410c', hint=isDark?'#6b707b':'#a3a8b3';
        const dot=document.getElementById('cat-legend-dot'); if(dot) dot.style.background=col;
        const court=n=>{ const m=String(n).split(' ')[0]; return m.length>5?m.slice(0,4)+'.':m; };
        const fmt=n=>(Math.round(n*100)/100).toFixed(n%1?2:0)+' €';
        if(catChartInstance){ catChartInstance.destroy(); catChartInstance=null; }
        const ctx=document.getElementById('catEvolutionChart'); if(!ctx) return;
        const ptCol=data.map((v,i)=>prevData[i]>0&&v>prevData[i]?danger:col);
        // Affiche le montant au-dessus de chaque point
        const valeurs={id:'valeursPoints',afterDatasetsDraw(chart){ const c=chart.ctx, meta=chart.getDatasetMeta(0); c.save(); c.font='700 11px Manrope, system-ui, sans-serif'; c.textAlign='center'; meta.data.forEach((p,i)=>{ c.fillStyle=ptCol[i]; c.fillText(Math.round(data[i])+' €',p.x,p.y-11); }); c.restore(); }};
        const g=ctx.getContext('2d').createLinearGradient(0,0,0,190); g.addColorStop(0,col+(isDark?'40':'26')); g.addColorStop(1,col+'00');
        catChartInstance=new Chart(ctx,{type:'line',plugins:[valeurs],data:{labels:sorted.map(a=>court(a.nom)),datasets:[{label:'Réel',data,borderColor:col,backgroundColor:g,borderWidth:3,pointRadius:5,pointHoverRadius:6,pointBackgroundColor:isDark?'#1a1b21':'#fff',pointBorderColor:ptCol,pointBorderWidth:3,fill:true,tension:0.35},{label:'Prévu',data:prevData,borderColor:hint,borderWidth:2,borderDash:[6,5],pointRadius:0,fill:false,tension:0}]},options:{responsive:true,maintainAspectRatio:false,layout:{padding:{top:18,left:6,right:18}},interaction:{mode:'index',intersect:false},plugins:{legend:{display:false},tooltip:{backgroundColor:isDark?'#1a1b21':'#fff',titleColor:isDark?'#e8eaf2':'#1a1d2e',bodyColor:tc,borderColor:isDark?'rgba(255,255,255,0.1)':'#e5e7eb',borderWidth:1,padding:10,callbacks:{title:items=>sorted[items[0].dataIndex].nom,label:c=>` ${c.dataset.label} : ${parseFloat(c.parsed.y).toFixed(2)} €`}}},scales:{x:{ticks:{color:tc,font:{family:'Manrope',size:11,weight:'600'}},grid:{display:false},border:{display:false}},y:{beginAtZero:true,ticks:{color:tc,font:{family:'Manrope',size:10},callback:v=>v+' €',maxTicksLimit:4},grid:{color:gc},border:{display:false}}},animation:{duration:600}}});
        // Tuiles : prévu, moyenne, écart avec le mois précédent
        const tiles=document.getElementById('cat-tiles');
        if(tiles){
            const prevActuel=prevData[prevData.length-1]||(db.previsions[selectedId]||0);
            const moy=data.reduce((a,b)=>a+b,0)/(data.length||1);
            const diff=data.length>1?data[data.length-1]-data[data.length-2]:0;
            const estEp=(cat&&cat.label.toLowerCase().includes('épargne'));
            const bon=estEp?diff>=0:diff<=0;
            tiles.innerHTML=`<div class="ar-tile"><span>Prévu</span><strong>${fmt(prevActuel)}</strong></div><div class="ar-tile"><span>Moyenne</span><strong>${fmt(Math.round(moy))}</strong></div>`
                +(data.length>1?`<div class="ar-tile ${diff===0?'':bon?'pos':'neg'}"><span>vs ${court(sorted[sorted.length-2].nom).toLowerCase()}</span><strong>${diff===0?'=':diff>0?'↑ ':'↓ '}${diff===0?'Stable':fmt(Math.abs(Math.round(diff)))}</strong></div>`:'');
        }
        // Tendances de toutes les catégories vs mois précédent
        const tendDiv=document.getElementById('cat-tendance'); if(!tendDiv||sorted.length<2){ if(tendDiv) tendDiv.innerHTML=''; return; }
        const dernierMois=sorted[sorted.length-1], moisPrec=sorted[sorted.length-2];
        const tendances=db.categories.map(c=>{ const getVal=(arc)=>{ let idx=arc.catIds?arc.catIds.indexOf(c.id):-1; if(idx===-1) idx=arc.labels.indexOf(c.label); return idx!==-1?(arc.data[idx]||0):0; }; const valPrec=getVal(moisPrec),valDern=getVal(dernierMois); if(valPrec===0&&valDern===0) return null; return{label:c.label,valPrec,valDern,diff:valDern-valPrec}; }).filter(Boolean).sort((a,b)=>Math.abs(b.diff)-Math.abs(a.diff));
        if(tendances.length===0){ tendDiv.innerHTML=''; return; }
        tendDiv.innerHTML=`<div class="ar-tend"><div class="ar-tend-h">Toutes les catégories vs ${moisPrec.nom}</div>`+tendances.map(t=>{ const k=t.diff===0?'flat':t.diff>0?'up':'down'; const ep=t.label.toLowerCase().includes('épargne'); const kc=k==='flat'?'flat':((k==='up')!==ep?'up':'down'); return `<div class="ar-tend-row"><span class="ar-tend-ico ${kc}">${k==='flat'?'=':k==='up'?'↑':'↓'}</span><div class="ar-tend-main"><div class="ar-tend-name">${t.label}</div><div class="ar-tend-det">${fmt(parseFloat(t.valPrec))} → ${fmt(parseFloat(t.valDern))}</div></div><span class="ar-tend-val ${kc}">${k==='flat'?'Stable':(t.diff>0?'+':'−')+fmt(Math.abs(t.diff))}</span></div>`; }).join('')+'</div>';
    }

    function supprimerArchive(id) { if(!confirm("Supprimer cette archive ?")) return; archives=archives.filter(a=>a.id!==id); localStorage.setItem('budget_archives',JSON.stringify(archives)); afficherArchives(); afficherEvolution(); showToast('Archive supprimée','warning'); }

    function telechargerPDF(id) {
        const arc=archives.find(a=>a.id===id); if(!arc) return;
        const canvasIds=[`arc-chart-${arc.id}-archives-container`,`arc-chart-${arc.id}-archives-container-d`];
        let chartImg=''; for(const cid of canvasIds){ const c=document.getElementById(cid); if(c){ chartImg=c.toDataURL('image/png'); break; } }
        const sc=arc.solde<0?'#ef4444':'#10b981';
        const compRows=arc.labels.map((label,i)=>{ const catId=arc.catIds?arc.catIds[i]:null,prev=(arc.previsions&&catId)?(arc.previsions[catId]||0):0,reel=arc.data[i]||0,pct=prev>0?Math.min(Math.round((reel/prev)*100),100):0,over=prev>0&&reel>prev,barColor=over?'#ef4444':pct>=80?'#f59e0b':'#5b5ef4',ecart=prev-reel; return`<tr><td style="padding:10px 12px;font-weight:600;font-size:0.85rem;">${label}</td><td style="padding:10px 12px;color:#6b7280;font-size:0.85rem;">${parseFloat(prev).toFixed(2)} €</td><td style="padding:10px 12px;font-weight:700;font-size:0.85rem;">${parseFloat(reel).toFixed(2)} €</td><td style="padding:10px 12px;color:${ecart<0?'#ef4444':'#10b981'};font-weight:700;font-size:0.85rem;">${ecart>=0?'+':''}${parseFloat(ecart).toFixed(2)} €</td><td style="padding:10px 12px;min-width:120px;"><div style="background:#f3f4f6;height:8px;border-radius:4px;overflow:hidden;margin-bottom:3px;"><div style="height:100%;width:${pct}%;background:${barColor};border-radius:4px;"></div></div><span style="font-size:0.72rem;color:#6b7280;">${prev>0?pct+'%':'—'}</span></td></tr>`; }).join('');
        const html=`<!DOCTYPE html><html lang="fr"><head><meta charset="UTF-8"><title>Bilan ${arc.nom}</title><style>@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;700;800&display=swap');*{box-sizing:border-box;margin:0;padding:0;}body{font-family:'Manrope',Arial,sans-serif;background:white;color:#1a1d2e;padding:40px 36px;max-width:700px;margin:auto;}.header{display:flex;justify-content:space-between;align-items:flex-end;border-bottom:3px solid #5b5ef4;padding-bottom:16px;margin-bottom:28px;}.title{font-family:'Manrope',serif;font-size:1.8rem;color:#5b5ef4;}.sub{color:#6b7280;font-size:0.82rem;margin-top:4px;}.header-right{text-align:right;font-size:0.8rem;color:#6b7280;}.stats-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:28px;}.stat{background:#f9fafb;border:1px solid #e5e7eb;border-radius:10px;padding:12px;text-align:center;}.stat .lbl{font-size:0.66rem;color:#6b7280;text-transform:uppercase;letter-spacing:0.08em;margin-bottom:5px;}.stat .val{font-size:1.3rem;font-weight:700;}.section-title{font-size:0.72rem;font-weight:700;color:#6b7280;text-transform:uppercase;letter-spacing:0.08em;margin-bottom:12px;padding-bottom:8px;border-bottom:1px solid #e5e7eb;}.chart-row{display:flex;gap:24px;align-items:center;margin-bottom:28px;}.chart-row img{width:140px;height:140px;flex-shrink:0;}.legend{flex:1;}.leg-item{display:flex;align-items:center;gap:8px;margin-bottom:6px;font-size:0.83rem;}.leg-dot{width:11px;height:11px;border-radius:50%;flex-shrink:0;}table{width:100%;border-collapse:collapse;margin-bottom:28px;}thead th{background:#f9fafb;padding:9px 12px;text-align:left;font-size:0.7rem;color:#6b7280;text-transform:uppercase;letter-spacing:0.06em;border-bottom:2px solid #e5e7eb;}tbody tr:nth-child(even){background:#fafafa;}tbody tr{border-bottom:1px solid #f3f4f6;}.footer{margin-top:24px;font-size:0.7rem;color:#9ca3af;text-align:center;border-top:1px solid #e5e7eb;padding-top:14px;}@media print{body{padding:20px;}}</style></head><body><div class="header"><div><div class="title">📊 ${arc.nom}</div><div class="sub">Archivé le ${arc.date} · Mon Coach Finance</div></div><div class="header-right">Revenus : <strong>${parseFloat(arc.revenu).toFixed(2)} €</strong></div></div><div class="stats-grid"><div class="stat"><div class="lbl">💸 Dépensé</div><div class="val">${parseFloat(arc.totalDep).toFixed(2)} €</div></div><div class="stat"><div class="lbl">🌱 Épargné</div><div class="val" style="color:#10b981">${parseFloat(arc.totalEp).toFixed(2)} €</div></div><div class="stat"><div class="lbl">⚖️ Solde</div><div class="val" style="color:${sc}">${parseFloat(arc.solde).toFixed(2)} €</div></div><div class="stat"><div class="lbl">📥 Revenus</div><div class="val">${parseFloat(arc.revenu).toFixed(2)} €</div></div></div><div class="section-title">Répartition des dépenses</div><div class="chart-row">${chartImg?`<img src="${chartImg}">`:''}<div class="legend">${arc.labels.map((l,i)=>`<div class="leg-item"><span class="leg-dot" style="background:${colors[i%colors.length]}"></span><span style="flex:1">${l}</span><strong>${parseFloat(arc.data[i]).toFixed(2)} €</strong></div>`).join('')}</div></div><div class="section-title">Prévu vs Réel par catégorie</div><table><thead><tr><th>Catégorie</th><th>Prévu</th><th>Réel</th><th>Écart</th><th>Progression</th></tr></thead><tbody>${compRows}</tbody></table><div class="footer">Généré par Mon Coach Finance · ${new Date().toLocaleDateString('fr-FR')}</div></body></html>`;
        const blob=new Blob([html],{type:'text/html'}); const url=URL.createObjectURL(blob); const win=window.open(url,'_blank'); if(win) win.addEventListener('load',()=>setTimeout(()=>win.print(),600)); showToast('PDF amélioré prêt 🎉','info',4000);
    }

    majAffichage(); afficherArchives(); afficherEvolution();
    document.getElementById('new_cat_name').addEventListener('keydown', e=>{ if(e.key==='Enter') ajouterNouvelleCategorie(); });

    /* ══════════════════════════════════
       BUDGET COUPLE — SUPABASE REALTIME
    ══════════════════════════════════ */
    const SUPABASE_URL = 'https://qridhnhidcrfffzejzgt.supabase.co';
    const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InFyaWRobmhpZGNyZmZmemVqemd0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzY2ODk0MzIsImV4cCI6MjA5MjI2NTQzMn0.aRZyOhnFNn-1uUY5fArvqVlmEoGgvLXNTAsJ2zlM1GM';

    const COUPLE_CATS = [
        { id:'sorties',  label:'Sorties / Loisirs', budget:100 },
        { id:'resto',    label:'Restaurants',        budget:80  },
        { id:'vacances', label:'Vacances',           budget:200 },
        { id:'courses',  label:'Courses communes',   budget:60  }
    ];

    let coupleSession = localStorage.getItem('couple_session') || '';
    let coupleDepenses = [];
    let coupleBudgets = JSON.parse(localStorage.getItem('couple_budgets')) || {};
    let coupleAuteur = 'moi';
    let coupleRealtimeChannel = null;

    // Init budgets par défaut
    COUPLE_CATS.forEach(c => { if (!coupleBudgets[c.id]) coupleBudgets[c.id] = c.budget; });

    async function supabaseFetch(path, options = {}) {
        const res = await fetch(SUPABASE_URL + '/rest/v1/' + path, {
            ...options,
            headers: {
                'apikey': SUPABASE_KEY,
                'Authorization': 'Bearer ' + SUPABASE_KEY,
                'Content-Type': 'application/json',
                'Prefer': 'return=representation',
                ...options.headers
            }
        });
        if (!res.ok) throw new Error(await res.text());
        return res.json().catch(() => []);
    }

    function selectAuteur(who) {
        coupleAuteur = who;
        document.getElementById('btn-moi').className  = who === 'moi'  ? 'btn btn-primary' : 'btn btn-secondary';
        document.getElementById('btn-elle').className = who === 'elle' ? 'btn btn-primary' : 'btn btn-secondary';
        document.getElementById('btn-elle').style.background = who === 'elle' ? '#db2777' : '';
    }

    async function rejoindreSession() {
        const input = document.getElementById('couple-session-input');
        const code = input.value.trim().toUpperCase();
        if (!code) { showToast('Entre un code de session', 'warning'); return; }
        coupleSession = code;
        localStorage.setItem('couple_session', code);
        input.value = '';
        await chargerDepensesCouple();
        demarrerRealtime();
    }

    async function chargerDepensesCouple() {
        if (!coupleSession) return;
        try {
            coupleDepenses = await supabaseFetch(`couple_budget?session_id=eq.${encodeURIComponent(coupleSession)}&order=created_at.desc`);
            await chargerArchivesCouple();
            majAffichageCouple();
            majSyncDot(true);
        } catch(e) {
            majSyncDot(false);
            showToast('Erreur de connexion Supabase', 'danger');
        }
    }

    async function chargerArchivesCouple() {
        try {
            const rows = await supabaseFetch(`couple_archives?session_id=eq.${encodeURIComponent(coupleSession)}&order=id.asc`);
            if (!rows || rows.length === 0) return;
            const local = JSON.parse(localStorage.getItem('couple_archives') || '[]');
            const localIds = new Set(local.map(a => a.id));
            rows.forEach(row => {
                if (row.data && !localIds.has(row.data.id)) {
                    local.push(row.data);
                    localIds.add(row.data.id);
                }
            });
            localStorage.setItem('couple_archives', JSON.stringify(local));
        } catch(e) {
            console.warn('Chargement archives couple Supabase échoué :', e);
        }
    }

    function demarrerRealtime() {
        if (coupleRealtimeChannel) clearInterval(coupleRealtimeChannel);
        // Polling toutes les 3 secondes (Supabase Realtime via WebSocket nécessite la lib JS)
        coupleRealtimeChannel = setInterval(chargerDepensesCouple, 3000);
        document.getElementById('session-label').textContent = coupleSession;
        majSyncDot(true);
    }

    function majSyncDot(ok) {
        const dot = document.getElementById('sync-dot');
        const label = document.getElementById('session-label');
        if (dot) { dot.className = 'sync-dot' + (ok ? '' : ' off'); }
        if (label && coupleSession) label.textContent = coupleSession;
    }

    function copierSessionCouple() {
        if (!coupleSession) { showToast('Pas de session active', 'warning'); return; }
        navigator.clipboard.writeText(coupleSession).then(() => showToast('Code "' + coupleSession + '" copié ! 📋', 'success'));
    }

    async function ajouterDepenseCouple() {
        if (!coupleSession) { showToast('Rejoins une session d\'abord', 'warning'); return; }
        const desc = document.getElementById('couple-desc').value.trim();
        const mt   = parseFloat(document.getElementById('couple-mt').value);
        const cat  = document.getElementById('couple-cat').value;
        if (!desc || isNaN(mt) || mt <= 0) { showToast('Remplis tous les champs', 'warning'); return; }
        try {
            await supabaseFetch('couple_budget', {
                method: 'POST',
                body: JSON.stringify({
                    session_id: coupleSession,
                    nom: desc, montant: mt, categorie: cat,
                    date: new Date().toLocaleDateString('fr-FR'),
                    auteur: coupleAuteur
                })
            });
            document.getElementById('couple-desc').value = '';
            document.getElementById('couple-mt').value = '';
            showToast(`${desc} — ${mt.toFixed(2)} € ajouté 💑`, 'success');
            await chargerDepensesCouple();
        } catch(e) { showToast('Erreur lors de l\'ajout', 'danger'); }
    }

    async function supprimerDepenseCouple(id) {
        try {
            await supabaseFetch(`couple_budget?id=eq.${id}`, { method: 'DELETE' });
            await chargerDepensesCouple();
            showToast('Dépense supprimée', 'danger');
        } catch(e) { showToast('Erreur suppression', 'danger'); }
    }

    function majAffichageCouple() {
        const hasSession = !!coupleSession;
        ['couple-main-card','couple-cats-card','couple-form-card','couple-history-card','couple-cloture-card'].forEach(id => {
            const el = document.getElementById(id);
            if (el) el.style.display = hasSession ? 'block' : 'none';
        });
        // Archives toujours visibles si on en a
        const arcCard = document.getElementById('couple-archives-card');
        const coupleArchives = JSON.parse(localStorage.getItem('couple_archives') || '[]');
        if (arcCard) arcCard.style.display = coupleArchives.length > 0 ? 'block' : 'none';
        if (!hasSession) { majArchivesCouple(); return; }

        // Peuple le select catégorie du formulaire
        const sel = document.getElementById('couple-cat');
        if (sel) {
            const curVal = sel.value;
            sel.innerHTML = '';
            COUPLE_CATS.forEach(c => { sel.innerHTML += `<option value="${c.id}">${c.label}</option>`; });
            if (curVal && COUPLE_CATS.find(c => c.id === curVal)) sel.value = curVal;
        }

        // Peuple le filtre catégorie historique
        const filtSel = document.getElementById('couple-filtre-cat');
        if (filtSel) {
            const curFilt = filtSel.value;
            filtSel.innerHTML = '<option value="">Toutes les catégories</option>';
            COUPLE_CATS.forEach(c => { filtSel.innerHTML += `<option value="${c.id}">${c.label}</option>`; });
            if (curFilt) filtSel.value = curFilt;
        }

        // Calcul totaux
        let totalDep = 0;
        const parCat = {};
        COUPLE_CATS.forEach(c => { parCat[c.id] = 0; });
        coupleDepenses.forEach(d => { totalDep += d.montant; if (parCat[d.categorie] !== undefined) parCat[d.categorie] += d.montant; });
        const totalBudget = COUPLE_CATS.reduce((s, c) => s + (coupleBudgets[c.id] || 0), 0);
        const pct = totalBudget > 0 ? Math.min((totalDep / totalBudget) * 100, 100) : 0;
        const reste = totalBudget - totalDep;

        // Mise à jour des labels noms
        var lblMoi = document.getElementById('label-moi');
        var lblPartner = document.getElementById('label-partner');
        if (lblMoi) lblMoi.textContent = appSettings.nomMoi || 'Moi';
        if (lblPartner) lblPartner.textContent = appSettings.nomPartner || 'Ma copine';

        document.getElementById('couple-total-dep').textContent = totalDep.toFixed(2) + ' €';
        document.getElementById('couple-total-budget').textContent = totalBudget.toFixed(2) + ' €';
        document.getElementById('couple-pct').textContent = pct.toFixed(0) + '%';
        document.getElementById('couple-reste').textContent = (reste < 0 ? '⚠️ Dépassé de ' + Math.abs(reste).toFixed(2) : 'Reste : ' + reste.toFixed(2)) + ' €';
        const bar = document.getElementById('couple-bar');
        if (bar) { bar.style.width = pct + '%'; bar.className = 'couple-budget-fill' + (reste < 0 ? ' over' : ''); }

        // Grille catégories
        const grid = document.getElementById('couple-cat-grid');
        if (grid) {
            grid.innerHTML = COUPLE_CATS.map(c => {
                const dep = parCat[c.id] || 0, bud = coupleBudgets[c.id] || 0;
                const p = bud > 0 ? Math.min((dep/bud)*100, 100) : 0;
                const over = dep > bud && bud > 0;
                return `<div class="couple-cat-box" onclick="filtrerParCat('${c.id}')" style="cursor:pointer;">
                    <div class="couple-cat-name">${c.label}</div>
                    <div class="couple-cat-amounts"><span>${dep.toFixed(2)} €</span><span style="color:var(--text-muted)">${bud} €</span></div>
                    <div class="couple-cat-bar"><div class="couple-cat-fill${over?' over':''}" style="width:${p}%"></div></div>
                </div>`;
            }).join('');
        }

        // Inputs budgets
        const inputs = document.getElementById('couple-budget-inputs');
        if (inputs && inputs.children.length === 0) {
            inputs.innerHTML = COUPLE_CATS.map(c => `
                <div class="budget-row" id="couple-row-${c.id}">
                    <button class="btn-icon-sm" onclick="supprimerCategorieCouple('${c.id}')">✕</button>
                    <span class="cat-label">${c.label}</span>
                    <input type="number" inputmode="decimal" id="couple-bud-${c.id}" value="${coupleBudgets[c.id]||0}" style="width:80px;margin:0;" onchange="miseAJourBudgetCouple('${c.id}', this.value)">
                </div>`).join('');
        } else if (inputs) {
            COUPLE_CATS.forEach(c => {
                const el = document.getElementById('couple-bud-' + c.id);
                if (el && document.activeElement !== el) el.value = coupleBudgets[c.id] || 0;
            });
        }

        // Historique avec filtre
        const filtre = (document.getElementById('couple-filtre-cat') || {}).value || '';
        const hist = document.getElementById('couple-history-list');
        const depFiltrees = filtre ? coupleDepenses.filter(d => d.categorie === filtre) : coupleDepenses;
        if (hist) {
            if (depFiltrees.length === 0) {
                hist.innerHTML = `<div class="empty-state" style="padding:20px"><div class="empty-icon">💸</div><p>${filtre ? 'Aucune dépense dans cette catégorie.' : 'Aucune dépense commune pour l\'instant.'}</p></div>`;
            } else {
                hist.innerHTML = depFiltrees.map(d => `
                    <div class="couple-dep-row">
                        <div>
                            <div style="font-weight:600">${d.nom}</div>
                            <div style="font-size:0.73rem;color:var(--text-muted)">${d.date} · ${COUPLE_CATS.find(c=>c.id===d.categorie)?.label||d.categorie}</div>
                        </div>
                        <div style="display:flex;align-items:center;gap:8px">
                            <span class="couple-dep-who ${d.auteur==='moi'?'who-moi':'who-elle'}">${d.auteur==='moi'?(appSettings.nomMoi||'Moi'):(appSettings.nomPartner||'Elle')}</span>
                            <strong>${parseFloat(d.montant).toFixed(2)} €</strong>
                            <button class="btn-delete" onclick="supprimerDepenseCouple('${d.id}')">✕</button>
                        </div>
                    </div>`).join('');
            }
        }

        majArchivesCouple();
    }

    function filtrerParCat(catId) {
        const sel = document.getElementById('couple-filtre-cat');
        if (!sel) return;
        sel.value = sel.value === catId ? '' : catId;
        majAffichageCouple();
    }

    /* ── CLÔTURE MENSUELLE COUPLE ── */
    function cloturerMoisCouple() {
        if (!coupleSession) return;
        const moisNoms = ['Janvier','Février','Mars','Avril','Mai','Juin','Juillet','Août','Septembre','Octobre','Novembre','Décembre'];
        const now = new Date();
        const nom = prompt('Nom du mois à archiver :', `${moisNoms[now.getMonth()]} ${now.getFullYear()}`);
        if (!nom) return;

        // Calcul des totaux
        let totalDep = 0, totalMoi = 0, totalElle = 0;
        const parCat = {};
        COUPLE_CATS.forEach(c => { parCat[c.id] = 0; });
        coupleDepenses.forEach(d => {
            totalDep += d.montant;
            if (d.auteur === 'moi') totalMoi += d.montant; else totalElle += d.montant;
            if (parCat[d.categorie] !== undefined) parCat[d.categorie] += d.montant;
        });
        const totalBudget = COUPLE_CATS.reduce((s, c) => s + (coupleBudgets[c.id] || 0), 0);

        const archiveId = Date.now();
        const archive = {
            id: archiveId, nom, date: now.toLocaleDateString('fr-FR'),
            totalDep, totalBudget, totalMoi, totalElle,
            parCat: { ...parCat },
            cats: COUPLE_CATS.map(c => ({ ...c })),
            budgets: { ...coupleBudgets },
            depenses: [...coupleDepenses]
        };

        // Sauvegarde locale
        const coupleArchives = JSON.parse(localStorage.getItem('couple_archives') || '[]');
        coupleArchives.push(archive);
        localStorage.setItem('couple_archives', JSON.stringify(coupleArchives));

        // Sauvegarde Supabase — partagée avec le/la partenaire
        supabaseFetch('couple_archives', {
            method: 'POST',
            body: JSON.stringify({ id: archiveId, session_id: coupleSession, data: archive })
        }).catch(e => console.warn('Sauvegarde archive Supabase échouée :', e));

        // Supprime les dépenses Supabase de cette session
        supprimerToutesDepensesCouple();
        showToast(`"${nom}" archivé 🎉`, 'success', 4000);
    }

    async function supprimerToutesDepensesCouple() {
        try {
            await supabaseFetch(`couple_budget?session_id=eq.${encodeURIComponent(coupleSession)}`, { method: 'DELETE' });
            coupleDepenses = [];
            majAffichageCouple();
        } catch(e) { showToast('Erreur suppression Supabase', 'danger'); }
    }

    function majArchivesCouple() {
        const arcCard = document.getElementById('couple-archives-card');
        const arcList = document.getElementById('couple-archives-list');
        if (!arcCard || !arcList) return;
        const coupleArchives = JSON.parse(localStorage.getItem('couple_archives') || '[]');
        if (coupleArchives.length === 0) { arcCard.style.display = 'none'; return; }
        arcCard.style.display = 'block';
        arcList.innerHTML = [...coupleArchives].reverse().map(arc => {
            const sc = arc.totalDep > arc.totalBudget ? 'var(--danger)' : 'var(--success)';
            return `<div class="couple-arc-card">
                <div class="couple-arc-header">
                    <div>
                        <div class="couple-arc-nom">💑 ${arc.nom}</div>
                        <div class="couple-arc-date">Archivé le ${arc.date}</div>
                    </div>
                    <button class="btn-delete" onclick="supprimerArchiveCouple(${arc.id})">✕</button>
                </div>
                <div class="couple-arc-stats">
                    <div class="couple-arc-stat"><div class="lbl">💸 Dépensé</div><div class="val" style="color:${sc}">${arc.totalDep.toFixed(2)} €</div></div>
                    <div class="couple-arc-stat"><div class="lbl">👤 Moi</div><div class="val" style="color:var(--main)">${arc.totalMoi.toFixed(2)} €</div></div>
                    <div class="couple-arc-stat"><div class="lbl">👤 Elle</div><div class="val" style="color:#db2777">${arc.totalElle.toFixed(2)} €</div></div>
                </div>
            </div>`;
        }).join('');
    }

    function supprimerArchiveCouple(id) {
        if (!confirm('Supprimer cette archive ?')) return;
        let coupleArchives = JSON.parse(localStorage.getItem('couple_archives') || '[]');
        coupleArchives = coupleArchives.filter(a => a.id !== id);
        localStorage.setItem('couple_archives', JSON.stringify(coupleArchives));
        majArchivesCouple();
        showToast('Archive supprimée', 'warning');
    }

    function miseAJourBudgetCouple(catId, val) {
        coupleBudgets[catId] = parseFloat(val) || 0;
        localStorage.setItem('couple_budgets', JSON.stringify(coupleBudgets));
        majAffichageCouple();
    }

    function supprimerCategorieCouple(catId) {
        const cat = COUPLE_CATS.find(c => c.id === catId);
        if (!confirm(`Supprimer la catégorie "${cat?.label}" ?`)) return;
        const idx = COUPLE_CATS.findIndex(c => c.id === catId);
        if (idx !== -1) COUPLE_CATS.splice(idx, 1);
        delete coupleBudgets[catId];
        localStorage.setItem('couple_budgets', JSON.stringify(coupleBudgets));
        // Recrée les inputs
        const inputs = document.getElementById('couple-budget-inputs');
        if (inputs) inputs.innerHTML = '';
        majAffichageCouple();
        showToast('Catégorie supprimée', 'warning');
    }

    function ajouterCategorieCouple() {
        const inp = document.getElementById('couple-new-cat');
        const name = inp ? inp.value.trim() : '';
        if (!name) { showToast('Écris le nom de la catégorie', 'warning'); return; }
        const newId = 'cat_' + Date.now();
        COUPLE_CATS.push({ id: newId, label: name, budget: 0 });
        coupleBudgets[newId] = 0;
        localStorage.setItem('couple_budgets', JSON.stringify(coupleBudgets));
        if (inp) inp.value = '';
        // Recrée les inputs
        const inputs = document.getElementById('couple-budget-inputs');
        if (inputs) inputs.innerHTML = '';
        majAffichageCouple();
        showToast(`Catégorie "${name}" ajoutée`, 'info');
    }

    // Init si session déjà enregistrée
    if (coupleSession) {
        document.getElementById('session-label').textContent = coupleSession;
        chargerDepensesCouple().then(() => demarrerRealtime());
    }
    function renderLogList(containerId) {
        const term = ((document.getElementById('search_input_tableau') || {}).value || '').toLowerCase();
        renderHistoriqueGroupe(term, containerId, false);
    }

    function renderEpList(containerId) {
        const container=document.getElementById(containerId); if(!container) return;
        if(db.historiqueEpargne.length===0){container.innerHTML=`<div style="text-align:center;padding:10px 0;font-size:0.82rem;color:var(--text-muted);">Aucune épargne ce mois-ci.</div>`;return;}
        const sorted=[...db.historiqueEpargne].sort((a,b)=>a.desc.toLowerCase().localeCompare(b.desc.toLowerCase()));
        container.innerHTML=sorted.map((d,i)=>{
            const border=i<sorted.length-1?'border-bottom:1px solid var(--bg2);':'';
            return `<div style="display:flex;justify-content:space-between;align-items:center;padding:8px 0;${border}">
                <div style="font-size:0.88rem;color:var(--text);">${d.desc}${d.recurring?'<span class="recurring-tag">🔄</span>':''}</div>
                <div style="display:flex;align-items:center;gap:8px;">
                    <strong style="font-size:0.9rem;color:var(--success);">${parseFloat(d.mt).toFixed(2)} €</strong>
                    <button class="btn-delete" onclick="supprimer(${d.id})">✕</button>
                </div>
            </div>`;
        }).join('');
    }

    function majSuggestions(totaux) {
        const card = document.getElementById('suggestions-card');
        const list = document.getElementById('suggestions-list');
        if (!card || !list) return;

        const suggestions = [];
        db.categories.forEach(cat => {
            if (cat.label.toLowerCase().includes('épargne')) return;
            const prev = db.previsions[cat.id] || 0;
            const reel = totaux[cat.id] || 0;

            // Calcule la moyenne historique
            let sommeMois = 0, nbMois = 0;
            archives.forEach(arc => {
                const idx = arc.catIds ? arc.catIds.indexOf(cat.id) : arc.labels.indexOf(cat.label);
                if (idx !== -1) { sommeMois += arc.data[idx] || 0; nbMois++; }
            });
            const moyenne = nbMois > 0 ? sommeMois / nbMois : 0;

            if (prev > 0 && reel > prev) {
                const depasse = reel - prev;
                const suggere = Math.ceil((prev + depasse * 0.5) / 5) * 5;
                suggestions.push({ label:cat.label, msg:`Dépassement de <strong>${depasse.toFixed(0)} €</strong> — essaie de viser <strong>${suggere} €</strong> le mois prochain.`, cls:'s-over', badgeTxt:'Dépassement' });
            } else if (prev > 0 && reel <= prev * 0.5 && reel > 0) {
                const suggere = Math.ceil(reel * 1.2 / 5) * 5;
                suggestions.push({ label:cat.label, msg:`Seulement <strong>${reel.toFixed(0)} €</strong> utilisés sur <strong>${prev} €</strong> prévus — tu pourrais réduire à <strong>${suggere} €</strong>.`, cls:'s-ok', badgeTxt:'Économie possible' });
            } else if (prev === 0 && reel > 0) {
                const suggere = Math.ceil((moyenne > 0 ? moyenne : reel) * 1.1 / 5) * 5;
                suggestions.push({ label:cat.label, msg:`<strong>${reel.toFixed(0)} €</strong> dépensés sans budget planifié — pense à prévoir <strong>${suggere} €</strong> le mois prochain.`, cls:'s-info', badgeTxt:'Sans budget' });
            } else if (prev > 0 && reel === 0) {
                suggestions.push({ label:cat.label, msg:`Budget de <strong>${prev} €</strong> prévu mais aucune dépense ce mois-ci.`, cls:'s-ok', badgeTxt:'Inutilisé' });
            } else if (prev > 0 && reel <= prev) {
                const pct = Math.round((reel / prev) * 100);
                const cls = pct >= 85 ? 's-warn' : 's-ok';
                const badge = pct >= 85 ? 'Attention' : 'Dans les clous';
                suggestions.push({ label:cat.label, msg:`<strong>${reel.toFixed(0)} €</strong> sur <strong>${prev} €</strong> prévus — ${pct}% du budget utilisé.`, cls, badgeTxt:badge });
            }
        });

        if (suggestions.length === 0) { card.style.display='none'; return; }
        card.style.display = 'block';
        list.innerHTML = suggestions.map(s => `
            <div class="suggestion-item ${s.cls}">
                <div class="suggestion-header">
                    <span class="suggestion-cat">${s.label}</span>
                    <span class="suggestion-badge">${s.badgeTxt}</span>
                </div>
                <div class="suggestion-msg">${s.msg}</div>
            </div>`).join('');
    }



    function scrollDepenses() { const el=document.getElementById('expense-scroll'); if(!el) return; const atBottom=el.scrollHeight-el.scrollTop-el.clientHeight<10; el.scrollTo({top:atBottom?0:el.scrollHeight,behavior:'smooth'}); }
    function updateScrollBtn() { const el=document.getElementById('expense-scroll'),btn=document.getElementById('scroll-down-btn'); if(!el||!btn) return; const atBottom=el.scrollHeight-el.scrollTop-el.clientHeight<10; btn.innerText=atBottom?'↑':'↓'; btn.style.opacity=el.scrollHeight>el.clientHeight+4?'1':'0'; btn.style.pointerEvents=el.scrollHeight>el.clientHeight+4?'auto':'none'; }
    document.getElementById('expense-scroll').addEventListener('scroll', updateScrollBtn);
    const _origMaj=majAffichage;
    majAffichage=function(){ _origMaj(); setTimeout(updateScrollBtn,100); };

    /* ══════════════════════════════════
       PROJECTION FIN DE MOIS
    ══════════════════════════════════ */
    function majProjection(totaux, totalGeneral) {
        const cycle = getCycle();
        const jourActuel = cycle.ecoules;
        const joursRestants = cycle.restants;
        const revenuTotal = (db.revenu || 0) + (db.caf || 0);

        // On sépare les charges fixes (payées une fois : loyer, abonnements, dépenses récurrentes)
        // des dépenses variables : seules les variables servent à calculer le rythme par jour.
        const estEpargne = cat => cat && cat.label.toLowerCase().includes('épargne');
        const estFixe = (d, cat) => d.recurring === true || d.ct === 'Fixe' || (cat && /loyer|charge|abonnement/i.test(cat.label));
        let totalEp = 0, fixes = 0, variables = 0;
        db.depenses.forEach(d => {
            const cat = db.categories.find(c => c.id === d.ct);
            if (estEpargne(cat)) totalEp += d.mt;
            else if (estFixe(d, cat)) fixes += d.mt;
            else variables += d.mt;
        });
        const rythme = jourActuel > 0 ? variables / jourActuel : 0;
        const estime = fixes + variables + (rythme * joursRestants);

        // Référence : budget prévu hors épargne (sinon les revenus)
        const totalPrevu = db.categories.filter(c => !estEpargne(c)).reduce((s, c) => s + (db.previsions[c.id] || 0), 0);
        const budgetRef = totalPrevu > 0 ? totalPrevu : revenuTotal;

        const solde = revenuTotal - totalGeneral - totalEp;
        const soldeParJour = joursRestants > 0 ? Math.max(0, solde) / joursRestants : Math.max(0, solde);

        // Trop tôt dans le cycle — projection non fiable
        const tropTot = jourActuel <= 3;

        const setEl = (id, v) => { const el = document.getElementById(id); if (el) el.innerText = v; };
        setEl('m_rythme', rythme.toFixed(0));
        setEl('m_estime', tropTot ? '—' : estime.toFixed(0));
        setEl('m_jours_restants', joursRestants);
        setEl('m_budget_jour', soldeParJour.toFixed(0));
        const note = document.getElementById('m_proj_note');
        if (note) note.innerText = tropTot
            ? 'Estimation disponible à partir du 4e jour du cycle.'
            : (fixes > 0 ? 'Charges fixes (' + fixes.toFixed(0) + ' €) comptées une seule fois, le rythme ne prend que les dépenses du quotidien.' : 'Rythme calculé sur tes dépenses du quotidien.');

        // « Tu peux dépenser » : rouge si le solde est épuisé
        const bjSpan = document.getElementById('m_budget_jour');
        if (bjSpan && bjSpan.parentElement) bjSpan.parentElement.style.color = solde <= 0 ? 'var(--danger)' : 'var(--text)';
        // « Tu dépenses en moyenne » : orange si tu vas plus vite que ce que tu peux, vert sinon
        const rw = document.getElementById('m_rythme_wrap');
        if (rw) rw.style.color = tropTot ? 'var(--text)' : (rythme > soldeParJour ? 'var(--warning)' : 'var(--success)');
    }


    /* ══════════════════════════════════
       DÉPENSES PRÉVUES MOIS PROCHAIN
    ══════════════════════════════════ */
    let previsionsMoisProchain = JSON.parse(localStorage.getItem('previsions_mois') || '[]');

    function majPrevisionsCatSelect() {
        const sel = document.getElementById('prev_cat_select');
        if (!sel) return;
        const cur = sel.value;
        sel.innerHTML = '<option value="">Catégorie (optionnel)</option>';
        db.categories.forEach(c => { sel.innerHTML += `<option value="${c.id}">${c.label}</option>`; });
        if (cur) sel.value = cur;
    }

    function ajouterPrevision() {
        const desc = document.getElementById('prev_desc')?.value.trim();
        const mt = parseFloat(document.getElementById('prev_mt')?.value);
        const cat = document.getElementById('prev_cat_select')?.value || '';
        if (!desc) { showToast('Écris le nom de la dépense prévue', 'warning'); return; }
        if (isNaN(mt) || mt <= 0) { showToast('Indique un montant estimé', 'warning'); return; }
        previsionsMoisProchain.push({ id: Date.now(), desc, mt, cat });
        localStorage.setItem('previsions_mois', JSON.stringify(previsionsMoisProchain));
        document.getElementById('prev_desc').value = '';
        document.getElementById('prev_mt').value = '';
        majAffichagePrevisions();
        showToast(`"${desc}" ajouté aux prévisions`, 'info');
        const _ps = document.getElementById('prev_cat_select'); if (_ps) _ps.value = '';
    }

    function supprimerPrevision(id) {
        previsionsMoisProchain = previsionsMoisProchain.filter(p => p.id !== id);
        localStorage.setItem('previsions_mois', JSON.stringify(previsionsMoisProchain));
        majAffichagePrevisions();
        showToast('Prévision supprimée', 'danger');
    }

    function toutTransferer() {
        if (previsionsMoisProchain.length === 0) return;
        if (!confirm(`Transférer les ${previsionsMoisProchain.length} dépense${previsionsMoisProchain.length > 1 ? 's' : ''} prévue${previsionsMoisProchain.length > 1 ? 's' : ''} dans le mois en cours ?`)) return;
        previsionsMoisProchain.forEach(prev => {
            const expense = { id: Date.now() + Math.random(), date: new Date().toLocaleDateString('fr-FR'), desc: prev.desc, mt: prev.mt, ct: prev.cat || (db.categories[0]?.id || ''), recurring: false, note: '' };
            db.depenses.push(expense);
            const catObj = db.categories.find(c => c.id === expense.ct);
            if (catObj && catObj.label.toLowerCase().includes('épargne')) db.historiqueEpargne.push(expense);
        });
        const nb = previsionsMoisProchain.length;
        previsionsMoisProchain = [];
        localStorage.setItem('previsions_mois', JSON.stringify(previsionsMoisProchain));
        localStorage.setItem('budget_vGestion', JSON.stringify(db));
        majAffichage();
        majAffichagePrevisions();
        showToast(`${nb} dépense${nb > 1 ? 's' : ''} transférée${nb > 1 ? 's' : ''} ✅`, 'success');
    }

    function validerPrevision(id) {
        const prev = previsionsMoisProchain.find(p => p.id === id);
        if (!prev) return;
        const expense = { id: Date.now(), date: new Date().toLocaleDateString('fr-FR'), desc: prev.desc, mt: prev.mt, ct: prev.cat || (db.categories[0]?.id || ''), recurring: false, note: '' };
        db.depenses.push(expense);
        const catObj = db.categories.find(c => c.id === expense.ct);
        if (catObj && catObj.label.toLowerCase().includes('épargne')) db.historiqueEpargne.push(expense);
        previsionsMoisProchain = previsionsMoisProchain.filter(p => p.id !== id);
        localStorage.setItem('previsions_mois', JSON.stringify(previsionsMoisProchain));
        localStorage.setItem('budget_vGestion', JSON.stringify(db));
        majAffichage();
        majAffichagePrevisions();
        showToast(`"${prev.desc}" ajouté comme dépense réelle ✅`, 'success');
    }

    function majAffichagePrevisions() {
        majPrevisionsCatSelect();
        const list = document.getElementById('previsions-list');
        const totalBox = document.getElementById('previsions-total');
        const totalVal = document.getElementById('previsions-total-val');
        if (!list) return;
        if (previsionsMoisProchain.length === 0) {
            list.innerHTML = `<div class="bg-empty">Aucune dépense prévue pour le moment.</div>`;
            if (totalBox) totalBox.style.display = 'none';
            const tb0 = document.getElementById('bg_transfer'); if (tb0) tb0.style.display = 'none';
            return;
        }
        let total = 0;
        list.innerHTML = previsionsMoisProchain.map(p => {
            total += p.mt;
            const cat = db.categories.find(c => c.id === p.cat);
            return `<div class="bg-prev">
                <span class="bg-dot" style="background:${cat ? (cat.color || '#64748b') : 'var(--text-hint)'};"></span>
                <div class="bg-main"><span class="bg-name" style="font-weight:700;">${p.desc}</span><span class="bg-sub">${cat ? cat.label : 'Sans catégorie'}</span></div>
                <strong style="font-size:14px;font-weight:800;white-space:nowrap;">${p.mt.toFixed(0)} €</strong>
                <button class="bg-ok" onclick="validerPrevision(${p.id})" aria-label="C'est arrivé : ajouter comme dépense réelle"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg></button>
                <button class="bg-x" onclick="supprimerPrevision(${p.id})" aria-label="Supprimer"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round"><path d="M18 6 6 18M6 6l12 12"/></svg></button>
            </div>`;
        }).join('');
        if (totalBox) totalBox.style.display = 'inline';
        if (totalVal) totalVal.innerText = total.toFixed(0) + ' €';
        const tb = document.getElementById('bg_transfer'); if (tb) tb.style.display = 'block';
    }

    // Initialisation
    majAffichagePrevisions();

    /* ══════════════════════════════════
       MODIFICATION DE DÉPENSE
    ══════════════════════════════════ */
    function ouvrirEditDepense(id) {
        const d = db.depenses.find(dep => dep.id === id);
        if (!d) return;
        document.getElementById('edit-id').value = d.id;
        document.getElementById('edit-desc').value = d.desc;
        document.getElementById('edit-mt').value = d.mt;
        document.getElementById('edit-note').value = d.note || '';
        document.getElementById('edit-recurring').checked = d.recurring || false;
        const sel = document.getElementById('edit-cat');
        sel.innerHTML = '';
        db.categories.forEach(c => { sel.innerHTML += `<option value="${c.id}">${c.label}</option>`; });
        sel.value = d.ct;
        document.getElementById('modal-edit-depense').style.display = 'flex';
        setTimeout(() => document.getElementById('edit-desc').focus(), 100);
    }
    function supprimerDepuisEdit() {
        const id = Number(document.getElementById('edit-id').value);
        if (!id || !confirm('Supprimer cette dépense ?')) return;
        fermerEditDepense();
        supprimer(id);
    }
    function fermerEditDepense() {
        document.getElementById('modal-edit-depense').style.display = 'none';
    }
    document.getElementById('modal-edit-depense').addEventListener('click', e => { if (e.target === e.currentTarget) fermerEditDepense(); });
    function sauvegarderEditDepense() {
        const id = Number(document.getElementById('edit-id').value);
        const desc = document.getElementById('edit-desc').value.trim();
        const mt = parseFloat(document.getElementById('edit-mt').value);
        const cat = document.getElementById('edit-cat').value;
        const note = document.getElementById('edit-note').value.trim();
        const recurring = document.getElementById('edit-recurring').checked;
        if (!desc || isNaN(mt) || mt <= 0) { showToast('Remplis tous les champs correctement', 'warning'); return; }
        const idx = db.depenses.findIndex(d => d.id === id);
        if (idx === -1) return;
        db.depenses[idx] = { ...db.depenses[idx], desc, mt, ct: cat, note, recurring };
        // Mise à jour historiqueEpargne si besoin
        const epIdx = db.historiqueEpargne.findIndex(d => d.id === id);
        const catObj = db.categories.find(c => c.id === cat);
        if (catObj && catObj.label.toLowerCase().includes('épargne')) {
            if (epIdx !== -1) db.historiqueEpargne[epIdx] = { ...db.historiqueEpargne[epIdx], desc, mt, note, recurring };
            else db.historiqueEpargne.push({ ...db.depenses[idx] });
        } else {
            if (epIdx !== -1) db.historiqueEpargne.splice(epIdx, 1);
        }
        localStorage.setItem('budget_vGestion', JSON.stringify(db));
        fermerEditDepense();
        majAffichage();
        showToast(`"${desc}" mis à jour ✅`, 'success');
    }

    /* ══════════════════════════════════
       CODE WIDGET PERSO — identifie cet appareil
       (pour que toi et ta copine ayez chacun votre propre ligne)
    ══════════════════════════════════ */
    function genererCodeWidget() {
        const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
        let code = '';
        for (let i = 0; i < 6; i++) code += chars[Math.floor(Math.random() * chars.length)];
        return code;
    }
    let soloDeviceCode = localStorage.getItem('solo_device_code');
    if (!soloDeviceCode) {
        soloDeviceCode = genererCodeWidget();
        localStorage.setItem('solo_device_code', soloDeviceCode);
    }
    function afficherCodeWidget() {
        navigator.clipboard?.writeText(soloDeviceCode).catch(() => {});
        showToast(`Ton code widget : <strong>${soloDeviceCode}</strong> (copié) — colle-le dans MON_CODE du script Scriptable`, 'info', 7000);
    }


    /* ══════════════════════════════════
       SYNC BUDGET PERSO → SUPABASE (pour le widget iPhone)
    ══════════════════════════════════ */
    async function syncSoloBudget() {
        try {
            let totaux = {};
            db.categories.forEach(c => { totaux[c.id] = 0; });
            let totalDep = 0, totalEp = 0;
            db.depenses.forEach(d => {
                if (d.ct in totaux) totaux[d.ct] += d.mt;
                const cat = db.categories.find(c => c.id === d.ct);
                if (cat && cat.label.toLowerCase().includes("épargne")) totalEp += d.mt;
                else totalDep += d.mt;
            });
            const revenuTotal = (db.revenu || 0) + (db.caf || 0);
            const solde = revenuTotal - totalDep - totalEp;
            const parCategorie = db.categories.map(c => ({
                label: c.label,
                prevu: db.previsions[c.id] || 0,
                reel: totaux[c.id] || 0
            }));

            await supabaseFetch('solo_budget', {
                method: 'POST',
                headers: { 'Prefer': 'resolution=merge-duplicates,return=representation' },
                body: JSON.stringify({
                    code: soloDeviceCode,
                    depense_totale: totalDep,
                    epargne_totale: totalEp,
                    solde: solde,
                    coffre: globalSavings,
                    revenu_total: revenuTotal,
                    par_categorie: parCategorie,
                    updated_at: new Date().toISOString()
                })
            });
            window._syncErrShown = false;
        } catch (e) {
            if (!window._syncErrShown) { window._syncErrShown = true; showToast('Sync widget échouée : ' + (e && e.message ? e.message : e), 'danger', 6000); }
            console.warn('Sync widget perso échouée :', e);
        }
    }


    const FONDS_CLAIR = [
        { nom:'Gris ardoise',  id:'slate',  bg:'#f2f3f6', bg2:'#e7e9ef', card:'#ffffff' },
        { nom:'Blanc pur',     id:'white',  bg:'#ffffff', bg2:'#eef0f3', card:'#ffffff' },
        { nom:'Crème chaud',   id:'cream',  bg:'#f7f3ec', bg2:'#ece6da', card:'#fffdf8' },
        { nom:'Teinte auto',   id:'tinted', bg:null,      bg2:null,      card:null       },
    ];
    const FONDS_SOMBRE = [
        { nom:'Noir profond',    id:'deep',    bg:'#0e0f13', bg2:'#24252c', card:'#1a1b21' },
        { nom:'Gris anthracite', id:'charcoal',bg:'#18181b', bg2:'#2c2c30', card:'#242427' },
        { nom:'Teinte auto',     id:'tinted',  bg:null,      bg2:null,      card:null       },
    ];

    function hexToRgb(hex) {
        var r = parseInt(hex.slice(1,3),16), g = parseInt(hex.slice(3,5),16), b = parseInt(hex.slice(5,7),16);
        return { r:r, g:g, b:b };
    }
    function mixColor(hex, light) {
        var c = hexToRgb(hex);
        if (light) {
            return 'rgb(' + Math.round(c.r*0.07+248*0.93) + ',' + Math.round(c.g*0.07+248*0.93) + ',' + Math.round(c.b*0.07+248*0.93) + ')';
        } else {
            return 'rgb(' + Math.round(c.r*0.12+15*0.88) + ',' + Math.round(c.g*0.12+15*0.88) + ',' + Math.round(c.b*0.12+15*0.88) + ')';
        }
    }
    function mixColor2(hex, light) {
        var c = hexToRgb(hex);
        if (light) {
            return 'rgb(' + Math.round(c.r*0.12+232*0.88) + ',' + Math.round(c.g*0.12+232*0.88) + ',' + Math.round(c.b*0.12+232*0.88) + ')';
        } else {
            return 'rgb(' + Math.round(c.r*0.15+28*0.85) + ',' + Math.round(c.g*0.15+28*0.85) + ',' + Math.round(c.b*0.15+28*0.85) + ')';
        }
    }
    function mixCard(hex, light) {
        var c = hexToRgb(hex);
        if (light) return '#ffffff';
        return 'rgb(' + Math.round(c.r*0.12+28*0.88) + ',' + Math.round(c.g*0.12+28*0.88) + ',' + Math.round(c.b*0.12+28*0.88) + ')';
    }

    function appliquerFond() {
        var mode = localStorage.getItem('theme') || 'light';
        var isLight = mode === 'light';
        var fonds = isLight ? FONDS_CLAIR : FONDS_SOMBRE;
        var fondId = isLight ? (appSettings.fondClair || 'slate') : (appSettings.fondSombre || (isLight ? 'slate' : 'deep'));
        var fond = fonds.find(function(f){ return f.id === fondId; }) || fonds[0];
        var couleur = appSettings.couleur || '#4f46e5';

        if (fond.id === 'tinted') {
            document.documentElement.style.setProperty('--bg', mixColor(couleur, isLight));
            document.documentElement.style.setProperty('--bg2', mixColor2(couleur, isLight));
            document.documentElement.style.setProperty('--card', mixCard(couleur, isLight));
        } else {
            document.documentElement.style.setProperty('--bg', fond.bg);
            document.documentElement.style.setProperty('--bg2', fond.bg2);
            document.documentElement.style.setProperty('--card', fond.card);
        }
    }

    function majBgSwatches() {
        var mode = localStorage.getItem('theme') || 'light';
        var isLight = mode === 'light';
        var couleur = appSettings.couleur || '#4f46e5';

        function renderSwatches(containerId, fonds, settingKey) {
            var container = document.getElementById(containerId);
            if (!container) return;
            var current = appSettings[settingKey] || fonds[0].id;
            container.innerHTML = '';
            fonds.forEach(function(f) {
                var previewBg = f.id === 'tinted' ? mixColor(couleur, isLight) : f.bg;
                var previewCard = f.id === 'tinted' ? mixCard(couleur, isLight) : f.card;
                var isSelected = f.id === current;
                var d = document.createElement('div');
                d.title = f.nom;
                d.style.cssText = 'display:flex;flex-direction:column;align-items:center;gap:5px;cursor:pointer;';
                d.innerHTML = '<div style="width:48px;height:36px;border-radius:8px;background:' + previewBg + ';border:2px solid ' + (isSelected?'var(--main)':'var(--bg2)') + ';display:flex;align-items:center;justify-content:center;box-shadow:0 1px 4px rgba(0,0,0,0.1);overflow:hidden;position:relative;"><div style="position:absolute;top:4px;left:4px;right:4px;bottom:4px;background:' + previewCard + ';border-radius:4px;"></div>' + (f.id==='tinted'?'<div style="position:absolute;top:2px;right:2px;font-size:9px;">✨</div>':'') + '</div><div style="font-size:0.65rem;color:var(--text-muted);white-space:nowrap;text-align:center;">' + f.nom + '</div>';
                d.onclick = function() {
                    appSettings[settingKey] = f.id;
                    localStorage.setItem('app_settings', JSON.stringify(appSettings));
                    majFondLabel();
                    appliquerFond();
                    majBgSwatches();
                };
                container.appendChild(d);
            });
        }

        renderSwatches('bg-swatches-light', FONDS_CLAIR, 'fondClair');
        renderSwatches('bg-swatches-dark', FONDS_SOMBRE, 'fondSombre');
    }

    /* ══════════════════════════════════
       PARAMÈTRES APPLICATION
    ══════════════════════════════════ */
    const THEMES_COULEURS = [
        { nom:'Violet',  main:'#4f46e5', light:'#818cf8', glow:'rgba(79,70,229,0.16)' },
        { nom:'Bleu',    main:'#1d4ed8', light:'#60a5fa', glow:'rgba(29,78,216,0.16)' },
        { nom:'Indigo',  main:'#4338ca', light:'#a5b4fc', glow:'rgba(67,56,202,0.16)' },
        { nom:'Cyan',    main:'#0e7490', light:'#22d3ee', glow:'rgba(14,116,144,0.16)' },
        { nom:'Vert',    main:'#047857', light:'#34d399', glow:'rgba(4,120,87,0.16)'  },
        { nom:'Rose',    main:'#be185d', light:'#f472b6', glow:'rgba(190,24,93,0.16)' },
        { nom:'Orange',  main:'#c2410c', light:'#fb923c', glow:'rgba(194,65,12,0.16)' },
        { nom:'Rouge',   main:'#b91c1c', light:'#f87171', glow:'rgba(185,28,28,0.16)' },
    ];

    let appSettings = JSON.parse(localStorage.getItem('app_settings') || '{}');
    let DEVISE = appSettings.devise || '\u20ac';

    function appliquerSettings() {
        // Couleur principale
        var ANCIENNES = {'#5b5ef4':'#4f46e5','#3b82f6':'#1d4ed8','#6366f1':'#4338ca','#06b6d4':'#0e7490','#10b981':'#047857','#ec4899':'#be185d','#f97316':'#c2410c','#ef4444':'#b91c1c'};
        if (appSettings.couleur && ANCIENNES[appSettings.couleur]) { appSettings.couleur = ANCIENNES[appSettings.couleur]; localStorage.setItem('app_settings', JSON.stringify(appSettings)); }
        var couleur = appSettings.couleur || '#4f46e5';
        var theme = THEMES_COULEURS.find(function(t){ return t.main === couleur; }) || THEMES_COULEURS[0];
        document.documentElement.style.setProperty('--main', theme.main);
        document.documentElement.style.setProperty('--main-light', theme.light);
        document.documentElement.style.setProperty('--main-glow', theme.glow);
        // Devise
        DEVISE = appSettings.devise || '\u20ac';
        // Fond
        appliquerFond();
        // Bouton partenaire dans la nav
        majNavVacances();
        var navCouple = document.getElementById('nav-couple');
        if (navCouple) navCouple.style.display = (appSettings.modeCouple !== false) ? 'flex' : 'none';
        var navPartner = document.getElementById('nav-partenaire');
        var navLabel = document.getElementById('nav-partner-label');
        if (navPartner) navPartner.style.display = appSettings.codePartner ? 'flex' : 'none';
        if (navLabel) navLabel.textContent = appSettings.nomPartner || 'Elle';
        // Prenom dans le header
        var prenom = appSettings.prenom || '';
        var titleEl = document.getElementById('mh-title');
        if (titleEl) titleEl.textContent = prenom ? ('Bonjour ' + prenom) : 'Mon Coach Finance';
        var avEl = document.getElementById('mh-avatar');
        if (avEl) avEl.textContent = prenom ? prenom.charAt(0).toUpperCase() : 'M';
        var subEl = document.getElementById('mh-sub');
        if (subEl) {
            var moisN = ['Janvier','Février','Mars','Avril','Mai','Juin','Juillet','Août','Septembre','Octobre','Novembre','Décembre'];
            var cyc = getCycle();
            subEl.textContent = (appSettings.jourDebut || 1) > 1
                ? 'Cycle du ' + cyc.start.getDate() + ' ' + moisN[cyc.start.getMonth()].toLowerCase()
                : moisN[cyc.start.getMonth()] + ' ' + cyc.start.getFullYear();
        }
        var qaCouple = document.getElementById('qa-couple');
        if (qaCouple) qaCouple.style.display = (appSettings.modeCouple !== false) ? 'flex' : 'none';
    }

    function ouvrirSettings() {
        var el = document.getElementById('modal-settings');
        if (!el) return;
        // Peuple les champs
        document.getElementById('set_prenom').value = appSettings.prenom || '';
        document.getElementById('set_nom_moi').value = appSettings.nomMoi || '';
        document.getElementById('set_nom_partner').value = appSettings.nomPartner || '';
        document.getElementById('set_code_partner').value = appSettings.codePartner || '';
        var vacToggle = document.getElementById('set_mode_vacances');
        if (vacToggle) {
            vacToggle.checked = !!appSettings.modeVacances;
            previewVacancesToggle(!!appSettings.modeVacances);
        }
        var coupleToggle = document.getElementById('set_mode_couple');
        if (coupleToggle) {
            coupleToggle.checked = appSettings.modeCouple !== false;
            previewCoupleToggle(appSettings.modeCouple !== false);
        }
        document.getElementById('set_devise').value = appSettings.devise || '\u20ac';
        // Jour debut mois
        var sel = document.getElementById('set_jour_debut');
        if (sel) {
            sel.innerHTML = '';
            for (var i = 1; i <= 28; i++) {
                var opt = document.createElement('option');
                opt.value = i; opt.textContent = i === 1 ? 'Le 1er du mois' : 'Le ' + i + ' du mois';
                if ((appSettings.jourDebut || 1) === i) opt.selected = true;
                sel.appendChild(opt);
            }
        }
        // Code widget
        var wc = document.getElementById('set_widget_code');
        if (wc && typeof soloDeviceCode !== 'undefined') wc.textContent = soloDeviceCode;
        // Swatches couleurs
        majSwatches();
        majBgSwatches();
        // Boutons mode
        var mode = localStorage.getItem('theme') || 'light';
        majSegMode(mode);
        majProfilSettings();
        majFondLabel();
        var fondsBox = document.getElementById('st-fonds'); if (fondsBox) fondsBox.style.display = 'none';
        var chev = document.getElementById('st-fond-chev'); if (chev) chev.classList.remove('open');
        el.style.display = 'flex';
    }

    function fermerSettings() {
        var el = document.getElementById('modal-settings');
        if (el) el.style.display = 'none';
    }

    function majSwatches() {
        var swatches = document.getElementById('theme-swatches');
        if (!swatches) return;
        var current = appSettings.couleur || '#4f46e5';
        swatches.innerHTML = '';
        THEMES_COULEURS.forEach(function(t) {
            var d = document.createElement('div');
            d.title = t.nom;
            d.style.cssText = 'width:36px;height:36px;border-radius:50%;background:' + t.main + ';cursor:pointer;border:3px solid ' + (t.main === current ? 'var(--text)' : 'transparent') + ';box-shadow:' + (t.main === current ? '0 0 0 2px var(--card)' : 'none') + ';transition:all 0.2s;flex-shrink:0;';
            d.onclick = function() { selectionnerCouleur(t.main); };
            swatches.appendChild(d);
        });
    }

    function selectionnerCouleur(couleur) {
        appSettings.couleur = couleur;
        localStorage.setItem('app_settings', JSON.stringify(appSettings));
        appliquerSettings();
        appliquerFond();
        majSwatches();
        majBgSwatches();
    }

    function appliquerModeTheme(mode) {
        document.documentElement.setAttribute('data-theme', mode);
        localStorage.setItem('theme', mode);
        appliquerFond();
        majAffichage(); afficherEvolution();
        majSegMode(mode);
        majBgSwatches();
    }

    function sauvegarderSettings() {
        var ancienneDevise = appSettings.devise || '\u20ac';
        appSettings.prenom     = document.getElementById('set_prenom').value.trim();
        appSettings.nomMoi     = document.getElementById('set_nom_moi').value.trim();
        appSettings.nomPartner = document.getElementById('set_nom_partner').value.trim();
        appSettings.codePartner = document.getElementById('set_code_partner').value.trim().toUpperCase();
        var vacEl = document.getElementById('set_mode_vacances');
        appSettings.modeVacances = vacEl ? vacEl.checked : false;
        var coupleEl = document.getElementById('set_mode_couple');
        appSettings.modeCouple = coupleEl ? coupleEl.checked : true;
        appSettings.devise     = document.getElementById('set_devise').value;
        appSettings.jourDebut  = parseInt(document.getElementById('set_jour_debut').value) || 1;
        localStorage.setItem('app_settings', JSON.stringify(appSettings));
        if ((appSettings.devise || '\u20ac') !== ancienneDevise) { location.reload(); return; }
        appliquerSettings();
        majAffichage();
        demarrerSyncPartenaire();
        fermerSettings();
        showToast('Paramètres sauvegardés', 'success');
    }

    function exporterDonnees() {
        var data = {
            db: db,
            globalSavings: globalSavings,
            archives: archives,
            previsions: previsionsMoisProchain,
            settings: appSettings,
            exportDate: new Date().toISOString()
        };
        var blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
        var url = URL.createObjectURL(blob);
        var a = document.createElement('a');
        a.href = url;
        a.download = 'coach-finance-' + new Date().toLocaleDateString('fr-FR').replace(/\//g, '-') + '.json';
        a.click();
        URL.revokeObjectURL(url);
        showToast('Données exportées', 'success');
    }

    function importerDonnees(event) {
        var file = event.target.files[0];
        if (!file) return;
        var reader = new FileReader();
        reader.onload = function(e) {
            try {
                var data = JSON.parse(e.target.result);
                if (!confirm('Importer ces données ? Tes données actuelles seront remplacées.')) return;
                if (data.db) { db = data.db; localStorage.setItem('budget_vGestion', JSON.stringify(db)); }
                if (data.globalSavings !== undefined) { globalSavings = data.globalSavings; localStorage.setItem('globalSavings', globalSavings); }
                if (data.archives) { archives = data.archives; localStorage.setItem('budget_archives', JSON.stringify(archives)); }
                if (data.previsions) { previsionsMoisProchain = data.previsions; localStorage.setItem('previsions_mois', JSON.stringify(previsionsMoisProchain)); }
                if (data.settings) { appSettings = data.settings; localStorage.setItem('app_settings', JSON.stringify(appSettings)); }
                appliquerSettings(); majAffichage(); afficherArchives(); afficherEvolution();
                showToast('Données importées', 'success', 4000);
                fermerSettings();
            } catch(err) { showToast('Fichier invalide', 'danger'); }
        };
        reader.readAsText(file);
        event.target.value = '';
    }

    function reinitialiserApp() {
        if (!confirm('Supprimer TOUTES les données ? Cette action est irréversible.')) return;
        if (!confirm('Dernière confirmation — tout sera effacé définitivement.')) return;
        ['budget_vGestion','globalSavings','budget_archives','couple_archives','couple_session','couple_budgets','solo_device_code','previsions_mois','app_settings'].forEach(function(k){ localStorage.removeItem(k); });
        location.reload();
    }


    function previewCoupleToggle(checked) {
        var track = document.getElementById('couple-toggle-track');
        var thumb = document.getElementById('couple-toggle-thumb');
        if (track) track.style.background = checked ? 'var(--main)' : 'var(--bg2)';
        if (thumb) thumb.style.transform = checked ? 'translateX(22px)' : 'translateX(0)';
    }

    function previewVacancesToggle(checked) {
        var track = document.getElementById('vac-toggle-track');
        var thumb = document.getElementById('vac-toggle-thumb');
        if (track) track.style.background = checked ? 'var(--main)' : 'var(--bg2)';
        if (thumb) thumb.style.transform = checked ? 'translateX(22px)' : 'translateX(0)';
    }

    function copierCodeWidget() {
        if (typeof soloDeviceCode !== 'undefined') {
            navigator.clipboard && navigator.clipboard.writeText(soloDeviceCode).catch(function(){});
            showToast('Code ' + soloDeviceCode + ' copié !', 'success');
        }
    }

    // Applique au chargement
    appliquerSettings();


    /* ══════════════════════════════════
       BUDGET PARTENAIRE — LECTURE SEULE
    ══════════════════════════════════ */
    var partnerData = null;
    var partnerInterval = null;

    function demarrerSyncPartenaire() {
        arreterSyncPartenaire();
        var code = appSettings.codePartner || '';
        if (!code) { majAffichagePartenaire(null); return; }
        chargerBudgetPartenaire();
        partnerInterval = setInterval(chargerBudgetPartenaire, 30000);
    }

    function arreterSyncPartenaire() {
        if (partnerInterval) { clearInterval(partnerInterval); partnerInterval = null; }
    }

    async function chargerBudgetPartenaire() {
        var code = appSettings.codePartner || '';
        if (!code) { majAffichagePartenaire(null); return; }
        var dot = document.getElementById('partner-sync-dot');
        if (dot) dot.style.background = '#9ca3af';
        try {
            var rows = await supabaseFetch('solo_budget?code=eq.' + encodeURIComponent(code) + '&limit=1');
            if (rows && rows.length > 0) {
                partnerData = rows[0];
                majAffichagePartenaire(partnerData);
                if (dot) dot.style.background = '#10b981';
            } else {
                majAffichagePartenaire(null);
                if (dot) dot.style.background = '#f59e0b';
            }
        } catch(e) {
            if (dot) dot.style.background = '#ef4444';
            console.warn('Chargement budget partenaire échoué :', e);
        }
    }

    function majAffichagePartenaire(data) {
        var code = appSettings.codePartner || '';
        var nomPartner = appSettings.nomPartner || 'Ton·ta partenaire';
        var noCodeCard = document.getElementById('partner-no-code-card');
        var budgetCard = document.getElementById('partner-budget-card');

        if (!code) {
            if (noCodeCard) noCodeCard.style.display = 'block';
            if (budgetCard) budgetCard.style.display = 'none';
            return;
        }
        if (noCodeCard) noCodeCard.style.display = 'none';
        if (budgetCard) budgetCard.style.display = 'block';

        // En-tête
        var title = document.getElementById('partner-card-title');
        if (title) title.textContent = 'Budget de ' + nomPartner;
        var av = document.getElementById('partner-avatar');
        if (av) av.textContent = (appSettings.nomPartner || 'P').charAt(0).toUpperCase();
        var cycleP = getCycle();
        var joursRestants = cycleP.restants, jourActuel = cycleP.ecoules;
        var jEl = document.getElementById('partner-jours'); if (jEl) jEl.textContent = joursRestants;
        var nomCourt = appSettings.nomPartner || 'Elle';
        var plEl = document.getElementById('partner-peut-lbl'); if (plEl) plEl.textContent = nomCourt + ' peut dépenser';
        var rlEl = document.getElementById('partner-rythme-lbl'); if (rlEl) rlEl.textContent = nomCourt + ' dépense en moyenne';
        var setT = function (id, v) { var el = document.getElementById(id); if (el) el.textContent = v; };

        if (!data) {
            // Code entré mais données pas encore dispo
            ['partner-dep','partner-ep','partner-coffre','partner-rythme','partner-peut','partner-bar-label','partner-bar-pct','partner-revenu-txt'].forEach(function (id) { setT(id, '—'); });
            setT('partner-solde-wrap', '— €');
            var barE = document.getElementById('partner-bar'); if (barE) barE.style.width = '0%';
            var catsE = document.getElementById('partner-cats');
            if (catsE) catsE.innerHTML = '<div class="empty-state" style="padding:18px 0;"><p>En attente de données…</p></div>';
            setT('partner-last-sync', 'Jamais synchronisé');
            return;
        }

        var dep = parseFloat(data.depense_totale) || 0;
        var ep = parseFloat(data.epargne_totale) || 0;
        var solde = parseFloat(data.solde) || 0;
        var coffre = parseFloat(data.coffre) || 0;
        var revenu = parseFloat(data.revenu_total) || 0;
        var cats = data.par_categorie || [];
        var estEp = function (c) { return (c.label || '').toLowerCase().indexOf('épargne') !== -1; };
        var estFixe = function (c) { return /loyer|charge|abonnement/i.test(c.label || ''); };

        // Solde & chiffres clés
        setT('partner-solde-wrap', solde.toFixed(2) + ' €');
        setT('partner-revenu-txt', 'sur ' + revenu.toFixed(2) + ' € de revenus');
        setT('partner-dep', dep.toFixed(2) + ' €');
        setT('partner-ep', ep.toFixed(2) + ' €');
        setT('partner-coffre', coffre.toFixed(2) + ' €');

        // Budget utilisé (référence : budget prévu hors épargne)
        var totalPrevu = cats.filter(function (c) { return !estEp(c); }).reduce(function (s, c) { return s + (c.prevu || 0); }, 0);
        var budgetRef = totalPrevu > 0 ? totalPrevu : revenu;
        var pct = budgetRef > 0 ? Math.round((dep / budgetRef) * 100) : 0;
        var barEl = document.getElementById('partner-bar');
        if (barEl) {
            barEl.style.width = Math.min(pct, 100) + '%';
            barEl.style.background = pct > 100 ? 'var(--danger)' : pct >= 85 ? 'var(--warning)' : 'var(--partner)';
        }
        setT('partner-bar-label', dep.toFixed(0) + ' € / ' + budgetRef.toFixed(0) + ' €');
        setT('partner-bar-pct', pct + ' % du budget prévu');

        // Fin de mois : ce qu'elle peut dépenser par jour vs son rythme (hors charges fixes)
        var variables = cats.filter(function (c) { return !estEp(c) && !estFixe(c); }).reduce(function (s, c) { return s + (c.reel || 0); }, 0);
        var rythme = jourActuel > 0 ? variables / jourActuel : 0;
        var peut = joursRestants > 0 ? Math.max(0, solde) / joursRestants : Math.max(0, solde);
        setT('partner-peut', peut.toFixed(0) + ' €');
        setT('partner-rythme', rythme.toFixed(0) + ' € / jour');
        var peutEl = document.getElementById('partner-peut');
        if (peutEl && peutEl.parentElement) peutEl.parentElement.style.color = solde <= 0 ? 'var(--danger)' : 'var(--text)';
        var ryEl = document.getElementById('partner-rythme');
        if (ryEl) ryEl.style.color = jourActuel <= 3 ? 'var(--text)' : (rythme > peut ? 'var(--warning)' : 'var(--success)');

        // Catégories : dépassements en premier
        var catsEl = document.getElementById('partner-cats');
        var lignes = cats.filter(function (c) { return (c.reel || 0) > 0 || (c.prevu || 0) > 0; }).map(function (c) {
            var prev = c.prevu || 0, reel = c.reel || 0, over = reel > prev;
            return { label: c.label, prev: prev, reel: reel, over: over, ratio: prev > 0 ? reel / prev : (reel > 0 ? Infinity : 0) };
        }).sort(function (a, b) { return (b.over - a.over) || (b.ratio - a.ratio); });
        if (catsEl && lignes.length > 0) {
            catsEl.innerHTML = lignes.map(function (c) {
                var p = c.prev > 0 ? Math.min(c.reel / c.prev * 100, 100) : (c.reel > 0 ? 100 : 0);
                var warn = !c.over && p >= 85;
                var col = c.over ? 'var(--danger)' : (warn ? '#d97706' : 'var(--partner)');
                return '<div class="pt-row"><div class="pt-row-top"><span class="n">' + c.label + '</span>'
                    + '<span class="v"><strong class="' + (c.over ? 'over' : '') + '">' + c.reel.toFixed(2) + ' €</strong> / ' + c.prev.toFixed(0) + ' €</span></div>'
                    + '<div class="bc-bar"><div class="bc-fill" style="width:' + p + '%;background:' + col + ';"></div></div></div>';
            }).join('');
        } else if (catsEl) {
            catsEl.innerHTML = '<div class="empty-state" style="padding:18px 0;"><p>Aucune catégorie disponible.</p></div>';
        }

        // Dernière synchro
        if (data.updated_at) {
            var d = new Date(data.updated_at);
            setT('partner-last-sync', 'Màj ' + d.toLocaleTimeString('fr-FR', { hour: '2-digit', minute: '2-digit' }));
        }
        var dot = document.getElementById('partner-sync-dot'); if (dot) dot.style.background = '#0b7a53';
    }

    // Lance le sync partenaire si code déjà configuré
    if (appSettings.codePartner) demarrerSyncPartenaire();


    /* ══════════════════════════════════
       MODE VACANCES
    ══════════════════════════════════ */
    var vacancesData = JSON.parse(localStorage.getItem('vacances_data') || '{}');

    var VAC_CATS = [
        { id:'transport',    label:'✈️ Transport'    },
        { id:'hebergement',  label:'🏨 Hébergement'  },
        { id:'resto',        label:'🍽️ Restaurants'  },
        { id:'activites',    label:'🎡 Activités'    },
        { id:'shopping',     label:'🛍️ Shopping'     },
        { id:'sante',        label:'💊 Santé'        },
        { id:'divers',       label:'💼 Divers'       },
    ];

    var VAC_DEVISES = [
        { code:'EUR', sym:'€',   nom:'Euro'          },
        { code:'USD', sym:'$',   nom:'Dollar US'     },
        { code:'GBP', sym:'£',   nom:'Livre sterling'},
        { code:'MAD', sym:'MAD', nom:'Dirham marocain'},
        { code:'TND', sym:'TND', nom:'Dinar tunisien'},
        { code:'AED', sym:'AED', nom:'Dirham EAU'    },
        { code:'JPY', sym:'¥',   nom:'Yen japonais'  },
        { code:'CHF', sym:'CHF', nom:'Franc suisse'  },
        { code:'CAD', sym:'CAD', nom:'Dollar canadien'},
        { code:'XOF', sym:'CFA', nom:'Franc CFA'     },
        { code:'MXN', sym:'MXN', nom:'Peso mexicain' },
        { code:'THB', sym:'฿',   nom:'Baht thaïlandais'},
    ];

    var taux_cache = {};

    async function convertirEnEuro(montant, devise) {
        if (devise === 'EUR') return montant;
        if (taux_cache[devise]) return Math.round(montant * taux_cache[devise] * 100) / 100;
        try {
            var url = 'https://cdn.jsdelivr.net/npm/@fawazahmed0/currency-api@latest/v1/currencies/' + devise.toLowerCase() + '.min.json';
            var resp = await fetch(url);
            var data = await resp.json();
            var taux = data[devise.toLowerCase()]['eur'];
            taux_cache[devise] = taux;
            return Math.round(montant * taux * 100) / 100;
        } catch(e) {
            return null;
        }
    }

    function sauvegarderVacances() {
        localStorage.setItem('vacances_data', JSON.stringify(vacancesData));
    }

    function majNavVacances() {
        var btn = document.getElementById('nav-vacances');
        if (!btn) return;
        var vd = (typeof vacancesData !== 'undefined') ? vacancesData : {};
        var actif = !!appSettings.modeVacances;
        btn.style.display = actif ? 'flex' : 'none';
        var lbl = document.getElementById('nav-vacances-label');
        if (lbl) lbl.textContent = (vd.nom ? vd.nom.split(' ')[0] : 'Vacances');
    }

    function ouvrirVacances() {
        majAffichageVacances();
    }

    function sauvegarderConfigVacances() {
        var nom = document.getElementById('vac-nom').value.trim();
        var budget = parseFloat(document.getElementById('vac-budget').value) || 0;
        if (!nom) { showToast('Donne un nom à ton séjour', 'warning'); return; }
        vacancesData.nom = nom;
        vacancesData.budget = budget;
        if (!vacancesData.depenses) vacancesData.depenses = [];
        sauvegarderVacances();
        majNavVacances();
        majAffichageVacances();
        showToast('Séjour "' + nom + '" configuré 🌍', 'success');
    }

    async function ajouterDepenseVacances() {
        var desc = document.getElementById('vac-desc').value.trim();
        var mt = parseFloat(document.getElementById('vac-mt').value);
        var devise = document.getElementById('vac-devise').value;
        var cat = document.getElementById('vac-cat').value;
        if (!desc || isNaN(mt) || mt <= 0) { showToast('Remplis tous les champs', 'warning'); return; }

        var btnAjouter = document.getElementById('vac-btn-ajouter');
        if (btnAjouter) { btnAjouter.disabled = true; btnAjouter.textContent = 'Conversion...'; }

        var mtEur = await convertirEnEuro(mt, devise);
        var devInfo = VAC_DEVISES.find(function(d){ return d.code === devise; }) || { sym: devise };

        if (btnAjouter) { btnAjouter.disabled = false; btnAjouter.textContent = '+ Ajouter'; }

        if (mtEur === null) {
            showToast('Conversion échouée — vérifie ta connexion', 'danger');
            return;
        }

        if (!vacancesData.depenses) vacancesData.depenses = [];
        vacancesData.depenses.push({
            id: Date.now(),
            date: new Date().toLocaleDateString('fr-FR'),
            desc: desc,
            mt: mtEur,
            mtOriginal: mt,
            deviseOriginal: devise,
            devSymbol: devInfo.sym,
            cat: cat
        });

        sauvegarderVacances();
        document.getElementById('vac-desc').value = '';
        document.getElementById('vac-mt').value = '';
        majAffichageVacances();
        showToast(desc + ' — ' + mt + ' ' + devInfo.sym + ' = ' + mtEur.toFixed(2) + ' €', 'success', 4000);
    }

    function supprimerDepenseVacances(id) {
        vacancesData.depenses = (vacancesData.depenses || []).filter(function(d){ return d.id !== id; });
        sauvegarderVacances();
        majAffichageVacances();
        showToast('Dépense supprimée', 'danger');
    }

    function reinitialiserVacances() {
        if (!confirm('Effacer toutes les dépenses du séjour ?')) return;
        vacancesData.depenses = [];
        sauvegarderVacances();
        majAffichageVacances();
        showToast('Séjour réinitialisé', 'warning');
    }

    function cloturerVoyage() {
        if (!vacancesData.nom) return;
        var depenses = vacancesData.depenses || [];
        if (!confirm('Clôturer "' + vacancesData.nom + '" et démarrer un nouveau voyage ?')) return;

        // Archive le voyage actuel
        var archives = JSON.parse(localStorage.getItem('vacances_archives') || '[]');
        var total = depenses.reduce(function(s,d){ return s + d.mt; }, 0);
        archives.push({
            id: Date.now(),
            nom: vacancesData.nom,
            budget: vacancesData.budget || 0,
            total: total,
            date: new Date().toLocaleDateString('fr-FR'),
            depenses: depenses.slice()
        });
        localStorage.setItem('vacances_archives', JSON.stringify(archives));

        // Réinitialise le voyage en cours
        vacancesData = { depenses: [] };
        sauvegarderVacances();
        majNavVacances();
        majAffichageVacances();
        showToast('"' + archives[archives.length-1].nom + '" archivé 🎉', 'success', 4000);
    }

    function majArchivesVoyages() {
        var archives = JSON.parse(localStorage.getItem('vacances_archives') || '[]');
        var section = document.getElementById('vac-archives-section');
        var list = document.getElementById('vac-archives-list');
        if (!section || !list) return;
        if (archives.length === 0) { section.style.display = 'none'; return; }
        section.style.display = 'block';
        list.innerHTML = [...archives].reverse().map(function(arc) {
            var sc = arc.total > arc.budget && arc.budget > 0 ? 'var(--danger)' : 'var(--success)';
            var pct = arc.budget > 0 ? Math.min(Math.round((arc.total/arc.budget)*100), 100) : 0;
            return '<div style="background:var(--bg);border-radius:12px;padding:12px;margin-bottom:8px;border:1px solid var(--bg2);">'
                + '<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">'
                + '<div><div style="font-size:0.88rem;font-weight:700;">' + arc.nom + '</div>'
                + '<div style="font-size:0.7rem;color:var(--text-muted);">Archivé le ' + arc.date + '</div></div>'
                + '<button class="btn-delete" onclick="supprimerArchiveVoyage(' + arc.id + ')">✕</button>'
                + '</div>'
                + '<div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;">'
                + '<div style="background:var(--card);border-radius:8px;padding:8px;text-align:center;">'
                + '<div style="font-size:0.65rem;color:var(--text-muted);">Dépensé</div>'
                + '<div style="font-size:0.95rem;font-weight:700;color:' + sc + ';">' + arc.total.toFixed(2) + ' €</div></div>'
                + '<div style="background:var(--card);border-radius:8px;padding:8px;text-align:center;">'
                + '<div style="font-size:0.65rem;color:var(--text-muted);">Budget</div>'
                + '<div style="font-size:0.95rem;font-weight:700;color:var(--main);">' + (arc.budget||0).toFixed(2) + ' €</div></div>'
                + '</div>'
                + (arc.budget > 0 ? '<div style="background:var(--bg2);border-radius:999px;height:5px;overflow:hidden;margin-top:8px;"><div style="height:100%;width:' + pct + '%;background:' + sc + ';border-radius:999px;"></div></div>' : '')
                + '</div>';
        }).join('');
    }

    function supprimerArchiveVoyage(id) {
        if (!confirm('Supprimer cette archive ?')) return;
        var archives = JSON.parse(localStorage.getItem('vacances_archives') || '[]');
        archives = archives.filter(function(a){ return a.id !== id; });
        localStorage.setItem('vacances_archives', JSON.stringify(archives));
        majArchivesVoyages();
        showToast('Archive supprimée', 'warning');
    }

    function majAffichageVacances() {
        var configCard = document.getElementById('vac-config-card');
        var mainCard = document.getElementById('vac-main-card');
        var formCard = document.getElementById('vac-form-card');
        var histCard = document.getElementById('vac-hist-card');

        var configured = !!(vacancesData.nom);
        if (configCard) configCard.style.display = configured ? 'none' : 'block';
        if (mainCard) mainCard.style.display = configured ? 'block' : 'none';
        if (formCard) formCard.style.display = configured ? 'block' : 'none';
        if (histCard) histCard.style.display = configured ? 'block' : 'none';

        if (!configured) return;

        // Titre
        var titreEl = document.getElementById('vac-titre');
        if (titreEl) titreEl.textContent = vacancesData.nom || 'Séjour';

        // Remplir le formulaire de config avec les valeurs actuelles
        var nomEl = document.getElementById('vac-nom');
        var budgetEl = document.getElementById('vac-budget');
        if (nomEl) nomEl.value = vacancesData.nom || '';
        if (budgetEl) budgetEl.value = vacancesData.budget || '';

        // Calcul totaux
        var depenses = vacancesData.depenses || [];
        var totalEur = depenses.reduce(function(s, d){ return s + d.mt; }, 0);
        var budget = vacancesData.budget || 0;
        var reste = budget - totalEur;
        var pct = budget > 0 ? Math.min(Math.round((totalEur/budget)*100), 100) : 0;

        var depEl = document.getElementById('vac-total-dep');
        if (depEl) depEl.textContent = totalEur.toFixed(2) + ' €';
        var resteEl = document.getElementById('vac-reste');
        if (resteEl) resteEl.textContent = (reste < 0 ? '⚠️ Dépassé de ' + Math.abs(reste).toFixed(2) : reste.toFixed(2)) + ' €';
        var budgetEl2 = document.getElementById('vac-total-budget');
        if (budgetEl2) budgetEl2.textContent = budget.toFixed(2) + ' €';
        var barEl = document.getElementById('vac-bar');
        if (barEl) { barEl.style.width = pct + '%'; barEl.style.background = pct >= 100 ? 'var(--danger)' : pct >= 85 ? 'var(--warning)' : 'var(--main)'; }
        var pctEl = document.getElementById('vac-pct');
        if (pctEl) pctEl.textContent = pct + '%';

        // Select devise formulaire
        var devSel = document.getElementById('vac-devise');
        if (devSel && devSel.children.length === 0) {
            VAC_DEVISES.forEach(function(d) {
                devSel.innerHTML += '<option value="' + d.code + '">' + d.sym + ' — ' + d.nom + '</option>';
            });
        }

        // Select catégorie formulaire
        var catSel = document.getElementById('vac-cat');
        if (catSel && catSel.children.length === 0) {
            VAC_CATS.forEach(function(c) {
                catSel.innerHTML += '<option value="' + c.id + '">' + c.label + '</option>';
            });
        }

        // Historique
        var hist = document.getElementById('vac-hist-list');
        if (hist) {
            if (depenses.length === 0) {
                hist.innerHTML = '<div style="text-align:center;padding:20px 0;color:var(--text-muted);font-size:0.82rem;">Aucune dépense pour ce séjour.</div>';
            } else {
                hist.innerHTML = [...depenses].reverse().map(function(d, i) {
                    var cat = VAC_CATS.find(function(c){ return c.id === d.cat; });
                    var border = i < depenses.length - 1 ? 'border-bottom:1px solid var(--bg2);' : '';
                    var hasConv = d.deviseOriginal && d.deviseOriginal !== 'EUR';
                    return '<div style="display:flex;justify-content:space-between;align-items:flex-start;padding:9px 0;' + border + '">'
                        + '<div style="flex:1;min-width:0;">'
                        + '<div style="font-size:0.88rem;font-weight:600;color:var(--text);">' + d.desc + '</div>'
                        + '<div style="font-size:0.72rem;color:var(--text-muted);margin-top:1px;">'
                        + d.date + ' · ' + (cat ? cat.label : d.cat)
                        + (hasConv ? ' · ' + d.mtOriginal + ' ' + d.devSymbol : '')
                        + '</div>'
                        + '</div>'
                        + '<div style="display:flex;align-items:center;gap:8px;margin-left:10px;flex-shrink:0;">'
                        + '<strong style="font-size:0.9rem;white-space:nowrap;">' + d.mt.toFixed(2) + ' €</strong>'
                        + '<button class="btn-delete" onclick="supprimerDepenseVacances(' + d.id + ')">✕</button>'
                        + '</div>'
                        + '</div>';
                }).join('');
            }
        }

        // Répartition par catégorie
        var catList = document.getElementById('vac-cats-list');
        if (catList) {
            var parCat = {};
            VAC_CATS.forEach(function(c){ parCat[c.id] = 0; });
            depenses.forEach(function(d){ if (parCat[d.cat] !== undefined) parCat[d.cat] += d.mt; });
            catList.innerHTML = VAC_CATS.filter(function(c){ return parCat[c.id] > 0; }).map(function(c) {
                var pctCat = budget > 0 ? Math.min(Math.round((parCat[c.id]/budget)*100), 100) : 0;
                return '<div style="margin-bottom:10px;">'
                    + '<div style="display:flex;justify-content:space-between;margin-bottom:3px;">'
                    + '<span style="font-size:0.82rem;">' + c.label + '</span>'
                    + '<span style="font-size:0.78rem;color:var(--text-muted);">' + parCat[c.id].toFixed(2) + ' €</span>'
                    + '</div>'
                    + '<div style="background:var(--bg2);border-radius:999px;height:5px;overflow:hidden;">'
                    + '<div style="height:100%;width:' + pctCat + '%;background:var(--main);border-radius:999px;"></div>'
                    + '</div></div>';
            }).join('') || '<div style="font-size:0.82rem;color:var(--text-muted);">Aucune dépense encore.</div>';
        }

        majArchivesVoyages();
    }

    // Init
    majNavVacances();
    majArchivesVoyages();



    /* Dépense : catégories en boutons (le select caché reste la source de la valeur) */
    function renderCatChips() {
        const box = document.getElementById('add_cat_chips'), sel = document.getElementById('add_cat');
        if (!box || !sel) return;
        if (!sel.value && db.categories[0]) sel.value = db.categories[0].id;
        box.innerHTML = db.categories.map(c => `<button type="button" class="cat-chip${c.id === sel.value ? ' on' : ''}" aria-pressed="${c.id === sel.value}" onclick="choisirCatChip('${c.id}')">${c.label}</button>`).join('');
    }
    function choisirCatChip(id) {
        const sel = document.getElementById('add_cat'); if (!sel) return;
        sel.value = id; renderCatChips();
    }

    /* Paramètres : enregistrement automatique */
    function autoSaveSettings() {
        var ancienneDevise = appSettings.devise || '\u20ac';
        var ancienCode = appSettings.codePartner || '';
        appSettings.prenom      = document.getElementById('set_prenom').value.trim();
        appSettings.nomMoi      = document.getElementById('set_nom_moi').value.trim();
        appSettings.nomPartner  = document.getElementById('set_nom_partner').value.trim();
        appSettings.codePartner = document.getElementById('set_code_partner').value.trim().toUpperCase();
        var vacEl = document.getElementById('set_mode_vacances'); appSettings.modeVacances = vacEl ? vacEl.checked : false;
        var coupleEl = document.getElementById('set_mode_couple'); appSettings.modeCouple = coupleEl ? coupleEl.checked : true;
        appSettings.devise    = document.getElementById('set_devise').value;
        appSettings.jourDebut = parseInt(document.getElementById('set_jour_debut').value) || 1;
        localStorage.setItem('app_settings', JSON.stringify(appSettings));
        if ((appSettings.devise || '\u20ac') !== ancienneDevise) { location.reload(); return; }
        appliquerSettings();
        majAffichage();
        if ((appSettings.codePartner || '') !== ancienCode) demarrerSyncPartenaire();
        majProfilSettings();
    }
    function majSegMode(mode) {
        var l = document.getElementById('btn_theme_light'), d = document.getElementById('btn_theme_dark');
        if (l) l.classList.toggle('on', mode === 'light');
        if (d) d.classList.toggle('on', mode === 'dark');
    }
    function majProfilSettings() {
        var p = appSettings.prenom || '';
        var av = document.getElementById('st-avatar'); if (av) av.textContent = p ? p.charAt(0).toUpperCase() : 'M';
        var nm = document.getElementById('st-pname'); if (nm) nm.textContent = p || 'Mon Coach Finance';
    }
    function majFondLabel() {
        var el = document.getElementById('st-fond-val'); if (!el) return;
        var clair = FONDS_CLAIR.find(function (f) { return f.id === (appSettings.fondClair || 'slate'); }) || FONDS_CLAIR[0];
        var sombre = FONDS_SOMBRE.find(function (f) { return f.id === (appSettings.fondSombre || 'deep'); }) || FONDS_SOMBRE[0];
        el.textContent = (localStorage.getItem('theme') === 'dark' ? sombre : clair).nom;
    }
    function toggleFondsSettings() {
        var box = document.getElementById('st-fonds'), chev = document.getElementById('st-fond-chev');
        if (!box) return;
        var open = box.style.display === 'none';
        box.style.display = open ? 'flex' : 'none';
        if (chev) chev.classList.toggle('open', open);
    }

    /* Bilan : total dépensé (hors épargne) vs budget prévu (hors épargne) */
    function renderBilanResume(totalDep) {
        const budget = db.categories.filter(c => !c.label.toLowerCase().includes('épargne')).reduce((s, c) => s + (db.previsions[c.id] || 0), 0);
        const pct = budget > 0 ? Math.round(totalDep / budget * 100) : 0;
        const reste = budget - totalDep;
        const cls = budget > 0 && totalDep > budget ? 'over' : (pct >= 85 ? 'warn' : 'ok');
        ['', '_d'].forEach(sfx => {
            const g = id => document.getElementById(id + sfx);
            if (!g('bl_dep')) return;
            g('bl_dep').innerText = totalDep.toFixed(2);
            g('bl_budget').innerText = budget.toFixed(0);
            const pill = g('bl_pill'); pill.className = 'bc-pill ' + cls; pill.innerText = budget > 0 ? pct + ' %' : 'Pas de budget';
            const bar = g('bl_bar'); bar.style.width = Math.min(pct, 100) + '%';
            bar.style.background = cls === 'over' ? 'var(--danger)' : cls === 'warn' ? '#d97706' : 'var(--main)';
            const r = g('bl_reste');
            r.innerText = budget <= 0 ? 'Définis tes limites dans Budget pour suivre ton total.' : (reste >= 0 ? 'Il te reste ' + reste.toFixed(2) + ' € à dépenser' : 'Budget dépassé de ' + Math.abs(reste).toFixed(2) + ' €');
            r.style.color = reste < 0 && budget > 0 ? 'var(--danger)' : '';
        });
    }

    /* Couleurs des catégories : 8 couleurs choisies, puis génération automatique.
       Chaque catégorie garde sa couleur (enregistrée à sa création). */
    function hslVersHex(h, s, l) {
        s /= 100; l /= 100;
        const k = n => (n + h / 30) % 12, a = s * Math.min(l, 1 - l);
        const f = n => l - a * Math.max(-1, Math.min(k(n) - 3, Math.min(9 - k(n), 1)));
        return '#' + [f(0), f(8), f(4)].map(x => Math.round(x * 255).toString(16).padStart(2, '0')).join('');
    }
    function nouvelleCouleurCategorie() {
        const prises = new Set(db.categories.map(c => (c.color || '').toLowerCase()).filter(Boolean));
        const PALETTE_CATEGORIES = ['#4f46e5', '#0e7490', '#d97706', '#be185d', '#047857', '#9333ea', '#64748b', '#c2410c'];
        const libre = PALETTE_CATEGORIES.find(c => !prises.has(c));
        if (libre) return libre;
        // Au-delà de la palette : rotation sur le cercle chromatique (angle d'or) pour espacer les teintes
        for (let n = prises.size; n < prises.size + 400; n++) {
            const hue = Math.round((n * 137.508 + 20) % 360);
            const lum = [44, 36, 52][n % 3];
            const col = hslVersHex(hue, 62, lum);
            if (!prises.has(col)) return col;
        }
        return '#64748b';
    }
    function assurerCouleursCategories() {
        let modifie = false;
        db.categories.forEach(c => { if (!c.color) { c.color = nouvelleCouleurCategorie(); modifie = true; } });
        if (modifie) localStorage.setItem('budget_vGestion', JSON.stringify(db));
    }

    /* Dépense : pastille de couleur de la catégorie choisie */
    function majPastilleCat() {
        const sel = document.getElementById('add_cat'), dot = document.getElementById('add_cat_dot');
        if (!sel || !dot) return;
        const cat = db.categories.find(c => c.id === sel.value);
        dot.style.background = cat && cat.color ? cat.color : 'var(--text-hint)';
    }
    /* Dépense : historique regroupé par jour */
    function renderHistoriqueGroupe(searchTerm, boxId, compteurs) {
        if (boxId === undefined) { boxId = 'dp_history'; compteurs = true; }
        const box = document.getElementById(boxId); if (!box) return;
        const reEmoji = /^(\p{Extended_Pictographic}(?:️|‍\p{Extended_Pictographic})*️?)\s*/u;
        const fmt = n => n.toFixed(2).replace('.', ',') + ' €';
        const parse = s => { const p = String(s || '').split('/'); return p.length === 3 ? new Date(+p[2], +p[1] - 1, +p[0]) : null; };
        const auj = new Date(); auj.setHours(0, 0, 0, 0);
        const hier = new Date(auj); hier.setDate(hier.getDate() - 1);
        const libelleJour = s => { const d = parse(s); if (!d) return s; if (d.getTime() === auj.getTime()) return "Aujourd'hui"; if (d.getTime() === hier.getTime()) return 'Hier'; const t = d.toLocaleDateString('fr-FR', { weekday: 'long', day: 'numeric', month: 'long' }); return t.charAt(0).toUpperCase() + t.slice(1); };
        const filtres = [...db.depenses].reverse().filter(d => {
            if (!searchTerm) return true;
            const cat = db.categories.find(c => c.id === d.ct);
            return d.desc.toLowerCase().includes(searchTerm) || (cat && cat.label.toLowerCase().includes(searchTerm)) || (d.note && d.note.toLowerCase().includes(searchTerm));
        });
        // compteur
        let totDep = 0;
        db.depenses.forEach(d => { const c = db.categories.find(x => x.id === d.ct); if (!(c && c.label.toLowerCase().includes('épargne'))) totDep += d.mt; });
        const cnt = compteurs ? document.getElementById('dp_count') : null;
        if (cnt) cnt.innerText = db.depenses.length ? db.depenses.length + ' opération' + (db.depenses.length > 1 ? 's' : '') + ' · ' + Math.round(totDep).toLocaleString('fr-FR') + ' € dépensés' : '';
        const hint = compteurs ? document.getElementById('dp_hint') : null; if (hint) hint.style.display = filtres.length ? 'block' : 'none';
        if (!filtres.length) { box.innerHTML = `<div class="bg-empty">${searchTerm ? 'Aucun résultat pour cette recherche.' : 'Aucune dépense ce mois-ci.'}</div>`; return; }
        // regroupement par date (ordre d'affichage conservé)
        const groupes = [];
        filtres.forEach(d => { let g = groupes.find(x => x.date === d.date); if (!g) { g = { date: d.date, ops: [] }; groupes.push(g); } g.ops.push(d); });
        box.innerHTML = groupes.map(g => {
            let totJour = 0;
            const lignes = g.ops.map(d => {
                const cat = db.categories.find(c => c.id === d.ct);
                const lbl = cat ? cat.label : 'Sans catégorie';
                const ep = lbl.toLowerCase().includes('épargne');
                if (!ep) totJour += d.mt;
                const m = lbl.match(reEmoji);
                const nom = m ? (lbl.slice(m[0].length).trim() || lbl) : lbl;
                const icon = m ? m[1] : (nom.trim().charAt(0) || '?').toUpperCase();
                return `<div class="dp-op" onclick="ouvrirEditDepense(${d.id})">
                    <span class="dp-ico${m ? ' emoji' : ''}" style="--c:${(cat && cat.color) || '#64748b'};">${icon}</span>
                    <div class="dp-main"><span class="dp-name">${d.desc}${d.recurring ? ' 🔄' : ''}</span><span class="dp-meta">${nom}${d.note ? ' · ' + d.note : ''}</span></div>
                    <span class="dp-amt${ep ? ' pos' : ''}">${ep ? '+' : '−'}${fmt(d.mt)}</span>
                </div>`;
            }).join('');
            return `<div class="dp-day"><span>${libelleJour(g.date)}</span><span>${totJour ? '−' + fmt(totJour) : ''}</span></div>${lignes}`;
        }).join('');
    }

    /* Budget mobile : revenus, répartition, limites par catégorie */
    function renderBudgetMobile(revenuTotal) {
        const div = document.getElementById('setup_categories'); if (!div) return;
        const reEmoji = /^(\p{Extended_Pictographic}(?:️|‍\p{Extended_Pictographic})*️?)\s*/u;
        let total = 0;
        div.innerHTML = db.categories.map(cat => {
            const val = db.previsions[cat.id] || 0; total += val;
            const m = cat.label.match(reEmoji);
            const name = m ? (cat.label.slice(m[0].length).trim() || cat.label) : cat.label;
            const icon = m ? m[1] : (name.trim().charAt(0) || '?').toUpperCase();
            const pct = revenuTotal > 0 ? Math.round(val / revenuTotal * 100) : 0;
            return `<div class="bg-row" style="--c:${cat.color || '#64748b'};">
                <div class="bg-ico${m ? ' emoji' : ''}">${icon}</div>
                <div class="bg-main"><span class="bg-name">${name}</span><span class="bg-sub">${revenuTotal > 0 ? pct + ' % des revenus' : '—'}</span></div>
                <label class="bg-amt"><input type="number" inputmode="decimal" class="prev-input" data-cat="${cat.id}" value="${val}" onchange="sauvegarder()" aria-label="Limite ${name}"><b>€</b></label>
                <button class="bg-del" aria-label="Supprimer ${name}" onclick="supprimerCategorie('${cat.id}')"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M8 6V4h8v2"/><path d="M19 6l-1 14H6L5 6"/></svg></button>
            </div>`;
        }).join('') || '<div class="bg-empty">Aucune catégorie pour le moment.</div>';
        const setT = (id, v) => { const el = document.getElementById(id); if (el) el.innerText = v; };
        setT('bg_rev_total', revenuTotal.toFixed(revenuTotal % 1 ? 2 : 0));
        setT('total_prevu_val', total.toFixed(0) + ' €');
        setT('bg_nb', db.categories.length + ' catégorie' + (db.categories.length > 1 ? 's' : ''));
        const reste = revenuTotal - total;
        setT('bg_reste_lbl', reste < 0 ? 'Dépassement' : 'Reste à répartir');
        const r = document.getElementById('bg_reste'); if (r) { r.innerText = Math.abs(reste).toFixed(0) + ' €'; r.classList.toggle('neg', reste < 0); }
        setT('bg_pct', revenuTotal > 0 ? (reste < 0 ? 'Tu as planifié ' + Math.abs(reste).toFixed(0) + ' € de plus que tes revenus' : Math.round(total / revenuTotal * 100) + ' % de tes revenus sont attribués à une catégorie') : 'Renseigne tes revenus pour voir la répartition');
        const st = document.getElementById('bg_stack');
        if (st) {
            const base = Math.max(revenuTotal, total) || 1;
            st.innerHTML = db.categories.filter(c => (db.previsions[c.id] || 0) > 0).map(c => `<div style="width:${((db.previsions[c.id] || 0) / base * 100).toFixed(2)}%;background:${c.color || '#64748b'};"></div>`).join('');
        }
    }

    /* Bilan mobile : cartes par catégorie, dépassements en premier */
    function renderBilanCartes(totaux) {
        const box = document.getElementById('bilan_cards'); if (!box) return;
        const PALETTE = ['#4f46e5', '#0e7490', '#d97706', '#be185d', '#047857', '#9333ea', '#64748b', '#c2410c'];
        const reEmoji = /^(\p{Extended_Pictographic}(?:️|‍\p{Extended_Pictographic})*️?)\s*/u;
        const fmt = n => (Math.round(n * 100) / 100).toFixed(n % 1 ? 2 : 0) + ' €';
        const rows = db.categories.map((cat, i) => {
            const prev = db.previsions[cat.id] || 0, reel = totaux[cat.id] || 0;
            const m = cat.label.match(reEmoji);
            const name = m ? cat.label.slice(m[0].length).trim() || cat.label : cat.label;
            const over = reel > prev, done = !over && prev > 0 && reel === prev;
            const pct = prev > 0 ? Math.min(reel / prev * 100, 100) : (reel > 0 ? 100 : 0);
            const warn = !over && !done && prev > 0 && pct >= 85;
            return {
                name, icon: m ? m[1] : (name.trim().charAt(0) || '?').toUpperCase(), emoji: !!m,
                prev, reel, over, done, warn, pct,
                ratio: prev > 0 ? reel / prev : (reel > 0 ? Infinity : 0),
                col: cat.color || PALETTE[i % PALETTE.length],
                epargne: cat.label.toLowerCase().includes('épargne')
            };
        });
        // Grand anneau : budget hors épargne, un arc par catégorie
        const budgetRows = rows.filter(r => !r.epargne && r.prev > 0);
        const budget = budgetRows.reduce((s, r) => s + r.prev, 0);
        const dep = rows.filter(r => !r.epargne).reduce((s, r) => s + r.reel, 0);
        // Anneau interactif (comme le camembert) : répartition de ce qui a été dépensé, hors épargne
        const canvas = document.getElementById('bd_canvas');
        if (canvas && typeof Chart !== 'undefined') {
            const parts = rows.filter(r => !r.epargne && r.reel > 0);
            const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
            const resteVal = 0;
            const resteCol = isDark ? 'rgba(255,255,255,0.12)' : 'rgba(17,19,26,0.08)';
            const cardCol = getComputedStyle(document.getElementById('bilan-card-m')).backgroundColor || (isDark ? '#1a1b21' : '#ffffff');
            const vide = parts.length === 0 && resteVal <= 0;
            if (window.bdChart) window.bdChart.destroy();
            window.bdChart = new Chart(canvas.getContext('2d'), {
                type: 'doughnut',
                data: {
                    labels: vide ? ['Aucune dépense'] : parts.map(r => r.name).concat(resteVal > 0 ? ['Reste'] : []),
                    datasets: [{
                        data: vide ? [1] : parts.map(r => Math.round(r.reel * 100) / 100).concat(resteVal > 0 ? [Math.round(resteVal * 100) / 100] : []),
                        backgroundColor: vide ? [resteCol] : parts.map(r => r.col).concat(resteVal > 0 ? [resteCol] : []),
                        hoverBackgroundColor: vide ? [resteCol] : parts.map(r => r.col).concat(resteVal > 0 ? [isDark ? 'rgba(255,255,255,0.20)' : 'rgba(17,19,26,0.14)'] : []),
                        hoverBorderColor: cardCol,
                        borderWidth: vide ? 0 : 3, borderColor: cardCol, borderRadius: 4, hoverOffset: 8
                    }]
                },
                options: {
                    responsive: true, maintainAspectRatio: false, cutout: '72%', layout: { padding: 8 },
                    plugins: {
                        legend: { display: false },
                        tooltip: { enabled: false }
                    },
                    // Toucher un segment : le centre affiche la catégorie ; toucher ailleurs : retour au total
                    onClick: (evt, els) => afficherCentreAnneau(els.length && !vide ? els[0].index : null),
                    onHover: (evt, els) => { if (evt.native && evt.native.pointerType !== 'touch' && evt.native.type === 'mousemove') afficherCentreAnneau(els.length && !vide ? els[0].index : null); },
                    animation: { animateRotate: true, duration: 600 }
                }
            });
        }
        const totalParts = rows.filter(r => !r.epargne && r.reel > 0);
        const sommeParts = totalParts.filter(r => !r.reste).reduce((s, r) => s + r.reel, 0);
        window.afficherCentreAnneau = idx => {
            const lbl = document.getElementById('bd_lbl');
            if (idx === null || idx === undefined || !totalParts[idx]) {
                if (lbl) lbl.innerText = budget > 0 && dep > budget ? 'Dépassé de' : 'Reste';
                setT('bd_dep', fmt(budget > 0 ? Math.abs(budget - dep) : 0)); setT('bd_budget', fmt(dep) + ' dépensés\nsur ' + fmt(budget));
                const amt = document.getElementById('bd_dep'); if (amt) amt.style.color = budget > 0 && dep > budget ? 'var(--danger)' : 'var(--success)';
                const p = document.getElementById('bd_pill');
                if (p) { p.className = 'bc-pill ' + clsTot; p.innerText = budget > 0 ? pctTot + ' % du budget' : 'Pas de budget'; p.style.cssText = ''; }
                if (window.bdChart) window.bdChart.setActiveElements([]), window.bdChart.update('none');
                return;
            }
            const r = totalParts[idx];
            if (r.reste) {
                if (lbl) lbl.innerText = 'Reste à dépenser';
                setT('bd_dep', fmt(r.reel)); setT('bd_budget', 'sur ' + fmt(budget));
                const pr = document.getElementById('bd_pill');
                if (pr) { pr.className = 'bc-pill ok'; pr.style.cssText = ''; pr.innerText = Math.round(r.reel / budget * 100) + ' % du budget'; }
                if (window.bdChart) { window.bdChart.setActiveElements([{ datasetIndex: 0, index: idx }]); window.bdChart.update('none'); }
                return;
            }
            if (lbl) lbl.innerText = r.name;
            const amtC = document.getElementById('bd_dep'); if (amtC) amtC.style.color = '';
            setT('bd_dep', fmt(r.reel));
            setT('bd_budget', 'sur ' + fmt(r.prev));
            const p = document.getElementById('bd_pill');
            if (p) { p.className = 'bc-pill'; p.innerText = Math.round(r.reel / sommeParts * 100) + ' % des dépenses'; const dk = document.documentElement.getAttribute('data-theme') === 'dark'; p.style.cssText = 'background:color-mix(in srgb,' + r.col + (dk ? ' 26%' : ' 14%') + ', var(--card));color:' + (dk ? 'color-mix(in srgb,' + r.col + ' 50%, #ffffff)' : r.col) + ';'; }
            if (window.bdChart) { window.bdChart.setActiveElements([{ datasetIndex: 0, index: idx }]); window.bdChart.update('none'); }
        };
        const pctTot = budget > 0 ? Math.round(dep / budget * 100) : 0;
        const clsTot = budget > 0 && dep > budget ? 'over' : (pctTot >= 85 ? 'warn' : 'ok');
        const setT = (id, v) => { const el = document.getElementById(id); if (el) el.innerText = v; };
        window.afficherCentreAnneau(null);
        const pill = document.getElementById('bd_pill');
        if (pill) { pill.className = 'bc-pill ' + clsTot; pill.innerText = budget > 0 ? pctTot + ' % du budget' : 'Pas de budget'; }
        const reste = document.getElementById('bd_reste');
        if (reste) {
            reste.innerText = budget <= 0 ? 'Définis tes limites dans Budget pour suivre ton total.' : (dep <= budget ? 'Il te reste ' + fmt(budget - dep) + ' à dépenser' : 'Budget dépassé de ' + fmt(dep - budget));
            reste.style.color = budget > 0 && dep > budget ? 'var(--danger)' : 'var(--success)';
        }
        // Lignes : dépassements en premier
        rows.sort((x, y) => (y.over - x.over) || (y.ratio - x.ratio));
        if (rows.length === 0) { box.innerHTML = '<div class="empty-state" style="padding:20px 0;"><p>Aucune catégorie.</p></div>'; return; }
        box.innerHTML = rows.map(r => {
            const col = r.over ? 'var(--danger)' : r.col;
            const cls = r.over ? 'over' : r.done ? 'done' : r.warn ? 'warn' : 'ok';
            const status = r.over ? (r.prev > 0 ? 'Dépassé' : 'Sans budget') : r.done ? 'Atteint' : r.warn ? 'Attention' : Math.round(r.pct) + ' %';
            const resteTxt = r.over ? 'Dépassé de ' + fmt(r.reel - r.prev) : r.done ? 'Budget atteint' : 'Reste ' + fmt(r.prev - r.reel);
            return `<div class="bm-row" style="--c:${r.col};">
                <div class="bm-ring" style="background:conic-gradient(${col} 0 ${r.pct}%, color-mix(in srgb, ${r.col} 18%, var(--card)) ${r.pct}% 100%);"><div class="bm-ico${r.emoji ? ' emoji' : ''}">${r.icon}</div></div>
                <div class="bm-main">
                    <div class="bm-top"><span class="bm-name">${r.name}</span><span class="bc-pill ${cls}">${status}</span></div>
                    <div class="bm-bar"><div class="bm-fill" style="width:${r.pct}%;background:${col};"></div></div>
                    <div class="bm-bot"><span><strong>${fmt(r.reel)}</strong> sur ${fmt(r.prev)}</span><span class="bm-reste ${r.over ? 'over' : r.done ? 'done' : ''}">${resteTxt}</span></div>
                </div>
            </div>`;
        }).join('');
    }
    /* ══════════════════════════════════
       REFONTE — menu « Plus », cycle budgétaire, devise
    ══════════════════════════════════ */
    function ouvrirPlus() { var s = document.getElementById('plus-sheet'); if (s) s.style.display = 'flex'; }
    function fermerPlus() { var s = document.getElementById('plus-sheet'); if (s) s.style.display = 'none'; }
    function allerPrevisions() {
        goTo('budget');
        setTimeout(function () {
            var el = document.getElementById('prev_desc');
            if (el && el.offsetParent !== null) el.scrollIntoView({ behavior: 'smooth', block: 'center' });
        }, 60);
    }

    // Cycle budgétaire : du « jour de début » choisi dans les paramètres jusqu'à la veille du même jour le mois suivant
    function getCycle() {
        var jd = 1;
        try { jd = parseInt(appSettings.jourDebut) || 1; } catch (e) { jd = 1; }
        jd = Math.min(Math.max(jd, 1), 28);
        var now = new Date();
        var today = new Date(now.getFullYear(), now.getMonth(), now.getDate());
        var start = new Date(now.getFullYear(), now.getMonth(), jd);
        if (today < start) start = new Date(now.getFullYear(), now.getMonth() - 1, jd);
        var next = new Date(start.getFullYear(), start.getMonth() + 1, jd);
        var jour = 86400000;
        var total = Math.round((next - start) / jour);
        var ecoules = Math.round((today - start) / jour) + 1;
        return { start: start, next: next, total: total, ecoules: ecoules, restants: total - ecoules };
    }

    // Devise : remplace « € » par la devise choisie partout sauf dans le mode Vacances (toujours converti en euros)
    function appliquerDeviseDOM(root) {
        if (!root || DEVISE === '€') return;
        var walker = document.createTreeWalker(root, NodeFilter.SHOW_TEXT);
        var n;
        while ((n = walker.nextNode())) {
            if (n.nodeValue.indexOf('€') === -1) continue;
            var p = n.parentElement;
            if (!p || p.closest('#page-vacances, #set_devise, script, style')) continue;
            n.nodeValue = n.nodeValue.split('€').join(DEVISE);
        }
        if (root.querySelectorAll) {
            root.querySelectorAll('input[placeholder*="€"]').forEach(function (inp) {
                if (!inp.closest('#page-vacances')) inp.placeholder = inp.placeholder.split('€').join(DEVISE);
            });
        }
    }
    var deviseObserver = new MutationObserver(function (muts) {
        if (DEVISE === '€') return;
        deviseObserver.disconnect();
        muts.forEach(function (m) {
            if (m.type === 'characterData') appliquerDeviseDOM(m.target.parentNode);
            else m.addedNodes.forEach(function (nd) {
                if (nd.nodeType === 3) appliquerDeviseDOM(nd.parentNode);
                else if (nd.nodeType === 1) appliquerDeviseDOM(nd);
            });
        });
        observerDevise();
    });
    function observerDevise() { deviseObserver.observe(document.body, { childList: true, subtree: true, characterData: true }); }

    // Init refonte
    majAffichage();
    appliquerDeviseDOM(document.body);
    observerDevise();

</script>
</body>
</html>
