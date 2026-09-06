# smart-sf6-prototype-
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart SF6 — Minimal Prototype</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Sans+Thai:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root{
    /* Minimal Modern Theme */
    --bg-page: #F8FAFC; 
    --bg-card: #FFFFFF;
    --border-light: #F1F5F9;
    --border-dark: #E2E8F0;
    
    /* Typography */
    --text-main: #0F172A; 
    --text-muted: #64748B;
    --text-light: #94A3B8;
    
    /* Accents (EGAT Yellow adapted for minimal design) */
    --accent-main: #F59E0B;
    --accent-light: #FEF3C7;
    --accent-hover: #D97706;

    /* Status */
    --success: #10B981; --success-bg: #D1FAE5;
    --warning: #F59E0B; --warning-bg: #FEF3C7;
    --danger: #EF4444; --danger-bg: #FEE2E2;
  }
  
  *{box-sizing:border-box; margin:0; padding:0;}
  body{
    font-family:'IBM Plex Sans','IBM Plex Sans Thai',sans-serif;
    background:var(--bg-page); color:var(--text-main);
    min-height:100vh;
  }
  h1,h2,h3{font-family:'Space Grotesk','IBM Plex Sans Thai',sans-serif;}
  .mono{font-family:'IBM Plex Mono',monospace;}

  /* ---------- Top Navbar ---------- */
  .navbar{
    display:flex; align-items:center; justify-content:space-between;
    padding:16px 32px; background:var(--bg-card); border-bottom:1px solid var(--border-dark);
    position:sticky; top:0; z-index:50;
  }
  .brand-logo{display:flex; align-items:center; gap:12px;}
  .logo-box{width:32px; height:32px; background:var(--text-main); border-radius:8px; display:flex; align-items:center; justify-content:center; color:#fff; font-weight:700; font-size:14px; font-family:'Space Grotesk',sans-serif;}
  .logo-text{font-size:16px; font-weight:600; color:var(--text-main); letter-spacing:-0.02em;}
  
  .nav-actions {display:flex; align-items:center; gap:20px;}
  
  .btn-lang {
    background: transparent; color: var(--text-muted); border: 1px solid var(--border-dark); border-radius: 8px;
    padding: 8px 16px; font-size: 13px; font-weight: 600; cursor: pointer; transition: 0.2s;
  }
  .btn-lang:hover { background: var(--bg-page); color: var(--text-main); }

  .view-toggle{display:flex; background:var(--bg-page); border-radius:10px; padding:4px;}
  .toggle-btn{
    font-family:'Space Grotesk',sans-serif; font-size:13px; font-weight:600; 
    color:var(--text-muted); background:transparent; border:none; padding:8px 20px; border-radius:8px; cursor:pointer;
    transition: 0.2s;
  }
  .toggle-btn.active{background:var(--bg-card); color:var(--text-main); box-shadow:0 1px 3px rgba(0,0,0,0.05);}
  
  /* Views */
  .view{display:none; animation:fadeIn .3s ease;}
  .view.active{display:block;}
  @keyframes fadeIn{from{opacity:0; transform:translateY(8px);} to{opacity:1; transform:translateY(0);}}

  /* ---------- WEB DASHBOARD ---------- */
  .layout-grid{ display:grid; grid-template-columns:260px 1fr; min-height:calc(100vh - 65px);}
  
  /* Minimal Sidebar */
  .sidebar{
    background:var(--bg-card); border-right:1px solid var(--border-dark); padding:32px 20px;
    display:flex; flex-direction:column; gap:32px;
  }
  .menu-group{display:flex; flex-direction:column; gap:6px;}
  .menu-label{font-size:11px; text-transform:uppercase; letter-spacing:0.1em; color:var(--text-light); padding:0 12px 8px; font-weight:600;}
  .menu-item{
    display:flex; align-items:center; gap:12px; padding:12px 14px; border-radius:8px; font-size:14px;
    color:var(--text-muted); cursor:pointer; transition:0.2s; font-weight:500;
  }
  .menu-item svg{width:18px;height:18px; opacity:0.6;}
  .menu-item.active{background:var(--border-light); color:var(--text-main); font-weight:600;}
  .menu-item.active svg{opacity:1; color:var(--accent-main);}
  .menu-item:hover:not(.active){background:var(--border-light); color:var(--text-main);}
  
  /* Main Content area */
  .main-content{padding:40px; overflow-x:hidden;}
  
  .page-header{display:flex; justify-content:space-between; align-items:flex-end; margin-bottom:32px;}
  .page-title{font-size:26px; font-weight:700; color:var(--text-main); letter-spacing:-0.02em;}
  .page-subtitle{font-size:14px; color:var(--text-muted); margin-top:8px;}
  
  /* Alert Box Minimal */
  .alert-box{
    display:flex; align-items:center; justify-content:space-between;
    background:var(--danger-bg); border-left:4px solid var(--danger);
    padding:16px 20px; border-radius:0 8px 8px 0; margin-bottom:32px;
  }
  .alert-text{font-size:14px; color:#991B1B;}
  .alert-btn{background:#fff; border:none; padding:8px 16px; border-radius:6px; font-size:13px; font-weight:600; color:#991B1B; cursor:pointer; box-shadow:0 1px 2px rgba(0,0,0,0.05);}
  
  /* KPI Cards */
  .kpi-grid{display:grid; grid-template-columns:repeat(4,1fr); gap:24px; margin-bottom:32px;}
  .kpi-card{
    background:var(--bg-card); border-radius:12px; padding:24px; 
    box-shadow:0 4px 6px -1px rgba(0,0,0,0.02), 0 2px 4px -1px rgba(0,0,0,0.02);
    border:1px solid var(--border-light);
  }
  .kpi-top{display:flex; justify-content:space-between; align-items:center; margin-bottom:16px;}
  .kpi-title{font-size:13px; color:var(--text-muted); font-weight:500;}
  .badge-trend{font-size:12px; font-weight:600; padding:4px 8px; border-radius:6px;}
  .badge-trend.positive{background:var(--success-bg); color:var(--success);}
  .badge-trend.neutral{background:var(--warning-bg); color:var(--warning);}
  .kpi-value{font-size:32px; font-weight:700; color:var(--text-main); font-family:'IBM Plex Mono',monospace; letter-spacing:-0.03em;}
  .kpi-unit{font-size:14px; color:var(--text-muted); font-weight:400; font-family:'IBM Plex Sans',sans-serif; margin-left:4px;}
  
  /* Content Sections */
  .content-grid{display:grid; grid-template-columns:1.8fr 1fr; gap:24px; margin-bottom:32px;}
  .card-panel{
    background:var(--bg-card); border-radius:12px; padding:28px;
    box-shadow:0 4px 6px -1px rgba(0,0,0,0.02); border:1px solid var(--border-light);
  }
  .card-header{display:flex; justify-content:space-between; align-items:center; margin-bottom:24px;}
  .card-title{font-size:16px; font-weight:600;}
  
  /* Export Button Style */
  .btn-export{
    display:inline-flex; align-items:center; gap:8px;
    background:#10B981; color:#fff; border:none; padding:8px 16px; border-radius:6px;
    font-size:13px; font-weight:600; cursor:pointer; transition:0.2s;
  }
  .btn-export:hover{background:#059669;}
  .btn-export svg{width:16px; height:16px;}

  /* Minimal Table */
  table.minimal-table{width:100%; border-collapse:collapse; font-size:14px;}
  table.minimal-table th{
    text-align:left; font-size:12px; color:var(--text-muted); font-weight:500;
    padding-bottom:16px; border-bottom:1px solid var(--border-dark);
  }
  table.minimal-table td{padding:16px 0; border-bottom:1px solid var(--border-light); color:var(--text-main);}
  table.minimal-table tr:last-child td{border-bottom:none; padding-bottom:0;}
  
  .status-dot{display:inline-block; width:8px; height:8px; border-radius:50%; margin-right:8px;}
  .status-dot.green{background:var(--success);}
  .status-dot.yellow{background:var(--warning);}

  /* Risk List */
  .risk-item{display:flex; justify-content:space-between; align-items:center; padding:16px 0; border-bottom:1px solid var(--border-light);}
  .risk-item:last-child{border-bottom:none; padding-bottom:0;}
  .risk-info .r-name{font-weight:600; font-size:14px; margin-bottom:4px;}
  .risk-info .r-desc{font-size:12px; color:var(--text-muted);}
  .risk-val{font-size:16px; font-weight:600; font-family:'IBM Plex Mono',monospace;}
  .risk-val.red{color:var(--danger);}
  .risk-val.orange{color:var(--warning);}

  /* ---------- MOBILE APP (Minimal) ---------- */
  #mobile{ background:var(--bg-page); min-height:calc(100vh - 65px); display:flex; justify-content:center; padding:48px 24px;}
  .mobile-device{
    width:375px; height:812px; background:var(--bg-page); border-radius:40px; border:12px solid #E2E8F0;
    position:relative; overflow:hidden; box-shadow:0 25px 50px -12px rgba(0,0,0,0.1);
  }
  .notch{position:absolute; top:0; left:50%; transform:translateX(-50%); width:120px; height:24px; background:#E2E8F0; border-radius:0 0 12px 12px; z-index:20;}
  
  .mob-header{padding:48px 24px 20px; background:var(--bg-card); border-bottom:1px solid var(--border-light);}
  .mob-title{font-size:22px; font-weight:700;}
  
  .mob-content{padding:20px; height:calc(100% - 160px); overflow-y:auto;}
  .mob-card{background:var(--bg-card); border-radius:12px; padding:20px; border:1px solid var(--border-light); margin-bottom:16px; box-shadow:0 2px 4px rgba(0,0,0,0.02);}
  .mob-lbl{font-size:12px; color:var(--text-muted); margin-bottom:4px; display:block;}
  .mob-val{font-size:16px; font-weight:600;}
  
  .mob-btn{width:100%; background:var(--text-main); color:#fff; border:none; padding:16px; border-radius:10px; font-size:15px; font-weight:600; margin-top:12px; cursor:pointer;}
  
  .mob-nav{position:absolute; bottom:0; width:100%; background:var(--bg-card); display:flex; padding:16px 20px 24px; border-top:1px solid var(--border-dark);}
  .mob-nav-item{flex:1; display:flex; flex-direction:column; align-items:center; gap:4px; font-size:11px; color:var(--text-muted); font-weight:500;}
  .mob-nav-item.active{color:var(--text-main);}
</style>
</head>
<body>

<nav class="navbar">
  <div class="brand-logo">
    <div class="logo-box">S6</div>
    <div class="logo-text" data-i18n="app_title">Smart SF6 Platform</div>
  </div>
  
  <div class="nav-actions">
    <button id="langBtn" class="btn-lang" onclick="toggleLang()">EN</button>
    <div class="view-toggle">
      <button class="toggle-btn active" data-target="dashboard" data-i18n="tab_web">Web Dashboard</button>
      <button class="toggle-btn" data-target="mobile" data-i18n="tab_mobile">Mobile App</button>
    </div>
  </div>
</nav>

<div class="view active" id="dashboard">
  <div class="layout-grid">
    <aside class="sidebar">
      <div class="menu-group">
        <div class="menu-label" data-i18n="menu_main">Main Menu</div>
        <div class="menu-item active">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg>
          <span data-i18n="menu_dash">Dashboard</span>
        </div>
        <div class="menu-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 20l-5.5-2V6L9 8m0 12l6-2m-6 2V8m6 10l5.5 2V8L15 6m0 12V6m0 0L9 8"/></svg>
          <span data-i18n="menu_assets">Assets</span>
        </div>
        <div class="menu-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 9v4m0 4h.01M10.3 3.9L2.6 17.5a1.5 1.5 0 001.3 2.3h16.2a1.5 1.5 0 001.3-2.3L13.7 3.9a1.5 1.5 0 00-2.6 0z"/></svg>
          <span data-i18n="menu_alerts">Alerts</span>
        </div>
      </div>
      
      <div class="menu-group">
        <div class="menu-label" data-i18n="menu_settings">Settings</div>
        <div class="menu-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="8"/><path d="M12 8v4l3 2"/></svg>
          <span data-i18n="menu_devices">Devices</span>
        </div>
        <div class="menu-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 3v18h18M7 15l4-6 3 3 5-8"/></svg>
          <span data-i18n="menu_reports">Reports</span>
        </div>
      </div>
    </aside>

    <main class="main-content">
      
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="page_title">Corporate Performance</h1>
          <div class="page-subtitle" data-i18n="page_subtitle">ข้อมูลอัปเดตล่าสุด: 06 ก.ย. 2569 · 24 สถานี · 312 อุปกรณ์</div>
        </div>
      </div>

      <div class="alert-box">
        <div class="alert-text"><strong data-i18n="alert_bold">Critical (2):</strong> <span data-i18n="alert_msg">คาดการณ์ความดันต่ำกว่าเกณฑ์ภายใน 7 วัน (บางปะกง, พระนครเหนือ)</span></div>
        <button class="alert-btn" data-i18n="alert_btn">ดูรายละเอียด</button>
      </div>

      <div class="kpi-grid">
        <div class="kpi-card">
          <div class="kpi-top">
            <span class="kpi-title" data-i18n="kpi1">SF6 กู้คืนสะสม</span>
            <span class="badge-trend positive">+8.2%</span>
          </div>
          <div class="kpi-value">142.6<span class="kpi-unit">kg</span></div>
        </div>
        
        <div class="kpi-card">
          <div class="kpi-top">
            <span class="kpi-title" data-i18n="kpi2">ลดคาร์บอนเทียบเท่า</span>
            <span class="badge-trend positive">+8.2%</span>
          </div>
          <div class="kpi-value">3,351<span class="kpi-unit">tCO2e</span></div>
        </div>
        
        <div class="kpi-card">
          <div class="kpi-top">
            <span class="kpi-title" data-i18n="kpi3">มูลค่าคาร์บอนเครดิต</span>
            <span class="badge-trend neutral" data-i18n="kpi3_badge">ราคาประเมิน</span>
          </div>
          <div class="kpi-value">335,050<span class="kpi-unit" data-i18n="currency">THB</span></div>
        </div>

        <div class="kpi-card">
          <div class="kpi-top">
            <span class="kpi-title" data-i18n="kpi4">ROI สะสม</span>
            <span class="badge-trend neutral" data-i18n="kpi4_badge">Payback 2.1y</span>
          </div>
          <div class="kpi-value">18.4<span class="kpi-unit">%</span></div>
        </div>
      </div>

      <div class="content-grid">
        <div class="card-panel">
          <div class="card-header">
            <h3 class="card-title" data-i18n="chart_title">Carbon Reduction Trend (tCO2e)</h3>
          </div>
          <canvas id="mainChart" height="140"></canvas>
        </div>

        <div class="card-panel">
          <div class="card-header">
            <h3 class="card-title" data-i18n="risk_title">Predictive Risk</h3>
          </div>
          <div class="risk-list">
            <div class="risk-item">
              <div class="risk-info">
                <div class="r-name" data-i18n="loc1">บางปะกง · GIS-B3</div>
                <div class="r-desc">Trend: -4.1%/wk</div>
              </div>
              <div class="risk-val red">5 <span style="font-size:12px;font-family:sans-serif;" data-i18n="days">วัน</span></div>
            </div>
            <div class="risk-item">
              <div class="risk-info">
                <div class="r-name" data-i18n="loc2">พระนครเหนือ · GIS-A1</div>
                <div class="r-desc">Trend: -3.6%/wk</div>
              </div>
              <div class="risk-val red">7 <span style="font-size:12px;font-family:sans-serif;" data-i18n="days">วัน</span></div>
            </div>
            <div class="risk-item">
              <div class="risk-info">
                <div class="r-name" data-i18n="loc3">สุราษฎร์ธานี · GIS-C2</div>
                <div class="r-desc">Trend: -1.8%/wk</div>
              </div>
              <div class="risk-val orange">12 <span style="font-size:12px;font-family:sans-serif;" data-i18n="days">วัน</span></div>
            </div>
          </div>
        </div>
      </div>

      <div class="card-panel">
        <div class="card-header">
          <h3 class="card-title" data-i18n="table_title">Regeneration Logs</h3>
          
          <button class="btn-export" onclick="exportTableToCSV('SF6_Data_Export.csv')">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="12" y1="18" x2="12" y2="12"></line><line x1="9" y1="15" x2="15" y2="15"></line></svg>
            <span data-i18n="btn_export">Export to Excel</span>
          </button>
        </div>
        
        <table class="minimal-table" id="dataTable">
          <thead>
            <tr>
              <th data-i18n="th_date">Date</th>
              <th data-i18n="th_loc">Station / Asset</th>
              <th data-i18n="th_rec">Recovered Gas</th>
              <th data-i18n="th_val">Carbon Value (THB)</th>
              <th data-i18n="th_stat">Status</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>04 Sep 2026</td>
              <td class="mono" data-i18n="tb_s1">บางปะกง / GIS-B1</td>
              <td class="mono">6.20 kg</td>
              <td class="mono">14,570</td>
              <td><span class="status-dot green"></span>Verified</td>
            </tr>
            <tr>
              <td>02 Sep 2026</td>
              <td class="mono" data-i18n="tb_s2">พระนครเหนือ / GIS-A3</td>
              <td class="mono">4.80 kg</td>
              <td class="mono">11,280</td>
              <td><span class="status-dot yellow"></span>Pending</td>
            </tr>
            <tr>
              <td>30 Aug 2026</td>
              <td class="mono" data-i18n="tb_s3">ชลบุรี 2 / GIS-A2</td>
              <td class="mono">3.10 kg</td>
              <td class="mono">7,285</td>
              <td><span class="status-dot green"></span>Verified</td>
            </tr>
            <tr>
              <td>27 Aug 2026</td>
              <td class="mono" data-i18n="tb_s4">สุราษฎร์ธานี / GIS-C1</td>
              <td class="mono">5.50 kg</td>
              <td class="mono">12,925</td>
              <td><span class="status-dot green"></span>Verified</td>
            </tr>
          </tbody>
        </table>
      </div>

    </main>
  </div>
</div>

<div class="view" id="mobile">
  <div class="mobile-device">
    <div class="notch"></div>
    <div class="mob-header">
      <div data-i18n="mob_greet">สวัสดี, สมชาย (Technician)</div>
      <div class="mob-title" data-i18n="mob_title">งานที่ต้องดำเนินการ</div>
    </div>
    
    <div class="mob-content">
      <div class="mob-card">
        <span class="mob-lbl" data-i18n="mob_lbl_priority">ความเร่งด่วน: สูง</span>
        <div class="mob-val" data-i18n="loc1">บางปะกง · GIS-B3</div>
        <div style="font-size:13px; color:var(--text-muted); margin-top:8px;" data-i18n="mob_desc1">ความดันลดลง 4.1% ภายใน 1 สัปดาห์ คาดว่าจะถึงจุดวิกฤตใน 5 วัน</div>
        <button class="mob-btn" data-i18n="mob_btn_scan">สแกนอุปกรณ์ (QR Code)</button>
      </div>

      <div class="mob-card" style="text-align:center; padding:32px 20px;">
        <svg viewBox="0 0 24 24" width="32" height="32" stroke="var(--success)" fill="none" stroke-width="2" style="margin-bottom:12px;"><polyline points="20 6 9 17 4 12"></polyline></svg>
        <div class="mob-val" data-i18n="mob_sync_t">ข้อมูลซิงค์ล่าสุด</div>
        <div style="font-size:12px; color:var(--text-muted); margin-top:4px;">09:42 AM · Online</div>
      </div>
    </div>

    <div class="mob-nav">
      <div class="mob-nav-item active">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/></svg>
        <span data-i18n="nav_tasks">Tasks</span>
      </div>
      <div class="mob-nav-item">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="4" width="6" height="6"/><rect x="14" y="4" width="6" height="6"/><rect x="4" y="14" width="6" height="6"/><rect x="14" y="14" width="6" height="6"/></svg>
        <span data-i18n="nav_scan">Scan</span>
      </div>
      <div class="mob-nav-item">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="4"/><path d="M4 21v-1a6 6 0 016-6h4a6 6 0 016 6v1"/></svg>
        <span data-i18n="nav_profile">Profile</span>
      </div>
    </div>
  </div>
</div>

<script>
  // --- View Switcher ---
  document.querySelectorAll('.toggle-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      document.getElementById(btn.dataset.target).classList.add('active');
    });
  });

  // --- Export to CSV (Excel) Logic ---
  function downloadCSV(csv, filename) {
    let csvFile;
    let downloadLink;
    // สร้างไฟล์ CSV พร้อมรองรับภาษาไทย (BOM)
    csvFile = new Blob(["\uFEFF"+csv], {type: "text/csv;charset=utf-8;"});
    downloadLink = document.createElement("a");
    downloadLink.download = filename;
    downloadLink.href = window.URL.createObjectURL(csvFile);
    downloadLink.style.display = "none";
    document.body.appendChild(downloadLink);
    downloadLink.click();
    document.body.removeChild(downloadLink);
  }

  function exportTableToCSV(filename) {
    let csv = [];
    // ดึงข้อมูลจากตาราง
    let rows = document.querySelectorAll("#dataTable tr");
    for (let i = 0; i < rows.length; i++) {
      let row = [], cols = rows[i].querySelectorAll("td, th");
      for (let j = 0; j < cols.length; j++) {
        // ลบช่องว่างและเครื่องหมายคำพูดเพื่อป้องกันไฟล์เพี้ยน
        row.push('"' + cols[j].innerText.trim().replace(/"/g, '""') + '"');
      }
      csv.push(row.join(","));
    }
    // โหลดไฟล์
    downloadCSV(csv.join("\n"), filename);
  }

  // --- Translation Dictionary ---
  const dict = {
    "th": {
      "app_title": "Smart SF6 (Minimal)", "tab_web": "แดชบอร์ดผู้บริหาร", "tab_mobile": "แอปหน้างาน",
      "menu_main": "เมนูหลัก", "menu_dash": "ภาพรวมระบบ", "menu_assets": "จัดการสถานี", "menu_alerts": "แจ้งเตือน",
      "menu_settings": "ตั้งค่า", "menu_devices": "อุปกรณ์ IoT", "menu_reports": "รายงาน",
      "page_title": "ข้อมูลภาพรวมองค์กร", "page_subtitle": "ข้อมูลอัปเดตล่าสุด: 06 ก.ย. 2569 · 24 สถานี · 312 อุปกรณ์",
      "alert_bold": "วิกฤต (2):", "alert_msg": "คาดการณ์ความดันต่ำกว่าเกณฑ์ภายใน 7 วัน (บางปะกง, พระนครเหนือ)", "alert_btn": "ดูรายละเอียด",
      "kpi1": "SF6 กู้คืนสะสม", "kpi2": "ลดคาร์บอนเทียบเท่า", "kpi3": "มูลค่าคาร์บอนเครดิต", "kpi4": "ROI สะสม",
      "kpi3_badge": "ราคาประเมิน", "kpi4_badge": "คืนทุน 2.1 ปี", "currency": "บาท",
      "chart_title": "แนวโน้มการลดคาร์บอนเครดิต (tCO2e)", "risk_title": "พยากรณ์ความเสี่ยง",
      "loc1": "บางปะกง · GIS-B3", "loc2": "พระนครเหนือ · GIS-A1", "loc3": "สุราษฎร์ธานี · GIS-C2", "days": "วัน",
      "table_title": "ประวัติการกู้คืนก๊าซ", "btn_export": "ส่งออก Excel",
      "th_date": "วันที่", "th_loc": "สถานี / อุปกรณ์", "th_rec": "ปริมาณกู้คืน", "th_val": "มูลค่า (บาท)", "th_stat": "สถานะ",
      "tb_s1": "บางปะกง / GIS-B1", "tb_s2": "พระนครเหนือ / GIS-A3", "tb_s3": "ชลบุรี 2 / GIS-A2", "tb_s4": "สุราษฎร์ธานี / GIS-C1",
      "mob_greet": "สวัสดี, สมชาย (ช่างเทคนิค)", "mob_title": "งานที่ต้องดำเนินการ",
      "mob_lbl_priority": "ความเร่งด่วน: สูง", "mob_desc1": "ความดันลดลง 4.1% ภายใน 1 สัปดาห์ คาดว่าจะถึงจุดวิกฤตใน 5 วัน",
      "mob_btn_scan": "สแกนอุปกรณ์ (QR Code)", "mob_sync_t": "ข้อมูลซิงค์ล่าสุด",
      "nav_tasks": "งาน", "nav_scan": "สแกน", "nav_profile": "โปรไฟล์"
    },
    "en": {
      "app_title": "Smart SF6 Platform", "tab_web": "Dashboard", "tab_mobile": "Mobile App",
      "menu_main": "Main Menu", "menu_dash": "Dashboard", "menu_assets": "Assets", "menu_alerts": "Alerts",
      "menu_settings": "Settings", "menu_devices": "IoT Devices", "menu_reports": "Reports",
      "page_title": "Corporate Performance", "page_subtitle": "Last updated: Sep 06, 2026 · 24 Stations · 312 Devices",
      "alert_bold": "Critical (2):", "alert_msg": "Pressure expected to drop below safe threshold in 7 days.", "alert_btn": "View Details",
      "kpi1": "Total SF6 Recovered", "kpi2": "Carbon Equivalent", "kpi3": "Carbon Credit Value", "kpi4": "Cumulative ROI",
      "kpi3_badge": "Estimated", "kpi4_badge": "Payback 2.1y", "currency": "THB",
      "chart_title": "Carbon Reduction Trend (tCO2e)", "risk_title": "Predictive Risk",
      "loc1": "Bang Pakong · GIS-B3", "loc2": "North BKK · GIS-A1", "loc3": "Surat Thani · GIS-C2", "days": "Days",
      "table_title": "Regeneration Logs", "btn_export": "Export to Excel",
      "th_date": "Date", "th_loc": "Station / Asset", "th_rec": "Recovered Gas", "th_val": "Value (THB)", "th_stat": "Status",
      "tb_s1": "Bang Pakong / GIS-B1", "tb_s2": "North BKK / GIS-A3", "tb_s3": "Chonburi 2 / GIS-A2", "tb_s4": "Surat Thani / GIS-C1",
      "mob_greet": "Hello, Somchai (Tech)", "mob_title": "Pending Tasks",
      "mob_lbl_priority": "Priority: High", "mob_desc1": "Pressure down 4.1% in 1 week. Est. critical in 5 days.",
      "mob_btn_scan": "Scan Device (QR Code)", "mob_sync_t": "Last Synced",
      "nav_tasks": "Tasks", "nav_scan": "Scan", "nav_profile": "Profile"
    }
  };

  let currentLang = "th";
  function toggleLang() {
    currentLang = currentLang === "th" ? "en" : "th";
    document.getElementById('langBtn').innerText = currentLang === "th" ? "EN" : "TH";
    document.querySelectorAll('[data-i18n]').forEach(el => {
      const key = el.getAttribute('data-i18n');
      if (dict[currentLang][key]) el.innerHTML = dict[currentLang][key];
    });
  }

  // --- Chart.js Setup ---
  document.addEventListener("DOMContentLoaded", function() {
    const ctx = document.getElementById('mainChart');
    if(ctx) {
      new Chart(ctx, {
        type:'line',
        data:{
          labels:['Apr','May','Jun','Jul','Aug','Sep'],
          datasets:[
            { label:'Actual', data:[2410,2680,2790,3020,3095,3351], borderColor:'#0F172A', backgroundColor:'rgba(15,23,42,0.05)', fill:true, tension:0.4, pointRadius:0, borderWidth:2 },
            { label:'Target', data:[2500,2700,2900,3100,3300,3500], borderColor:'#94A3B8', borderDash:[5,5], fill:false, tension:0.4, pointRadius:0, borderWidth:2 }
          ]
        },
        options:{ responsive:true, plugins:{ legend:{display:false} }, scales:{ x:{ grid:{display:false}, ticks:{color:'#64748B', font:{family:'IBM Plex Sans'}} }, y:{ grid:{color:'#F1F5F9'}, ticks:{color:'#64748B', font:{family:'IBM Plex Mono'}} } } }
      });
    }
  });
</script>
</body>
</html>

