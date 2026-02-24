<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Mon Coach Finance - Planning & Réel</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root { --main: #6366f1; --bg: #f1f5f9; --card: #ffffff; --text: #1e293b; --danger: #ef4444; --success: #22c55e; --warning: #f59e0b; }
        body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--text); padding: 20px; line-height: 1.5; }
        .container { max-width: 1200px; margin: auto; }
        
        .grid-top { display: grid; grid-template-columns: 1fr 2fr; gap: 20px; margin-bottom: 20px; }
        .card { background: var(--card); padding: 20px; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); }
        
        h2 { margin-top: 0; font-size: 1.2rem; border-bottom: 2px solid var(--bg); padding-bottom: 10px; }
        input, select, button { width: 100%; padding: 10px; margin: 5px 0 15px 0; border-radius: 8px; border: 1px solid #e2e8f0; box-sizing: border-box; }
        button { background: var(--main); color: white; border: none; font-weight: bold; cursor: pointer; }
        
        .budget-row { display: flex; gap: 10px; align-items: center; margin-bottom: 5px; }
        .budget-row label { flex: 1; font-size: 0.9rem; }
        .budget-row input { width: 80px; margin: 0; }

        .stat-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin-bottom: 20px; }
        .stat-box { background: var(--card); padding: 15px; border-radius: 12px; text-align: center; font-weight: bold; }
        
        .status-badge { padding: 3px 8px; border-radius: 12px; font-size: 0.75rem; font-weight: bold; color: white; }
        .bg-success { background: var(--success); }
        .bg-danger { background: var(--danger); }

        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th { text-align: left; font-size: 0.8rem; color: #64748b; padding: 10px; }
        td { padding: 10px; border-top: 1px solid #f1f5f9; font-size: 0.9rem; }
        
        .progress-bar-bg { background: #e2e8f0; height: 8px; border-radius: 4px; overflow: hidden; margin-top: 5px; }
        .progress-bar-fill { height: 100%; background: var(--main); transition: 0.3s; }
    </style>
</head>
<body>

<div class="container">
    <header style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
        <h1>📊 Mon Budget : Prévu vs Réel</h1>
        <button onclick="resetMois()" style="width: auto; background: #94a3b8;">Nouveau Mois</button>
    </header>

    <div class="grid-top">
        <div class="card">
            <h2>1. Je prévois mon mois</h2>
            <label>Salaire / Revenus (€)</label>
            <input type="number" id="prev_revenu" placeholder="Ex: 1800" onchange="sauvegarder()">
            
            <p style="font-size: 0.8rem; color: #64748b; margin-bottom: 10px;">Fixe tes limites par catégorie :</p>
            <div id="setup_categories">
                </div>
            <button onclick="sauvegarder()">Enregistrer mon plan</button>
        </div>

        <div class="card">
            <h2>2. J'ajoute mes dépenses</h2>
            <div style="display: grid; grid-template-columns: 1fr 1fr 1fr auto; gap: 10px;">
                <input type="text" id="add_desc" placeholder="Objet">
                <input type="number" id="add_mt" placeholder="Montant">
                <select id="add_cat"></select>
                <button onclick="ajouterDepense()" style="margin: 5px 0;">+</button>
            </div>
            
            <div class="stat-grid">
                <div class="stat-box">Dépensé : <span id="view_total_dep">0</span> €</div>
                <div class="stat-box">Épargné : <span id="view_total_ep">0</span> €</div>
                <div class="stat-box" id="view_solde_box">Solde : <span id="view_solde">0</span> €</div>
            </div>

            <h2>Historique</h2>
            <div style="max-height: 250px; overflow-y: auto;">
                <table id="log_table">
                    <thead><tr><th>Date</th><th>Nom</th><th>Cat.</th><th>Prix</th><th>Action</th></tr></thead>
                    <tbody></tbody>
                </table>
            </div>
        </div>
    </div>

    <div class="card">
        <h2>3. Bilan : Ai-je respecté mes engagements ?</h2>
        <table id="bilan_table">
            <thead>
                <tr>
                    <th>Catégorie</th>
                    <th>Prévu (Budget)</th>
                    <th>Réel (Dépensé)</th>
                    <th>Écart</th>
                    <th>Progression</th>
                    <th>Statut</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>
</div>

<script>
    const CATS = {
        "Fixe": "🏠 Loyers/Charges",
        "Courses": "🛒 Alimentation",
        "Loisirs": "🎉 Sorties/Plaisirs",
        "Epargne": "💰 Épargne",
        "Autres": "✨ Divers"
    };

    let db = JSON.parse(localStorage.getItem('budget_v3')) || {
        revenu: 0,
        previsions: { Fixe:0, Courses:0, Loisirs:0, Epargne:0, Autres:0 },
        depenses: []
    };

    // Initialisation de l'interface de planification
    const setupDiv = document.getElementById('setup_categories');
    const selectAdd = document.getElementById('add_cat');
    Object.keys(CATS).forEach(key => {
        setupDiv.innerHTML += `
            <div class="budget-row">
                <label>${CATS[key]}</label>
                <input type="number" class="prev-input" data-cat="${key}" value="${db.previsions[key] || 0}" onchange="sauvegarder()">
            </div>`;
        selectAdd.innerHTML += `<option value="${key}">${CATS[key]}</option>`;
    });

    function ajouterDepense() {
        const desc = document.getElementById('add_desc').value;
        const mt = parseFloat(document.getElementById('add_mt').value);
        const ct = document.getElementById('add_cat').value;
        if(!desc || isNaN(mt)) return;
        
        db.depenses.push({ id: Date.now(), date: new Date().toLocaleDateString('fr-FR'), desc, mt, ct });
        document.getElementById('add_desc').value = '';
        document.getElementById('add_mt').value = '';
        sauvegarder();
    }

    function supprimer(id) {
        db.depenses = db.depenses.filter(d => d.id !== id);
        sauvegarder();
    }

    function sauvegarder() {
        db.revenu = parseFloat(document.getElementById('prev_revenu').value) || 0;
        document.querySelectorAll('.prev-input').forEach(input => {
            db.previsions[input.dataset.cat] = parseFloat(input.value) || 0;
        });
        localStorage.setItem('budget_v3', JSON.stringify(db));
        majAffichage();
    }

    function majAffichage() {
        document.getElementById('prev_revenu').value = db.revenu;
        
        // Calculs
        let totaux = { Fixe:0, Courses:0, Loisirs:0, Epargne:0, Autres:0 };
        let totalGeneral = 0;
        let tableBody = document.querySelector('#log_table tbody');
        tableBody.innerHTML = '';

        db.depenses.reverse().forEach(d => {
            totaux[d.ct] += d.mt;
            if(d.ct !== 'Epargne') totalGeneral += d.mt;
            tableBody.innerHTML += `<tr><td>${d.date}</td><td>${d.desc}</td><td>${d.ct}</td><td>${d.mt}€</td><td><button onclick="supprimer(${d.id})" style="background:none; color:red; padding:0; margin:0; width:auto;">✕</button></td></tr>`;
        });
        db.depenses.reverse(); // remettre à l'endroit pour le calcul

        // Dashboard
        document.getElementById('view_total_dep').innerText = totalGeneral.toFixed(0);
        document.getElementById('view_total_ep').innerText = totaux.Epargne.toFixed(0);
        const solde = db.revenu - totalGeneral - totaux.Epargne;
        document.getElementById('view_solde').innerText = solde.toFixed(0);
        document.getElementById('view_solde_box').style.color = solde < 0 ? 'var(--danger)' : 'var(--success)';

        // Table Bilan
        const bilanBody = document.querySelector('#bilan_table tbody');
        bilanBody.innerHTML = '';
        Object.keys(CATS).forEach(cat => {
            const prev = db.previsions[cat] || 0;
            const reel = totaux[cat];
            const ecart = prev - reel;
            const pct = prev > 0 ? Math.min((reel/prev)*100, 100) : 0;
            const status = reel <= prev ? '<span class="status-badge bg-success">OK</span>' : '<span class="status-badge bg-danger">DÉPASSÉ</span>';
            
            bilanBody.innerHTML += `
                <tr>
                    <td>${CATS[cat]}</td>
                    <td>${prev} €</td>
                    <td>${reel} €</td>
                    <td style="color:${ecart < 0 ? 'red' : 'green'}">${ecart > 0 ? '+' : ''}${ecart.toFixed(0)} €</td>
                    <td>
                        <div class="progress-bar-bg"><div class="progress-bar-fill" style="width:${pct}%; background:${reel > prev ? 'var(--danger)' : 'var(--main)'}"></div></div>
                    </td>
                    <td>${status}</td>
                </tr>`;
        });
    }

    function resetMois() {
        if(confirm("Effacer les dépenses ? Tes objectifs seront conservés.")) {
            db.depenses = [];
            sauvegarder();
        }
    }

    majAffichage();
</script>
</body>
</html>
