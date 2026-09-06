# smart-sf6-prototype-
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart SF6 — Prototype</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Sans+Thai:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root{
    /* Light & Yellow EGAT Theme */
    --dk-bg:#F4F6F8; --dk-surface:#FFFFFF; --dk-surface2:#F9FAFB; --dk-border:#E4E7EB;
    --dk-text:#1B2430; --dk-muted:#6B7580; --dk-teal:#F2A900; --dk-amber:#F0A93E; --dk-red:#E5484D;
    --mb-bg:#F4F6F8; --mb-surface:#FFFFFF; --mb-border:#E4E7EB; --mb-text:#1B2430; --mb-muted:#6B7580;
    --mb-orange:#F2A900; --mb-orange-dk:#D99700; --mb-green:#2F9E68; --mb-amber:#E2A93B; --mb-red:#D64545;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  body{
    font-family:'IBM Plex Sans','IBM Plex Sans Thai',sans-serif;
    background:var(--dk-bg); color:var(--dk-text);
    min-height:100vh;
  }
  h1,h2,h3,.display{font-family:'Space Grotesk','IBM Plex Sans Thai',sans-serif;}
  .mono{font-family:'IBM Plex Mono',monospace;}

  /* ---------- top switcher ---------- */
  .switcher-bar{
    display:flex; align-items:center; justify-content:space-between;
    padding:14px 28px; background:var(--dk-surface); border-bottom:1px solid var(--dk-border);
    position:sticky; top:0; z-index:50;
  }
  .switcher-title{display:flex; align-items:center; gap:10px; font-size:14px; color:#6B7684; font-weight:600;}
  .switcher-title .dot{width:8px;height:8px;border-radius:50%;background:var(--dk-teal);}
  
  .top-actions {display:flex; align-items:center; gap:16px;}
  
  .lang-toggle {
    background: #FFF4D6; color: #D99700; border: 1px solid #F2A900; border-radius: 8px;
    padding: 6px 12px; font-size: 13px; font-weight: 700; cursor: pointer; transition: 0.2s;
  }
  .lang-toggle:hover { background: #F2A900; color: #fff; }

  .tabs{display:flex; gap:4px; background:var(--dk-bg); border:1px solid var(--dk-border); border-radius:10px; padding:4px;}
  .tab-btn{
    font-family:'Space Grotesk',sans-serif; font-size:13px; font-weight:600; letter-spacing:.02em;
    color:var(--dk-muted); background:transparent; border:none; padding:8px 18px; border-radius:7px; cursor:pointer;
    transition:background .15s, color .15s;
  }
  .tab-btn.active{background:var(--dk-teal); color:#fff;}

  .view{display:none; animation:fadein .25s ease;}
  .view.active{display:block;}
  @keyframes fadein{from{opacity:0; transform:translateY(4px);} to{opacity:1; transform:translateY(0);}}

  /* =========================================================
     WEB DASHBOARD
     ========================================================= */
  #dashboard{ background:var(--dk-bg); min-height:calc(100vh - 57px); }
  .dash-shell{ display:grid; grid-template-columns:230px 1fr; min-height:calc(100vh - 57px);}
  .sidebar{
    background:var(--dk-surface); border-right:1px solid var(--dk-border); padding:24px 16px;
    display:flex; flex-direction:column; gap:28px;
  }
  .brand{display:flex; flex-direction:column; gap:2px; padding:0 8px;}
  .brand-mark{display:flex; align-items:center; gap:9px;}
  .brand-icon{width:30px;height:30px;border-radius:8px;background:linear-gradient(135deg,#FFC000,#F2A900); display:flex;align-items:center;justify-content:center; font-weight:700; color:#fff; font-size:14px;}
  .brand-name{font-size:15px; font-weight:600;}
  .nav-group{display:flex; flex-direction:column; gap:2px;}
  .nav-label{font-size:10.5px; text-transform:uppercase; letter-spacing:.08em; color:var(--dk-muted); padding:0 12px 6px; font-weight:600;}
  .nav-item{
    display:flex; align-items:center; gap:10px; padding:9px 12px; border-radius:8px; font-size:13.5px;
    color:var(--dk-muted); cursor:pointer; transition:background .15s,color .15s;
  }
  .nav-item svg{width:16px;height:16px; opacity:.85; flex-shrink:0;}
  .nav-item.active{background:#FFF4D6; color:#D99700;}
  .nav-item.active svg{opacity:1; color:var(--dk-teal);}
  .nav-item:not(.active):hover{background:var(--dk-bg); color:var(--dk-text);}

  .dash-main{padding:24px 32px 48px; overflow-x:hidden;}
  .dash-panel{display:none;}
  .dash-panel.active{display:block; animation:fadein .2s ease;}
  .dash-header{display:flex; align-items:center; justify-content:space-between; margin-bottom:22px; flex-wrap:wrap; gap:12px;}
  .dash-header h1{font-size:22px; font-weight:600;}
  .dash-header .subtitle{font-size:13px; color:var(--dk-muted); margin-top:3px;}
  .chip{padding:7px 14px; border-radius:8px; border:1px solid var(--dk-border); background:var(--dk-surface); font-size:12.5px; color:var(--dk-muted); cursor:pointer;}
  .chip.active{background:var(--dk-teal); color:#fff; border-color:var(--dk-teal); font-weight:600;}

  .alert-banner{
    display:flex; align-items:center; gap:12px; background:rgba(229,72,77,.12); border:1px solid rgba(229,72,77,.35);
    color:#D64545; padding:12px 16px; border-radius:10px; font-size:13px; margin-bottom:22px;
  }
  .alert-banner b{color:#E5484D;}

  .kpi-row{display:grid; grid-template-columns:repeat(4,1fr); gap:16px; margin-bottom:22px;}
  .kpi-card{background:var(--dk-surface); border:1px solid var(--dk-border); border-radius:14px; padding:18px 18px 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.02);}
  .kpi-label{font-size:12px; color:var(--dk-muted); display:flex; align-items:center; justify-content:space-between;}
  .kpi-trend{font-size:11px; padding:2px 7px; border-radius:20px; font-weight:600;}
  .kpi-trend.up{background:rgba(242,169,0,.15); color:var(--dk-teal);}
  .kpi-value{font-size:26px; font-weight:600; margin-top:10px; font-family:'IBM Plex Mono',monospace;}
  .kpi-unit{font-size:13px; color:var(--dk-muted); font-weight:400; margin-left:4px;}
  
  /* Mobile styling (simplified for brevity) */
  #mobile{ background:var(--dk-bg); min-height:calc(100vh - 57px); display:flex; align-items:flex-start; justify-content:center; gap:48px; padding:48px 24px; flex-wrap:wrap;}
  .phone{width:375px; height:780px; background:var(--mb-bg); border-radius:44px; border:10px solid #DDE1DA; box-shadow:0 20px 40px rgba(0,0,0,.08); position:relative; overflow:hidden;}
</style>
</head>
<body>

<div class="switcher-bar">
  <div class="switcher-title"><span class="dot"></span> <span data-i18n="app_title">Smart SF6 Leakage &amp; Regeneration</span></div>
  
  <div class="top-actions">
    <button id="langToggleBtn" class="lang-toggle" onclick="toggleLanguage()">EN</button>
    
    <div class="tabs">
      <button class="tab-btn active" data-view="dashboard">Web Dashboard</button>
      <button class="tab-btn" data-view="mobile">Mobile App</button>
    </div>
  </div>
</div>

<div class="view active" id="dashboard">
<div class="dash-shell">
  <aside class="sidebar">
    <div class="brand">
      <div class="brand-mark">
        <div class="brand-icon">S6</div>
        <div class="brand-name">Smart SF6</div>
      </div>
    </div>

    <div class="nav-group">
      <div class="nav-label" data-i18n="menu_overview">Overview</div>
      <div class="nav-item active" data-panel="overview">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg>
        <span data-i18n="menu_dashboard">Dashboard</span>
      </div>
      <div class="nav-item" data-panel="assets">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 20l-5.5-2V6L9 8m0 12l6-2m-6 2V8m6 10l5.5 2V8L15 6m0 12V6m0 0L9 8"/></svg>
        <span data-i18n="menu_assets">Asset / Station</span>
      </div>
    </div>
  </aside>

  <main class="dash-main">
  <div class="dash-panel active" data-panel="overview">
    <div class="dash-header">
      <div>
        <h1 data-i18n="header_title">ภาพรวมองค์กร — Carbon &amp; Predictive Insight</h1>
        <div class="subtitle" data-i18n="header_sub">อัปเดตล่าสุด 06 ก.ย. 2569 · ข้อมูลจาก 24 สถานี</div>
      </div>
    </div>

    <div class="alert-banner">
      ⚠️ <span data-i18n="alert_msg"><b>ความเสี่ยงสูง 2 จุด</b> คาดการณ์ความดันจะต่ำกว่าเกณฑ์ปลอดภัยภายใน 7 วัน</span>
    </div>

    <div class="kpi-row">
      <div class="kpi-card">
        <div class="kpi-label"><span data-i18n="kpi_1">SF6 กู้คืนสะสม</span> <span class="kpi-trend up">+8.2%</span></div>
        <div class="kpi-value">142.6<span class="kpi-unit">kg</span></div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label"><span data-i18n="kpi_2">เทียบเท่าคาร์บอน</span> <span class="kpi-trend up">+8.2%</span></div>
        <div class="kpi-value">3,351<span class="kpi-unit">tCO2e</span></div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label" data-i18n="kpi_3">มูลค่าคาร์บอนเครดิต</div>
        <div class="kpi-value">335,050<span class="kpi-unit">THB</span></div>
      </div>
    </div>

  </div>
  </main>
</div>
</div>

<div class="view" id="mobile">
  <div class="phone">
    <div class="phone-notch"></div>
    <div style="padding: 60px 20px; text-align: center; color: #6B7580;">
      <h3 data-i18n="mobile_placeholder">หน้าจอ Mobile App</h3>
      <p style="font-size: 13px; margin-top: 10px;" data-i18n="mobile_desc">ระบบเปลี่ยนภาษาครอบคลุมถึงหน้าจอนี้เช่นกัน</p>
    </div>
  </div>
</div>

<script>
  // 1. View Switcher Logic
  document.querySelectorAll('.tab-btn').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
      document.getElementById(btn.dataset.view).classList.add('active');
    });
  });

  // 2. Language Dictionary (พจนานุกรมคำศัพท์)
  const translations = {
    "th": {
      "app_title": "Smart SF6 Leakage & Regeneration",
      "menu_overview": "ภาพรวม",
      "menu_dashboard": "แดชบอร์ด",
      "menu_assets": "สินทรัพย์ / สถานี",
      "header_title": "ภาพรวมองค์กร — ข้อมูลคาร์บอนและการพยากรณ์",
      "header_sub": "อัปเดตล่าสุด 06 ก.ย. 2569 · ข้อมูลจาก 24 สถานี",
      "alert_msg": "<b>ความเสี่ยงสูง 2 จุด</b> คาดการณ์ความดันจะต่ำกว่าเกณฑ์ปลอดภัยภายใน 7 วัน",
      "kpi_1": "SF6 กู้คืนสะสม",
      "kpi_2": "ลดคาร์บอนเทียบเท่า",
      "kpi_3": "มูลค่าคาร์บอนเครดิต",
      "mobile_placeholder": "หน้าจอ Mobile App",
      "mobile_desc": "ระบบเปลี่ยนภาษาครอบคลุมถึงหน้าจอนี้เช่นกัน"
    },
    "en": {
      "app_title": "Smart SF6 Leakage & Regeneration",
      "menu_overview": "Overview",
      "menu_dashboard": "Dashboard",
      "menu_assets": "Assets / Stations",
      "header_title": "Corporate Overview — Carbon & Predictive Insights",
      "header_sub": "Last updated Sep 06, 2026 · Data from 24 stations",
      "alert_msg": "<b>2 High Risks Detected</b> Pressure expected to drop below safe threshold within 7 days",
      "kpi_1": "Total SF6 Recovered",
      "kpi_2": "Carbon Equivalent",
      "kpi_3": "Carbon Credit Value",
      "mobile_placeholder": "Mobile App Preview",
      "mobile_desc": "The language toggle applies seamlessly to this screen as well."
    }
  };

  // 3. Language Toggle Logic
  let currentLang = "th";

  function toggleLanguage() {
    // สลับตัวแปรภาษา
    currentLang = currentLang === "th" ? "en" : "th";
    
    // เปลี่ยนข้อความบนปุ่ม
    const btn = document.getElementById('langToggleBtn');
    btn.innerText = currentLang === "th" ? "EN" : "TH";

    // ค้นหาและเปลี่ยนข้อความทั้งหมดที่มี attribute data-i18n
    const elements = document.querySelectorAll('[data-i18n]');
    elements.forEach(el => {
      const key = el.getAttribute('data-i18n');
      if (translations[currentLang][key]) {
        el.innerHTML = translations[currentLang][key]; // ใช้ innerHTML เพื่อรองรับ tag <b>
      }
    });
  }
</script>
</body>
</html>
