<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>محفظة NovaCoin</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;700&family=Tajawal:wght@300;400;500;700;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #050810;
    --surface: #0c1120;
    --surface2: #111827;
    --border: #1e2d45;
    --accent: #00e5ff;
    --accent2: #7c3aed;
    --accent3: #10b981;
    --danger: #ef4444;
    --text: #e2e8f0;
    --muted: #64748b;
    --glow: 0 0 30px rgba(0,229,255,0.15);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Tajawal', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* GRID BACKGROUND */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,229,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,229,255,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* AURORA */
  body::after {
    content: '';
    position: fixed;
    top: -200px; left: -200px;
    width: 600px; height: 600px;
    background: radial-gradient(circle, rgba(124,58,237,0.12) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
    animation: drift 8s ease-in-out infinite alternate;
  }

  @keyframes drift {
    from { transform: translate(0,0); }
    to { transform: translate(100px, 80px); }
  }

  .app {
    position: relative;
    z-index: 1;
    max-width: 480px;
    margin: 0 auto;
    padding: 20px 16px 100px;
  }

  /* HEADER */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 0 24px;
  }

  .logo {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .logo-icon {
    width: 36px; height: 36px;
    border-radius: 10px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
    box-shadow: 0 0 20px rgba(0,229,255,0.3);
  }

  .logo-text {
    font-size: 20px;
    font-weight: 900;
    background: linear-gradient(135deg, var(--accent), #7c3aed);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: 1px;
  }

  .network-badge {
    background: rgba(16,185,129,0.1);
    border: 1px solid rgba(16,185,129,0.3);
    color: var(--accent3);
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 11px;
    font-family: 'IBM Plex Mono', monospace;
    display: flex; align-items: center; gap: 5px;
  }

  .dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--accent3);
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.3; }
  }

  /* WALLET CARD */
  .wallet-card {
    background: linear-gradient(135deg, #0f1f3d 0%, #1a1040 100%);
    border: 1px solid var(--border);
    border-radius: 24px;
    padding: 28px;
    margin-bottom: 20px;
    position: relative;
    overflow: hidden;
    box-shadow: var(--glow), 0 20px 60px rgba(0,0,0,0.5);
  }

  .wallet-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
    opacity: 0.6;
  }

  .wallet-card::after {
    content: '';
    position: absolute;
    top: -80px; right: -80px;
    width: 200px; height: 200px;
    background: radial-gradient(circle, rgba(0,229,255,0.08) 0%, transparent 70%);
    pointer-events: none;
  }

  .wallet-label {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 8px;
    font-family: 'IBM Plex Mono', monospace;
  }

  .wallet-address {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    color: var(--accent);
    background: rgba(0,229,255,0.06);
    border: 1px solid rgba(0,229,255,0.15);
    padding: 8px 12px;
    border-radius: 8px;
    margin-bottom: 28px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    transition: all 0.2s;
  }

  .wallet-address:hover {
    background: rgba(0,229,255,0.1);
    border-color: rgba(0,229,255,0.3);
  }

  .copy-icon { font-size: 14px; opacity: 0.6; }

  .balance-section {
    margin-bottom: 8px;
  }

  .balance-amount {
    font-size: 52px;
    font-weight: 900;
    line-height: 1;
    background: linear-gradient(135deg, #fff 40%, var(--accent) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 4px;
  }

  .balance-symbol {
    font-size: 22px;
    color: var(--muted);
    font-weight: 400;
  }

  .balance-usd {
    font-size: 14px;
    color: var(--muted);
    margin-bottom: 20px;
  }

  .balance-change {
    color: var(--accent3);
    font-size: 12px;
  }

  /* TOKEN INFO */
  .token-info {
    display: flex;
    gap: 12px;
    margin-bottom: 0;
  }

  .token-stat {
    flex: 1;
    background: rgba(255,255,255,0.03);
    border: 1px solid rgba(255,255,255,0.06);
    border-radius: 12px;
    padding: 12px;
    text-align: center;
  }

  .token-stat-label {
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-bottom: 4px;
  }

  .token-stat-value {
    font-size: 14px;
    font-weight: 700;
    color: var(--text);
    font-family: 'IBM Plex Mono', monospace;
  }

  /* ACTION BUTTONS */
  .actions {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 12px;
    margin-bottom: 24px;
  }

  .action-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 16px 8px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    cursor: pointer;
    transition: all 0.25s;
    color: var(--text);
    font-family: 'Tajawal', sans-serif;
    font-size: 13px;
    font-weight: 500;
  }

  .action-btn:hover {
    background: rgba(0,229,255,0.06);
    border-color: rgba(0,229,255,0.3);
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(0,0,0,0.3);
  }

  .action-icon {
    width: 40px; height: 40px;
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
  }

  .action-btn:nth-child(1) .action-icon { background: rgba(0,229,255,0.1); }
  .action-btn:nth-child(2) .action-icon { background: rgba(124,58,237,0.1); }
  .action-btn:nth-child(3) .action-icon { background: rgba(16,185,129,0.1); }

  /* MODALS */
  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(5,8,16,0.85);
    backdrop-filter: blur(8px);
    z-index: 100;
    align-items: flex-end;
    justify-content: center;
  }

  .modal-overlay.active { display: flex; }

  .modal {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 28px 28px 0 0;
    padding: 32px 24px 40px;
    width: 100%;
    max-width: 480px;
    animation: slideUp 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  }

  @keyframes slideUp {
    from { transform: translateY(100%); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
  }

  .modal-handle {
    width: 40px; height: 4px;
    background: var(--border);
    border-radius: 2px;
    margin: 0 auto 24px;
  }

  .modal-title {
    font-size: 22px;
    font-weight: 700;
    margin-bottom: 24px;
  }

  .input-group {
    margin-bottom: 16px;
  }

  .input-label {
    font-size: 12px;
    color: var(--muted);
    margin-bottom: 8px;
    display: block;
    letter-spacing: 1px;
  }

  .input-field {
    width: 100%;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 14px 16px;
    color: var(--text);
    font-size: 16px;
    font-family: 'IBM Plex Mono', monospace;
    outline: none;
    transition: border-color 0.2s;
    direction: ltr;
    text-align: left;
  }

  .input-field:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(0,229,255,0.08);
  }

  .input-field::placeholder { color: var(--muted); }

  .submit-btn {
    width: 100%;
    padding: 16px;
    border-radius: 14px;
    border: none;
    font-size: 16px;
    font-weight: 700;
    font-family: 'Tajawal', sans-serif;
    cursor: pointer;
    transition: all 0.25s;
    margin-top: 8px;
  }

  .submit-btn.send {
    background: linear-gradient(135deg, var(--accent2), var(--accent));
    color: #000;
  }

  .submit-btn.receive {
    background: linear-gradient(135deg, var(--accent3), #059669);
    color: #000;
  }

  .submit-btn.mint {
    background: linear-gradient(135deg, #f59e0b, #ef4444);
    color: #000;
  }

  .submit-btn:hover { transform: translateY(-1px); filter: brightness(1.1); }
  .submit-btn:active { transform: scale(0.98); }

  /* TRANSACTIONS */
  .section-title {
    font-size: 13px;
    color: var(--muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 14px;
    font-family: 'IBM Plex Mono', monospace;
  }

  .tx-list { display: flex; flex-direction: column; gap: 10px; }

  .tx-item {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 14px 16px;
    display: flex;
    align-items: center;
    gap: 14px;
    animation: fadeIn 0.4s ease;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(8px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .tx-icon {
    width: 40px; height: 40px;
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
    flex-shrink: 0;
  }

  .tx-icon.send { background: rgba(239,68,68,0.1); }
  .tx-icon.receive { background: rgba(16,185,129,0.1); }
  .tx-icon.mint { background: rgba(245,158,11,0.1); }

  .tx-info { flex: 1; min-width: 0; }

  .tx-desc {
    font-size: 14px;
    font-weight: 500;
    margin-bottom: 2px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .tx-addr {
    font-size: 11px;
    color: var(--muted);
    font-family: 'IBM Plex Mono', monospace;
  }

  .tx-amount {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 14px;
    font-weight: 700;
    text-align: left;
  }

  .tx-amount.send { color: var(--danger); }
  .tx-amount.receive { color: var(--accent3); }
  .tx-amount.mint { color: #f59e0b; }

  /* RECEIVE */
  .qr-wrapper {
    display: flex; flex-direction: column; align-items: center; gap: 16px;
    margin: 20px 0;
  }

  .qr-box {
    width: 180px; height: 180px;
    background: white;
    border-radius: 16px;
    display: flex; align-items: center; justify-content: center;
    font-size: 60px;
    box-shadow: 0 0 40px rgba(0,229,255,0.2);
  }

  .qr-address {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    text-align: center;
    word-break: break-all;
    background: rgba(0,229,255,0.06);
    padding: 10px;
    border-radius: 10px;
    border: 1px solid rgba(0,229,255,0.15);
    width: 100%;
  }

  /* CLOSE BTN */
  .close-btn {
    position: absolute;
    top: 16px; left: 20px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    width: 32px; height: 32px;
    display: flex; align-items: center; justify-content: center;
    cursor: pointer;
    color: var(--muted);
    font-size: 18px;
    transition: all 0.2s;
  }

  .close-btn:hover { color: var(--text); background: var(--border); }

  .modal { position: relative; }

  /* TOAST */
  .toast {
    position: fixed;
    bottom: 100px; left: 50%; transform: translateX(-50%) translateY(20px);
    background: var(--surface);
    border: 1px solid var(--accent3);
    color: var(--accent3);
    padding: 12px 24px;
    border-radius: 50px;
    font-size: 14px;
    z-index: 200;
    opacity: 0;
    transition: all 0.3s;
    pointer-events: none;
    white-space: nowrap;
  }

  .toast.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }

  /* CHART */
  .chart-section {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 18px;
    padding: 20px;
    margin-bottom: 20px;
  }

  .chart-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
  }

  .chart-title { font-size: 14px; color: var(--muted); }
  .chart-value { font-size: 16px; font-weight: 700; color: var(--accent3); }

  canvas#priceChart { width: 100% !important; height: 80px !important; }

  /* BOTTOM NAV */
  .bottom-nav {
    position: fixed;
    bottom: 0; left: 0; right: 0;
    background: rgba(12,17,32,0.95);
    backdrop-filter: blur(20px);
    border-top: 1px solid var(--border);
    display: flex;
    justify-content: space-around;
    padding: 12px 0 20px;
    z-index: 50;
  }

  .nav-item {
    display: flex; flex-direction: column; align-items: center; gap: 4px;
    cursor: pointer;
    color: var(--muted);
    font-size: 11px;
    transition: color 0.2s;
    font-family: 'Tajawal', sans-serif;
  }

  .nav-item.active { color: var(--accent); }
  .nav-icon { font-size: 22px; }

  /* TOKEN CREATION */
  .token-create-section {
    background: linear-gradient(135deg, rgba(124,58,237,0.1), rgba(0,229,255,0.05));
    border: 1px solid rgba(124,58,237,0.3);
    border-radius: 18px;
    padding: 20px;
    margin-bottom: 20px;
  }

  .badge {
    display: inline-block;
    background: rgba(124,58,237,0.2);
    color: #a78bfa;
    font-size: 10px;
    padding: 3px 8px;
    border-radius: 4px;
    letter-spacing: 1px;
    margin-bottom: 8px;
    font-family: 'IBM Plex Mono', monospace;
  }

  .fee-note {
    font-size: 12px;
    color: var(--muted);
    margin-top: 12px;
    padding: 10px;
    background: rgba(255,255,255,0.02);
    border-radius: 8px;
    line-height: 1.6;
  }
</style>
</head>
<body>

<div class="app">
  <!-- HEADER -->
  <div class="header">
    <div class="logo">
      <div class="logo-icon">◈</div>
      <div class="logo-text">NovaCoin</div>
    </div>
    <div class="network-badge">
      <div class="dot"></div>
      Solana Devnet
    </div>
  </div>

  <!-- WALLET CARD -->
  <div class="wallet-card">
    <div class="wallet-label">عنوان المحفظة</div>
    <div class="wallet-address" onclick="copyAddress()">
      <span id="short-addr">7xKp...mN9q</span>
      <span class="copy-icon">⎘</span>
    </div>

    <div class="balance-section">
      <div class="balance-amount">
        <span id="balance">10,000</span>
        <span class="balance-symbol"> NVC</span>
      </div>
      <div class="balance-usd">≈ $<span id="usd-val">2,450.00</span> USD
        <span class="balance-change">▲ +4.2%</span>
      </div>
    </div>

    <div class="token-info">
      <div class="token-stat">
        <div class="token-stat-label">السعر</div>
        <div class="token-stat-value" id="price">$0.245</div>
      </div>
      <div class="token-stat">
        <div class="token-stat-label">الإمداد</div>
        <div class="token-stat-value">1M NVC</div>
      </div>
      <div class="token-stat">
        <div class="token-stat-label">الرسوم</div>
        <div class="token-stat-value">0.00025◎</div>
      </div>
    </div>
  </div>

  <!-- ACTION BUTTONS -->
  <div class="actions">
    <button class="action-btn" onclick="openModal('send')">
      <div class="action-icon">↑</div>
      إرسال
    </button>
    <button class="action-btn" onclick="openModal('receive')">
      <div class="action-icon">↓</div>
      استقبال
    </button>
    <button class="action-btn" onclick="openModal('mint')">
      <div class="action-icon">⊕</div>
      سك عملة
    </button>
  </div>

  <!-- PRICE CHART -->
  <div class="chart-section">
    <div class="chart-header">
      <div class="chart-title">سعر NVC (7 أيام)</div>
      <div class="chart-value">$0.245 ▲</div>
    </div>
    <canvas id="priceChart"></canvas>
  </div>

  <!-- TOKEN INFO SECTION -->
  <div class="token-create-section">
    <div class="badge">SPL TOKEN</div>
    <div style="font-size:16px; font-weight:700; margin-bottom:6px;">عملة NovaCoin (NVC)</div>
    <div style="font-size:13px; color:var(--muted); line-height:1.7;">
      عملة رقمية مبنية على شبكة Solana باستخدام معيار SPL Token.
      سريعة، آمنة، ورسوم معاملات منخفضة جداً (~$0.0005 لكل معاملة).
    </div>
    <div class="fee-note">
      🔗 <strong>عنوان العقد الذكي:</strong><br>
      <span style="font-family:'IBM Plex Mono',monospace; font-size:11px; color:var(--accent);">
        NVC1...DEMO...TOKEN...ADDRESS
      </span>
    </div>
  </div>

  <!-- TRANSACTIONS -->
  <div class="section-title">آخر المعاملات</div>
  <div class="tx-list" id="tx-list">
    <div class="tx-item">
      <div class="tx-icon mint">✦</div>
      <div class="tx-info">
        <div class="tx-desc">سك العملة الأولية</div>
        <div class="tx-addr">Genesis Mint</div>
      </div>
      <div class="tx-amount mint">+1,000,000</div>
    </div>
    <div class="tx-item">
      <div class="tx-icon receive">↓</div>
      <div class="tx-info">
        <div class="tx-desc">استقبال NVC</div>
        <div class="tx-addr">3aX7...pQ2r</div>
      </div>
      <div class="tx-amount receive">+500</div>
    </div>
    <div class="tx-item">
      <div class="tx-icon send">↑</div>
      <div class="tx-info">
        <div class="tx-desc">إرسال NVC</div>
        <div class="tx-addr">9mKj...wL5s</div>
      </div>
      <div class="tx-amount send">-200</div>
    </div>
  </div>
</div>

<!-- BOTTOM NAV -->
<div class="bottom-nav">
  <div class="nav-item active">
    <div class="nav-icon">◈</div>
    محفظة
  </div>
  <div class="nav-item">
    <div class="nav-icon">⟆</div>
    تداول
  </div>
  <div class="nav-item">
    <div class="nav-icon">◉</div>
    NFT
  </div>
  <div class="nav-item">
    <div class="nav-icon">⚙</div>
    إعدادات
  </div>
</div>

<!-- SEND MODAL -->
<div class="modal-overlay" id="modal-send">
  <div class="modal">
    <div class="close-btn" onclick="closeModal('send')">×</div>
    <div class="modal-handle"></div>
    <div class="modal-title">↑ إرسال NVC</div>
    <div class="input-group">
      <label class="input-label">عنوان المستلم</label>
      <input class="input-field" id="send-addr" type="text" placeholder="أدخل عنوان Solana...">
    </div>
    <div class="input-group">
      <label class="input-label">الكمية</label>
      <input class="input-field" id="send-amount" type="number" placeholder="0.00" min="1">
    </div>
    <div style="font-size:12px; color:var(--muted); margin-bottom:16px;">
      رصيدك: <span id="modal-balance" style="color:var(--accent)">10,000 NVC</span>
    </div>
    <button class="submit-btn send" onclick="sendTokens()">تأكيد الإرسال</button>
  </div>
</div>

<!-- RECEIVE MODAL -->
<div class="modal-overlay" id="modal-receive">
  <div class="modal">
    <div class="close-btn" onclick="closeModal('receive')">×</div>
    <div class="modal-handle"></div>
    <div class="modal-title">↓ استقبال NVC</div>
    <div class="qr-wrapper">
      <div class="qr-box">⊞</div>
      <div class="qr-address" id="full-addr">7xKpBz3mFqRn8wEcAhJkTv2sYdXpLgN9q</div>
    </div>
    <button class="submit-btn receive" onclick="copyAddress(); closeModal('receive')">نسخ العنوان</button>
  </div>
</div>

<!-- MINT MODAL -->
<div class="modal-overlay" id="modal-mint">
  <div class="modal">
    <div class="close-btn" onclick="closeModal('mint')">×</div>
    <div class="modal-handle"></div>
    <div class="modal-title">⊕ سك عملة جديدة</div>
    <div class="input-group">
      <label class="input-label">كمية السك</label>
      <input class="input-field" id="mint-amount" type="number" placeholder="أدخل الكمية..." min="1">
    </div>
    <div style="font-size:12px; color:var(--muted); margin-bottom:16px; line-height:1.6;">
      ⚠️ صلاحية السك متاحة فقط لصاحب العقد الذكي<br>
      رسوم الشبكة: ~0.00025 SOL
    </div>
    <button class="submit-btn mint" onclick="mintTokens()">سك العملة</button>
  </div>
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<script>
let balance = 10000;
const WALLET_ADDR = "7xKpBz3mFqRn8wEcAhJkTv2sYdXpLgN9q";
const PRICE = 0.245;

function updateDisplay() {
  document.getElementById('balance').textContent = balance.toLocaleString();
  document.getElementById('usd-val').textContent = (balance * PRICE).toFixed(2);
  document.getElementById('modal-balance').textContent = balance.toLocaleString() + ' NVC';
}

function openModal(type) {
  document.getElementById('modal-' + type).classList.add('active');
}

function closeModal(type) {
  document.getElementById('modal-' + type).classList.remove('active');
}

document.querySelectorAll('.modal-overlay').forEach(el => {
  el.addEventListener('click', e => {
    if (e.target === el) el.classList.remove('active');
  });
});

function showToast(msg, type = 'success') {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.style.borderColor = type === 'error' ? 'var(--danger)' : 'var(--accent3)';
  t.style.color = type === 'error' ? 'var(--danger)' : 'var(--accent3)';
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3000);
}

function copyAddress() {
  navigator.clipboard?.writeText(WALLET_ADDR).catch(() => {});
  showToast('✓ تم نسخ العنوان');
}

function addTx(type, desc, addr, amount) {
  const list = document.getElementById('tx-list');
  const icons = { send: '↑', receive: '↓', mint: '✦' };
  const labels = { send: 'إرسال NVC', receive: 'استقبال NVC', mint: 'سك عملة' };
  const amts = { send: '-' + amount, receive: '+' + amount, mint: '+' + amount };

  const el = document.createElement('div');
  el.className = 'tx-item';
  el.innerHTML = `
    <div class="tx-icon ${type}">${icons[type]}</div>
    <div class="tx-info">
      <div class="tx-desc">${labels[type]}</div>
      <div class="tx-addr">${addr}</div>
    </div>
    <div class="tx-amount ${type}">${amts[type].toLocaleString()}</div>
  `;
  list.insertBefore(el, list.firstChild);
}

function sendTokens() {
  const addr = document.getElementById('send-addr').value.trim();
  const amount = parseInt(document.getElementById('send-amount').value);

  if (!addr) { showToast('⚠ أدخل عنوان المستلم', 'error'); return; }
  if (!amount || amount <= 0) { showToast('⚠ أدخل كمية صحيحة', 'error'); return; }
  if (amount > balance) { showToast('⚠ رصيد غير كافٍ', 'error'); return; }

  balance -= amount;
  updateDisplay();
  addTx('send', '', addr.slice(0,8) + '...' + addr.slice(-4), amount.toLocaleString());
  showToast(`✓ تم إرسال ${amount.toLocaleString()} NVC`);
  closeModal('send');
  document.getElementById('send-addr').value = '';
  document.getElementById('send-amount').value = '';
}

function mintTokens() {
  const amount = parseInt(document.getElementById('mint-amount').value);
  if (!amount || amount <= 0) { showToast('⚠ أدخل كمية صحيحة', 'error'); return; }

  balance += amount;
  updateDisplay();
  addTx('mint', '', 'Mint Authority', amount.toLocaleString());
  showToast(`✓ تم سك ${amount.toLocaleString()} NVC`);
  closeModal('mint');
  document.getElementById('mint-amount').value = '';
}

// PRICE CHART
function drawChart() {
  const canvas = document.getElementById('priceChart');
  const ctx = canvas.getContext('2d');
  canvas.width = canvas.offsetWidth;
  canvas.height = 80;

  const prices = [0.18, 0.21, 0.19, 0.23, 0.22, 0.24, 0.245];
  const w = canvas.width, h = canvas.height;
  const min = Math.min(...prices) * 0.98;
  const max = Math.max(...prices) * 1.02;
  const step = w / (prices.length - 1);

  const pts = prices.map((p, i) => ({
    x: i * step,
    y: h - ((p - min) / (max - min)) * h
  }));

  // Gradient fill
  const grad = ctx.createLinearGradient(0, 0, 0, h);
  grad.addColorStop(0, 'rgba(0,229,255,0.25)');
  grad.addColorStop(1, 'rgba(0,229,255,0)');

  ctx.beginPath();
  ctx.moveTo(pts[0].x, pts[0].y);
  for (let i = 1; i < pts.length; i++) {
    const cx = (pts[i-1].x + pts[i].x) / 2;
    ctx.bezierCurveTo(cx, pts[i-1].y, cx, pts[i].y, pts[i].x, pts[i].y);
  }
  ctx.lineTo(w, h); ctx.lineTo(0, h); ctx.closePath();
  ctx.fillStyle = grad;
  ctx.fill();

  // Line
  ctx.beginPath();
  ctx.moveTo(pts[0].x, pts[0].y);
  for (let i = 1; i < pts.length; i++) {
    const cx = (pts[i-1].x + pts[i].x) / 2;
    ctx.bezierCurveTo(cx, pts[i-1].y, cx, pts[i].y, pts[i].x, pts[i].y);
  }
  ctx.strokeStyle = '#00e5ff';
  ctx.lineWidth = 2;
  ctx.stroke();

  // Last point dot
  const last = pts[pts.length - 1];
  ctx.beginPath();
  ctx.arc(last.x, last.y, 5, 0, Math.PI * 2);
  ctx.fillStyle = '#00e5ff';
  ctx.fill();
  ctx.beginPath();
  ctx.arc(last.x, last.y, 9, 0, Math.PI * 2);
  ctx.strokeStyle = 'rgba(0,229,255,0.3)';
  ctx.lineWidth = 2;
  ctx.stroke();
}

window.addEventListener('load', drawChart);
window.addEventListener('resize', drawChart);
</script>
</body>
</html>
