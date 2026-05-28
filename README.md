# impianto_anti_icing_e_de_icing-<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gestione del Ghiaccio negli Aeromobili</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,600;1,300&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0e1a;
    --bg2: #0f1528;
    --panel: #141a2e;
    --border: #1e2a4a;
    --accent: #00b4d8;
    --accent2: #f77f00;
    --accent3: #ef233c;
    --text: #c8d6f0;
    --text-dim: #5a6a8a;
    --white: #e8f0ff;
    --card1: #0d2137;
    --card2: #1a1a0d;
    --card3: #0d1a1a;
    --card4: #1a0d1a;
    --card5: #1a1010;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* BACKGROUND GRID */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,180,216,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,180,216,0.04) 1px, transparent 1px);
    background-size: 60px 60px;
    pointer-events: none;
    z-index: 0;
  }

  /* HEADER */
  header {
    position: relative;
    padding: 60px 24px 40px;
    text-align: center;
    overflow: hidden;
    z-index: 1;
  }

  header::after {
    content: '';
    position: absolute;
    bottom: 0; left: 50%;
    transform: translateX(-50%);
    width: 600px; height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
  }

  .header-glow {
    position: absolute;
    top: -60px; left: 50%;
    transform: translateX(-50%);
    width: 500px; height: 300px;
    background: radial-gradient(ellipse, rgba(0,180,216,0.15) 0%, transparent 70%);
    pointer-events: none;
  }

  .badge {
    display: inline-block;
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.15em;
    color: var(--accent);
    border: 1px solid var(--accent);
    padding: 4px 14px;
    border-radius: 2px;
    margin-bottom: 20px;
    opacity: 0;
    animation: fadeUp 0.6s ease forwards 0.1s;
  }

  h1 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(2.8rem, 7vw, 5.5rem);
    letter-spacing: 0.03em;
    line-height: 0.95;
    color: var(--white);
    opacity: 0;
    animation: fadeUp 0.7s ease forwards 0.2s;
  }

  h1 span { color: var(--accent); }

  .subtitle {
    margin-top: 16px;
    font-size: 0.95rem;
    color: var(--text-dim);
    font-weight: 300;
    opacity: 0;
    animation: fadeUp 0.7s ease forwards 0.35s;
  }

  .author-strip {
    margin-top: 28px;
    display: inline-flex;
    align-items: center;
    gap: 12px;
    background: rgba(0,180,216,0.07);
    border: 1px solid rgba(0,180,216,0.2);
    padding: 8px 20px;
    border-radius: 2px;
    font-family: 'Space Mono', monospace;
    font-size: 0.72rem;
    color: var(--accent);
    opacity: 0;
    animation: fadeUp 0.7s ease forwards 0.5s;
  }

  .author-strip::before {
    content: '✦';
    font-size: 0.6rem;
  }

  /* INTRO STATS */
  .stats-row {
    display: flex;
    justify-content: center;
    gap: 8px;
    flex-wrap: wrap;
    padding: 32px 24px 0;
    position: relative; z-index: 1;
    opacity: 0;
    animation: fadeUp 0.7s ease forwards 0.65s;
  }

  .stat-pill {
    background: var(--panel);
    border: 1px solid var(--border);
    padding: 10px 20px;
    border-radius: 2px;
    text-align: center;
  }

  .stat-pill .num {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.8rem;
    color: var(--accent2);
    display: block;
  }

  .stat-pill .lab {
    font-size: 0.68rem;
    color: var(--text-dim);
    font-family: 'Space Mono', monospace;
    letter-spacing: 0.08em;
  }

  /* SECTION LABEL */
  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.2em;
    color: var(--text-dim);
    text-transform: uppercase;
    padding: 48px 24px 20px;
    position: relative; z-index: 1;
    max-width: 1200px;
    margin: 0 auto;
  }

  .section-label::before {
    content: '//';
    margin-right: 8px;
    color: var(--accent);
  }

  /* GRID */
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
    padding: 0 24px 80px;
    max-width: 1200px;
    margin: 0 auto;
    position: relative; z-index: 1;
  }

  /* CARDS */
  .card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 28px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    transition: transform 0.25s ease, border-color 0.25s ease, box-shadow 0.25s ease;
    opacity: 0;
    transform: translateY(30px);
  }

  .card.visible {
    opacity: 1;
    transform: translateY(0);
    transition: opacity 0.5s ease, transform 0.5s ease, border-color 0.25s ease, box-shadow 0.25s ease;
  }

  .card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, var(--card-accent, rgba(0,180,216,0.05)), transparent 60%);
    pointer-events: none;
  }

  .card:hover {
    transform: translateY(-4px);
    border-color: var(--card-color, var(--accent));
    box-shadow: 0 12px 40px rgba(0,0,0,0.4), 0 0 0 1px var(--card-color, var(--accent)) inset;
  }

  .card-icon {
    font-size: 2.2rem;
    margin-bottom: 16px;
    display: block;
  }

  .card-num {
    position: absolute;
    top: 20px; right: 20px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 3rem;
    color: rgba(255,255,255,0.04);
    line-height: 1;
  }

  .card h2 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.5rem;
    letter-spacing: 0.04em;
    color: var(--white);
    margin-bottom: 10px;
  }

  .card p {
    font-size: 0.85rem;
    color: var(--text-dim);
    line-height: 1.65;
    font-weight: 300;
  }

  .card-tag {
    display: inline-block;
    margin-top: 16px;
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem;
    letter-spacing: 0.12em;
    padding: 3px 10px;
    border-radius: 2px;
    background: rgba(0,180,216,0.1);
    color: var(--accent);
    border: 1px solid rgba(0,180,216,0.3);
  }

  .card-arrow {
    position: absolute;
    bottom: 20px; right: 20px;
    width: 28px; height: 28px;
    border: 1px solid var(--border);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 0.75rem;
    color: var(--text-dim);
    transition: all 0.2s;
  }

  .card:hover .card-arrow {
    border-color: var(--card-color, var(--accent));
    color: var(--card-color, var(--accent));
    transform: translate(2px, -2px);
  }

  /* MODAL OVERLAY */
  .overlay {
    position: fixed;
    inset: 0;
    background: rgba(5,8,18,0.92);
    backdrop-filter: blur(12px);
    z-index: 100;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding: 24px;
    overflow-y: auto;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
  }

  .overlay.open {
    opacity: 1;
    pointer-events: all;
  }

  .modal {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 6px;
    max-width: 860px;
    width: 100%;
    position: relative;
    margin: auto;
    transform: translateY(30px) scale(0.97);
    transition: transform 0.35s cubic-bezier(0.16,1,0.3,1);
    overflow: hidden;
  }

  .overlay.open .modal {
    transform: translateY(0) scale(1);
  }

  .modal-header {
    padding: 32px 36px 24px;
    border-bottom: 1px solid var(--border);
    position: relative;
  }

  .modal-header::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 3px;
    background: var(--modal-color, var(--accent));
  }

  .modal-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem;
    letter-spacing: 0.2em;
    color: var(--modal-color, var(--accent));
    margin-bottom: 10px;
  }

  .modal h2 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 2.2rem;
    color: var(--white);
    letter-spacing: 0.04em;
  }

  .modal-body {
    padding: 32px 36px;
    line-height: 1.75;
    color: var(--text);
  }

  .modal-body h3 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.25rem;
    color: var(--modal-color, var(--accent));
    letter-spacing: 0.06em;
    margin: 28px 0 10px;
  }

  .modal-body h3:first-child { margin-top: 0; }

  .modal-body p {
    font-size: 0.9rem;
    margin-bottom: 12px;
    font-weight: 300;
  }

  .modal-body ul {
    padding-left: 18px;
    margin-bottom: 12px;
  }

  .modal-body li {
    font-size: 0.88rem;
    font-weight: 300;
    margin-bottom: 6px;
    color: var(--text);
  }

  .modal-body li::marker { color: var(--modal-color, var(--accent)); }

  .formula-box {
    background: var(--bg);
    border: 1px solid var(--border);
    border-left: 3px solid var(--modal-color, var(--accent));
    padding: 16px 20px;
    border-radius: 0 4px 4px 0;
    margin: 16px 0;
    font-family: 'Space Mono', monospace;
    font-size: 0.85rem;
    color: var(--white);
    overflow-x: auto;
  }

  .formula-box .formula-label {
    font-size: 0.6rem;
    color: var(--text-dim);
    letter-spacing: 0.15em;
    margin-bottom: 8px;
    text-transform: uppercase;
  }

  .example-box {
    background: rgba(247,127,0,0.07);
    border: 1px solid rgba(247,127,0,0.25);
    border-radius: 4px;
    padding: 20px;
    margin: 20px 0;
  }

  .example-box .ex-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem;
    letter-spacing: 0.15em;
    color: var(--accent2);
    margin-bottom: 10px;
  }

  .example-box p, .example-box li {
    font-size: 0.85rem;
    color: var(--text);
  }

  .table-wrap { overflow-x: auto; margin: 16px 0; }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.82rem;
  }

  th {
    background: rgba(0,180,216,0.12);
    color: var(--accent);
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.1em;
    padding: 10px 14px;
    text-align: left;
    border-bottom: 1px solid var(--border);
  }

  td {
    padding: 9px 14px;
    border-bottom: 1px solid rgba(30,42,74,0.5);
    color: var(--text);
    font-weight: 300;
  }

  tr:hover td { background: rgba(0,180,216,0.04); }

  .close-btn {
    position: absolute;
    top: 20px; right: 20px;
    width: 36px; height: 36px;
    border: 1px solid var(--border);
    background: transparent;
    border-radius: 50%;
    cursor: pointer;
    color: var(--text-dim);
    font-size: 1rem;
    display: flex; align-items: center; justify-content: center;
    transition: all 0.2s;
  }

  .close-btn:hover {
    border-color: var(--accent3);
    color: var(--accent3);
    transform: rotate(90deg);
  }

  /* SVG diagrams */
  .svg-container {
    margin: 20px 0;
    text-align: center;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 16px;
    overflow-x: auto;
  }

  .svg-container svg {
    max-width: 100%;
    height: auto;
  }

  /* COMPARISON TABLE COLORS */
  .c-green { color: #4caf50; }
  .c-red { color: var(--accent3); }
  .c-yellow { color: var(--accent2); }

  /* Highlight */
  .hl {
    color: var(--modal-color, var(--accent));
    font-weight: 600;
  }

  /* Footer */
  footer {
    position: relative; z-index: 1;
    text-align: center;
    padding: 32px 24px;
    border-top: 1px solid var(--border);
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--text-dim);
    letter-spacing: 0.08em;
  }

  /* ANIMATIONS */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* scrollbar */
  ::-webkit-scrollbar { width: 6px; height: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--accent); }

  /* responsive */
  @media (max-width: 600px) {
    .grid { grid-template-columns: 1fr; padding: 0 16px 60px; }
    .modal-header, .modal-body { padding: 24px 20px; }
    .modal h2 { font-size: 1.7rem; }
    h1 { font-size: 2.4rem; }
    .stats-row { gap: 6px; }
    .stat-pill { padding: 8px 12px; }
    .stat-pill .num { font-size: 1.4rem; }
  }
</style>
</head>
<body>

<header>
  <div class="header-glow"></div>
  <div class="badge">ATA 30 — ICE & RAIN PROTECTION</div>
  <h1>SICUREZZA IN VOLO<br><span>GHIACCIO</span> NEGLI AEROMOBILI</h1>
  <p class="subtitle">Formazione, Effetti e Sistemi di Mitigazione — Impianto Antighiaccio</p>
  <div class="author-strip">GABRIELE EVANGELISTA &nbsp;|&nbsp; CLASSE 3ACA &nbsp;|&nbsp; A.S. 2025/26</div>
</header>

<div class="stats-row">
  <div class="stat-pill"><span class="num">−40°C</span><span class="lab">Temp. min formazione</span></div>
  <div class="stat-pill"><span class="num">40%</span><span class="lab">Riduzione CL<sub>max</sub> (glaze)</span></div>
  <div class="stat-pill"><span class="num">450°C</span><span class="lab">Bleed-air dal compressore</span></div>
  <div class="stat-pill"><span class="num">5×</span><span class="lab">Aumento resistenza aerodin.</span></div>
  <div class="stat-pill"><span class="num">62–63°C</span><span class="lab">Temperatura fluido de-icing</span></div>
</div>

<div class="section-label">SEZIONI INTERATTIVE — CLICCA PER APPROFONDIRE</div>

<div class="grid" id="cardGrid">

  <div class="card" data-id="0" style="--card-color:#00b4d8; --card-accent:rgba(0,180,216,0.08)">
    <span class="card-num">01</span>
    <span class="card-icon">❄️</span>
    <h2>Formazione del Ghiaccio</h2>
    <p>Condizioni meteorologiche, gocce sopraffuse, LWC e tipologie di ghiaccio sulle superfici aerodinamiche.</p>
    <span class="card-tag">METEOROLOGIA</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="1" style="--card-color:#f77f00; --card-accent:rgba(247,127,0,0.08)">
    <span class="card-num">02</span>
    <span class="card-icon">📐</span>
    <h2>Accrescimento e Calcolo</h2>
    <p>Coefficiente di cattura, area bagnata, formula di massa accumulata e parametri d'inerzia.</p>
    <span class="card-tag">AERODINAMICA</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="2" style="--card-color:#ef233c; --card-accent:rgba(239,35,60,0.08)">
    <span class="card-num">03</span>
    <span class="card-icon">⚠️</span>
    <h2>Rischi per il Volo</h2>
    <p>Effetti del ghiaccio su profilo alare, strumenti, superfici mobili, motori e manovrabilità.</p>
    <span class="card-tag">SICUREZZA</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="3" style="--card-color:#4cc9f0; --card-accent:rgba(76,201,240,0.08)">
    <span class="card-num">04</span>
    <span class="card-icon">🧪</span>
    <h2>Sistema Chimico</h2>
    <p>De-icing e anti-icing a terra: fluidi glicolici, holdover time, procedura one-step e two-step.</p>
    <span class="card-tag">DE-ICING A TERRA</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="4" style="--card-color:#80b918; --card-accent:rgba(128,185,24,0.08)">
    <span class="card-num">05</span>
    <span class="card-icon">💨</span>
    <h2>Sistema Pneumatico</h2>
    <p>Boots pneumatici de-icing: gonfiaggio ciclico, sistema Goodrich, bridging effect e limiti.</p>
    <span class="card-tag">MECCANICO</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="5" style="--card-color:#e040fb; --card-accent:rgba(224,64,251,0.08)">
    <span class="card-num">06</span>
    <span class="card-icon">🔥</span>
    <h2>Sistema Bleed-Air</h2>
    <p>Aria calda dal compressore turbofan CFM56-7, piccolo tube, applicazione su L.E. e slats.</p>
    <span class="card-tag">TERMICO — ANTI-ICE</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="6" style="--card-color:#f4a261; --card-accent:rgba(244,162,97,0.08)">
    <span class="card-num">07</span>
    <span class="card-icon">⚡</span>
    <h2>Sistema Elettrico</h2>
    <p>Riscaldatori a superficie e lineari: sonde Pitot, parabrezza, sensori AOA e rilevatori di ghiaccio.</p>
    <span class="card-tag">ELETTRICO</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="7" style="--card-color:#06d6a0; --card-accent:rgba(6,214,160,0.08)">
    <span class="card-num">08</span>
    <span class="card-icon">🎛️</span>
    <h2>Controlli in Cockpit</h2>
    <p>Valvole WAI, indicatori, modalità AUTO/ON/OFF, condizioni di inibizione e sistema A320.</p>
    <span class="card-tag">AVIONICA</span>
    <div class="card-arrow">→</div>
  </div>

  <div class="card" data-id="8" style="--card-color:#ff6b6b; --card-accent:rgba(255,107,107,0.08)">
    <span class="card-num">09</span>
    <span class="card-icon">⚖️</span>
    <h2>Confronto Sistemi</h2>
    <p>Tabella comparativa tra chimico, pneumatico, termico bleed-air ed elettrico: pro, contro, applicazioni.</p>
    <span class="card-tag">ANALISI COMPARATIVA</span>
    <div class="card-arrow">→</div>
  </div>

</div>

<!-- MODAL -->
<div class="overlay" id="overlay" role="dialog" aria-modal="true">
  <div class="modal" id="modal">
    <div class="modal-header">
      <div class="modal-label" id="modalLabel"></div>
      <h2 id="modalTitle"></h2>
      <button class="close-btn" id="closeBtn" aria-label="Chiudi">✕</button>
    </div>
    <div class="modal-body" id="modalBody"></div>
  </div>
</div>

<footer>
  GABRIELE EVANGELISTA — CLASSE 3ACA — A.S. 2025/26 &nbsp;|&nbsp; IMPIANTO ANTIGHIACCIO AEROMOBILI &nbsp;|&nbsp; ATA 30
</footer>

<script>
const MODAL_COLOR = ['#00b4d8','#f77f00','#ef233c','#4cc9f0','#80b918','#e040fb','#f4a261','#06d6a0','#ff6b6b'];

const CONTENT = [

/* 0 - Formazione */
{
  label: '01 // FATTORE AMBIENTALE',
  title: 'Formazione del Ghiaccio',
  body: `
<h3>Condizioni Necessarie</h3>
<p>Il ghiaccio si forma sulle superfici esterne di un aeromobile quando coesistono tre condizioni: <span class="hl">nuvole o aria umida</span>, <span class="hl">temperature comprese tra −40 °C e +5 °C</span> e la presenza di superfici con temperatura inferiore allo zero.</p>

<h3>Gocce d'Acqua Sopraffuse (SLD)</h3>
<p>Il pericolo principale non sono le particelle di ghiaccio solido (già congelate e quindi inerti), bensì le <span class="hl">gocce d'acqua sopraffuse</span>: rimangono allo stato liquido anche sotto 0 °C perché mancano di nuclei di condensazione. All'impatto con il profilo alare, che funge da nucleo, solidificano istantaneamente.</p>
<ul>
  <li><strong>Droplets:</strong> D &lt; 100 µm (media 10 µm) — tra −15 °C e 0 °C</li>
  <li><strong>Raindrops:</strong> D &gt; 100 µm (media 1 mm) — tra −30 °C e 0 °C</li>
  <li><strong>Supercooled Large Droplets (SLD):</strong> dimensioni molto grandi, sfuggono ai sistemi di protezione standard (nessuna misura preventiva efficace)</li>
</ul>

<h3>LWC — Liquid Water Content</h3>
<p>Il parametro <span class="hl">LWC (g/m³)</span> quantifica la massa d'acqua liquida per unità di volume d'aria. Valori elevati di LWC accelerano l'accrescimento del ghiaccio e determinano la tipologia di deposito.</p>

<div class="svg-container">
<svg viewBox="0 0 560 220" xmlns="http://www.w3.org/2000/svg" style="font-family:'DM Sans',sans-serif">
  <rect width="560" height="220" fill="#0a0e1a"/>
  <!-- Axis -->
  <line x1="60" y1="180" x2="510" y2="180" stroke="#1e2a4a" stroke-width="1.5"/>
  <line x1="60" y1="20" x2="60" y2="180" stroke="#1e2a4a" stroke-width="1.5"/>
  <text x="285" y="210" fill="#5a6a8a" font-size="10" text-anchor="middle">Temperatura (°C)</text>
  <text x="20" y="100" fill="#5a6a8a" font-size="10" transform="rotate(-90,20,100)">% nuvole senza ghiaccio</text>
  <!-- temp labels -->
  <text x="60" y="196" fill="#5a6a8a" font-size="9" text-anchor="middle">−40</text>
  <text x="172" y="196" fill="#5a6a8a" font-size="9" text-anchor="middle">−30</text>
  <text x="285" y="196" fill="#5a6a8a" font-size="9" text-anchor="middle">−20</text>
  <text x="397" y="196" fill="#5a6a8a" font-size="9" text-anchor="middle">−10</text>
  <text x="510" y="196" fill="#5a6a8a" font-size="9" text-anchor="middle">0</text>
  <!-- y labels -->
  <text x="52" y="183" fill="#5a6a8a" font-size="9" text-anchor="end">0</text>
  <text x="52" y="148" fill="#5a6a8a" font-size="9" text-anchor="end">25</text>
  <text x="52" y="113" fill="#5a6a8a" font-size="9" text-anchor="end">50</text>
  <text x="52" y="78" fill="#5a6a8a" font-size="9" text-anchor="end">75</text>
  <text x="52" y="43" fill="#5a6a8a" font-size="9" text-anchor="end">100</text>
  <!-- Curve: SLD distribution % -->
  <polyline points="60,175 172,172 285,165 350,148 420,118 470,75 510,28" fill="none" stroke="#00b4d8" stroke-width="2.5" stroke-linejoin="round"/>
  <!-- Area fill -->
  <polygon points="60,175 172,172 285,165 350,148 420,118 470,75 510,28 510,180 60,180" fill="url(#areaGrad)" opacity="0.25"/>
  <defs>
    <linearGradient id="areaGrad" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#00b4d8"/>
      <stop offset="100%" stop-color="transparent"/>
    </linearGradient>
  </defs>
  <text x="380" y="50" fill="#00b4d8" font-size="10">Gocce sopraffuse</text>
  <text x="380" y="63" fill="#5a6a8a" font-size="9">presenti</text>
</svg>
</div>

<h3>Tipologie di Deposito</h3>
<div class="table-wrap">
<table>
  <tr><th>TIPO</th><th>NOME INGLESE</th><th>ASPETTO</th><th>PERICOLOSITÀ</th></tr>
  <tr><td>Granuloso/Opaco</td><td>Rime ice</td><td>Bianco opaco, poroso</td><td class="c-yellow">Media</td></tr>
  <tr><td>Vetrone/Trasparente</td><td>Glaze ice</td><td>Trasparente, liscio</td><td class="c-red">Molto alta</td></tr>
  <tr><td>Misto</td><td>Mixed ice</td><td>Combinazione</td><td class="c-yellow">Alta</td></tr>
  <tr><td>Brinoso</td><td>Hoar frost</td><td>Cristallini bianchi</td><td class="c-green">Bassa</td></tr>
  <tr><td>A gradino</td><td>Ice step</td><td>Scalino al bordo</td><td class="c-yellow">Media</td></tr>
</table>
</div>

<div class="example-box">
  <div class="ex-label">✦ ESEMPIO PRATICO</div>
  <p>Un Boeing 737 in avvicinamento vola a −8 °C attraverso strati nuvolosi con LWC = 0,3 g/m³. Le gocce sopraffuse di circa 20 µm colpiscono il bordo d'attacco e solidificano in forma di <strong>glaze ice</strong>, creando corna laterali che modificano il profilo NACA e riducono la portanza fino al 40%. Il sistema bleed-air deve essere attivato prima di entrare in zona IMC.</p>
</div>
`},

/* 1 - Accrescimento e calcolo */
{
  label: '02 // AERODINAMICA',
  title: 'Accrescimento e Calcolo',
  body: `
<h3>Estensione dell'Area Bagnata</h3>
<p>Quando le gocce d'acqua sopraffuse investono il profilo alare, la zona colpita dipende dalla dimensione delle gocce rispetto al campo aerodinamico:</p>
<ul>
  <li><strong>Piccole gocce:</strong> seguono le linee di flusso → impatto vicino al <span class="hl">punto di ristagno</span></li>
  <li><strong>Grandi gocce (SLD):</strong> seguono traiettorie rettilinee per inerzia → impatto su zone più arretrate</li>
</ul>

<h3>Coefficiente di Cattura E</h3>
<p>L'<span class="hl">efficienza di cattura E</span> è il rapporto tra la portata massica d'acqua che impatta sul profilo e quella che attraversa una sezione di riferimento di dimensioni maggiori.</p>

<div class="formula-box">
  <div class="formula-label">PORTATA ACQUA SUL PROFILO [kg/(s·m)]</div>
  ṁ = LWC · E · U∞ · h
  <br><br>
  dove:<br>
  LWC = contenuto d'acqua liquida [kg/m³]<br>
  E   = coefficiente di cattura [adimensionale, 0–1]<br>
  U∞  = velocità di volo [m/s]<br>
  h   = corda del profilo [m]
</div>

<h3>Formula di Massa Accumulata</h3>
<div class="formula-box">
  <div class="formula-label">MASSA DI GHIACCIO ACCUMULATA PER UNITÀ DI TEMPO</div>
  M = Γ · A_f · v · δ
  <br><br>
  dove:<br>
  Γ   = coefficiente globale di accumulo (da grafici NACA)<br>
  A_f = area frontale della sezione esposta [m²]<br>
  v   = velocità relativa [m/s]<br>
  δ   = contenuto acqua nell'aria [kg/m³]
</div>

<h3>Parametri di Langmuir</h3>
<div class="formula-box">
  <div class="formula-label">PARAMETRO D'INERZIA K</div>
  K = (ρ_d · U∞ · D²) / (18 · μ_a · C)
  <br><br>
  <div class="formula-label">PARAMETRO DELLE CURVE Φ</div>
  Φ = 18 · (ρ_e / ρ_d) · (U∞ · C · ρ_a) / μ_a
  <br><br>
  dove:<br>
  ρ_d = densità della goccia [kg/m³]<br>
  ρ_e = densità equivalente [kg/m³]<br>
  ρ_a = densità dell'aria [kg/m³]<br>
  μ_a = viscosità dinamica aria [Pa·s]<br>
  D   = diametro medio goccia [m]<br>
  C   = corda del profilo [m]
</div>

<div class="svg-container">
<svg viewBox="0 0 560 200" xmlns="http://www.w3.org/2000/svg" style="font-family:'DM Sans',sans-serif">
  <rect width="560" height="200" fill="#0a0e1a"/>
  <!-- Airfoil shape -->
  <path d="M80,100 C120,60 220,50 320,100 C220,150 120,140 80,100 Z" fill="none" stroke="#1e2a4a" stroke-width="1.5"/>
  <!-- Stagnation point -->
  <circle cx="80" cy="100" r="4" fill="#f77f00"/>
  <text x="62" y="104" fill="#f77f00" font-size="9" text-anchor="end">Ris.</text>
  <!-- Ice build-up top -->
  <path d="M80,100 C95,80 115,68 135,65 C145,64 140,60 130,58" fill="none" stroke="#00b4d8" stroke-width="3" stroke-linecap="round"/>
  <!-- Ice build-up bottom -->
  <path d="M80,100 C95,120 115,132 135,135 C145,136 140,140 130,142" fill="none" stroke="#00b4d8" stroke-width="3" stroke-linecap="round"/>
  <!-- Small droplets -->
  <g fill="#4cc9f0" opacity="0.7">
    <circle cx="30" cy="80" r="3"/><circle cx="45" cy="95" r="2.5"/><circle cx="35" cy="110" r="3"/>
    <circle cx="20" cy="100" r="2"/><circle cx="50" cy="75" r="2"/>
  </g>
  <!-- Large droplets -->
  <g fill="#f77f00" opacity="0.7">
    <circle cx="30" cy="60" r="5"/><circle cx="50" cy="130" r="5"/><circle cx="20" cy="140" r="4"/>
  </g>
  <!-- Arrows small -->
  <line x1="10" y1="80" x2="28" y2="80" stroke="#4cc9f0" stroke-width="1.2" marker-end="url(#arr1)"/>
  <line x1="10" y1="95" x2="43" y2="95" stroke="#4cc9f0" stroke-width="1.2" marker-end="url(#arr1)"/>
  <!-- Arrows large -->
  <line x1="10" y1="60" x2="27" y2="60" stroke="#f77f00" stroke-width="1.5" marker-end="url(#arr2)"/>
  <line x1="10" y1="130" x2="47" y2="130" stroke="#f77f00" stroke-width="1.5" marker-end="url(#arr2)"/>
  <defs>
    <marker id="arr1" markerWidth="6" markerHeight="6" refX="3" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#4cc9f0"/>
    </marker>
    <marker id="arr2" markerWidth="6" markerHeight="6" refX="3" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#f77f00"/>
    </marker>
  </defs>
  <text x="400" y="80" fill="#4cc9f0" font-size="10">Gocce piccole (20µm)</text>
  <text x="400" y="95" fill="#4cc9f0" font-size="9">→ seguono linee di flusso</text>
  <text x="400" y="120" fill="#f77f00" font-size="10">Gocce grandi (SLD)</text>
  <text x="400" y="135" fill="#f77f00" font-size="9">→ traiettorie rettilinee</text>
  <text x="80" y="30" fill="#00b4d8" font-size="10" text-anchor="middle">Deposito di ghiaccio (glaze)</text>
</svg>
</div>

<div class="example-box">
  <div class="ex-label">✦ ESEMPIO APPLICATO — B737 NG</div>
  <p>Dati operativi: U∞ = 100 m/s, LWC = 0,2 g/m³, D = 20 µm, corda C = 2 m. Con K ≈ 0,12 e Γ ≈ 0,6 (dai grafici NACA), la portata d'acqua sul bordo d'attacco è:<br><strong>ṁ = 0,0002 × 0,6 × 100 × 2 = 0,024 kg/(s·m)</strong><br>In 5 minuti si accumulano circa 7,2 kg/m di ghiaccio sul bordo, massa sufficiente a modificare sensibilmente il profilo.</p>
</div>
`},

/* 2 - Rischi */
{
  label: '03 // SICUREZZA DEL VOLO',
  title: 'Rischi per il Volo',
  body: `
<h3>Effetti sul Profilo Alare</h3>
<p>Il ghiaccio sul <span class="hl">bordo d'attacco (Leading Edge)</span> è la minaccia più critica poiché altera direttamente la forma aerodinamica del profilo:</p>

<div class="table-wrap">
<table>
  <tr><th>PARAMETRO</th><th>GHIACCIO VETRONE</th><th>MISTO</th><th>GRANULOSO</th></tr>
  <tr><td>Riduzione C<sub>L max</sub></td><td class="c-red">−40%</td><td class="c-yellow">−20%</td><td class="c-green">−10%</td></tr>
  <tr><td>Riduzione angolo di stallo</td><td class="c-red">−5°</td><td class="c-yellow">−2,5°</td><td class="c-yellow">−2,5°</td></tr>
  <tr><td>Aumento C<sub>D</sub></td><td class="c-red">4–5×</td><td class="c-yellow">3–4×</td><td class="c-yellow">2–3×</td></tr>
</table>
</div>

<div class="svg-container">
<svg viewBox="0 0 560 220" xmlns="http://www.w3.org/2000/svg">
  <rect width="560" height="220" fill="#0a0e1a"/>
  <!-- Axes -->
  <line x1="60" y1="185" x2="520" y2="185" stroke="#1e2a4a" stroke-width="1.5"/>
  <line x1="60" y1="20" x2="60" y2="185" stroke="#1e2a4a" stroke-width="1.5"/>
  <text x="290" y="210" fill="#5a6a8a" font-size="10" text-anchor="middle">Angolo di incidenza α</text>
  <text x="18" y="103" fill="#5a6a8a" font-size="10" transform="rotate(-90,18,103)">C_L</text>
  <!-- Clean airfoil -->
  <polyline points="60,185 150,150 240,115 310,80 370,50 390,35 370,45 320,90" fill="none" stroke="#00b4d8" stroke-width="2.5"/>
  <!-- Rime ice -->
  <polyline points="60,185 150,158 240,130 290,105 330,85 345,78 325,88" fill="none" stroke="#80b918" stroke-width="2" stroke-dasharray="5,3"/>
  <!-- Glaze ice -->
  <polyline points="60,185 150,168 230,145 270,125 295,112 305,108 285,118" fill="none" stroke="#ef233c" stroke-width="2" stroke-dasharray="3,3"/>
  <!-- Legend -->
  <line x1="320" y1="160" x2="360" y2="160" stroke="#00b4d8" stroke-width="2.5"/>
  <text x="365" y="164" fill="#00b4d8" font-size="10">Profilo pulito</text>
  <line x1="320" y1="178" x2="360" y2="178" stroke="#80b918" stroke-width="2" stroke-dasharray="5,3"/>
  <text x="365" y="182" fill="#80b918" font-size="10">Rime ice (−10% CL)</text>
  <line x1="320" y1="196" x2="360" y2="196" stroke="#ef233c" stroke-width="2" stroke-dasharray="3,3"/>
  <text x="365" y="200" fill="#ef233c" font-size="10">Glaze ice (−40% CL)</text>
</svg>
</div>

<h3>Effetti su Sistemi e Strumenti</h3>
<ul>
  <li><strong>Tubo di Pitot:</strong> ostruzione → velocità errata → possibile stall non rilevato</li>
  <li><strong>Sonda EPR e prese statiche:</strong> dati di pressione falsati → indicatori fuori scala</li>
  <li><strong>Antenne:</strong> distorsione segnale e rischio rottura meccanica per accumulo</li>
  <li><strong>Parabrezza:</strong> riduzione visibilità in tutte le fasi del volo</li>
  <li><strong>Prese d'aria motori:</strong> riduzione portata → calo di spinta</li>
</ul>

<h3>Jamming e Aileron Snatch</h3>
<p>Il ghiaccio può <span class="hl">bloccare le superfici mobili</span> (alettoni, flap, slat) per congelamento nei leveraggi. In casi estremi si verifica l'<span class="hl">Aileron Snatch</span>: forze aerodinamiche anomali generate dal ghiaccio dislocano gli alettoni verso il fondo-corsa in modo incontrollato, provocando rollio improvviso.</p>

<div class="example-box">
  <div class="ex-label">✦ CASO STORICO — Air Florida B737, 13 gennaio 1982</div>
  <p>Un Boeing 737-222 decollò dall'aeroporto di Washington National con ghiaccio sulle ali e sonde EPR intasate. Il dato errato di spinta convinse l'equipaggio che i motori erogassero potenza sufficiente. L'aereo cadde sul Ponte 14th Street sul Potomac River. 78 vittime. Causa primaria: mancato de-icing e anti-icing inadeguato.</p>
</div>
`},

/* 3 - Chimico */
{
  label: '04 // SISTEMI DI GESTIONE',
  title: 'Sistema Chimico (De-Icing)',
  body: `
<h3>Definizione</h3>
<p>Il sistema chimico è l'unico sistema di gestione del ghiaccio che opera <span class="hl">prevalentemente a terra</span>, prima del decollo. Si divide in:</p>
<ul>
  <li><strong>De-icing:</strong> rimuove il ghiaccio/brina/neve già presenti sulle superfici</li>
  <li><strong>Anti-icing:</strong> crea uno strato protettivo per impedire nuova formazione durante il rullaggio e il decollo</li>
</ul>

<h3>Procedura Operativa</h3>
<p>Due o più automezzi de-icer si posizionano ai lati del velivolo e spruzzano una <span class="hl">miscela di acqua calda (62–63 °C) e glicole propilenico</span> su ali, coda e fusoliera. L'acqua calda scioglie il ghiaccio per effetto termico, mentre la pressione del getto lo rimuove meccanicamente.</p>

<h3>Procedura One-Step vs Two-Step</h3>
<div class="table-wrap">
<table>
  <tr><th>PROCEDURA</th><th>FASE 1</th><th>FASE 2</th><th>USO</th></tr>
  <tr><td>One-Step</td><td>Fluido tipo I riscaldato</td><td>—</td><td>Condizioni mild, poco ghiaccio</td></tr>
  <tr><td>Two-Step</td><td>Fluido tipo I riscaldato (de-ice)</td><td>Fluido tipo II/IV puro (anti-ice)</td><td>Condizioni severe, neve intensa</td></tr>
</table>
</div>

<h3>Tipi di Fluido</h3>
<div class="table-wrap">
<table>
  <tr><th>TIPO</th><th>COMPOSIZIONE</th><th>VISCOSITÀ</th><th>HOLDOVER TIME</th></tr>
  <tr><td>Tipo I</td><td>Etilen/Propilen glicolato &gt;80%</td><td>Newtoniano</td><td>Breve (5–10 min)</td></tr>
  <tr><td>Tipo II</td><td>Glicolato + polimeri</td><td>Non-newtoniano</td><td>Medio (20–45 min)</td></tr>
  <tr><td>Tipo III</td><td>Intermedio</td><td>Intermedio</td><td>Medio</td></tr>
  <tr><td>Tipo IV</td><td>Glicolato + polimeri avanzati</td><td>Non-newtoniano</td><td>Lungo (fino a 80 min)</td></tr>
</table>
</div>

<h3>Holdover Time</h3>
<p>Il <span class="hl">holdover time</span> è il periodo durante il quale il fluido mantiene la sua efficacia protettiva. I fluidi di tipo II e IV sono di colore verde o arancione brillante: il colore si decolora col tempo, permettendo ai piloti di verificare visivamente se la protezione è ancora attiva.</p>

<div class="example-box">
  <div class="ex-label">✦ ESEMPIO PRATICO — Aeroporto con neve intensa, OAT = −5 °C</div>
  <p>Procedura two-step: 1) applicazione di fluido tipo I riscaldato a 63 °C per rimozione ghiaccio (4 minuti). 2) applicazione di fluido tipo IV puro per protezione anti-ice. Con neve leggera e OAT = −5 °C, il holdover time è circa 35–55 minuti: il velivolo deve decollare entro quel limite. Superato il tempo, la procedura viene ripetuta.</p>
</div>

<h3>Impatto Ambientale</h3>
<p>I fluidi glicolici hanno elevato BOD (Biochemical Oxygen Demand) e sono tossici per gli organismi acquatici. Per questo le operazioni si svolgono in <span class="hl">aree dedicate (de-icing pad)</span> con sistemi di raccolta e trattamento dei reflui.</p>
`},

/* 4 - Pneumatico */
{
  label: '05 // SISTEMI MECCANICI',
  title: 'Sistema Pneumatico (Boots)',
  body: `
<h3>Principio di Funzionamento</h3>
<p>Il sistema pneumatico de-ice utilizza <span class="hl">boots</span>: membrane in neoprene o gomma vulcanizzata installate sul bordo d'attacco dell'ala e dell'impennaggio. Invece di prevenire la formazione di ghiaccio, lo lasciano accumulare fino ad uno spessore minimo, poi lo rompono ciclicamente.</p>

<div class="svg-container">
<svg viewBox="0 0 560 160" xmlns="http://www.w3.org/2000/svg">
  <rect width="560" height="160" fill="#0a0e1a"/>
  <!-- State 1: deflated -->
  <g transform="translate(20,20)">
    <text x="60" y="12" fill="#5a6a8a" font-size="9" text-anchor="middle">BOOT SGONFIO + GHIACCIO</text>
    <path d="M10,80 C30,50 90,45 130,80 C90,115 30,110 10,80Z" fill="none" stroke="#1e2a4a" stroke-width="1.5"/>
    <!-- ice layer -->
    <path d="M10,80 C30,46 90,41 130,80" fill="none" stroke="#4cc9f0" stroke-width="5" stroke-linecap="round" opacity="0.8"/>
    <text x="60" y="140" fill="#4cc9f0" font-size="9" text-anchor="middle">Ghiaccio accumulato</text>
  </g>
  <!-- Arrow -->
  <text x="195" y="85" fill="#80b918" font-size="20" text-anchor="middle">→</text>
  <!-- State 2: inflated -->
  <g transform="translate(215,20)">
    <text x="60" y="12" fill="#5a6a8a" font-size="9" text-anchor="middle">BOOT GONFIATO</text>
    <path d="M10,80 C25,40 95,35 130,80 C95,125 25,120 10,80Z" fill="rgba(128,185,24,0.15)" stroke="#80b918" stroke-width="2"/>
    <!-- ice cracking -->
    <path d="M10,80 C25,36 95,31 130,80" fill="none" stroke="#4cc9f0" stroke-width="3" stroke-dasharray="8,4" opacity="0.6"/>
    <text x="60" y="140" fill="#80b918" font-size="9" text-anchor="middle">Gonfiaggio → frattura ghiaccio</text>
  </g>
  <!-- Arrow -->
  <text x="415" y="85" fill="#80b918" font-size="20" text-anchor="middle">→</text>
  <!-- State 3: shed -->
  <g transform="translate(420,20)">
    <text x="60" y="12" fill="#5a6a8a" font-size="9" text-anchor="middle">GHIACCIO ESPULSO</text>
    <path d="M10,80 C30,50 90,45 130,80 C90,115 30,110 10,80Z" fill="none" stroke="#1e2a4a" stroke-width="1.5"/>
    <!-- flying ice pieces -->
    <rect x="35" y="30" width="8" height="5" fill="#4cc9f0" transform="rotate(-20,39,32)" opacity="0.7"/>
    <rect x="70" y="25" width="6" height="4" fill="#4cc9f0" transform="rotate(15,73,27)" opacity="0.6"/>
    <rect x="90" y="35" width="7" height="4" fill="#4cc9f0" transform="rotate(-10,93,37)" opacity="0.5"/>
    <text x="60" y="140" fill="#5a6a8a" font-size="9" text-anchor="middle">Superficie pulita</text>
  </g>
</svg>
</div>

<h3>Ciclo Operativo</h3>
<p>L'aria compressa (prelevata dai motori o da un compressore apposito) gonfia i boots a intervalli regolati da una <span class="hl">valvola timer</span>. La sequenza tipica:</p>
<ul>
  <li>Fase di attesa: accumulo di 6–12 mm di ghiaccio (spessore minimo critico)</li>
  <li>Gonfiaggio: durata 4–6 secondi a 1,7–2,1 bar</li>
  <li>Deflazione: il ghiaccio fratturato viene rimosso dal vento relativo</li>
  <li>Riposo: 60–180 secondi prima del ciclo successivo</li>
</ul>

<h3>Bridging Effect — Difetto Critico</h3>
<p>Il <span class="hl">bridging effect</span> si verifica quando il ghiaccio si accumula in uno strato così sottile da "coprire" il boot gonfiato senza fratturarsi, formando un ponte rigido. In questo caso il boot, sgonfiandosi, lascia uno spazio tra superficie e ghiaccio che peggiora l'aerodinamica. Per questo i boots non devono essere attivati troppo presto.</p>

<h3>Applicazioni e Limiti</h3>
<div class="table-wrap">
<table>
  <tr><th>ASPETTO</th><th>VALORE</th></tr>
  <tr><td>Tipo di protezione</td><td>Solo de-ice (non anti-ice)</td></tr>
  <tr><td>Velivoli tipici</td><td>ATR 72, Dash 8, turboeliche in generale</td></tr>
  <tr><td>Limite principale</td><td>Non efficace con SLD (gocce grandi)</td></tr>
  <tr><td>Fonte aria</td><td>Motori o compressore dedicato</td></tr>
</table>
</div>

<div class="example-box">
  <div class="ex-label">✦ ESEMPIO — ATR 72 in rotta invernale</div>
  <p>L'ATR 72 è equipaggiato con boots Goodrich sul bordo d'attacco delle ali (slat 3÷5 esclusi per geometria) e sull'impennaggio. In condizioni di ghiacciamento moderato il sistema viene attivato ogni 90 secondi. Il pilota verifica la formazione su una <span class="hl">bacchetta luminosa</span> (ice evidence probe) posta sul muso prima di abilitare il ciclo.</p>
</div>
`},

/* 5 - Bleed-Air */
{
  label: '06 // SISTEMA AEROTERMICO',
  title: 'Sistema Bleed-Air (Termico)',
  body: `
<h3>Principio</h3>
<p>Il sistema termico bleed-air sfrutta l'<span class="hl">aria compressa e calda spillata dal compressore del turbofan</span> per riscaldare le superfici critiche, impedendo la formazione di ghiaccio (anti-ice) o rimuovendolo (de-ice in modalità ciclica).</p>

<h3>Sorgenti Bleed-Air</h3>
<ul>
  <li><strong>Motore (primaria):</strong> aria estratta dal 5° o 9° stadio dell'HPC a ~450 °C</li>
  <li><strong>APU (Auxiliary Power Unit):</strong> operativa a terra e in fase di avviamento</li>
  <li><strong>Ground External Source:</strong> carrello a terra per manutenzione/test</li>
</ul>

<div class="svg-container">
<svg viewBox="0 0 560 200" xmlns="http://www.w3.org/2000/svg">
  <rect width="560" height="200" fill="#0a0e1a"/>
  <!-- Engine schematic -->
  <ellipse cx="80" cy="100" rx="55" ry="70" fill="none" stroke="#1e2a4a" stroke-width="1.5"/>
  <text x="80" y="60" fill="#5a6a8a" font-size="8" text-anchor="middle">TURBOFAN</text>
  <text x="80" y="72" fill="#5a6a8a" font-size="8" text-anchor="middle">CFM56-7</text>
  <!-- 5th stage arrow -->
  <line x1="115" y1="85" x2="165" y2="70" stroke="#e040fb" stroke-width="1.8" stroke-dasharray="4,2"/>
  <text x="168" y="68" fill="#e040fb" font-size="9">5° stadio</text>
  <!-- 9th stage arrow -->
  <line x1="125" y1="100" x2="165" y2="90" stroke="#e040fb" stroke-width="1.8" stroke-dasharray="4,2"/>
  <text x="168" y="88" fill="#e040fb" font-size="9">9° stadio HPC</text>
  <!-- Precooler -->
  <rect x="200" y="78" width="60" height="30" fill="rgba(224,64,251,0.1)" stroke="#e040fb" stroke-width="1.5" rx="3"/>
  <text x="230" y="96" fill="#e040fb" font-size="8" text-anchor="middle">PRE-</text>
  <text x="230" y="106" fill="#e040fb" font-size="8" text-anchor="middle">COOLER</text>
  <!-- Duct -->
  <line x1="260" y1="93" x2="320" y2="93" stroke="#f77f00" stroke-width="3"/>
  <text x="290" y="85" fill="#f77f00" font-size="9" text-anchor="middle">~200°C</text>
  <!-- Wing leading edge cross section -->
  <path d="M340,50 C355,30 390,28 420,50 C390,72 355,70 340,50Z" fill="rgba(0,180,216,0.1)" stroke="#00b4d8" stroke-width="1.5"/>
  <!-- Piccolo tube -->
  <circle cx="370" cy="50" r="8" fill="none" stroke="#f77f00" stroke-width="2"/>
  <text x="370" y="53" fill="#f77f00" font-size="7" text-anchor="middle">PT</text>
  <!-- Hot air flow arrows -->
  <line x1="320" y1="93" x2="355" y2="60" stroke="#f77f00" stroke-width="2" marker-end="url(#farr)"/>
  <!-- Exhaust -->
  <line x1="420" y1="50" x2="480" y2="50" stroke="#4cc9f0" stroke-width="1.5" stroke-dasharray="3,3"/>
  <text x="490" y="53" fill="#4cc9f0" font-size="9">Scarico</text>
  <defs>
    <marker id="farr" markerWidth="6" markerHeight="6" refX="3" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#f77f00"/>
    </marker>
  </defs>
  <text x="375" y="30" fill="#00b4d8" font-size="9" text-anchor="middle">Leading Edge</text>
  <text x="370" y="140" fill="#5a6a8a" font-size="8" text-anchor="middle">PT = Piccolo Tube</text>
  <!-- Labels bottom -->
  <text x="80" y="185" fill="#5a6a8a" font-size="9" text-anchor="middle">450°C → HPC</text>
  <text x="230" y="185" fill="#5a6a8a" font-size="9" text-anchor="middle">Raffreddamento</text>
  <text x="375" y="185" fill="#5a6a8a" font-size="9" text-anchor="middle">Distribuzione uniforme</text>
</svg>
</div>

<h3>Piccolo Tube</h3>
<p>Il <span class="hl">piccolo tube</span> è un tubo forato di piccolo diametro posizionato all'interno del bordo d'attacco. L'aria calda viene sparata attraverso i fori verso la pelle interna del L.E., riscaldandola uniformemente. L'aria esausta viene poi convogliata verso l'esterno attraverso passaggi interni dell'ala.</p>

<h3>Applicazione sugli Slats</h3>
<p>Gli slat sono superfici mobili: il condotto di distribuzione deve seguirne il movimento. Per questo si utilizzano <span class="hl">condotti telescopici a sezione "T"</span> con giunzioni flessibili che mantengono la tenuta in tutte le posizioni operative.</p>

<h3>Canalizzazione</h3>
<p>I condotti sono realizzati in <span class="hl">titanio, acciaio inossidabile o fibra di vetro</span>, rivestiti con materiali termoisolanti (tipicamente fibra di vetro + foil metallico) per ridurre le dispersioni termiche lungo il percorso.</p>

<div class="example-box">
  <div class="ex-label">✦ ESEMPIO — B737 NG con CFM56-7B</div>
  <p>Il B737 NG utilizza aria dal 5° stadio a bassa spinta e dal 9° stadio ad alta spinta. La valvola di spillamento seleziona automaticamente lo stadio in base alla fase di volo. A crociera (FL350) l'aria è estratta dal 9° stadio a ~450 °C, passa attraverso il precooler (che la porta a ~180–220 °C) e raggiunge il L.E. tramite il piccolo tube. Il sistema è attivo in modalità AUTO quando il TAT scende sotto 10 °C.</p>
</div>
`},

/* 6 - Elettrico */
{
  label: '07 // SISTEMI DI BORDO',
  title: 'Sistema Elettrico',
  body: `
<h3>Principio</h3>
<p>Il sistema elettrico utilizza <span class="hl">elementi riscaldanti (resistenze)</span> integrati in superfici critiche per produrre calore per effetto Joule. È adatto a componenti di piccole dimensioni dove la bleed-air non può arrivare in modo efficiente.</p>

<h3>Componenti Protetti</h3>
<ul>
  <li><strong>Tubi di Pitot:</strong> riscaldatori a resistenza integrati nel corpo del tubo — attivazione automatica all'accensione</li>
  <li><strong>Sonde TAT / AOA (angolo d'attacco):</strong> riscaldamento continuo o ciclico</li>
  <li><strong>Parabrezza cabina di pilotaggio:</strong> strati conduttivi trasparenti (ITO) per de-icing e visibilità</li>
  <li><strong>Prese statiche:</strong> riscaldamento integrato nel bordo</li>
  <li><strong>Antenne e sensori:</strong> protezione puntuale</li>
</ul>

<h3>Riscaldatori a Superficie vs Lineari</h3>
<div class="table-wrap">
<table>
  <tr><th>TIPO</th><th>APPLICAZIONE</th><th>DENSITÀ DI POTENZA</th></tr>
  <tr><td>A superficie (mat riscaldante)</td><td>Bordi d'attacco piccoli velivoli, elicotteri</td><td>1–5 W/cm²</td></tr>
  <tr><td>Lineari (filo resistivo)</td><td>Parabrezza, sensori</td><td>2–8 W/cm²</td></tr>
  <tr><td>ITO (ossido di indio-stagno)</td><td>Parabrezza trasparente</td><td>3–6 W/cm²</td></tr>
</table>
</div>

<h3>Rilevatori di Ghiaccio</h3>
<p>Esistono tre tipologie principali di ice detector:</p>
<ul>
  <li><strong>Visual detection (spot lights):</strong> luci sul bordo d'attacco + bacchetta luminosa (hot rod) davanti al parabrezza</li>
  <li><strong>Vibrating rod:</strong> bacchetta magnetostrittiva che vibra a 40 kHz — il ghiaccio riduce la frequenza di risonanza, inviando il segnale di allerta</li>
  <li><strong>Serrated rotor detector:</strong> rotore dentato in lega di nichel — il ghiaccio crea coppia aggiuntiva rilevata dall'elettronica</li>
</ul>

<div class="formula-box">
  <div class="formula-label">POTENZA RISCALDANTE — EFFETTO JOULE</div>
  P = V² / R = I² · R = V · I
  <br><br>
  Per un tubo di Pitot tipico:<br>
  P ≈ 100–200 W per sonda<br>
  Densità termica ≈ 3–5 W/cm² sulla superficie riscaldante
</div>

<div class="example-box">
  <div class="ex-label">✦ ESEMPIO — Pitot Heat su A320</div>
  <p>L'A320 dispone di 3 tubi di Pitot per lato (captain, first officer, standby), ciascuno con riscaldatore autonomo da ~120 W. Il sistema è attivato automaticamente al primo avvio motore e rimane sempre acceso in volo. Guasto al pitot heat senza accorgimento: incidente AF447 (2009) — le sonde ghiacciate hanno fornito dati inconsistenti all'ADIRS, disconnettendo l'autopilota.</p>
</div>
`},

/* 7 - Cockpit */
{
  label: '08 // AVIONICA E PROCEDURE',
  title: 'Controlli in Cockpit',
  body: `
<h3>Forward Overhead Panel — WAI</h3>
<p>Il sistema Wing Anti-Ice (WAI) sul B737 è comandato dal <span class="hl">Forward Overhead Panel</span> e dispone di tre modalità operative:</p>
<ul>
  <li><strong>AUTO:</strong> attivazione automatica basata su parametri TAT e condizioni di volo</li>
  <li><strong>ON:</strong> attivazione manuale forzata indipendentemente dalle condizioni</li>
  <li><strong>OFF:</strong> sistema disattivato</li>
</ul>

<h3>Condizioni di Inibizione</h3>
<p>Con selettore in AUTO, il sistema è inibito (non si attiva) se:</p>
<ul>
  <li>Aereo a terra (eccetto durante test BITE)</li>
  <li>TAT &gt; +10 °C con meno di 5 minuti dal decollo</li>
  <li>Funzionamento automatico degli slat</li>
  <li>Pompa idraulica pneumatica in funzione</li>
  <li>Avviamento motore in corso (ENGINE START)</li>
  <li>Temperatura bleed-air &lt; 93 °C (200 °F)</li>
</ul>

<h3>Componenti del Sistema di Controllo</h3>
<div class="table-wrap">
<table>
  <tr><th>COMPONENTE</th><th>FUNZIONE</th></tr>
  <tr><td>Engine Bleed Air Valve</td><td>Valvola principale di spillamento motore</td></tr>
  <tr><td>WAI Valve</td><td>Valvola ON/OFF del circuito wing anti-ice</td></tr>
  <tr><td>WAI Pressure Sensor</td><td>Monitoraggio pressione nei condotti WAI</td></tr>
  <tr><td>Bleed Trip Sensors</td><td>Rilevano sovratemperature → trip automatico</td></tr>
  <tr><td>Duct Pressure Transmitters</td><td>Misurano pressione bleed-air nei condotti</td></tr>
  <tr><td>Isolation Valve</td><td>Separa i circuiti sinistro/destro</td></tr>
  <tr><td>Wing-Body Overheat Light</td><td>Allarme surriscaldamento ala/fusoliera</td></tr>
  <tr><td>Dual Bleed Light</td><td>Avvisa se entrambi i motori spillano aria</td></tr>
  <tr><td>Trip Reset Switch</td><td>Ripristina valvole dopo un trip</td></tr>
</table>
</div>

<h3>Sistema A320 — Ice & Rain Protection</h3>
<p>L'A320 protegge tramite bleed-air: i bordi d'attacco delle ali (slat 3 e 5 più esterni), le prese d'aria dei motori e il piano di coda. Solo gli slat 3 e 5 più esterni sono interessati dall'anti-ice perché quelli interni, per caratteristiche aerodinamiche, non necessitano protezione.</p>

<div class="example-box">
  <div class="ex-label">✦ CHECK-LIST PRATICA — Attivazione WAI B737</div>
  <p>Prima di entrare in zona IMC con TAT &lt; +5 °C: 1) Verificare che la bleed-air sia operativa (duct pressure &gt; 25 PSI). 2) Selezionare WING ANTI-ICE → ON. 3) Verificare accensione della WING ANTI-ICE VALVE OPEN LIGHT. 4) Monitorare DUCT PRESS e WING BODY OVHT durante tutta la fase. 5) In caso di WING BODY OVHT: selezionare OFF e verificare condotto per eventuale perdita.</p>
</div>
`},

/* 8 - Confronto */
{
  label: '09 // ANALISI COMPARATIVA',
  title: 'Confronto Sistemi Anti/De-Ice',
  body: `
<h3>Panoramica</h3>
<p>I quattro sistemi principali hanno caratteristiche molto diverse per applicazione, efficacia, peso e impatto sulle prestazioni del velivolo. Nessun sistema è universalmente superiore: la scelta dipende dal tipo di velivolo, dalla fase di volo e dalle condizioni meteorologiche attese.</p>

<div class="table-wrap">
<table>
  <tr><th>CRITERIO</th><th>CHIMICO</th><th>PNEUMATICO</th><th>BLEED-AIR</th><th>ELETTRICO</th></tr>
  <tr><td>Tipo</td><td>Anti + De</td><td>De</td><td>Anti + De</td><td>Anti + De</td></tr>
  <tr><td>Fase operativa</td><td>Terra</td><td>Volo</td><td>Volo</td><td>Terra + Volo</td></tr>
  <tr><td>Efficacia su SLD</td><td class="c-yellow">Parziale</td><td class="c-red">No</td><td class="c-green">Sì</td><td class="c-green">Sì (puntuale)</td></tr>
  <tr><td>Impatto su spinta</td><td class="c-green">Nessuno</td><td class="c-yellow">Lieve</td><td class="c-red">Significativo (−1–3%)</td><td class="c-yellow">Lieve (carico el.)</td></tr>
  <tr><td>Peso aggiuntivo</td><td class="c-yellow">Fluidi</td><td class="c-yellow">Condotti + boots</td><td class="c-red">Condotti pesanti</td><td class="c-green">Basso</td></tr>
  <tr><td>Manutenzione</td><td class="c-green">Bassa</td><td class="c-yellow">Media (boots)</td><td class="c-yellow">Media</td><td class="c-yellow">Media (elementi)</td></tr>
  <tr><td>Costo operativo</td><td class="c-red">Alto (fluidi)</td><td class="c-green">Basso</td><td class="c-yellow">Medio</td><td class="c-yellow">Medio</td></tr>
  <tr><td>Impatto ambientale</td><td class="c-red">Alto</td><td class="c-green">Basso</td><td class="c-green">Basso</td><td class="c-green">Basso</td></tr>
  <tr><td>Applicazioni tipiche</td><td>Tutti (terra)</td><td>ATR, Q400</td><td>B737, A320, A330</td><td>Pitot, parabrezza, tutti</td></tr>
</table>
</div>

<h3>Vantaggi e Svantaggi Bleed-Air</h3>
<div class="table-wrap">
<table>
  <tr><th class="c-green">✓ VANTAGGI</th><th class="c-red">✗ SVANTAGGI</th></tr>
  <tr><td>Alta affidabilità</td><td>Richiede grande quantità di aria calda</td></tr>
  <tr><td>Efficace come anti-ice</td><td>Degrada le prestazioni del motore</td></tr>
  <tr><td>Bassa complessità tecnica</td><td>Aumenta i consumi di carburante</td></tr>
  <tr><td>Nessun componente mobile aggiuntivo</td><td>Limita l'uso di materiali compositi</td></tr>
  <tr><td>Applicabile a grandi superfici</td><td>Non usato su Boeing 787 (sistema elettrico)</td></tr>
</table>
</div>

<h3>Il Caso Boeing 787 Dreamliner</h3>
<p>Il B787 ha abbandonato il sistema bleed-air tradizionale, adottando un'architettura <span class="hl">no-bleed</span> con sistema elettrico ad alta tensione (235 V AC). I vantaggi: minore consumo complessivo, struttura alare in fibra di carbonio senza i vincoli termici del bleed-air, minori perdite di spinta. La potenza richiesta dai sistemi elettrici viene generata da generatori azionati dai motori ad alta efficienza.</p>

<div class="svg-container">
<svg viewBox="0 0 560 180" xmlns="http://www.w3.org/2000/svg">
  <rect width="560" height="180" fill="#0a0e1a"/>
  <!-- Radar chart simplified as bar chart -->
  <text x="280" y="18" fill="#5a6a8a" font-size="10" text-anchor="middle">CONFRONTO PUNTEGGIO SISTEMI (scala 1–5)</text>
  <!-- bars -->
  <g transform="translate(30,30)">
    <!-- categories -->
    <text x="0" y="20" fill="#5a6a8a" font-size="9">Efficacia</text>
    <text x="0" y="50" fill="#5a6a8a" font-size="9">Peso ridotto</text>
    <text x="0" y="80" fill="#5a6a8a" font-size="9">Costo basso</text>
    <text x="0" y="110" fill="#5a6a8a" font-size="9">Amb. basso</text>
    <!-- Chimico -->
    <rect x="90" y="8" width="40" height="14" fill="#4cc9f0" opacity="0.7" rx="2"/>
    <rect x="90" y="38" width="56" height="14" fill="#4cc9f0" opacity="0.7" rx="2"/>
    <rect x="90" y="68" width="24" height="14" fill="#4cc9f0" opacity="0.7" rx="2"/>
    <rect x="90" y="98" width="16" height="14" fill="#4cc9f0" opacity="0.7" rx="2"/>
    <!-- Pneumatico -->
    <rect x="155" y="8" width="32" height="14" fill="#80b918" opacity="0.7" rx="2"/>
    <rect x="155" y="38" width="48" height="14" fill="#80b918" opacity="0.7" rx="2"/>
    <rect x="155" y="68" width="56" height="14" fill="#80b918" opacity="0.7" rx="2"/>
    <rect x="155" y="98" width="56" height="14" fill="#80b918" opacity="0.7" rx="2"/>
    <!-- Bleed-air -->
    <rect x="220" y="8" width="64" height="14" fill="#e040fb" opacity="0.7" rx="2"/>
    <rect x="220" y="38" width="24" height="14" fill="#e040fb" opacity="0.7" rx="2"/>
    <rect x="220" y="68" width="40" height="14" fill="#e040fb" opacity="0.7" rx="2"/>
    <rect x="220" y="98" width="48" height="14" fill="#e040fb" opacity="0.7" rx="2"/>
    <!-- Elettrico -->
    <rect x="293" y="8" width="56" height="14" fill="#f77f00" opacity="0.7" rx="2"/>
    <rect x="293" y="38" width="64" height="14" fill="#f77f00" opacity="0.7" rx="2"/>
    <rect x="293" y="68" width="40" height="14" fill="#f77f00" opacity="0.7" rx="2"/>
    <rect x="293" y="98" width="56" height="14" fill="#f77f00" opacity="0.7" rx="2"/>
    <!-- legend -->
    <rect x="90" y="130" width="12" height="12" fill="#4cc9f0" rx="2"/>
    <text x="106" y="141" fill="#5a6a8a" font-size="9">Chimico</text>
    <rect x="155" y="130" width="12" height="12" fill="#80b918" rx="2"/>
    <text x="171" y="141" fill="#5a6a8a" font-size="9">Pneumatico</text>
    <rect x="230" y="130" width="12" height="12" fill="#e040fb" rx="2"/>
    <text x="246" y="141" fill="#5a6a8a" font-size="9">Bleed-Air</text>
    <rect x="305" y="130" width="12" height="12" fill="#f77f00" rx="2"/>
    <text x="321" y="141" fill="#5a6a8a" font-size="9">Elettrico</text>
  </g>
</svg>
</div>
`}
];

/* INIT CARDS with scroll reveal */
const cards = document.querySelectorAll('.card');
const observer = new IntersectionObserver((entries) => {
  entries.forEach((e, i) => {
    if (e.isIntersecting) {
      setTimeout(() => e.target.classList.add('visible'), i * 80);
      observer.unobserve(e.target);
    }
  });
}, { threshold: 0.1 });

cards.forEach(c => {
  observer.observe(c);
  c.addEventListener('click', () => openModal(parseInt(c.dataset.id)));
});

/* MODAL */
const overlay = document.getElementById('overlay');
const modal = document.getElementById('modal');
const modalLabel = document.getElementById('modalLabel');
const modalTitle = document.getElementById('modalTitle');
const modalBody = document.getElementById('modalBody');
const closeBtn = document.getElementById('closeBtn');

function openModal(id) {
  const d = CONTENT[id];
  const color = MODAL_COLOR[id];
  modal.style.setProperty('--modal-color', color);
  modalLabel.textContent = d.label;
  modalTitle.textContent = d.title;
  modalBody.innerHTML = d.body;
  overlay.classList.add('open');
  document.body.style.overflow = 'hidden';
  overlay.scrollTop = 0;
}

function closeModal() {
  overlay.classList.remove('open');
  document.body.style.overflow = '';
}

closeBtn.addEventListener('click', closeModal);
overlay.addEventListener('click', e => { if (e.target === overlay) closeModal(); });
document.addEventListener('keydown', e => { if (e.key === 'Escape') closeModal(); });
</script>
</body>
</html>
