<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FLUXON PRO</title>
  <meta name="description" content="FLUXON PRO - CONTROLE FINANCEIRO." />
  <style>
    :root {
      --bg: #f5f7fb;
      --bg-soft: #ffffff;
      --card: rgba(255,255,255,0.85);
      --card-solid: #ffffff;
      --text: #10131a;
      --muted: #5d6574;
      --line: rgba(16,19,26,0.08);
      --shadow: 0 16px 40px rgba(16, 19, 26, 0.10);
      --green: #18c964;
      --green-2: #0ea95a;
      --red: #ff4d5e;
      --red-2: #c92c3f;
      --accent: #7c3aed;
      --danger-soft: rgba(255, 77, 94, 0.12);
      --success-soft: rgba(24, 201, 100, 0.14);
      --glow: rgba(24, 201, 100, 0.22);
      --radius: 24px;
    }
    body.dark {
      --bg: #000000; --bg-soft: #070707;
      --card: rgba(255,255,255,0.05); --card-solid: #0e0e0e;
      --text: #f5f7fb; --muted: #b2b8c5;
      --line: rgba(255,255,255,0.09);
      --shadow: 0 16px 40px rgba(0,0,0,0.55);
      --danger-soft: rgba(255,77,94,0.18);
      --success-soft: rgba(24,201,100,0.18);
      --glow: rgba(24,201,100,0.34);
    }
    * { box-sizing: border-box; }
    html, body {
      margin: 0; padding: 0;
      font-family: Inter, Arial, sans-serif;
      background: radial-gradient(circle at top, rgba(124,58,237,0.08), transparent 28%), var(--bg);
      color: var(--text); min-height: 100%;
      transition: background 0.25s ease, color 0.25s ease;
      text-transform: uppercase;
    }
    button, input, select, textarea { font: inherit; }
    @supports (padding: env(safe-area-inset-bottom)) {
      @media (max-width: 640px) {
        .history-card { margin-bottom: calc(20px + env(safe-area-inset-bottom)); }
      }
    }
    body.loading .app { opacity: 0; transform: translateY(6px); }
    .page-loader {
      position: fixed; inset: 0; display: grid; place-items: center;
      background: var(--bg); z-index: 999;
      transition: opacity 0.3s ease, visibility 0.3s ease;
    }
    body.loaded .page-loader { opacity: 0; visibility: hidden; pointer-events: none; }
    .loader-card {
      width: min(420px, calc(100vw - 32px));
      background: var(--card-solid); border: 1px solid var(--line);
      border-radius: 28px; box-shadow: var(--shadow); padding: 28px; text-align: center;
    }
    .loader-logo { font-size: 1.45rem; font-weight: 900; letter-spacing: 0.12em; margin-bottom: 14px; }
    .loader-bar { height: 10px; border-radius: 999px; overflow: hidden; background: rgba(124,58,237,0.12); position: relative; }
    .loader-bar::after {
      content: ""; position: absolute; inset: 0; width: 45%; border-radius: inherit;
      background: linear-gradient(135deg, var(--green), var(--accent));
      animation: loaderMove 1.05s infinite ease-in-out;
    }
    @keyframes loaderMove { 0% { transform: translateX(-100%); } 100% { transform: translateX(260%); } }
    .app { max-width: 1440px; margin: 0 auto; padding: 24px; transition: opacity 0.2s ease, transform 0.2s ease; }
    .topbar {
      position: sticky; top: 0; z-index: 30;
      display: flex; justify-content: space-between; align-items: center;
      gap: 16px; padding: 16px 0 24px; backdrop-filter: blur(14px);
    }
    .brand h1 { margin: 0; font-size: clamp(1.6rem, 2vw, 2.35rem); letter-spacing: 0.06em; font-weight: 900; }
    .brand p { margin: 8px 0 0; color: var(--muted); font-size: 0.95rem; letter-spacing: 0.04em; }
    .top-actions { display: flex; gap: 12px; flex-wrap: wrap; align-items: center; }
    .btn {
      border: 1px solid var(--line); border-radius: 16px; padding: 12px 18px;
      color: var(--text); background: var(--card-solid); cursor: pointer;
      transition: transform 0.16s ease, opacity 0.16s ease, box-shadow 0.2s ease, background 0.25s ease;
      box-shadow: var(--shadow); font-weight: 800; letter-spacing: 0.06em; text-transform: uppercase;
    }
    .btn:hover { transform: translateY(-2px); }
    .btn:active { transform: translateY(0); }
    .btn-primary { background: linear-gradient(135deg, var(--green), var(--green-2)); color: #fff; border: none; box-shadow: 0 12px 24px rgba(24,201,100,0.28); }
    .btn-danger { background: var(--danger-soft); color: var(--red); border-color: rgba(255,77,94,0.22); box-shadow: none; }
    .btn-ghost { background: transparent; box-shadow: none; }
    .stats {
      display: grid; grid-template-columns: repeat(4, minmax(0, 1fr));
      gap: 16px; margin-bottom: 20px; content-visibility: auto; contain-intrinsic-size: 360px;
    }
    .card {
      background: var(--card); border: 1px solid var(--line); border-radius: var(--radius);
      padding: 18px; box-shadow: var(--shadow); backdrop-filter: blur(10px); position: relative; overflow: hidden;
    }
    .card h3 { margin: 0; font-size: 0.92rem; color: var(--muted); font-weight: 800; letter-spacing: 0.09em; }
    .card .value { margin-top: 10px; font-size: clamp(1.6rem, 2.2vw, 2.2rem); font-weight: 900; letter-spacing: -0.03em; }
    .card .chip { margin-top: 12px; display: inline-flex; align-items: center; gap: 8px; padding: 8px 12px; border-radius: 999px; font-size: 0.82rem; font-weight: 800; width: fit-content; letter-spacing: 0.05em; }
    .income-chip { background: var(--success-soft); color: var(--green); }
    .expense-chip { background: var(--danger-soft); color: var(--red); }
    .neutral-chip { background: rgba(124,58,237,0.12); color: #7c3aed; }
    .content-grid { display: grid; grid-template-columns: 1.05fr 1.4fr; gap: 20px; margin-bottom: 20px; }
    .lazy-panel { content-visibility: auto; contain-intrinsic-size: 480px; }
    .panel-title { display: flex; justify-content: space-between; align-items: center; gap: 12px; margin-bottom: 18px; }
    .panel-title h2 { margin: 0; font-size: 1.05rem; letter-spacing: 0.06em; }
    .panel-title p { margin: 4px 0 0; color: var(--muted); font-size: 0.9rem; letter-spacing: 0.04em; }
    .chart-box { height: 340px; position: relative; display: grid; place-items: center; }
    .chart-placeholder { display: grid; gap: 12px; justify-items: center; text-align: center; color: var(--muted); }
    .chart-placeholder strong { color: var(--text); }
    .pulse-dot { width: 14px; height: 14px; border-radius: 999px; background: linear-gradient(135deg, var(--green), var(--accent)); box-shadow: 0 0 0 0 rgba(24,201,100,0.4); animation: pulse 1.4s infinite; }
    @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(24,201,100,0.45); } 70% { box-shadow: 0 0 0 16px rgba(24,201,100,0); } 100% { box-shadow: 0 0 0 0 rgba(24,201,100,0); } }
    .chart-box canvas { position: relative; z-index: 2; width: 100%; height: 100%; }
    .line-card { position: relative; isolation: isolate; }
    .line-card .chart-box { position: relative; }
    .line-card .chart-box::after { content: ""; position: absolute; left: 10%; right: 10%; bottom: 0; height: 60px; background: radial-gradient(circle, var(--glow), transparent 68%); filter: blur(18px); z-index: 0; opacity: 0.95; pointer-events: none; }
    .range-switcher { display: flex; flex-wrap: nowrap; gap: 8px; align-items: center; }
    .range-btn { border: 1px solid var(--line); background: transparent; color: var(--text); padding: 10px 14px; border-radius: 999px; cursor: pointer; font-weight: 800; letter-spacing: 0.05em; text-transform: uppercase; }
    .range-btn.active { background: rgba(124,58,237,0.16); border-color: rgba(124,58,237,0.3); color: var(--text); }
    .history-card { padding: 0; }
    .history-head { display: flex; justify-content: space-between; align-items: center; gap: 12px; padding: 18px 18px 12px; border-bottom: 1px solid var(--line); }
    .history-head h2 { margin: 0; font-size: 1.1rem; letter-spacing: 0.05em; }
    .history-head p { margin: 4px 0 0; color: var(--muted); letter-spacing: 0.04em; }
    .table-wrap { overflow: auto; }
    table { width: 100%; border-collapse: collapse; min-width: 1160px; }
    th, td { text-align: left; padding: 15px 18px; border-bottom: 1px solid var(--line); font-size: 0.92rem; vertical-align: middle; letter-spacing: 0.04em; }
    th { color: var(--muted); font-size: 0.78rem; letter-spacing: 0.1em; }
    tbody tr:hover { background: rgba(124,58,237,0.05); }
    .type-badge, .category-badge { display: inline-flex; align-items: center; padding: 7px 12px; border-radius: 999px; font-size: 0.78rem; font-weight: 900; letter-spacing: 0.06em; }
    .type-entrada { background: var(--success-soft); color: var(--green); }
    .type-saida { background: var(--danger-soft); color: var(--red); }
    .category-badge { background: rgba(124,58,237,0.12); color: var(--text); }
    .amount-positive { color: var(--green); font-weight: 900; }
    .amount-negative { color: var(--red); font-weight: 900; }
    .obs-cell { max-width: 300px; white-space: normal; }
    .row-actions { display: flex; gap: 8px; flex-wrap: wrap; }
    .mini-btn { border: 1px solid var(--line); border-radius: 12px; padding: 9px 12px; background: var(--card-solid); color: var(--text); cursor: pointer; font-weight: 800; letter-spacing: 0.05em; text-transform: uppercase; }
    .empty { padding: 28px 18px 40px; text-align: center; color: var(--muted); letter-spacing: 0.04em; }
    dialog { width: min(680px, calc(100vw - 24px)); border: 1px solid var(--line); border-radius: 28px; background: var(--card-solid); color: var(--text); box-shadow: 0 32px 70px rgba(0,0,0,0.35); padding: 0; }
    dialog::backdrop { background: rgba(0,0,0,0.55); backdrop-filter: blur(4px); }
    .modal-head { padding: 22px 24px 8px; display: flex; justify-content: space-between; align-items: start; gap: 12px; }
    .modal-head h2 { margin: 0; letter-spacing: 0.06em; }
    .modal-head p { margin: 6px 0 0; color: var(--muted); letter-spacing: 0.04em; }
    .form-grid { padding: 14px 24px 24px; display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 14px; }
    .field { display: flex; flex-direction: column; gap: 8px; }
    .field.full { grid-column: 1 / -1; }
    .field label { color: var(--muted); font-weight: 800; font-size: 0.82rem; letter-spacing: 0.08em; }
    .field input, .field select, .field textarea { background: var(--card-solid); border: 1px solid var(--line); border-radius: 14px; padding: 13px 14px; color: var(--text); outline: none; text-transform: uppercase; }
    .field select option { background: var(--card-solid); color: var(--text); }
    .field textarea { min-height: 110px; resize: vertical; }
    .field input::placeholder, .field textarea::placeholder { text-transform: uppercase; letter-spacing: 0.05em; }
    .field input:focus, .field select:focus, .field textarea:focus { border-color: rgba(124,58,237,0.48); box-shadow: 0 0 0 4px rgba(124,58,237,0.14); }
    .helper { color: var(--muted); font-size: 0.75rem; letter-spacing: 0.05em; }
    .error-box { grid-column: 1 / -1; border-radius: 14px; padding: 12px 14px; background: rgba(255,77,94,0.12); border: 1px solid rgba(255,77,94,0.28); color: var(--red); font-weight: 800; letter-spacing: 0.05em; }
    .error-box[hidden] { display: none; }
    .modal-actions { grid-column: 1 / -1; display: flex; justify-content: flex-end; gap: 10px; margin-top: 8px; }
    .footer-note { margin-top: 16px; color: var(--muted); font-size: 0.86rem; text-align: center; letter-spacing: 0.05em; }
    .deploy-note { margin-top: 14px; display: grid; gap: 10px; border: 1px dashed rgba(124,58,237,0.35); background: rgba(124,58,237,0.08); border-radius: 20px; padding: 16px 18px; color: var(--text); letter-spacing: 0.04em; }
    .deploy-note strong { color: var(--accent); }

    /* ── DESKTOP 16:9 ── */
    @media (max-width: 1100px) { .content-grid { grid-template-columns: 1fr; } }
    @media (max-width: 900px) {
      .stats { grid-template-columns: repeat(2, minmax(0, 1fr)); }
      .topbar { flex-direction: column; align-items: stretch; }
      .top-actions { justify-content: flex-start; }
    }

    /* ── MOBILE 9:16 ── */
    @media (max-width: 640px) {
      html, body { font-size: 15px; }
      .app { padding: 0; max-width: 100vw; overflow-x: hidden; }
      .topbar { flex-direction: row; align-items: center; justify-content: space-between; padding: 14px 16px; gap: 10px; background: var(--bg); border-bottom: 1px solid var(--line); backdrop-filter: blur(18px); }
      .brand h1 { font-size: 1.35rem; letter-spacing: 0.07em; }
      .brand p { display: none; }
      .top-actions { flex-wrap: nowrap; gap: 8px; }
      #themeToggle { padding: 10px 12px; font-size: 0; border-radius: 14px; min-width: 44px; min-height: 44px; position: relative; }
      #themeToggle::after { content: "☀"; font-size: 1.1rem; position: absolute; inset: 0; display: grid; place-items: center; }
      body.dark #themeToggle::after { content: "🌙"; }
      #newTransactionBtn { padding: 10px 14px; font-size: 0.72rem; border-radius: 14px; white-space: nowrap; }
      .stats { grid-template-columns: 1fr 1fr; gap: 10px; padding: 14px 14px 4px; margin-bottom: 0; contain-intrinsic-size: auto; }
      .card { border-radius: 18px; padding: 14px; }
      .card h3 { font-size: 0.72rem; letter-spacing: 0.08em; }
      .card .value { font-size: 1.25rem; margin-top: 6px; }
      .card .chip { margin-top: 8px; padding: 5px 10px; font-size: 0.68rem; }
      .content-grid { grid-template-columns: 1fr; gap: 10px; padding: 10px 14px 0; margin-bottom: 0; }
      .lazy-panel { contain-intrinsic-size: auto; }
      .panel-title { flex-direction: column; align-items: flex-start; gap: 10px; margin-bottom: 12px; }
      .panel-title h2 { font-size: 0.85rem; letter-spacing: 0.06em; }
      .range-switcher { width: 100%; overflow-x: auto; flex-wrap: nowrap; padding-bottom: 2px; gap: 6px; -webkit-overflow-scrolling: touch; scrollbar-width: none; }
      .range-switcher::-webkit-scrollbar { display: none; }
      .range-btn { padding: 8px 12px; font-size: 0.72rem; flex-shrink: 0; border-radius: 999px; }
      .chart-box { height: 240px; }
      .history-card { margin: 10px 14px 20px; border-radius: 18px; overflow: hidden; }
      .history-head { flex-direction: row; align-items: center; gap: 8px; padding: 14px 14px 10px; }
      .history-head h2 { font-size: 0.85rem; letter-spacing: 0.05em; }
      .history-head p { font-size: 0.72rem; }
      #resetSamplesBtn { padding: 8px 10px; font-size: 0.68rem; border-radius: 10px; white-space: nowrap; }
      .table-wrap { overflow: visible; }
      table { min-width: unset; width: 100%; border-collapse: collapse; }
      table thead { display: none; }
      table tbody tr { display: flex; flex-direction: column; gap: 0; border-bottom: 1px solid var(--line); padding: 12px 14px; position: relative; }
      table tbody tr:last-child { border-bottom: none; }
      table tbody td { display: block; padding: 2px 0; border: none; font-size: 0.85rem; letter-spacing: 0.03em; }
      table tbody td:nth-child(1) { font-weight: 900; font-size: 0.92rem; color: var(--text); margin-bottom: 4px; padding-right: 80px; }
      table tbody td:nth-child(2) { order: 3; display: inline-block; }
      table tbody td:nth-child(3) { display: inline-block; margin-left: 6px; order: 4; }
      table tbody td:nth-child(4) { order: 2; color: var(--muted); font-size: 0.75rem; margin-bottom: 4px; }
      table tbody td:nth-child(5) { position: absolute; top: 12px; right: 14px; font-size: 1rem; font-weight: 900; order: 0; }
      table tbody td:nth-child(6) { order: 5; color: var(--muted); font-size: 0.75rem; max-width: 100%; white-space: normal; margin-top: 4px; }
      table tbody td:nth-child(6):empty { display: none; }
      table tbody td:nth-child(7) { order: 6; margin-top: 8px; }
      .row-actions { gap: 6px; }
      .mini-btn { padding: 7px 11px; font-size: 0.72rem; border-radius: 10px; }
      .empty { padding: 24px 14px 32px; font-size: 0.85rem; }
      dialog { width: 100vw; max-width: 100vw; border-radius: 24px 24px 0 0; margin: auto auto 0; max-height: 92vh; overflow-y: auto; }
      .form-grid { grid-template-columns: 1fr; padding: 14px 16px 20px; gap: 12px; }
      .modal-head { padding: 18px 16px 8px; }
      .modal-head h2 { font-size: 1rem; }
      .field.full { grid-column: 1; }
      .modal-actions { flex-direction: column-reverse; gap: 8px; }
      .modal-actions .btn { width: 100%; justify-content: center; }
      .footer-note { padding: 0 14px 20px; font-size: 0.78rem; }
      .deploy-note { margin: 0 14px 20px; }
      .loader-card { padding: 22px 20px; }
    }
  </style>
</head>
<body class="dark loading">
  <div class="page-loader" id="pageLoader">
    <div class="loader-card">
      <div class="loader-logo">FLUXON PRO</div>
      <div class="loader-bar"></div>
      <p>CARREGANDO PAINEL FINANCEIRO...</p>
    </div>
  </div>

  <div class="app">
    <header class="topbar">
      <div class="brand"><h1>FLUXON PRO</h1></div>
      <div class="top-actions">
        <button class="btn" id="themeToggle" type="button">ALTERNAR TEMA</button>
        <button class="btn btn-primary" id="newTransactionBtn" type="button">NOVA TRANSAÇÃO</button>
      </div>
    </header>

    <section class="stats">
      <div class="card"><h3>ENTRADAS</h3><div class="value" id="incomeValue">R$ 0,00</div><div class="chip income-chip">FLUXO POSITIVO</div></div>
      <div class="card"><h3>SAÍDAS</h3><div class="value" id="expenseValue">R$ 0,00</div><div class="chip expense-chip">FLUXO NEGATIVO</div></div>
      <div class="card"><h3>SALDO</h3><div class="value" id="balanceValue">R$ 0,00</div><div class="chip neutral-chip" id="balanceChip">RESULTADO GERAL</div></div>
      <div class="card"><h3>TRANSAÇÕES</h3><div class="value" id="countValue">0</div><div class="chip neutral-chip">HISTÓRICO ATIVO</div></div>
    </section>

    <section class="content-grid">
      <article class="card lazy-panel" id="chartSectionPie">
        <div class="panel-title"><div><h2>DISTRIBUIÇÃO POR CATEGORIA</h2></div></div>
        <div class="chart-box">
          <div class="chart-placeholder" id="piePlaceholder" hidden></div>
          <canvas id="pieChart" hidden></canvas>
        </div>
      </article>
      <article class="card line-card lazy-panel" id="chartSectionLine">
        <div class="panel-title">
          <div><h2>ACOMPANHAMENTO FINANCEIRO</h2></div>
          <div class="range-switcher" id="rangeSwitcher">
            <button class="range-btn active" data-range="daily" type="button">DIÁRIO</button>
            <button class="range-btn" data-range="weekly" type="button">SEMANAL</button>
            <button class="range-btn" data-range="monthly" type="button">MENSAL</button>
            <button class="range-btn" data-range="yearly" type="button">ANUAL</button>
          </div>
        </div>
        <div class="chart-box">
          <div class="chart-placeholder" id="linePlaceholder" hidden></div>
          <canvas id="lineChart" hidden></canvas>
        </div>
      </article>
    </section>

    <section class="card history-card">
      <div class="history-head">
        <div><h2>HISTÓRICO DE TRANSAÇÕES</h2></div>
        <button class="btn btn-ghost" id="resetSamplesBtn" type="button">RESTAURAR EXEMPLOS</button>
      </div>
      <div class="table-wrap">
        <table>
          <thead>
            <tr>
              <th>DESCRIÇÃO</th><th>TIPO</th><th>CATEGORIA</th>
              <th>DATA</th><th>VALOR</th><th>OBSERVAÇÕES</th><th>AÇÕES</th>
            </tr>
          </thead>
          <tbody id="historyBody"></tbody>
        </table>
        <div class="empty" id="emptyState" hidden>SEM TRANSAÇÕES NO MOMENTO. CLIQUE EM <strong>NOVA TRANSAÇÃO</strong> PARA COMEÇAR.</div>
      </div>
    </section>
  </div>

  <dialog id="transactionModal">
    <form method="dialog" id="transactionForm" novalidate>
      <div class="modal-head">
        <div><h2 id="modalTitle">NOVA TRANSAÇÃO</h2></div>
        <button class="btn btn-ghost" type="button" id="closeModalBtn">FECHAR</button>
      </div>
      <div class="form-grid">
        <div class="error-box" id="formError" hidden></div>
        <div class="field full"><label for="description">DESCRIÇÃO</label><input id="description" name="description" type="text" maxlength="80" placeholder="EX.: SALÁRIO, MERCADO, UBER" required /></div>
        <div class="field"><label for="amount">VALOR</label><input id="amount" name="amount" type="number" min="0.01" step="0.01" placeholder="0,00" required /></div>
        <div class="field"><label for="type">TIPO</label><select id="type" name="type" required><option value="entrada">ENTRADA</option><option value="saida">SAÍDA</option></select></div>
        <div class="field"><label for="dateDisplay">DATA</label><input id="dateDisplay" name="dateDisplay" type="text" inputmode="numeric" maxlength="8" placeholder="DDMMAA" required /></div>
        <div class="field"><label for="category">CATEGORIA</label><select id="category" name="category" required></select></div>
        <div class="field full"><label for="notes">OBSERVAÇÕES</label><textarea id="notes" name="notes" maxlength="240" placeholder="DETALHES ADICIONAIS DA TRANSAÇÃO"></textarea></div>
        <input type="hidden" id="editingId" name="editingId" />
        <div class="modal-actions">
          <button class="btn" type="button" id="cancelBtn">CANCELAR</button>
          <button class="btn btn-primary" type="submit">SALVAR</button>
        </div>
      </div>
    </form>
  </dialog>

  <script>
    const STORAGE_KEY = 'fluxon_pro_transactions_v2';
    const THEME_KEY = 'fluxon_pro_theme_v2';
    const RANGE_KEY = 'fluxon_pro_range_v2';
    const CHART_CDN = 'https://cdn.jsdelivr.net/npm/chart.js';
    const CATEGORIES = ['DESPESAS','MERCADO','FINANCEIRO','PET ESTIMAÇÃO','TRANSPORTE','MORADIA','EDUCAÇÃO','PESSOAL','TRABALHO','FREELANCER','OUTROS'];

    function pad(v) { return String(v).padStart(2,'0'); }
    function toISODate(d) { return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}`; }
    function todayISO() { return toISODate(new Date()); }
    function daysAgo(n) { const d=new Date(); d.setDate(d.getDate()-n); return toISODate(d); }
    function formatDateDisplayFromISO(iso) { if(!iso||!/^\d{4}-\d{2}-\d{2}$/.test(iso))return''; const[y,m,d]=iso.split('-'); return`${d}/${m}/${y.slice(-2)}`; }
    function parseDisplayDateToISO(v) { const dg=String(v||'').replace(/\D/g,''); if(dg.length!==6)return null; const dy=Number(dg.slice(0,2)),mo=Number(dg.slice(2,4)),yr=2000+Number(dg.slice(4,6)); const dt=new Date(yr,mo-1,dy); return(dt.getFullYear()===yr&&dt.getMonth()===mo-1&&dt.getDate()===dy)?toISODate(dt):null; }
    function maskDateInput(v) { const d=String(v||'').replace(/\D/g,'').slice(0,6); if(d.length<=2)return d; if(d.length<=4)return`${d.slice(0,2)}/${d.slice(2)}`; return`${d.slice(0,2)}/${d.slice(2,4)}/${d.slice(4)}`; }
    function startOfWeek(d) { const c=new Date(d),day=c.getDay(),diff=(day===0?-6:1)-day; c.setDate(c.getDate()+diff); c.setHours(0,0,0,0); return c; }
    function getWeekNumber(d) { const t=new Date(Date.UTC(d.getFullYear(),d.getMonth(),d.getDate())),dn=t.getUTCDay()||7; t.setUTCDate(t.getUTCDate()+4-dn); const ys=new Date(Date.UTC(t.getUTCFullYear(),0,1)); return Math.ceil((((t-ys)/86400000)+1)/7); }
    function formatBRL(v) { return new Intl.NumberFormat('pt-BR',{style:'currency',currency:'BRL'}).format(v); }
    function formatDate(iso) { return formatDateDisplayFromISO(iso); }
    function createId() { return window.crypto&&crypto.randomUUID?crypto.randomUUID():`id-${Date.now()}-${Math.random().toString(16).slice(2)}`; }
    function normalizeUpper(v) { return String(v||'').trim().toUpperCase(); }
    function escapeHTML(v) { return String(v).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;').replace(/'/g,'&#39;'); }

    const SAMPLE_TRANSACTIONS = [
      {id:createId(),description:'SALÁRIO MENSAL',type:'entrada',category:'TRABALHO',amount:4200,date:daysAgo(18),notes:'RECEBIMENTO FIXO'},
      {id:createId(),description:'FREELA DE DESIGN',type:'entrada',category:'FREELANCER',amount:850,date:daysAgo(6),notes:'PROJETO ENTREGUE'},
      {id:createId(),description:'COMPRA DO MERCADO',type:'saida',category:'MERCADO',amount:312.45,date:daysAgo(4),notes:'ITENS DA SEMANA'},
      {id:createId(),description:'CONTA DE ALUGUEL',type:'saida',category:'MORADIA',amount:1450,date:daysAgo(10),notes:'PAGAMENTO MENSAL'},
      {id:createId(),description:'CORRIDA POR APLICATIVO',type:'saida',category:'TRANSPORTE',amount:38.9,date:daysAgo(2),notes:'IDA AO CENTRO'},
      {id:createId(),description:'RESERVA FINANCEIRA',type:'entrada',category:'FINANCEIRO',amount:500,date:daysAgo(1),notes:'APORTE EXTRA'}
    ];

    function sanitizeTransaction(tx) {
      const iso=/^\d{4}-\d{2}-\d{2}$/.test(tx?.date||'')?tx.date:todayISO();
      return{id:tx?.id||createId(),description:normalizeUpper(tx?.description),type:tx?.type==='entrada'?'entrada':'saida',category:CATEGORIES.includes(normalizeUpper(tx?.category))?normalizeUpper(tx?.category):'OUTROS',amount:Number(tx?.amount)>0?Number(tx.amount):0,date:iso,notes:normalizeUpper(tx?.notes||'')};
    }
    function loadTransactions() {
      const s=localStorage.getItem(STORAGE_KEY);
      if(!s){const i=structuredClone?structuredClone(SAMPLE_TRANSACTIONS):JSON.parse(JSON.stringify(SAMPLE_TRANSACTIONS));localStorage.setItem(STORAGE_KEY,JSON.stringify(i));return i;}
      try{const p=JSON.parse(s);if(!Array.isArray(p))throw 0;return p.map(sanitizeTransaction).filter(t=>t.description&&t.amount>0);}
      catch{const f=structuredClone?structuredClone(SAMPLE_TRANSACTIONS):JSON.parse(JSON.stringify(SAMPLE_TRANSACTIONS));localStorage.setItem(STORAGE_KEY,JSON.stringify(f));return f;}
    }
    function saveTransactions(){localStorage.setItem(STORAGE_KEY,JSON.stringify(state.transactions));}
    function saveTheme(){localStorage.setItem(THEME_KEY,state.theme);}
    function saveRange(){localStorage.setItem(RANGE_KEY,state.range);}

    const state={theme:localStorage.getItem(THEME_KEY)||'dark',range:localStorage.getItem(RANGE_KEY)||'daily',transactions:loadTransactions(),pieChart:null,lineChart:null,chartReady:false,chartLoading:false,chartRequested:false};

    const els={pageLoader:document.getElementById('pageLoader'),incomeValue:document.getElementById('incomeValue'),expenseValue:document.getElementById('expenseValue'),balanceValue:document.getElementById('balanceValue'),balanceChip:document.getElementById('balanceChip'),countValue:document.getElementById('countValue'),historyBody:document.getElementById('historyBody'),emptyState:document.getElementById('emptyState'),transactionModal:document.getElementById('transactionModal'),transactionForm:document.getElementById('transactionForm'),modalTitle:document.getElementById('modalTitle'),themeToggle:document.getElementById('themeToggle'),newTransactionBtn:document.getElementById('newTransactionBtn'),closeModalBtn:document.getElementById('closeModalBtn'),cancelBtn:document.getElementById('cancelBtn'),resetSamplesBtn:document.getElementById('resetSamplesBtn'),rangeSwitcher:document.getElementById('rangeSwitcher'),pieCanvas:document.getElementById('pieChart'),lineCanvas:document.getElementById('lineChart'),piePlaceholder:document.getElementById('piePlaceholder'),linePlaceholder:document.getElementById('linePlaceholder'),categorySelect:document.getElementById('category'),description:document.getElementById('description'),type:document.getElementById('type'),amount:document.getElementById('amount'),dateDisplay:document.getElementById('dateDisplay'),notes:document.getElementById('notes'),editingId:document.getElementById('editingId'),formError:document.getElementById('formError'),chartSectionPie:document.getElementById('chartSectionPie'),chartSectionLine:document.getElementById('chartSectionLine')};

    function renderCategories(){els.categorySelect.innerHTML=CATEGORIES.map(c=>`<option value="${c}">${c}</option>`).join('');}
    function showLoaderDone(){document.body.classList.remove('loading');document.body.classList.add('loaded');}
    function applyTheme(){document.body.classList.toggle('dark',state.theme==='dark');els.themeToggle.textContent=state.theme==='dark'?'MODO CLARO':'MODO ESCURO';saveTheme();if(state.chartReady)renderCharts();}
    function setRangeButtons(){Array.from(els.rangeSwitcher.querySelectorAll('.range-btn')).forEach(b=>b.classList.toggle('active',b.dataset.range===state.range));}
    function getSignedAmount(tx){return tx.type==='entrada'?Number(tx.amount):-Number(tx.amount);}
    function renderSummary(){
      const inc=state.transactions.filter(t=>t.type==='entrada').reduce((s,t)=>s+Number(t.amount),0);
      const exp=state.transactions.filter(t=>t.type==='saida').reduce((s,t)=>s+Number(t.amount),0);
      const bal=inc-exp;
      els.incomeValue.textContent=formatBRL(inc);els.expenseValue.textContent=formatBRL(exp);els.balanceValue.textContent=formatBRL(bal);els.countValue.textContent=String(state.transactions.length);
      els.balanceChip.textContent=bal>=0?'SALDO POSITIVO':'SALDO NEGATIVO';els.balanceChip.className='chip '+(bal>=0?'income-chip':'expense-chip');
    }
    function renderHistory(){
      const items=[...state.transactions].sort((a,b)=>new Date(b.date)-new Date(a.date));
      els.historyBody.innerHTML='';els.emptyState.hidden=items.length>0;
      if(!items.length)return;
      for(const tx of items){const r=document.createElement('tr');r.innerHTML=`<td>${escapeHTML(tx.description)}</td><td><span class="type-badge type-${tx.type}">${tx.type==='entrada'?'ENTRADA':'SAÍDA'}</span></td><td><span class="category-badge">${escapeHTML(tx.category)}</span></td><td>${formatDate(tx.date)}</td><td class="${tx.type==='entrada'?'amount-positive':'amount-negative'}">${tx.type==='entrada'?'+':'-'} ${formatBRL(Number(tx.amount))}</td><td class="obs-cell">${escapeHTML(tx.notes||'-')}</td><td><div class="row-actions"><button class="mini-btn" data-action="edit" data-id="${tx.id}" type="button">EDITAR</button><button class="mini-btn btn-danger" data-action="delete" data-id="${tx.id}" type="button">EXCLUIR</button></div></td>`;els.historyBody.appendChild(r);}
    }
    function getThemeTextColor(){return getComputedStyle(document.body).getPropertyValue('--text').trim();}
    function getThemeLineColor(){return getComputedStyle(document.body).getPropertyValue('--line').trim();}
    function getPieData(){
      const totals=new Map();state.transactions.forEach(tx=>{const s=getSignedAmount(tx);totals.set(tx.category,(totals.get(tx.category)||0)+s);});
      const filtered=Array.from(totals.entries()).filter(([,v])=>Math.abs(v)>0);
      if(!filtered.length)return{labels:['SEM DADOS'],values:[1],colors:['rgba(124,58,237,0.35)']};
      const gp=['#18c964','#0ea95a','#0cbf7c','#41d889','#86efac'],rp=['#ff4d5e','#ef4444','#dc2626','#fb7185','#fca5a5'];
      return{labels:filtered.map(([l])=>l),values:filtered.map(([,v])=>Math.abs(v)),colors:filtered.map(([,v],i)=>v>=0?gp[i%gp.length]:rp[i%rp.length])};
    }
    function getRangeData(range){
      const now=new Date(),txs=[...state.transactions].sort((a,b)=>new Date(a.date)-new Date(b.date));let buckets=[];
      if(range==='daily'){for(let i=6;i>=0;i--){const d=new Date(now);d.setDate(now.getDate()-i);const k=toISODate(d);buckets.push({key:k,label:`${pad(d.getDate())}/${pad(d.getMonth()+1)}`,total:0});}txs.forEach(tx=>{const b=buckets.find(b=>b.key===tx.date);if(b)b.total+=getSignedAmount(tx);});}
      if(range==='weekly'){for(let i=7;i>=0;i--){const d=new Date(now);d.setDate(now.getDate()-i*7);const ws=startOfWeek(d),k=toISODate(ws);buckets.push({key:k,label:`SEM ${getWeekNumber(ws)}`,total:0});}txs.forEach(tx=>{const ws=toISODate(startOfWeek(new Date(`${tx.date}T00:00:00`))),b=buckets.find(b=>b.key===ws);if(b)b.total+=getSignedAmount(tx);});}
      if(range==='monthly'){for(let i=11;i>=0;i--){const d=new Date(now.getFullYear(),now.getMonth()-i,1),k=`${d.getFullYear()}-${pad(d.getMonth()+1)}`;buckets.push({key:k,label:new Intl.DateTimeFormat('pt-BR',{month:'short'}).format(d).replace('.','').toUpperCase(),total:0});}txs.forEach(tx=>{const d=new Date(`${tx.date}T00:00:00`),k=`${d.getFullYear()}-${pad(d.getMonth()+1)}`,b=buckets.find(b=>b.key===k);if(b)b.total+=getSignedAmount(tx);});}
      if(range==='yearly'){const yrs=new Set(txs.map(tx=>new Date(`${tx.date}T00:00:00`).getFullYear()));for(let i=4;i>=0;i--)yrs.add(now.getFullYear()-i);buckets=Array.from(yrs).sort((a,b)=>a-b).map(y=>({key:String(y),label:String(y),total:0}));txs.forEach(tx=>{const y=String(new Date(`${tx.date}T00:00:00`).getFullYear()),b=buckets.find(b=>b.key===y);if(b)b.total+=getSignedAmount(tx);});}
      return{labels:buckets.map(b=>b.label),values:buckets.map(b=>Number(b.total.toFixed(2)))};
    }
    function createAreaGradient(ctx,ca){const g=ctx.createLinearGradient(0,ca.top,0,ca.bottom);g.addColorStop(0,'rgba(24,201,100,0.30)');g.addColorStop(0.55,'rgba(124,58,237,0.12)');g.addColorStop(1,'rgba(255,77,94,0.22)');return g;}
    function revealChartCanvases(){els.piePlaceholder.hidden=true;els.linePlaceholder.hidden=true;els.pieCanvas.hidden=false;els.lineCanvas.hidden=false;}
    function ensureChartLibrary(){
      if(window.Chart){state.chartReady=true;revealChartCanvases();return Promise.resolve();}
      if(state.chartLoading){return new Promise((res,rej)=>{const t=setInterval(()=>{if(window.Chart){clearInterval(t);state.chartReady=true;revealChartCanvases();res();}},80);setTimeout(()=>{clearInterval(t);rej(new Error('TIMEOUT'));},12000);});}
      state.chartLoading=true;
      return new Promise((res,rej)=>{const s=document.createElement('script');s.src=CHART_CDN;s.async=true;s.onload=()=>{state.chartLoading=false;state.chartReady=true;revealChartCanvases();res();};s.onerror=()=>{state.chartLoading=false;rej(new Error('CHART_LOAD_FAILED'));};document.head.appendChild(s);});
    }
    async function requestChartsRender(){state.chartRequested=true;try{await ensureChartLibrary();renderCharts();}catch{els.piePlaceholder.hidden=false;els.linePlaceholder.hidden=false;els.piePlaceholder.innerHTML='<strong>FALHA AO CARREGAR O GRÁFICO</strong>';els.linePlaceholder.innerHTML='<strong>FALHA AO CARREGAR O GRÁFICO</strong>';}}
    function renderCharts(){
      if(!state.chartReady||!window.Chart)return;
      const tc=getThemeTextColor(),lc=getThemeLineColor(),pie=getPieData(),rd=getRangeData(state.range);
      if(state.pieChart)state.pieChart.destroy();
      state.pieChart=new Chart(els.pieCanvas,{type:'doughnut',data:{labels:pie.labels,datasets:[{data:pie.values,backgroundColor:pie.colors,borderColor:document.body.classList.contains('dark')?'#000000':'#ffffff',borderWidth:3,hoverOffset:10}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'bottom',labels:{color:tc,padding:18,usePointStyle:true,font:{weight:'700'}}},tooltip:{callbacks:{label:(c)=>`${c.label}: ${formatBRL(c.raw)}`}}},cutout:'62%'}});
      if(state.lineChart)state.lineChart.destroy();
      state.lineChart=new Chart(els.lineCanvas,{type:'line',data:{labels:rd.labels,datasets:[{label:'RESULTADO LÍQUIDO',data:rd.values,fill:true,tension:0.35,borderWidth:4,borderColor:'#18c964',pointRadius:5,pointHoverRadius:7,pointBorderWidth:2,pointBackgroundColor:rd.values.map(v=>v>=0?'#18c964':'#ff4d5e'),pointBorderColor:document.body.classList.contains('dark')?'#000000':'#ffffff',backgroundColor:(ctx)=>{const{ctx:c,chartArea:ca}=ctx.chart;if(!ca)return'rgba(24,201,100,0.18)';return createAreaGradient(c,ca);},segment:{borderColor:(ctx)=>(ctx.p0.parsed.y<0||ctx.p1.parsed.y<0)?'#ff4d5e':'#18c964'}}]},options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},plugins:{legend:{display:true,labels:{color:tc,usePointStyle:true,font:{weight:'700'}}},tooltip:{callbacks:{label:(c)=>`${c.dataset.label}: ${formatBRL(c.raw)}`}}},scales:{x:{ticks:{color:tc},grid:{color:lc}},y:{ticks:{color:tc,callback:(v)=>formatBRL(v)},grid:{color:lc}}}}});
    }
    function rerender(){renderSummary();renderHistory();setRangeButtons();saveTransactions();if(state.chartReady)renderCharts();else if(state.chartRequested)requestChartsRender();}
    function showError(msg=''){const t=normalizeUpper(msg);els.formError.textContent=t;els.formError.hidden=!t;}
    function openModal(tx=null){els.transactionForm.reset();showError('');els.dateDisplay.value=formatDateDisplayFromISO(todayISO());els.editingId.value='';els.modalTitle.textContent=tx?'EDITAR TRANSAÇÃO':'NOVA TRANSAÇÃO';if(tx){els.description.value=tx.description;els.amount.value=tx.amount;els.type.value=tx.type;els.dateDisplay.value=formatDateDisplayFromISO(tx.date);els.categorySelect.value=tx.category;els.notes.value=tx.notes||'';els.editingId.value=tx.id;}else{els.categorySelect.value=CATEGORIES[0];}els.transactionModal.showModal();setTimeout(()=>els.description.focus(),60);}
    function closeModal(){els.transactionModal.close();}
    function deleteTransaction(id){state.transactions=state.transactions.filter(t=>t.id!==id);rerender();}
    function editTransaction(id){const tx=state.transactions.find(t=>t.id===id);if(tx)openModal(tx);}
    function validatePayload(p){if(!p.description)return'INFORME A DESCRIÇÃO.';if(!(p.amount>0))return'INFORME UM VALOR MAIOR QUE ZERO.';if(!p.date)return'INFORME A DATA NO FORMATO DD/MM/AA.';if(!CATEGORIES.includes(p.category))return'SELECIONE UMA CATEGORIA VÁLIDA.';if(!['entrada','saida'].includes(p.type))return'SELECIONE UM TIPO VÁLIDO.';return'';}
    function setupChartLazyLoading(){
      const start=()=>requestChartsRender();
      if('IntersectionObserver' in window){const obs=new IntersectionObserver((entries)=>{if(entries.some(e=>e.isIntersecting)){obs.disconnect();start();}},{rootMargin:'180px'});obs.observe(els.chartSectionPie);obs.observe(els.chartSectionLine);}else setTimeout(start,450);
      if('requestIdleCallback' in window)requestIdleCallback(()=>{if(!state.chartReady)start();},{timeout:1800});else setTimeout(()=>{if(!state.chartReady)start();},1600);
    }

    els.newTransactionBtn.addEventListener('click',()=>openModal());
    els.closeModalBtn.addEventListener('click',closeModal);
    els.cancelBtn.addEventListener('click',closeModal);
    els.description.addEventListener('input',()=>{els.description.value=els.description.value.toUpperCase();});
    els.notes.addEventListener('input',()=>{els.notes.value=els.notes.value.toUpperCase();});
    els.dateDisplay.addEventListener('input',()=>{els.dateDisplay.value=maskDateInput(els.dateDisplay.value);});
    els.themeToggle.addEventListener('click',()=>{state.theme=state.theme==='dark'?'light':'dark';applyTheme();});
    els.rangeSwitcher.addEventListener('click',(e)=>{const b=e.target.closest('.range-btn');if(!b)return;state.range=b.dataset.range;saveRange();setRangeButtons();if(state.chartReady)renderCharts();else requestChartsRender();});
    els.historyBody.addEventListener('click',(e)=>{const b=e.target.closest('[data-action]');if(!b)return;const{action,id}=b.dataset;if(action==='edit')editTransaction(id);if(action==='delete')deleteTransaction(id);});
    els.transactionForm.addEventListener('submit',(e)=>{e.preventDefault();const isoDate=parseDisplayDateToISO(els.dateDisplay.value);const payload={id:els.editingId.value||createId(),description:normalizeUpper(els.description.value),amount:Number(els.amount.value),type:els.type.value,date:isoDate,category:normalizeUpper(els.categorySelect.value),notes:normalizeUpper(els.notes.value)};const err=validatePayload(payload);if(err){showError(err);return;}showError('');const idx=state.transactions.findIndex(t=>t.id===payload.id);if(idx>=0)state.transactions[idx]=payload;else state.transactions.push(payload);rerender();closeModal();});
    els.resetSamplesBtn.addEventListener('click',()=>{state.transactions=structuredClone?structuredClone(SAMPLE_TRANSACTIONS):JSON.parse(JSON.stringify(SAMPLE_TRANSACTIONS));rerender();});

    renderCategories();applyTheme();rerender();setupChartLazyLoading();
    window.addEventListener('load',()=>{setTimeout(showLoaderDone,120);});
  </script>
</body>
</html>
