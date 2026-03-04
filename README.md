<!DOCTYPE html>
<html lang="fr" data-theme="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <title>Mon Coach Finance - Gestion Totale</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --main: #6366f1;
            --main-light: #818cf8;
            --main-glow: rgba(99,102,241,0.25);
            --bg: #f0f4ff;
            --bg2: #e8ecf8;
            --card: rgba(255,255,255,0.75);
            --card-border: rgba(255,255,255,0.9);
            --text: #1e293b;
            --text-muted: #64748b;
            --danger: #ef4444;
            --success: #22c55e;
            --warning: #f59e0b;
            --shadow: 0 8px 32px rgba(99,102,241,0.10);
            --shadow-hover: 0 16px 48px rgba(99,102,241,0.18);
            --glass-blur: blur(18px);
            --radius: 18px;
            --transition: 0.28s cubic-bezier(.4,0,.2,1);
        }
        [data-theme="dark"] {
            --bg: #0f1021;
            --bg2: #181a2e;
            --card: rgba(24,26,46,0.85);
            --card-border: rgba(99,102,241,0.18);
            --text: #e2e8f0;
            --text-muted: #94a3b8;
            --shadow: 0 8px 32px rgba(0,0,0,0.4);
            --shadow-hover: 0 16px 48px rgba(99,102,241,0.25);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg);
            color: var(--text);
            min-height: 100vh;
            transition: background var(--transition), color var(--transition);
        }
        body::before {
            content: ''; position: fixed; top: -200px; left: -200px;
            width: 600px; height: 600px;
            background: radial-gradient(circle, rgba(99,102,241,0.12) 0%, transparent 70%);
            pointer-events: none; z-index: 0;
        }
        body::after {
            content: ''; position: fixed; bottom: -200px; right: -100px;
            width: 500px; height: 500px;
            background: radial-gradient(circle, rgba(34,197,94,0.08) 0%, transparent 70%);
            pointer-events: none; z-index: 0;
        }

        .container { max-width: 1240px; margin: auto; padding: 28px 20px; position: relative; z-index: 1; }

        /* HEADER */
        header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 32px; gap: 16px; flex-wrap: wrap; }
        .header-title h1 {
            font-family: 'Inter', sans-serif; font-size: 1.8rem; font-weight: 800;
            background: linear-gradient(135deg, #6366f1, #a78bfa, #22c55e);
            -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
        }
        .header-title p { color: var(--text-muted); font-size: 0.85rem; margin-top: 2px; }
        .header-actions { display: flex; gap: 10px; align-items: center; }

        /* DARK MODE TOGGLE */
        .theme-toggle {
            width: 52px; height: 28px; background: var(--bg2); border-radius: 999px;
            border: 2px solid var(--card-border); cursor: pointer; position: relative;
            transition: background var(--transition); flex-shrink: 0;
        }
        .theme-toggle::after {
            content: '☀️'; position: absolute; top: 1px; left: 2px;
            width: 22px; height: 22px; border-radius: 50%; background: white;
            display: flex; align-items: center; justify-content: center;
            font-size: 12px; transition: transform var(--transition); line-height: 22px; text-align: center;
        }
        [data-theme="dark"] .theme-toggle::after { content: '🌙'; transform: translateX(24px); }

        /* CARDS */
        .card {
            background: var(--card); backdrop-filter: var(--glass-blur); -webkit-backdrop-filter: var(--glass-blur);
            padding: 24px; border-radius: var(--radius); border: 1px solid var(--card-border);
            box-shadow: var(--shadow); margin-bottom: 22px;
            transition: box-shadow var(--transition), transform var(--transition), background var(--transition);
            animation: slideUp 0.4s ease both;
        }
        .card:hover { box-shadow: var(--shadow-hover); }
        @keyframes slideUp { from { opacity: 0; transform: translateY(16px); } to { opacity: 1; transform: translateY(0); } }

        h2 {
            font-family: 'Inter', sans-serif; font-size: 1rem; font-weight: 700;
            letter-spacing: 0.02em; border-bottom: 2px solid var(--bg2);
            padding-bottom: 10px; margin-bottom: 16px; color: var(--text);
        }
        h2 .step-badge {
            display: inline-flex; align-items: center; justify-content: center;
            width: 24px; height: 24px; background: var(--main); color: white;
            border-radius: 50%; font-size: 0.75rem; margin-right: 8px;
        }

        /* INPUTS */
        input[type="text"], input[type="number"], select {
            width: 100%; padding: 10px 14px; margin: 4px 0 12px 0;
            border-radius: 10px; border: 1.5px solid var(--bg2);
            background: var(--bg); color: var(--text);
            font-family: 'Inter', sans-serif; font-size: 0.9rem;
            transition: border-color var(--transition), box-shadow var(--transition), background var(--transition);
            outline: none;
        }
        input:focus, select:focus { border-color: var(--main); box-shadow: 0 0 0 3px var(--main-glow); }
        label { font-size: 0.82rem; font-weight: 500; color: var(--text-muted); display: block; margin-bottom: 2px; }

        button {
            width: 100%; padding: 11px 18px; margin: 6px 0; border-radius: 10px; border: none;
            font-family: 'Inter', sans-serif; font-weight: 600; font-size: 0.9rem;
            cursor: pointer; transition: all var(--transition);
        }
        .btn-primary { background: linear-gradient(135deg, #6366f1, #818cf8); color: white; box-shadow: 0 4px 14px var(--main-glow); }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 24px var(--main-glow); }
        .btn-primary:active { transform: translateY(0); }
        .btn-secondary { background: var(--bg2); color: var(--text-muted); }
        .btn-secondary:hover { background: #dde3f5; color: var(--text); }
        [data-theme="dark"] .btn-secondary:hover { background: #2a2d47; }
        .btn-danger-soft { background: #fee2e2; color: var(--danger); width: 32px; height: 32px; padding: 0; border-radius: 8px; border: none; font-size: 0.8rem; flex-shrink: 0; }
        .btn-danger-soft:hover { background: var(--danger); color: white; transform: scale(1.1); }
        .btn-icon { background: none; border: none; color: red; padding: 0; margin: 0; width: auto; font-size: 1rem; cursor: pointer; transition: transform var(--transition); }
        .btn-icon:hover { transform: scale(1.2); }

        /* STAT GRID */
        .stat-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin-bottom: 24px; }
        .stat-box {
            background: var(--card); backdrop-filter: var(--glass-blur); -webkit-backdrop-filter: var(--glass-blur);
            padding: 18px 16px; border-radius: var(--radius); border: 1px solid var(--card-border);
            box-shadow: var(--shadow); text-align: center; transition: all var(--transition);
            animation: slideUp 0.4s ease both; cursor: default;
        }
        .stat-box:hover { transform: translateY(-3px); box-shadow: var(--shadow-hover); }
        .stat-box .stat-label { font-size: 0.75rem; color: var(--text-muted); font-weight: 500; text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 8px; }
        .stat-box .stat-value { font-family: 'Inter', sans-serif; font-size: 2.4rem; font-weight: 800; display: flex; align-items: baseline; justify-content: center; gap: 4px; }
        .stat-box .stat-value span { font-size: 2rem; font-weight: 800; color: inherit; }
        .stat-box:nth-child(4) { border: 2px solid var(--warning); }
        .stat-box-gold .stat-value { color: #b45309; }
        .stat-box-gold .stat-label { color: #d97706; }

        .grid-top { display: grid; grid-template-columns: 1fr 2fr; gap: 22px; margin-bottom: 22px; }

        .budget-row { display: flex; gap: 8px; align-items: center; margin-bottom: 8px; animation: fadeIn 0.3s ease; }
        .budget-row .cat-label { flex: 1; font-size: 0.88rem; font-weight: 500; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .budget-row input { width: 90px; margin: 0; }
        @keyframes fadeIn { from { opacity:0; transform:translateX(-8px); } to { opacity:1; transform:translateX(0); } }

        .add-cat-box { display: flex; gap: 8px; margin-top: 14px; padding-top: 14px; border-top: 1px dashed var(--bg2); }
        .add-cat-box input { margin: 0; flex: 1; }
        .add-cat-box .btn-add-cat { background: linear-gradient(135deg, #6366f1, #818cf8); color: white; width: 42px; padding: 0; margin: 0; height: 42px; border-radius: 10px; font-size: 1.3rem; line-height: 1; }

        .plan-summary { margin-top: 14px; padding: 14px 16px; border-radius: 12px; background: var(--bg); border: 1px solid var(--bg2); font-size: 0.88rem; }
        .plan-summary .row { display: flex; justify-content: space-between; margin-bottom: 6px; align-items: center; }
        .plan-summary .row:last-child { margin-bottom: 0; }
        .text-danger { color: var(--danger); font-weight: 700; }
        .text-success { color: var(--success); font-weight: 700; }

        .add-expense-grid { display: grid; grid-template-columns: 1fr 1fr 1fr 46px; gap: 10px; align-items: end; }
        .add-expense-grid button { height: 42px; border-radius: 10px; background: linear-gradient(135deg, #6366f1, #818cf8); color: white; font-size: 1.3rem; width: 46px; padding: 0; margin: 0; }
        .recurring-row { display: flex; align-items: center; gap: 8px; font-size: 0.85rem; color: var(--text-muted); margin: 8px 0 14px; }
        .recurring-row input[type="checkbox"] { width: 16px; height: 16px; accent-color: var(--main); cursor: pointer; margin: 0; }

        .search-wrap { margin-bottom: 12px; }
        .search-wrap input { margin: 0; }

        /* TABLE */
        .table-wrap { max-height: 250px; overflow-y: auto; border-radius: 10px; border: 1px solid var(--bg2); background: var(--card); }
        .table-wrap::-webkit-scrollbar { width: 6px; }
        .table-wrap::-webkit-scrollbar-track { background: transparent; }
        .table-wrap::-webkit-scrollbar-thumb { background: var(--main-light); border-radius: 3px; }
        table { width: 100%; border-collapse: collapse; }
        thead { position: sticky; top: 0; z-index: 2; }
        th { text-align: left; font-size: 0.75rem; color: var(--text-muted); padding: 10px 12px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; border-bottom: 2px solid var(--bg2); background: var(--bg); }
        td { padding: 10px 12px; border-top: 1px solid var(--bg2); font-size: 0.88rem; color: var(--text); background: transparent; }
        tbody tr { transition: background var(--transition); }
        tbody tr:hover { background: var(--bg2); }
        [data-theme="dark"] thead th { background: var(--bg2); }
        [data-theme="dark"] tbody tr:hover { background: rgba(99,102,241,0.08); }
        .new-row td { animation: rowIn 0.35s ease; }
        @keyframes rowIn { from { opacity:0; background: rgba(99,102,241,0.08); } to { opacity:1; } }

        .status-badge { padding: 2px 6px; border-radius: 20px; font-size: 0.62rem; font-weight: 700; letter-spacing: 0.02em; white-space: nowrap; }
        .bg-success { background: rgba(34,197,94,0.15); color: #16a34a; border: 1px solid rgba(34,197,94,0.3); }
        .bg-danger { background: rgba(239,68,68,0.12); color: #dc2626; border: 1px solid rgba(239,68,68,0.25); }

        .progress-wrap { display: flex; align-items: center; gap: 8px; min-width: 120px; }
        .progress-bar-bg { background: var(--bg2); height: 7px; border-radius: 4px; overflow: hidden; flex: 1; }
        .progress-bar-fill { height: 100%; background: linear-gradient(90deg, #6366f1, #818cf8); border-radius: 4px; transition: width 0.8s cubic-bezier(.4,0,.2,1); }
        .progress-bar-fill.over { background: linear-gradient(90deg, #ef4444, #f87171); }
        .progress-pct { font-size: 0.72rem; color: var(--text-muted); width: 32px; text-align: right; flex-shrink: 0; }

        .bilan-layout { display: flex; gap: 24px; flex-wrap: wrap; }
        .bilan-table-side { flex: 2; min-width: 300px; }
        #bilan_table th { font-size: 0.7rem; padding: 10px 14px; }
        #bilan_table td { padding: 12px 14px; vertical-align: middle; font-size: 0.85rem; }
        #bilan_table tbody tr { border-bottom: 1px solid var(--bg2); }
        #bilan_table tbody tr:last-child { border-bottom: none; }
        .bilan-cat-name { font-weight: 600; font-size: 0.88rem; color: var(--text); }
        .bilan-chart-side { flex: 0 0 280px; width: 280px; height: 280px; display: flex; align-items: center; justify-content: center; }

        .recurring-tag { font-size: 0.68rem; color: var(--main); font-weight: 600; margin-left: 5px; background: var(--main-glow); padding: 1px 5px; border-radius: 20px; }

        .epargne-total-row { display: flex; justify-content: space-between; align-items: center; margin-top: 12px; padding: 12px 16px; background: linear-gradient(135deg, rgba(245,158,11,0.08), rgba(245,158,11,0.04)); border: 1px solid rgba(245,158,11,0.3); border-radius: 12px; }
        .epargne-total-row .label { font-size: 0.85rem; color: #b45309; font-weight: 600; }
        .epargne-total-row .value { font-family: 'Inter', sans-serif; font-size: 1.3rem; font-weight: 800; color: #b45309; }

        /* TOAST */
        #toast-container { position: fixed; top: 20px; right: 20px; z-index: 9999; display: flex; flex-direction: column; gap: 10px; pointer-events: none; }
        .toast { padding: 12px 20px; border-radius: 12px; font-size: 0.88rem; font-weight: 600; color: white; box-shadow: 0 8px 24px rgba(0,0,0,0.2); backdrop-filter: blur(12px); animation: toastIn 0.35s ease both; display: flex; align-items: center; gap: 10px; pointer-events: auto; }
        .toast.success { background: linear-gradient(135deg, #22c55e, #16a34a); }
        .toast.danger { background: linear-gradient(135deg, #ef4444, #dc2626); }
        .toast.info { background: linear-gradient(135deg, #6366f1, #818cf8); }
        .toast.warning { background: linear-gradient(135deg, #f59e0b, #d97706); }
        @keyframes toastIn { from { opacity: 0; transform: translateX(40px); } to { opacity: 1; transform: translateX(0); } }
        @keyframes toastOut { from { opacity: 1; transform: translateX(0); } to { opacity: 0; transform: translateX(40px); } }

        /* ── MODAL ── */
        .modal-overlay {
            position: fixed; inset: 0; background: rgba(0,0,0,0.55);
            backdrop-filter: blur(6px); z-index: 1000;
            display: flex; align-items: center; justify-content: center;
        }
        .modal-box {
            background: var(--card); border: 1px solid var(--card-border);
            border-radius: var(--radius); padding: 32px; width: 90%; max-width: 420px;
            box-shadow: 0 24px 64px rgba(0,0,0,0.3);
            animation: slideUp 0.3s ease;
        }
        .modal-box h3 { font-size: 1.2rem; font-weight: 800; margin-bottom: 8px; color: var(--text); }
        .modal-box p { color: var(--text-muted); font-size: 0.88rem; margin-bottom: 20px; line-height: 1.6; }
        .modal-box label { margin-bottom: 6px; }
        .modal-actions { display: flex; gap: 10px; margin-top: 4px; }
        .modal-actions button { flex: 1; margin: 0; }
        .btn-cancel { background: var(--bg2); color: var(--text-muted); }
        .btn-cancel:hover { background: #dde3f5; color: var(--text); }

        /* ── ARCHIVES ── */
        .archives-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(210px, 1fr)); gap: 18px; margin-top: 4px; }
        .archive-card {
            background: var(--bg); border: 1px solid var(--bg2);
            border-radius: 14px; padding: 18px 16px; transition: all var(--transition);
            animation: slideUp 0.35s ease both;
        }
        .archive-card:hover { transform: translateY(-3px); box-shadow: var(--shadow-hover); }
        .arc-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 14px; gap: 8px; }
        .arc-name { font-weight: 800; font-size: 0.95rem; color: var(--text); line-height: 1.3; }
        .arc-date { font-size: 0.7rem; color: var(--text-muted); white-space: nowrap; margin-top: 2px; }
        .arc-chart-wrap { display: flex; justify-content: center; margin-bottom: 14px; }
        .arc-stats { font-size: 0.78rem; color: var(--text-muted); display: flex; flex-direction: column; gap: 5px; }
        .arc-stat-row { display: flex; justify-content: space-between; align-items: center; }
        .arc-stat-row strong { color: var(--text); font-weight: 700; }
        .arc-actions { display: flex; gap: 8px; margin-top: 14px; }
        .arc-actions button { flex: 1; margin: 0; padding: 8px 10px; font-size: 0.78rem; border-radius: 8px; }
        .btn-pdf { background: linear-gradient(135deg, #6366f1, #818cf8); color: white; }
        .btn-pdf:hover { opacity: 0.9; transform: translateY(-1px); }
        .btn-del-arc { background: var(--bg2); color: var(--text-muted); }
        .btn-del-arc:hover { background: #fee2e2; color: var(--danger); }
        .empty-archives { text-align: center; padding: 48px 20px; color: var(--text-muted); }
        .empty-icon { font-size: 3rem; margin-bottom: 12px; }
        .empty-archives p { font-size: 0.88rem; line-height: 1.6; }

        /* RESPONSIVE */
        /* ── TABLET ── */
        @media (max-width: 900px) {
            .stat-grid { grid-template-columns: repeat(2, 1fr); }
            .grid-top { grid-template-columns: 1fr; }
            .add-expense-grid { grid-template-columns: 1fr 1fr; }
            .add-expense-grid button { grid-column: span 2; width: 100%; }
            .bilan-chart-side { flex: 0 0 220px; width: 220px; height: 220px; }
        }

        /* ── MOBILE ── */
        @media (max-width: 600px) {
            .container { padding: 14px 12px; }

            /* Header */
            header { flex-direction: column; align-items: flex-start; margin-bottom: 20px; gap: 10px; }
            .header-title h1 { font-size: 1.4rem; }
            .header-actions { width: 100%; justify-content: space-between; }
            .header-actions .btn-secondary { flex: 1; text-align: center; font-size: 0.82rem; padding: 9px 10px; }

            /* Stats */
            .stat-grid { grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 16px; }
            .stat-box { padding: 14px 10px; }
            .stat-box .stat-label { font-size: 0.68rem; margin-bottom: 6px; }
            .stat-box .stat-value { font-size: 1.6rem; }
            .stat-box .stat-value span { font-size: 1.3rem !important; }

            /* Cards */
            .card { padding: 16px 14px; border-radius: 14px; margin-bottom: 14px; }
            h2 { font-size: 0.92rem; }

            /* Inputs */
            input[type="text"], input[type="number"], select { padding: 10px 12px; font-size: 0.88rem; }

            /* Ajout dépense : 1 colonne */
            .add-expense-grid { grid-template-columns: 1fr 1fr; gap: 8px; }
            .add-expense-grid > div:nth-child(3) { grid-column: span 2; }
            .add-expense-grid button { grid-column: span 2; width: 100%; height: 44px; font-size: 1rem; }

            /* Budget row */
            .budget-row input { width: 80px; }

            /* Tableau historique : masquer colonne date */
            #log_table thead tr th:first-child,
            #log_table tbody tr td:first-child { display: none; }

            /* Bilan : tout en colonne */
            .bilan-layout { flex-direction: column; }
            .bilan-chart-side { flex: none; width: 220px; height: 220px; margin: 0 auto; }
            /* Bilan mobile : masquer Progression */
            #bilan_table thead tr th:nth-child(5),
            #bilan_table tbody tr td:nth-child(5) { display: none; }
            /* Réduire padding bilan sur mobile */
            #bilan_table th { padding: 8px 8px; font-size: 0.65rem; }
            #bilan_table td { padding: 10px 8px; font-size: 0.8rem; }
            .bilan-cat-name { font-size: 0.82rem; }

            /* Toast plus petit */
            #toast-container { top: 10px; right: 10px; left: 10px; }
            .toast { font-size: 0.82rem; padding: 10px 14px; }

            /* Modal */
            .modal-box { padding: 24px 18px; }

            /* Archives */
            .archives-grid { grid-template-columns: 1fr 1fr; gap: 12px; }
            .arc-name { font-size: 0.82rem; }
            .arc-date { font-size: 0.65rem; }
            .arc-stats { font-size: 0.72rem; }
            .arc-actions button { font-size: 0.72rem; padding: 6px 6px; }

            /* Graphique évolution */
            #evolution-section > div:last-child { height: 180px !important; }
        }

        /* ── TRÈS PETIT MOBILE (≤380px) ── */
        @media (max-width: 380px) {
            .stat-grid { grid-template-columns: repeat(2, 1fr); }
            .stat-box .stat-value { font-size: 1.3rem; }
            .stat-box .stat-value span { font-size: 1.1rem !important; }
            .archives-grid { grid-template-columns: 1fr; }
            header { gap: 8px; }
        }
    </style>
</head>
<body>
<div id="toast-container"></div>

<!-- ══ MODAL CLÔTURE ══ -->
<div id="modal-cloture" class="modal-overlay" style="display:none;">
    <div class="modal-box">
        <h3>📁 Clôturer le mois</h3>
        <p>Donne un nom à ce mois pour l'archiver avec son camembert. Tu pourras le télécharger en PDF et comparer les mois entre eux.</p>
        <label>Nom du mois</label>
        <input type="text" id="modal-mois-nom" placeholder="Ex : Janvier 2025...">
        <div class="modal-actions">
            <button class="btn-cancel" onclick="fermerModal()">Annuler</button>
            <button class="btn-primary" onclick="confirmerCloture()">✅ Archiver et clôturer</button>
        </div>
    </div>
</div>

<div class="container">
    <header>
        <div class="header-title">
            <h1>Mon Coach Finance</h1>
            <p>Prévu vs Réel — suivi de budget mensuel</p>
        </div>
        <div class="header-actions">
            <button class="theme-toggle" onclick="toggleTheme()" title="Changer de thème"></button>
            <button class="btn-secondary" onclick="ouvrirModal()" style="width:auto; padding: 10px 18px;">📁 Clôturer le mois</button>
        </div>
    </header>

    <div class="stat-grid">
        <div class="stat-box">
            <div class="stat-label">💸 Dépensé</div>
            <div class="stat-value"><span id="view_total_dep">0</span><span style="font-size:2rem;font-weight:800;color:inherit;"> €</span></div>
        </div>
        <div class="stat-box">
            <div class="stat-label">🌱 Épargné</div>
            <div class="stat-value" style="color: var(--success)"><span id="view_total_ep">0</span><span style="font-size:2rem;font-weight:800;color:inherit;"> €</span></div>
        </div>
        <div class="stat-box">
            <div class="stat-label">⚖️ Solde</div>
            <div class="stat-value" id="view_solde_wrapper"><span id="view_solde">0</span><span style="font-size:2rem;font-weight:800;color:inherit;"> €</span></div>
        </div>
        <div class="stat-box stat-box-gold">
            <div class="stat-label">🏦 Coffre-fort</div>
            <div class="stat-value"><span id="view_global_ep">0</span><span style="font-size:2rem;font-weight:800;color:#b45309;"> €</span></div>
            <button onclick="resetCoffre()" style="background:none; border:1px solid #d97706; padding:2px 10px; font-size:0.72rem; color:#b45309; margin-top:8px; width: auto; font-weight:600;">Reset</button>
        </div>
    </div>

    <div class="grid-top">
        <div class="card">
            <h2><span class="step-badge">1</span> Je prévois mon mois</h2>
            <label>Salaire / Revenus (€)</label>
            <input type="number" inputmode="decimal" id="prev_revenu" placeholder="Ex: 1800" onchange="sauvegarder()">
            <p style="font-size: 0.8rem; color: var(--text-muted); margin-bottom: 10px; font-weight: 500;">Fixe tes limites par catégorie :</p>
            <div id="setup_categories"></div>
            <div class="add-cat-box">
                <input type="text" id="new_cat_name" placeholder="Nouvelle catégorie...">
                <button class="btn-add-cat" onclick="ajouterNouvelleCategorie()">+</button>
            </div>
            <div class="plan-summary">
                <div class="row"><span>Total planifié</span> <strong id="total_prevu_val">0 €</strong></div>
                <div class="row" id="status_plan_container">
                    <span>Reste à répartir</span> <span class="text-success" id="reste_a_repartir">0 €</span>
                </div>
            </div>
            <button class="btn-primary" onclick="sauvegarder()" style="margin-top: 14px;">Mettre à jour les calculs</button>
        </div>

        <div class="card">
            <h2><span class="step-badge">2</span> J'ajoute mes dépenses</h2>
            <div class="add-expense-grid">
                <div><label>Objet</label><input type="text" id="add_desc" placeholder="Ex: Courses Lidl"></div>
                <div><label>Montant (€)</label><input type="number" inputmode="decimal" id="add_mt" placeholder="0.00"></div>
                <div><label>Catégorie</label><select id="add_cat"></select></div>
                <button onclick="ajouterDepense()" style="align-self:end; margin-bottom: 12px;">+</button>
            </div>
            <div class="recurring-row">
                <input type="checkbox" id="add_recurring">
                <label for="add_recurring" style="margin: 0; font-size: 0.85rem; color: var(--text-muted); cursor:pointer;">🔄 Dépense récurrente (conservée en fin de mois)</label>
            </div>
            <h2 style="margin-top: 8px;"><span class="step-badge" style="background:#94a3b8;">🔎</span> Historique</h2>
            <div class="search-wrap">
                <input type="text" id="search_input" placeholder="Rechercher par catégorie ou nom..." oninput="majAffichage()">
            </div>
            <div class="table-wrap">
                <table id="log_table">
                    <thead><tr><th>Date</th><th>Nom</th><th>Cat.</th><th>Prix</th><th></th></tr></thead>
                    <tbody></tbody>
                </table>
            </div>
        </div>
    </div>

    <div class="card">
        <h2><span class="step-badge">3</span> Bilan : Ai-je respecté mes engagements ?</h2>
        <div class="bilan-layout">
            <div class="bilan-table-side">
                <table id="bilan_table">
                    <thead><tr><th>Catégorie</th><th>Prévu</th><th>Réel</th><th>Écart</th><th>Progression</th><th>Statut</th></tr></thead>
                    <tbody></tbody>
                </table>
            </div>
            <div class="bilan-chart-side">
                <canvas id="budgetChart" style="max-width:280px; max-height:280px;"></canvas>
            </div>
        </div>
    </div>

    <div class="card">
        <h2>🏦 Espace Épargne (Suivi détaillé)</h2>
        <div class="table-wrap">
            <table id="epargne_history_table">
                <thead><tr><th>Date</th><th>Nom</th><th>Montant</th><th></th></tr></thead>
                <tbody></tbody>
            </table>
        </div>
        <div class="epargne-total-row">
            <span class="label">Total de cet espace</span>
            <span class="value"><span id="local_epargne_total">0</span> €</span>
        </div>
    </div>

    <!-- ══ ARCHIVES ══ -->
    <div class="card">
        <h2>📅 Historique des mois archivés</h2>

        <!-- GRAPHIQUE ÉVOLUTION -->
        <div id="evolution-section" style="display:none; margin-bottom:28px;">
            <div style="font-size:0.78rem;font-weight:600;color:var(--text-muted);text-transform:uppercase;letter-spacing:0.06em;margin-bottom:14px;">📈 Évolution mois par mois</div>
            <div style="position:relative;height:220px;"><canvas id="evolutionChart"></canvas></div>
        </div>

        <div id="archives-container"></div>
    </div>
</div>

<script>
    let db = JSON.parse(localStorage.getItem('budget_vGestion')) || {
        revenu: 0,
        categories: [
            { id: "Fixe", label: "Loyers/Charges" },
            { id: "Courses", label: "Alimentation" },
            { id: "Loisirs", label: "Sorties/Plaisirs" },
            { id: "Epargne", label: "Épargne" },
            { id: "Autres", label: "Divers" }
        ],
        previsions: { Fixe: 0, Courses: 0, Loisirs: 0, Epargne: 0, Autres: 0 },
        depenses: [],
        historiqueEpargne: []
    };

    let globalSavings = parseFloat(localStorage.getItem('globalSavings')) || 0;
    let archives = JSON.parse(localStorage.getItem('budget_archives')) || [];
    let chartInstance = null;
    const colors = ['#6366f1','#22c55e','#f59e0b','#ef4444','#94a3b8','#06b6d4','#a855f7','#f97316'];

    // THEME
    const savedTheme = localStorage.getItem('theme') || 'light';
    document.documentElement.setAttribute('data-theme', savedTheme);
    function toggleTheme() {
        const current = document.documentElement.getAttribute('data-theme');
        const next = current === 'light' ? 'dark' : 'light';
        document.documentElement.setAttribute('data-theme', next);
        localStorage.setItem('theme', next);
        majAffichage();
        afficherEvolution();
    }

    // TOAST
    function showToast(message, type = 'success', duration = 3000) {
        const icons = { success: '✅', danger: '❌', info: 'ℹ️', warning: '⚠️' };
        const container = document.getElementById('toast-container');
        const toast = document.createElement('div');
        toast.className = `toast ${type}`;
        toast.innerHTML = `<span>${icons[type]||'📢'}</span> ${message}`;
        container.appendChild(toast);
        setTimeout(() => {
            toast.style.animation = 'toastOut 0.35s ease both';
            setTimeout(() => toast.remove(), 350);
        }, duration);
    }

    // ── MODAL ──
    function ouvrirModal() {
        const now = new Date();
        const moisNoms = ['Janvier','Février','Mars','Avril','Mai','Juin','Juillet','Août','Septembre','Octobre','Novembre','Décembre'];
        document.getElementById('modal-mois-nom').value = `${moisNoms[now.getMonth()]} ${now.getFullYear()}`;
        document.getElementById('modal-cloture').style.display = 'flex';
        setTimeout(() => document.getElementById('modal-mois-nom').select(), 100);
    }
    function fermerModal() {
        document.getElementById('modal-cloture').style.display = 'none';
    }
    document.getElementById('modal-cloture').addEventListener('click', e => { if (e.target === e.currentTarget) fermerModal(); });
    document.getElementById('modal-mois-nom').addEventListener('keydown', e => {
        if (e.key === 'Enter') confirmerCloture();
        if (e.key === 'Escape') fermerModal();
    });

    function confirmerCloture() {
        const nom = document.getElementById('modal-mois-nom').value.trim() || 'Mois sans nom';
        fermerModal();

        // Calculer les totaux
        let totaux = {};
        db.categories.forEach(c => totaux[c.id] = 0);
        let totalDep = 0, totalEp = 0;
        db.depenses.forEach(d => {
            if (totaux.hasOwnProperty(d.ct)) totaux[d.ct] += d.mt;
            const cat = db.categories.find(c => c.id === d.ct);
            if (cat && cat.label.toLowerCase().includes("épargne")) totalEp += d.mt;
            else totalDep += d.mt;
        });

        // Sauvegarder l'archive
        const archive = {
            id: Date.now(),
            nom,
            date: new Date().toLocaleDateString('fr-FR'),
            revenu: db.revenu,
            totalDep,
            totalEp,
            solde: db.revenu - totalDep - totalEp,
            labels: db.categories.map(c => c.label),
            data: db.categories.map(c => totaux[c.id] || 0)
        };
        archives.push(archive);
        localStorage.setItem('budget_archives', JSON.stringify(archives));

        // Clôture du mois
        let epargneMois = 0;
        db.depenses.forEach(d => {
            const cat = db.categories.find(c => c.id == d.ct);
            if (cat && cat.label.toLowerCase().includes("épargne")) epargneMois += d.mt;
        });
        globalSavings += epargneMois;
        localStorage.setItem('globalSavings', globalSavings);
        db.depenses = db.depenses.filter(d => d.recurring === true);
        sauvegarder();

        afficherArchives();
        afficherEvolution();
        showToast(`"${nom}" archivé ! +${epargneMois}€ au coffre-fort 🎉`, 'success', 4500);
    }

    // ── AFFICHER ARCHIVES ──
    function afficherArchives() {
        const container = document.getElementById('archives-container');
        if (archives.length === 0) {
            container.innerHTML = `
                <div class="empty-archives">
                    <div class="empty-icon">📭</div>
                    <p>Aucun mois archivé pour l'instant.<br>Clôture ton premier mois pour le voir apparaître ici.</p>
                </div>`;
            return;
        }

        container.innerHTML = '<div class="archives-grid" id="archives-grid"></div>';
        const grid = document.getElementById('archives-grid');

        archives.slice().reverse().forEach(arc => {
            const card = document.createElement('div');
            card.className = 'archive-card';
            const soldeColor = arc.solde < 0 ? 'var(--danger)' : 'var(--success)';
            card.innerHTML = `
                <div class="arc-header">
                    <div>
                        <div class="arc-name">${arc.nom}</div>
                        <div class="arc-date">Archivé le ${arc.date}</div>
                    </div>
                </div>
                <div class="arc-chart-wrap">
                    <canvas id="arc-chart-${arc.id}" width="150" height="150"></canvas>
                </div>
                <div class="arc-stats">
                    <div class="arc-stat-row"><span>💸 Dépensé</span><strong>${arc.totalDep} €</strong></div>
                    <div class="arc-stat-row"><span>🌱 Épargné</span><strong style="color:var(--success)">${arc.totalEp} €</strong></div>
                    <div class="arc-stat-row"><span>⚖️ Solde</span><strong style="color:${soldeColor}">${arc.solde} €</strong></div>
                    <div class="arc-stat-row"><span>📥 Revenus</span><strong>${arc.revenu} €</strong></div>
                </div>
                <div class="arc-actions">
                    <button class="btn-pdf" onclick="telechargerPDF(${arc.id})">📄 Télécharger PDF</button>
                    <button class="btn-del-arc" onclick="supprimerArchive(${arc.id})" title="Supprimer">🗑️</button>
                </div>`;
            grid.appendChild(card);

            // Dessiner le camembert
            setTimeout(() => {
                const ctx = document.getElementById(`arc-chart-${arc.id}`);
                if (!ctx) return;
                new Chart(ctx, {
                    type: 'pie',
                    data: {
                        labels: arc.labels,
                        datasets: [{
                            data: arc.data,
                            backgroundColor: colors.slice(0, arc.labels.length),
                            borderWidth: 2,
                            borderColor: '#ffffff',
                            hoverOffset: 5
                        }]
                    },
                    options: {
                        responsive: false,
                        
                        plugins: {
                            legend: { display: false },
                            tooltip: { callbacks: { label: c => ` ${c.label}: ${c.parsed}€` } }
                        },
                        animation: { duration: 800 }
                    }
                });
            }, 60);
        });
    }

    function supprimerArchive(id) {
        if (!confirm("Supprimer cette archive ?")) return;
        archives = archives.filter(a => a.id !== id);
        localStorage.setItem('budget_archives', JSON.stringify(archives));
        afficherArchives();
        afficherEvolution();
        showToast('Archive supprimée', 'warning');
    }

    // ── EXPORT PDF ──
    function telechargerPDF(id) {
        const arc = archives.find(a => a.id === id);
        if (!arc) return;

        const chartCanvas = document.getElementById(`arc-chart-${arc.id}`);
        const chartImg = chartCanvas ? chartCanvas.toDataURL('image/png') : '';
        const soldeColor = arc.solde < 0 ? '#ef4444' : '#22c55e';

        const legendRows = arc.labels.map((l, i) => `
            <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
                <span style="display:inline-block;width:12px;height:12px;border-radius:50%;background:${colors[i % colors.length]};flex-shrink:0;"></span>
                <span style="flex:1;">${l}</span>
                <strong>${arc.data[i]} €</strong>
            </div>`).join('');

        const html = `<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Bilan ${arc.nom}</title>
<style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap');
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Inter', Arial, sans-serif; background: white; color: #1e293b; padding: 48px 40px; max-width: 620px; margin: auto; }
    .header { border-bottom: 3px solid #6366f1; padding-bottom: 18px; margin-bottom: 28px; }
    .title { font-size: 1.7rem; font-weight: 800; color: #6366f1; margin-bottom: 4px; }
    .sub { color: #64748b; font-size: 0.85rem; }
    .chart-section { display: flex; gap: 32px; align-items: center; margin-bottom: 28px; }
    .chart-section img { width: 160px; height: 160px; }
    .legend { flex: 1; font-size: 0.85rem; }
    .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 28px; }
    .stat { background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 12px; padding: 16px; text-align: center; }
    .stat .lbl { font-size: 0.7rem; color: #64748b; text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 6px; }
    .stat .val { font-size: 1.6rem; font-weight: 800; }
    .footer { margin-top: 32px; font-size: 0.72rem; color: #94a3b8; text-align: center; border-top: 1px solid #e2e8f0; padding-top: 16px; }
    @media print { body { padding: 20px; } }
</style>
</head>
<body>
<div class="header">
    <div class="title">📊 ${arc.nom}</div>
    <div class="sub">Archivé le ${arc.date} · Mon Coach Finance</div>
</div>

<div class="chart-section">
    ${chartImg ? `<img src="${chartImg}">` : ''}
    <div class="legend">${legendRows}</div>
</div>

<div class="stats-grid">
    <div class="stat"><div class="lbl">💸 Dépensé</div><div class="val">${arc.totalDep} €</div></div>
    <div class="stat"><div class="lbl">🌱 Épargné</div><div class="val" style="color:#22c55e">${arc.totalEp} €</div></div>
    <div class="stat"><div class="lbl">⚖️ Solde</div><div class="val" style="color:${soldeColor}">${arc.solde} €</div></div>
    <div class="stat"><div class="lbl">📥 Revenus</div><div class="val">${arc.revenu} €</div></div>
</div>

<div class="footer">Généré automatiquement par Mon Coach Finance</div>
</body>
</html>`;

        const blob = new Blob([html], { type: 'text/html' });
        const url = URL.createObjectURL(blob);
        const win = window.open(url, '_blank');
        if (win) win.addEventListener('load', () => setTimeout(() => win.print(), 500));
        showToast('PDF prêt — utilise "Enregistrer en PDF" dans l\'impression', 'info', 4000);
    }

    // ── LOGIQUE PRINCIPALE ──
    function ajouterNouvelleCategorie() {
        const input = document.getElementById('new_cat_name');
        const name = input.value.trim();
        if (name === "") return;
        const newId = "cat_" + Date.now();
        db.categories.push({ id: newId, label: name });
        db.previsions[newId] = 0;
        input.value = "";
        sauvegarder();
        showToast(`Catégorie "${name}" ajoutée`, 'info');
    }

    function supprimerCategorie(id) {
        const cat = db.categories.find(c => c.id === id);
        if (confirm(`Supprimer la catégorie "${cat ? cat.label : ''}" ?`)) {
            db.categories = db.categories.filter(c => c.id !== id);
            delete db.previsions[id];
            sauvegarder();
            showToast('Catégorie supprimée', 'warning');
        }
    }

    function ajouterDepense() {
        const desc = document.getElementById('add_desc').value;
        const mt = parseFloat(document.getElementById('add_mt').value);
        const ctId = document.getElementById('add_cat').value;
        const isRecurring = document.getElementById('add_recurring').checked;
        if (!desc || isNaN(mt) || mt <= 0) { showToast('Remplis tous les champs correctement', 'warning'); return; }
        const expense = { id: Date.now(), date: new Date().toLocaleDateString('fr-FR'), desc, mt, ct: ctId, recurring: isRecurring };
        db.depenses.push(expense);
        const catObj = db.categories.find(c => c.id === ctId);
        if (catObj && catObj.label.toLowerCase().includes("épargne")) db.historiqueEpargne.push(expense);
        document.getElementById('add_desc').value = '';
        document.getElementById('add_mt').value = '';
        document.getElementById('add_recurring').checked = false;
        sauvegarder();
        showToast(`${desc} — ${mt}€ ajouté${isRecurring ? ' (récurrent)' : ''}`, 'success');
    }

    function supprimer(id) {
        const idx = db.historiqueEpargne.findIndex(d => d.id === id);
        if (idx !== -1) {
            globalSavings = Math.max(0, globalSavings - db.historiqueEpargne[idx].mt);
            localStorage.setItem('globalSavings', globalSavings);
            db.historiqueEpargne.splice(idx, 1);
        }
        db.depenses = db.depenses.filter(d => d.id !== id);
        sauvegarder();
        showToast('Dépense supprimée', 'danger');
    }

    function sauvegarder() {
        db.revenu = parseFloat(document.getElementById('prev_revenu').value) || 0;
        document.querySelectorAll('.prev-input').forEach(input => {
            db.previsions[input.dataset.cat] = parseFloat(input.value) || 0;
        });
        localStorage.setItem('budget_vGestion', JSON.stringify(db));
        majAffichage();
    }

    function resetCoffre() {
        if (confirm("ATTENTION : Voulez-vous vraiment vider tout le coffre-fort ET l'historique d'épargne ?")) {
            globalSavings = 0;
            db.historiqueEpargne = [];
            localStorage.setItem('globalSavings', 0);
            sauvegarder();
            showToast('Coffre-fort vidé', 'warning');
        }
    }

    function animateValue(el, from, to, duration = 600) {
        const start = performance.now();
        const update = (now) => {
            const elapsed = now - start;
            const progress = Math.min(elapsed / duration, 1);
            const eased = 1 - Math.pow(1 - progress, 3);
            el.innerText = Math.round(from + (to - from) * eased);
            if (progress < 1) requestAnimationFrame(update);
        };
        requestAnimationFrame(update);
    }

    let prevStats = { dep: 0, ep: 0, solde: 0, coffre: 0 };

    function majAffichage() {
        const searchTerm = document.getElementById('search_input').value.toLowerCase();

        db.historiqueEpargne.sort((a, b) => a.desc.toLowerCase().localeCompare(b.desc.toLowerCase()));
        const epTable = document.querySelector('#epargne_history_table tbody');
        epTable.innerHTML = '';
        let totalEpLocal = 0;
        db.historiqueEpargne.forEach(d => {
            epTable.innerHTML += `<tr>
                <td>${d.date}</td>
                <td>${d.desc} ${d.recurring ? '<span class="recurring-tag">🔄</span>' : ''}</td>
                <td><strong>${d.mt}€</strong></td>
                <td><button class="btn-icon" onclick="supprimer(${d.id})">✕</button></td>
            </tr>`;
            totalEpLocal += d.mt;
        });
        document.getElementById('local_epargne_total').innerText = totalEpLocal.toFixed(0);

        const setupDiv = document.getElementById('setup_categories');
        const selectAdd = document.getElementById('add_cat');
        setupDiv.innerHTML = "";
        selectAdd.innerHTML = "";
        let totalPlanifie = 0;
        db.categories.forEach(cat => {
            const val = db.previsions[cat.id] || 0;
            totalPlanifie += val;
            setupDiv.innerHTML += `
                <div class="budget-row">
                    <button class="btn-danger-soft" onclick="supprimerCategorie('${cat.id}')">✕</button>
                    <span class="cat-label">${cat.label}</span>
                    <input type="number" inputmode="decimal" class="prev-input" data-cat="${cat.id}" value="${val}" onchange="sauvegarder()">
                </div>`;
            selectAdd.innerHTML += `<option value="${cat.id}">${cat.label}</option>`;
        });

        document.getElementById('total_prevu_val').innerText = totalPlanifie.toFixed(0) + " €";
        const reste = db.revenu - totalPlanifie;
        const statusCont = document.getElementById('status_plan_container');
        statusCont.innerHTML = reste < 0
            ? `<span>Dépassement</span> <span class="text-danger">${Math.abs(reste).toFixed(0)} €</span>`
            : `<span>Reste à répartir</span> <span class="text-success">${reste.toFixed(0)} €</span>`;
        document.getElementById('prev_revenu').value = db.revenu;

        let totaux = {};
        db.categories.forEach(c => totaux[c.id] = 0);
        let totalGeneral = 0, totalEpargne = 0;
        let tableBody = document.querySelector('#log_table tbody');
        tableBody.innerHTML = '';

        [...db.depenses].reverse().forEach(d => {
            if (totaux.hasOwnProperty(d.ct)) totaux[d.ct] += d.mt;
            const catObj = db.categories.find(c => c.id === d.ct);
            const catLabel = catObj ? catObj.label.toLowerCase() : '';
            if (d.desc.toLowerCase().includes(searchTerm) || catLabel.includes(searchTerm)) {
                if (catObj && catObj.label.toLowerCase().includes("épargne")) totalEpargne += d.mt;
                else totalGeneral += d.mt;
                tableBody.innerHTML += `<tr class="new-row">
                    <td style="color:var(--text-muted); font-size:0.8rem;">${d.date}</td>
                    <td>${d.desc} ${d.recurring ? '<span class="recurring-tag">🔄</span>' : ''}</td>
                    <td><span style="background:var(--bg2); padding:2px 8px; border-radius:20px; font-size:0.78rem;">${catObj ? catObj.label : 'N/A'}</span></td>
                    <td><strong>${d.mt}€</strong></td>
                    <td><button class="btn-icon" onclick="supprimer(${d.id})">✕</button></td>
                </tr>`;
            }
        });

        const solde = db.revenu - totalGeneral - totalEpargne;
        animateValue(document.getElementById('view_total_dep'), prevStats.dep, totalGeneral, 600);
        animateValue(document.getElementById('view_total_ep'), prevStats.ep, totalEpargne, 600);
        animateValue(document.getElementById('view_solde'), prevStats.solde, solde, 600);
        animateValue(document.getElementById('view_global_ep'), prevStats.coffre, globalSavings, 600);
        prevStats = { dep: totalGeneral, ep: totalEpargne, solde, coffre: globalSavings };
        document.getElementById('view_solde_wrapper').style.color = solde < 0 ? 'var(--danger)' : 'var(--success)';

        const bilanBody = document.querySelector('#bilan_table tbody');
        bilanBody.innerHTML = '';
        let labelsChart = [], dataChart = [];

        db.categories.forEach((cat, i) => {
            const prev = db.previsions[cat.id] || 0;
            const reel = totaux[cat.id] || 0;
            const pct = prev > 0 ? Math.min((reel / prev) * 100, 100) : 0;
            const over = reel > prev;
            const ecart = prev - reel;
            const status = !over ? '<span class="status-badge bg-success">✓ OK</span>' : '<span class="status-badge bg-danger">⚠ DÉPASSÉ</span>';
            const rowBg = over ? 'background: rgba(239,68,68,0.04);' : '';
            bilanBody.innerHTML += `<tr style="${rowBg}">
                <td><span class="bilan-cat-name">${cat.label}</span></td>
                <td style="white-space:nowrap; color:var(--text-muted);">${prev} €</td>
                <td style="font-weight:700; white-space:nowrap;">${reel} €</td>
                <td style="color:${ecart < 0 ? 'var(--danger)' : 'var(--success)'}; font-weight:700; white-space:nowrap">${ecart >= 0 ? '+' : ''}${ecart.toFixed(0)} €</td>
                <td>
                    <div class="progress-wrap">
                        <div class="progress-bar-bg" style="width:90px">
                            <div class="progress-bar-fill ${over ? 'over' : ''}" style="width:${pct}%"></div>
                        </div>
                        <span class="progress-pct">${pct.toFixed(0)}%</span>
                    </div>
                </td>
                <td>${status}</td>
            </tr>`;
            labelsChart.push(cat.label);
            dataChart.push(reel);
        });

        if (chartInstance) chartInstance.destroy();
        const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
        const ctx = document.getElementById('budgetChart').getContext('2d');
        chartInstance = new Chart(ctx, {
            type: 'pie',
            data: {
                labels: labelsChart,
                datasets: [{
                    data: dataChart,
                    backgroundColor: colors.slice(0, labelsChart.length),
                    borderWidth: 3,
                    borderColor: isDark ? '#181a2e' : '#ffffff',
                    hoverBorderWidth: 4,
                    hoverOffset: 8
                }]
            },
            options: {
                responsive: true, maintainAspectRatio: true, aspectRatio: 1, 
                plugins: {
                    legend: {
                        position: 'bottom',
                        labels: { color: isDark ? '#94a3b8' : '#64748b', font: { family: 'Inter', size: 11 }, padding: 12, usePointStyle: true, pointStyleWidth: 8 }
                    },
                    tooltip: { callbacks: { label: ctx => ` ${ctx.label}: ${ctx.parsed}€` } }
                },
                animation: { animateRotate: true, duration: 900 }
            }
        });
    }


    let evolutionChartInstance = null;

    function afficherEvolution() {
        const section = document.getElementById('evolution-section');
        if (archives.length < 1) { section.style.display = 'none'; return; }
        section.style.display = 'block';

        const sorted = [...archives].sort((a, b) => a.id - b.id);
        const labels = sorted.map(a => a.nom);
        const revenus = sorted.map(a => a.revenu);
        const depenses = sorted.map(a => a.totalDep);
        const epargnes = sorted.map(a => a.totalEp);
        const soldes = sorted.map(a => a.solde);

        const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
        const gridColor = isDark ? 'rgba(255,255,255,0.07)' : 'rgba(0,0,0,0.06)';
        const textColor = isDark ? '#94a3b8' : '#64748b';

        if (evolutionChartInstance) evolutionChartInstance.destroy();
        const ctx = document.getElementById('evolutionChart').getContext('2d');
        evolutionChartInstance = new Chart(ctx, {
            type: 'line',
            data: {
                labels,
                datasets: [
                    {
                        label: 'Revenus',
                        data: revenus,
                        borderColor: '#6366f1',
                        backgroundColor: 'rgba(99,102,241,0.08)',
                        borderWidth: 2.5,
                        pointBackgroundColor: '#6366f1',
                        pointRadius: 5,
                        pointHoverRadius: 7,
                        fill: false,
                        tension: 0.35
                    },
                    {
                        label: 'Dépenses',
                        data: depenses,
                        borderColor: '#ef4444',
                        backgroundColor: 'rgba(239,68,68,0.07)',
                        borderWidth: 2.5,
                        pointBackgroundColor: '#ef4444',
                        pointRadius: 5,
                        pointHoverRadius: 7,
                        fill: false,
                        tension: 0.35
                    },
                    {
                        label: 'Épargne',
                        data: epargnes,
                        borderColor: '#22c55e',
                        backgroundColor: 'rgba(34,197,94,0.07)',
                        borderWidth: 2.5,
                        pointBackgroundColor: '#22c55e',
                        pointRadius: 5,
                        pointHoverRadius: 7,
                        fill: false,
                        tension: 0.35
                    },
                    {
                        label: 'Solde',
                        data: soldes,
                        borderColor: '#f59e0b',
                        backgroundColor: 'rgba(245,158,11,0.07)',
                        borderWidth: 2,
                        borderDash: [6, 3],
                        pointBackgroundColor: '#f59e0b',
                        pointRadius: 4,
                        pointHoverRadius: 6,
                        fill: false,
                        tension: 0.35
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                interaction: { mode: 'index', intersect: false },
                plugins: {
                    legend: {
                        position: 'top',
                        labels: {
                            color: textColor,
                            font: { family: 'Inter', size: 11 },
                            padding: 16,
                            usePointStyle: true,
                            pointStyleWidth: 8
                        }
                    },
                    tooltip: {
                        backgroundColor: isDark ? '#1e2235' : '#ffffff',
                        titleColor: isDark ? '#e2e8f0' : '#1e293b',
                        bodyColor: isDark ? '#94a3b8' : '#64748b',
                        borderColor: isDark ? 'rgba(99,102,241,0.3)' : '#e2e8f0',
                        borderWidth: 1,
                        padding: 12,
                        callbacks: {
                            label: ctx => ` ${ctx.dataset.label} : ${ctx.parsed.y} €`
                        }
                    }
                },
                scales: {
                    x: {
                        ticks: { color: textColor, font: { family: 'Inter', size: 11 } },
                        grid: { color: gridColor }
                    },
                    y: {
                        ticks: {
                            color: textColor,
                            font: { family: 'Inter', size: 11 },
                            callback: v => v + ' €'
                        },
                        grid: { color: gridColor }
                    }
                },
                animation: { duration: 900 }
            }
        });
    }

    majAffichage();
    afficherArchives();
    afficherEvolution();
</script>
</body>
</html>
