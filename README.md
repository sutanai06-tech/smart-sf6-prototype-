# smart-sf6-prototype
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart SF6 — Enterprise Platform</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Sans+Thai:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root {
    --bg-main: #F4F6F9;
    --bg-card: #FFFFFF;
    --border-color: #E2E8F0;
    --text-main: #0F172A;
    --text-muted: #64748B;
    
    /* EGAT Yellow & Navy Blue Theme */
    --navy-dark: #0A192F;
    --navy-light: #1E293B;
    --egat-yellow: #F59E0B;
    --egat-yellow-hover: #D97706;
    --yellow-bg: #FEF3C7;
    
    --success: #10B981; --success-bg: #D1FAE5;
    --danger: #EF4444; --danger-bg: #FEE2E2;
    --warning: #F59E0B; --warning-bg: #FEF3C7;
  }
  * { box-sizing:border-box; margin:0; padding:0; }
  body {
    font-family:'IBM Plex Sans','IBM Plex Sans Thai',sans-serif;
    background:var(--bg-main); color:var(--text-main); min-height:100vh;
  }
  h1,h2,h3 { font-family:'Space Grotesk','IBM Plex Sans Thai',sans-serif; }
  .mono { font-family:'IBM Plex Mono',monospace; }

  /* --- Top Navigation --- */
  .topnav {
    display:flex; align-items:center; justify-content:space-between;
    padding:12px 32px; background:var(--navy-dark); color:#fff;
    position:sticky; top:0; z-index:100; border-bottom:3px solid var(--egat-yellow);
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }
  .brand-container { display:flex; align-items:center; gap:12px; }
  .brand-badge { width:38px; height:38px; background:var(--egat-yellow); color:#000; font-weight:700; border-radius:8px; display:flex; align-items:center; justify-content:center; font-family:'Space Grotesk',sans-serif; font-size:16px; }
  .brand-title { font-size:18px; font-weight:700; letter-spacing:-0.01em; }
  .brand-subtitle { font-size:11px; color:#94A3B8; text-transform:uppercase; letter-spacing:0.05em; }

  .nav-controls { display:flex; align-items:center; gap:16px; }
  .lang-btn {
    background:var(--navy-light); color:#fff; border:1px solid #334155; padding:6px 14px; border-radius:6px;
    font-size:12px; font-weight:600; cursor:pointer; transition:0.2s;
  }
  .lang-btn:hover { background:var(--egat-yellow); color:#000; }

  .platform-switch { display:flex; background:var(--navy-light); padding:4px; border-radius:8px; }
  .platform-btn {
    background:transparent; color:#94A3B8; border:none; padding:8px 18px; border-radius:6px;
    font-size:13px; font-weight:600; cursor:pointer; transition:0.2s;
  }
  .platform-btn.active { background:var(--egat-yellow); color:#000; box-shadow:0 1px 3px rgba(0,0,0,0.2); }

  /* --- Web Layout --- */
  .app-container { display:grid; grid-template-columns:250px 1fr; min-height:calc(100vh - 65px); }
  
  .sidebar {
    background:var(--bg-card); border-right:1px solid var(--border-color); padding:24px 16px;
    display:flex; flex-direction:column; gap:8px; box-shadow: 2px 0 10px rgba(0,0,0,0.02);
  }
  .nav-item {
    display:flex; align-items:center; gap:12px; padding:12px 16px; border-radius:8px; font-size:14px;
    color:var(--text-muted); font-weight:500; cursor:pointer; transition:0.2s;
  }
  .nav-item svg { width:18px; height:18px; }
  .nav-item.active { background:var(--yellow-bg); color:var(--text-main); font-weight:600; border-left:4px solid var(--egat-yellow); }
  .nav-item:hover:not(.active) { background:var(--bg-main); color:var(--text-main); }

  .content-area { padding:28px 40px; overflow-y:auto; }
  .page-view { display:none; animation:fadeIn 0.3s ease; }
  .page-view.active { display:block; }
  @keyframes fadeIn { from{opacity:0; transform:translateY(8px);} to{opacity:1; transform:translateY(0);} }

  .page-header { display:flex; justify-content:space-between; align-items:flex-end; margin-bottom:24px; border-bottom:1px solid var(--border-color); padding-bottom:16px; }
  .page-title { font-size:24px; font-weight:700; }
  .page-sub { font-size:13px; color:var(--text-muted); margin-top:6px; }

  /* --- Web Components --- */
  .kpi-grid { display:grid; grid-template-columns:repeat(4,1fr); gap:20px; margin-bottom:24px; }
  .kpi-card { background:var(--bg-card); border:1px solid var(--border-color); border-radius:12px; padding:20px; box-shadow:0 2px 4px rgba(0,0,0,0.02); }
  .kpi-lbl { font-size:12px; color:var(--text-muted); font-weight:600; text-transform:uppercase; margin-bottom:8px; }
  .kpi-val { font-size:28px; font-weight:700; font-family:'IBM Plex Mono',monospace; color:var(--navy-dark); }
  .kpi-unit { font-size:14px; font-weight:500; color:var(--text-muted); margin-left:4px; font-family:'IBM Plex Sans',sans-serif; }

  .card { background:var(--bg-card); border:1px solid var(--border-color); border-radius:12px; padding:24px; margin-bottom:24px; box-shadow:0 2px 4px rgba(0,0,0,0.02); }
  .card-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:16px; }
  .card-title { font-size:16px; font-weight:700; }

  .btn-export {
    display:inline-flex; align-items:center; gap:8px; background:var(--success); color:#fff; border:none;
    padding:10px 18px; border-radius:6px; font-size:13px; font-weight:600; cursor:pointer; transition:0.2s;
  }
  .btn-export:hover { background:#059669; box-shadow:0 4px 6px rgba(16,185,129,0.2); }
  .btn-export svg { width:16px; height:16px; }

  table.data-table { width:100%; border-collapse:collapse; font-size:13px; }
  table.data-table th { text-align:left; color:var(--text-muted); font-weight:600; padding:12px 10px; border-bottom:2px solid var(--border-color); background:#F8FAFC; }
  table.data-table td { padding:14px 10px; border-bottom:1px solid var(--border-color); }
  table.data-table tr:hover td { background:#F8FAFC; }

  .badge { display:inline-block; padding:4px 10px; border-radius:6px; font-size:11px; font-weight:600; }
  .badge.success { background:var(--success-bg); color:var(--success); border:1px solid #A7F3D0; }
  .badge.danger { background:var(--danger-bg); color:var(--danger); border:1px solid #FECACA; }
  .badge.warning { background:var(--warning-bg); color:#B45309; border:1px solid #FDE68A; }

  /* ================= MOBILE APP UI ================= */
  #mobilePlatform { display:none; justify-content:center; padding:32px; background:var(--navy-light); min-height:calc(100vh - 65px); }
  .mobile-shell {
    width:375px; height:780px; background:#F8FAFC; border-radius:40px; border:12px solid #0F172A;
    box-shadow:0 20px 50px rgba(0,0,0,0.3); position:relative; overflow:hidden; display:flex; flex-direction:column;
  }
  .notch { position:absolute; top:0; left:50%; transform:translateX(-50%); width:120px; height:24px; background:#0F172A; border-radius:0 0 12px 12px; z-index:20; }
  
  .mob-header { background:var(--bg-card); padding:40px 20px 16px; border-bottom:1px solid var(--border-color); text-align:left; }
  .mob-greet { font-size:12px; color:var(--text-muted); font-weight:600; margin-bottom:4px; }
  .mob-title { font-size:22px; font-weight:700; color:var(--navy-dark); }
  
  .mob-body { flex:1; overflow-y:auto; padding:16px; position:relative; }
  .mob-view { display:none; animation:fadeIn 0.2s ease; }
  .mob-view.active { display:block; }

  .mob-card { background:var(--bg-card); border:1px solid var(--border-color); border-radius:12px; padding:16px; margin-bottom:16px; box-shadow:0 2px 6px rgba(0,0,0,0.03); }
  .mob-btn {
    width:100%; background:var(--navy-dark); color:#fff; border:none; padding:14px; border-radius:8px;
    font-size:14px; font-weight:700; cursor:pointer; transition:0.2s; margin-top:8px;
  }
  .mob-btn:active { transform:scale(0.98); }
  .mob-btn.yellow { background:var(--egat-yellow); color:#000; }

  /* Input form mobile */
  .mob-input-group { margin-bottom:16px; }
  .mob-input-group label { display:block; font-size:12px; font-weight:600; color:var(--text-muted); margin-bottom:6px; }
  .mob-input { width:100%; padding:12px; border:1px solid var(--border-color); border-radius:8px; font-family:'IBM Plex Mono',monospace; font-size:16px; background:#F8FAFC; outline:none; }
  .mob-input:focus { border-color:var(--egat-yellow); background:#fff; }

  /* Bottom Navigation */
  .mob-nav { display:flex; background:var(--bg-card); padding:12px 16px 24px; border-top:1px solid var(--border-color); box-shadow:0 -4px 12px rgba(0,0,0,0.03); }
  .mob-tab { flex:1; display:flex; flex-direction:column; align-items:center; gap:4px; color:#94A3B8; font-size:10px; font-weight:600; cursor:pointer; }
  .mob-tab svg { width:22px; height:22px; }
  .mob-tab.active { color:var(--egat-yellow); }

  /* Scanner Animation */
  .scanner-box { width:100%; height:250px; background:#0F172A; border-radius:12px; position:relative; overflow:hidden; margin-bottom:16px; }
  .scan-line { width:100%; height:2px; background:var(--egat-yellow); position:absolute; top:0; box-shadow:0 0 10px var(--egat-yellow); animation:scanMove 2s infinite linear; }
  @keyframes scanMove { 0%{top:0;} 50%{top:100%;} 100%{top:0;} }
</style>
</head>
<body>

<nav class="topnav">
  <div class="brand-container">
    <div class="brand-badge">S6</div>
    <div>
      <div class="brand-title">Smart SF6 Platform</div>
      <div class="brand-subtitle">EGAT Enterprise Grid</div>
    </div>
  </div>
  <div class="nav-controls">
    <button class="lang-btn" id="langToggle" onclick="toggleLanguage()">EN</button>
    <div class="platform-switch">
      <button class="platform-btn active" onclick="switchPlatform('web')" data-i18n="sw_web">Web Dashboard</button>
      <button class="platform-btn" onclick="switchPlatform('mobile')" data-i18n="sw_mob">Mobile App</button>
    </div>
  </div>
</nav>

<div id="webPlatform" class="app-container">
  <aside class="sidebar">
    <div class="nav-item active" onclick="switchWebPage('dashboard', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg>
      <span data-i18n="nav_dash">แดชบอร์ดหลัก</span>
    </div>
    <div class="nav-item" onclick="switchWebPage('stations', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 20l-5.5-2V6L9 8m0 12l6-2m-6 2V8m6 10l5.5 2V8L15 6m0 12V6m0 0L9 8"/></svg>
      <span data-i18n="nav_stations">สถานะอุปกรณ์ (Assets)</span>
    </div>
    <div class="nav-item" onclick="switchWebPage('history', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
      <span data-i18n="nav_history">ประวัติย้อนหลัง</span>
    </div>
    <div class="nav-item" onclick="switchWebPage('reports', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/></svg>
      <span data-i18n="nav_reports">รายงาน & ส่งออก Excel</span>
    </div>
  </aside>

  <main class="content-area">
    
    <div id="page-dashboard" class="page-view active">
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="p1_title">ภาพรวมระบบกู้คืนก๊าซ SF6</h1>
          <div class="page-sub" data-i18n="p1_sub">ข้อมูลอัปเดตแบบเรียลไทม์ · เครือข่าย กฟผ. 24 สถานี</div>
        </div>
      </div>
      <div class="kpi-grid">
        <div class="kpi-card"><div class="kpi-lbl" data-i18n="kpi_1">SF6 กู้คืนสะสม</div><div class="kpi-val">142.6<span class="kpi-unit">kg</span></div></div>
        <div class="kpi-card"><div class="kpi-lbl" data-i18n="kpi_2">ลดคาร์บอนเทียบเท่า</div><div class="kpi-val">3,351<span class="kpi-unit">tCO2e</span></div></div>
        <div class="kpi-card"><div class="kpi-lbl" data-i18n="kpi_3">มูลค่าคาร์บอนเครดิต</div><div class="kpi-val">335,050<span class="kpi-unit">THB</span></div></div>
        <div class="kpi-card"><div class="kpi-lbl" data-i18n="kpi_4">สถานะเซนเซอร์ปกติ</div><div class="kpi-val" style="color:var(--success);">309<span class="kpi-unit">/ 312</span></div></div>
      </div>
      <div style="display:grid; grid-template-columns: 2fr 1fr; gap:24px;">
        <div class="card">
          <div class="card-header"><h3 class="card-title" data-i18n="chart_title">แนวโน้มการลดก๊าซเรือนกระจก (tCO2e)</h3></div>
          <canvas id="mainChart" height="130"></canvas>
        </div>
        <div class="card">
          <div class="card-header"><h3 class="card-title" data-i18n="risk_title">จุดเสี่ยงที่ต้องเฝ้าระวัง</h3></div>
          <div style="padding:14px; background:#FEF2F2; border-left:4px solid var(--danger); border-radius:8px; margin-bottom:10px;">
            <div style="font-weight:700; font-size:14px;">บางปะกง · GIS-B3</div>
            <div style="font-size:12px; color:var(--text-muted); margin-top:4px;">ความดันลดลง 4.1%/wk (วิกฤตใน 5 วัน)</div>
          </div>
          <div style="padding:14px; background:#FEFBEC; border-left:4px solid var(--warning); border-radius:8px;">
            <div style="font-weight:700; font-size:14px;">สุราษฎร์ธานี · GIS-C2</div>
            <div style="font-size:12px; color:var(--text-muted); margin-top:4px;">ความดันลดลง 1.8%/wk (เฝ้าระวัง)</div>
          </div>
        </div>
      </div>
    </div>

    <div id="page-stations" class="page-view">
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="p2_title">สถานะอุปกรณ์ (Asset Monitoring)</h1>
          <div class="page-sub" data-i18n="p2_sub">ตรวจสอบแรงดัน อุณหภูมิ และสถานะเซนเซอร์ของแต่ละถัง</div>
        </div>
      </div>
      <div class="card">
        <table class="data-table">
          <thead>
            <tr>
              <th data-i18n="th_st">สถานีไฟฟ้า</th>
              <th data-i18n="th_bay">Bay / Tank ID</th>
              <th data-i18n="th_press">แรงดัน (kPa)</th>
              <th data-i18n="th_stat">สถานะความเสี่ยง</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>บางปะกง</td><td class="mono">GIS-B3 / TK-0231</td><td class="mono" style="color:var(--danger); font-weight:bold;">548 ↓</td><td><span class="badge danger" data-i18n="st_high">เสี่ยงสูง</span></td></tr>
            <tr><td>พระนครเหนือ</td><td class="mono">GIS-A1 / TK-0104</td><td class="mono" style="color:var(--danger); font-weight:bold;">561 ↓</td><td><span class="badge danger" data-i18n="st_high">เสี่ยงสูง</span></td></tr>
            <tr><td>ชลบุรี 2</td><td class="mono">GIS-A4 / TK-0056</td><td class="mono">602</td><td><span class="badge warning" data-i18n="st_warn">เฝ้าระวัง</span></td></tr>
            <tr><td>สุราษฎร์ธานี</td><td class="mono">GIS-C2 / TK-0087</td><td class="mono">615</td><td><span class="badge success" data-i18n="st_norm">ปกติ</span></td></tr>
          </tbody>
        </table>
      </div>
    </div>

    <div id="page-history" class="page-view">
      <div class="page-header">
        <div><h1 class="page-title" data-i18n="p3_title">ประวัติข้อมูลย้อนหลัง</h1></div>
      </div>
      <div class="card">
        <div style="margin-bottom:16px; display:flex; gap:12px;">
          <input type="date" value="2026-09-01" style="padding:8px; border:1px solid var(--border-color); border-radius:6px;">
          <button style="background:var(--navy-light); color:#fff; border:none; padding:8px 16px; border-radius:6px; cursor:pointer;" data-i18n="btn_filter">ค้นหา</button>
        </div>
        <table class="data-table">
          <thead><tr><th data-i18n="th_date">วันที่เวลา</th><th data-i18n="th_loc">สถานที่</th><th data-i18n="th_event">เหตุการณ์ / บันทึก</th></tr></thead>
          <tbody>
            <tr><td class="mono">05.09.2026 14:12</td><td>บางปะกง GIS-B3</td><td>แจ้งเตือนแรงดันตกต่ำกว่าเกณฑ์ (548 kPa)</td></tr>
            <tr><td class="mono">04.09.2026 10:30</td><td>บางปะกง GIS-B1</td><td>ดำเนินการ Regeneration สำเร็จโดย สมชาย ใจดี</td></tr>
          </tbody>
        </table>
      </div>
    </div>

    <div id="page-reports" class="page-view">
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="p4_title">รายงานและการส่งออกข้อมูล</h1>
          <div class="page-sub" data-i18n="p4_sub">ดึงข้อมูลลงไฟล์ Excel (.csv) เพื่อนำไปใช้งานต่อ หรือยื่น อบก.</div>
        </div>
        <button class="btn-export" onclick="exportToExcel()">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="12" y1="18" x2="12" y2="12"/><line x1="9" y1="15" x2="15" y2="15"/></svg>
          <span data-i18n="btn_excel">Export to Excel</span>
        </button>
      </div>
      <div class="card">
        <h3 class="card-title" style="margin-bottom:16px;" data-i18n="rep_sample">พรีวิวข้อมูลเตรียมส่งออก (Data Preview)</h3>
        <table class="data-table" id="exportTable">
          <thead>
            <tr>
              <th data-i18n="th_date">วันที่</th><th data-i18n="th_st">สถานี</th><th data-i18n="th_rec">ก๊าซกู้คืน (kg)</th><th data-i18n="th_tco2">ลดคาร์บอน (tCO2e)</th><th data-i18n="th_stat">สถานะตรวจสอบ</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>04.09.2026</td><td>บางปะกง</td><td>6.2</td><td>145.7</td><td>Verified</td></tr>
            <tr><td>02.09.2026</td><td>พระนครเหนือ</td><td>4.8</td><td>112.8</td><td>Pending</td></tr>
            <tr><td>30.08.2026</td><td>ชลบุรี 2</td><td>3.1</td><td>72.8</td><td>Verified</td></tr>
          </tbody>
        </table>
      </div>
    </div>

  </main>
</div>

<div id="mobilePlatform">
  <div class="mobile-shell">
    <div class="notch"></div>
    
    <div class="mob-header">
      <div class="mob-greet" data-i18n="mob_greet">ระบบปฏิบัติการภาคสนาม (Field Ops)</div>
      <div class="mob-title" data-i18n="mob_title">งานของฉัน</div>
    </div>
    
    <div class="mob-body">
      
      <div id="mob-tasks" class="mob-view active">
        <div style="font-size:12px; font-weight:700; color:var(--text-muted); margin-bottom:8px; text-transform:uppercase;" data-i18n="mob_urg_h">ความเร่งด่วน: วิกฤต</div>
        <div class="mob-card" style="border-left:4px solid var(--danger);">
          <div style="display:flex; justify-content:space-between; align-items:flex-start;">
            <div style="font-weight:700; font-size:16px; color:var(--navy-dark);">บางปะกง · GIS-B3</div>
            <div class="badge danger">5 วัน</div>
          </div>
          <div style="font-size:13px; color:var(--text-muted); margin:8px 0 12px; line-height:1.4;" data-i18n="mob_desc1">
            รหัสถัง TK-0231<br>ความดันล่าสุด: 548 kPa (ลดลงต่อเนื่อง)
          </div>
          <button class="mob-btn" onclick="switchMobView('mob-scan')" data-i18n="btn_accept">รับงาน & สแกนอุปกรณ์</button>
        </div>

        <div style="font-size:12px; font-weight:700; color:var(--text-muted); margin:16px 0 8px; text-transform:uppercase;" data-i18n="mob_urg_m">ความเร่งด่วน: ปานกลาง</div>
        <div class="mob-card" style="border-left:4px solid var(--warning); opacity:0.8;">
          <div style="display:flex; justify-content:space-between; align-items:flex-start;">
            <div style="font-weight:700; font-size:16px; color:var(--navy-dark);">สุราษฎร์ธานี · GIS-C2</div>
            <div class="badge warning" style="background:#FEF3C7; color:#B45309;">12 วัน</div>
          </div>
        </div>
      </div>

      <div id="mob-scan" class="mob-view">
        <div style="text-align:center; margin-bottom:16px;">
          <h3 style="color:var(--navy-dark);" data-i18n="scan_title">สแกน QR Code ถัง SF6</h3>
          <p style="font-size:12px; color:var(--text-muted); margin-top:4px;" data-i18n="scan_sub">บางปะกง · TK-0231</p>
        </div>
        <div class="scanner-box">
          <div class="scan-line"></div>
          <div style="position:absolute; top:50%; left:50%; transform:translate(-50%, -50%); color:rgba(255,255,255,0.3); font-size:12px; text-align:center;">Camera Preview<br>[ จำลองกล้อง ]</div>
        </div>
        <p style="font-size:13px; text-align:center; color:var(--text-muted); margin-bottom:24px;" data-i18n="scan_hint">นำกล้องจ่อที่สติกเกอร์ QR หรือสแกน RFID Tag บนตัวถัง</p>
        <button class="mob-btn yellow" onclick="switchMobView('mob-form')" data-i18n="btn_sim_scan">จำลอง: สแกนสำเร็จ</button>
        <button class="mob-btn" style="background:transparent; color:var(--text-muted); border:1px solid var(--border-color);" onclick="switchMobView('mob-tasks')" data-i18n="btn_cancel">ยกเลิก</button>
      </div>

      <div id="mob-form" class="mob-view">
        <div style="margin-bottom:20px;">
          <div style="font-weight:700; font-size:18px; color:var(--success);">✓ ยืนยันอุปกรณ์ TK-0231</div>
          <p style="font-size:12px; color:var(--text-muted); margin-top:4px;" data-i18n="form_sub">กรุณากรอกข้อมูลหลังดำเนินการ Regeneration</p>
        </div>

        <div class="mob-card" style="padding:20px;">
          <div class="mob-input-group">
            <label data-i18n="lbl_gas">ปริมาณก๊าซ SF6 ที่กู้คืนได้ (kg)</label>
            <input type="number" id="gasInput" class="mob-input" placeholder="0.00" oninput="calculateCarbon()">
          </div>
          <div class="mob-input-group">
            <label data-i18n="lbl_press">ความดันหลังชาร์จ (kPa)</label>
            <input type="number" class="mob-input" placeholder="620">
          </div>
        </div>

        <div style="background:var(--navy-dark); color:#fff; border-radius:12px; padding:20px; margin-bottom:16px;">
          <div style="font-size:11px; color:#94A3B8; text-transform:uppercase; font-weight:600;" data-i18n="est_title">ประเมินมูลค่าคาร์บอนเครดิต</div>
          <div style="font-size:28px; font-weight:700; color:var(--egat-yellow); font-family:'IBM Plex Mono',monospace; margin-top:4px;">
            <span id="thbValue">0</span> <span style="font-size:14px; color:#fff; font-family:'IBM Plex Sans',sans-serif;">THB</span>
          </div>
          <div style="font-size:12px; color:#94A3B8; margin-top:8px;">(<span id="co2Value">0.0</span> tCO2e)</div>
        </div>

        <button class="mob-btn" onclick="submitForm()" data-i18n="btn_submit">บันทึกข้อมูลและปิดงาน</button>
      </div>

      <div id="mob-success" class="mob-view" style="text-align:center; padding-top:40px;">
        <div style="width:80px; height:80px; background:var(--success-bg); color:var(--success); border-radius:50%; font-size:40px; display:flex; align-items:center; justify-content:center; margin:0 auto 24px;">✓</div>
        <h2 style="color:var(--navy-dark); margin-bottom:8px;" data-i18n="done_title">บันทึกข้อมูลสำเร็จ</h2>
        <p style="font-size:14px; color:var(--text-muted); margin-bottom:32px;" data-i18n="done_sub">ข้อมูล Regeneration ถูกส่งเข้าระบบส่วนกลางเรียบร้อยแล้ว</p>
        <button class="mob-btn yellow" onclick="switchMobView('mob-tasks')" data-i18n="btn_home">กลับหน้าหลัก</button>
      </div>

      <div id="mob-history" class="mob-view">
        <h3 style="margin-bottom:16px;" data-i18n="hist_title">ประวัติการปฏิบัติงาน (เดือนนี้)</h3>
        <div class="mob-card" style="border-left:4px solid var(--success);">
          <div style="font-weight:700; font-size:14px;">ชลบุรี 2 · GIS-A2</div>
          <div style="font-size:12px; color:var(--text-muted); margin-top:4px;">กู้คืนได้ 3.10 kg · (30 ส.ค. 2026)</div>
        </div>
        <div class="mob-card" style="border-left:4px solid var(--success);">
          <div style="font-weight:700; font-size:14px;">สุราษฎร์ธานี · GIS-C1</div>
          <div style="font-size:12px; color:var(--text-muted); margin-top:4px;">กู้คืนได้ 5.50 kg · (27 ส.ค. 2026)</div>
        </div>
      </div>

    </div>

    <div class="mob-nav">
      <div class="mob-tab active" onclick="switchMobView('mob-tasks', this)">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/></svg>
        <span data-i18n="m_tab1">งาน</span>
      </div>
      <div class="mob-tab" onclick="switchMobView('mob-scan', this)">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="4" width="6" height="6"/><rect x="14" y="4" width="6" height="6"/><rect x="4" y="14" width="6" height="6"/><rect x="14" y="14" width="6" height="6"/></svg>
        <span data-i18n="m_tab2">สแกน</span>
      </div>
      <div class="mob-tab" onclick="switchMobView('mob-history', this)">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
        <span data-i18n="m_tab3">ประวัติ</span>
      </div>
    </div>
  </div>
</div>

<script>
  // ================= WEB NAVIGATION =================
  function switchPlatform(type) {
    document.querySelectorAll('.platform-btn').forEach(b => b.classList.remove('active'));
    event.target.classList.add('active');
    
    if(type === 'web') {
      document.getElementById('webPlatform').style.display = 'grid';
      document.getElementById('mobilePlatform').style.display = 'none';
    } else {
      document.getElementById('webPlatform').style.display = 'none';
      document.getElementById('mobilePlatform').style.display = 'flex';
      // Reset mobile view to task list when opening app
      switchMobView('mob-tasks');
    }
  }

  function switchWebPage(pageId, el) {
    document.querySelectorAll('.sidebar .nav-item').forEach(i => i.classList.remove('active'));
    document.querySelectorAll('.page-view').forEach(p => p.classList.remove('active'));
    el.classList.add('active');
    document.getElementById('page-' + pageId).classList.add('active');
  }

  // ================= MOBILE APP LOGIC =================
  function switchMobView(viewId, tabEl = null) {
    // ซ่อนหน้าจอทั้งหมด และล้างปุ่ม active
    document.querySelectorAll('.mob-view').forEach(v => v.classList.remove('active'));
    document.getElementById(viewId).classList.add('active');
    
    // อัปเดตแถบเมนูด้านล่าง (ถ้ากดจากปุ่ม tab ให้เน้นสี ถ้าเปิดจากปุ่ม inline ให้ลบสีออก)
    if(tabEl) {
      document.querySelectorAll('.mob-tab').forEach(t => t.classList.remove('active'));
      tabEl.classList.add('active');
    } else {
      document.querySelectorAll('.mob-tab').forEach(t => t.classList.remove('active'));
      // จับคู่ view กับ tab ด้านล่าง
      if(viewId === 'mob-tasks') document.querySelectorAll('.mob-tab')[0].classList.add('active');
      if(viewId === 'mob-scan') document.querySelectorAll('.mob-tab')[1].classList.add('active');
      if(viewId === 'mob-history') document.querySelectorAll('.mob-tab')[2].classList.add('active');
    }
    
    // ล้างฟอร์มเมื่อกลับมาหน้า tasks
    if(viewId === 'mob-tasks') {
      document.getElementById('gasInput').value = '';
      calculateCarbon();
    }
  }

  function calculateCarbon() {
    // สูตร: ก๊าซ (kg) * GWP 23,500 / 1000 = tCO2e.  แล้วคูณ 100 บาท
    const gasVal = parseFloat(document.getElementById('gasInput').value) || 0;
    const tco2e = (gasVal * 23500) / 1000;
    const thb = tco2e * 100;
    
    document.getElementById('co2Value').innerText = tco2e.toFixed(1);
    document.getElementById('thbValue').innerText = thb.toLocaleString('en-US', {maximumFractionDigits: 0});
  }

  function submitForm() {
    const gasVal = document.getElementById('gasInput').value;
    if(!gasVal || gasVal <= 0) {
      alert(currentLang === 'th' ? 'กรุณากรอกปริมาณก๊าซให้ถูกต้อง' : 'Please enter a valid gas amount.');
      return;
    }
    switchMobView('mob-success');
  }

  // ================= EXCEL EXPORT =================
  function exportToExcel() {
    let csv = [];
    let rows = document.querySelectorAll("#exportTable tr");
    for (let i = 0; i < rows.length; i++) {
      let row = [], cols = rows[i].querySelectorAll("td, th");
      for (let j = 0; j < cols.length; j++) {
        row.push('"' + cols[j].innerText.trim().replace(/"/g, '""') + '"');
      }
      csv.push(row.join(","));
    }
    let csvFile = new Blob(["\uFEFF"+csv.join("\n")], {type: "text/csv;charset=utf-8;"});
    let downloadLink = document.createElement("a");
    downloadLink.download = "SmartSF6_Data.csv";
    downloadLink.href = window.URL.createObjectURL(csvFile);
    downloadLink.style.display = "none";
    document.body.appendChild(downloadLink);
    downloadLink.click();
    document.body.removeChild(downloadLink);
  }

  // ================= LANGUAGE TOGGLE =================
  const langDict = {
    "th": {
      "sw_web": "Web Dashboard", "sw_mob": "Mobile App",
      "nav_dash": "แดชบอร์ดหลัก", "nav_stations": "สถานะอุปกรณ์ (Assets)", "nav_history": "ประวัติย้อนหลัง", "nav_reports": "รายงาน & ส่งออก Excel",
      "p1_title": "ภาพรวมระบบกู้คืนก๊าซ SF6", "p1_sub": "ข้อมูลอัปเดตแบบเรียลไทม์ · เครือข่าย กฟผ. 24 สถานี",
      "kpi_1": "SF6 กู้คืนสะสม", "kpi_2": "ลดคาร์บอนเทียบเท่า", "kpi_3": "มูลค่าคาร์บอนเครดิต", "kpi_4": "สถานะเซนเซอร์ปกติ",
      "chart_title": "แนวโน้มการลดก๊าซเรือนกระจก (tCO2e)", "risk_title": "จุดเสี่ยงที่ต้องเฝ้าระวัง",
      "p2_title": "สถานะอุปกรณ์ (Asset Monitoring)", "p2_sub": "ตรวจสอบแรงดัน อุณหภูมิ และสถานะเซนเซอร์ของแต่ละถัง",
      "th_st": "สถานีไฟฟ้า", "th_bay": "Bay / Tank ID", "th_press": "แรงดัน (kPa)", "th_temp": "อุณหภูมิ (°C)", "th_stat": "สถานะความเสี่ยง",
      "st_high": "เสี่ยงสูง", "st_warn": "เฝ้าระวัง", "st_norm": "ปกติ",
      "p3_title": "ประวัติข้อมูลย้อนหลัง", "p3_sub": "เรียกดูข้อมูลเซนเซอร์และกิจกรรมการบำรุงรักษาย้อนหลัง", "btn_filter": "ค้นหา",
      "th_date": "วันที่เวลา", "th_loc": "สถานที่", "th_event": "เหตุการณ์ / บันทึก",
      "p4_title": "รายงานและการส่งออกข้อมูล", "p4_sub": "ดึงข้อมูลลงไฟล์ Excel (.csv) เพื่อนำไปใช้งานต่อ หรือยื่น อบก.", "btn_excel": "Export to Excel",
      "rep_sample": "พรีวิวข้อมูลเตรียมส่งออก (Data Preview)", "th_rec": "ก๊าซกู้คืน (kg)", "th_tco2": "ลดคาร์บอน (tCO2e)",
      
      // Mobile
      "mob_greet": "ระบบปฏิบัติการภาคสนาม (Field Ops)", "mob_title": "งานของฉัน",
      "mob_urg_h": "ความเร่งด่วน: วิกฤต", "mob_desc1": "รหัสถัง TK-0231<br>ความดันล่าสุด: 548 kPa (ลดลงต่อเนื่อง)", "btn_accept": "รับงาน & สแกนอุปกรณ์",
      "mob_urg_m": "ความเร่งด่วน: ปานกลาง", 
      "scan_title": "สแกน QR Code ถัง SF6", "scan_sub": "บางปะกง · TK-0231", "scan_hint": "นำกล้องจ่อที่สติกเกอร์ QR หรือสแกน RFID Tag บนตัวถัง", "btn_sim_scan": "จำลอง: สแกนสำเร็จ", "btn_cancel": "ยกเลิก",
      "form_sub": "กรุณากรอกข้อมูลหลังดำเนินการ Regeneration", "lbl_gas": "ปริมาณก๊าซ SF6 ที่กู้คืนได้ (kg)", "lbl_press": "ความดันหลังชาร์จ (kPa)",
      "est_title": "ประเมินมูลค่าคาร์บอนเครดิต", "btn_submit": "บันทึกข้อมูลและปิดงาน",
      "done_title": "บันทึกข้อมูลสำเร็จ", "done_sub": "ข้อมูล Regeneration ถูกส่งเข้าระบบส่วนกลางเรียบร้อยแล้ว", "btn_home": "กลับหน้าหลัก",
      "hist_title": "ประวัติการปฏิบัติงาน (เดือนนี้)",
      "m_tab1": "งาน", "m_tab2": "สแกน", "m_tab3": "ประวัติ"
    },
    "en": {
      "sw_web": "Web Dashboard", "sw_mob": "Mobile App",
      "nav_dash": "Dashboard", "nav_stations": "Assets Status", "nav_history": "Historical Data", "nav_reports": "Reports & Export",
      "p1_title": "SF6 Recovery Overview", "p1_sub": "Real-time updates · EGAT Network 24 Substations",
      "kpi_1": "Total SF6 Recovered", "kpi_2": "Carbon Offset (tCO2e)", "kpi_3": "Carbon Credit Value", "kpi_4": "Active Sensors",
      "chart_title": "GHG Reduction Trend (tCO2e)", "risk_title": "Predictive Risk Alerts",
      "p2_title": "Asset Monitoring", "p2_sub": "Track pressure, temp, and sensor health per tank",
      "th_st": "Substation", "th_bay": "Bay / Tank ID", "th_press": "Pressure (kPa)", "th_temp": "Temp (°C)", "th_stat": "Risk Level",
      "st_high": "Critical", "st_warn": "Warning", "st_norm": "Normal",
      "p3_title": "Historical Data Logs", "p3_sub": "Review sensor telemetry and maintenance logs", "btn_filter": "Search",
      "th_date": "Timestamp", "th_loc": "Location", "th_event": "Event / Log",
      "p4_title": "Reports & Data Export", "p4_sub": "Export data to Excel (.csv) for T-VER verification", "btn_excel": "Export to Excel",
      "rep_sample": "Export Data Preview", "th_rec": "Recovered (kg)", "th_tco2": "Offset (tCO2e)",
      
      // Mobile
      "mob_greet": "Field Operations System", "mob_title": "My Tasks",
      "mob_urg_h": "Priority: Critical", "mob_desc1": "Tank ID: TK-0231<br>Current Pressure: 548 kPa (Dropping)", "btn_accept": "Accept Task & Scan",
      "mob_urg_m": "Priority: Medium",
      "scan_title": "Scan SF6 Tank QR", "scan_sub": "Bang Pakong · TK-0231", "scan_hint": "Align camera with QR sticker or RFID tag on the tank", "btn_sim_scan": "Simulate: Scan Success", "btn_cancel": "Cancel",
      "form_sub": "Please enter post-regeneration data", "lbl_gas": "SF6 Gas Recovered (kg)", "lbl_press": "Pressure After Proc. (kPa)",
      "est_title": "Est. Carbon Credit Value", "btn_submit": "Submit & Close Task",
      "done_title": "Task Completed", "done_sub": "Regeneration data has been sent to the central system.", "btn_home": "Back to Home",
      "hist_title": "Work History (This Month)",
      "m_tab1": "Tasks", "m_tab2": "Scan", "m_tab3": "History"
    }
  };

  let currentLang = "th";
  function toggleLanguage() {
    currentLang = currentLang === "th" ? "en" : "th";
    document.getElementById('langToggle').innerText = currentLang === "th" ? "EN" : "TH";
    document.querySelectorAll('[data-i18n]').forEach(el => {
      let key = el.getAttribute('data-i18n');
      if(langDict[currentLang][key]) {
        el.innerHTML = langDict[currentLang][key];
      }
    });
  }

  // --- Initialize Chart ---
  document.addEventListener("DOMContentLoaded", function() {
    const ctx = document.getElementById('mainChart');
    if(ctx) {
      new Chart(ctx, {
        type:'line',
        data:{
          labels:['Apr','May','Jun','Jul','Aug','Sep'],
          datasets:[
            { label:'Actual', data:[2410,2680,2790,3020,3095,3351], borderColor:'#0A192F', backgroundColor:'rgba(245,158,11,0.08)', fill:true, tension:0.4, borderWidth:2.5, pointRadius:3 },
            { label:'Target', data:[2500,2700,2900,3100,3300,3500], borderColor:'#94A3B8', borderDash:[5,5], fill:false, tension:0.4, borderWidth:2, pointRadius:0 }
          ]
        },
        options:{ responsive:true, plugins:{ legend:{display:false} }, scales:{ x:{ grid:{display:false} }, y:{ grid:{color:'#E2E8F0'} } } }
      });
    }
  });
</script>
</body>
</html>

