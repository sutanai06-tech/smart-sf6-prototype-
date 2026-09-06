# smart-sf6-prototype
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart SF6 — Enterprise Platform</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Sans+Thai:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root{
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
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  body{
    font-family:'IBM Plex Sans','IBM Plex Sans Thai',sans-serif;
    background:var(--bg-main); color:var(--text-main);
    min-height:100vh;
  }
  h1,h2,h3{font-family:'Space Grotesk','IBM Plex Sans Thai',sans-serif;}
  .mono{font-family:'IBM Plex Mono',monospace;}

  /* --- Top Navigation --- */
  .topnav{
    display:flex; align-items:center; justify-content:space-between;
    padding:14px 32px; background:var(--navy-dark); color:#fff;
    position:sticky; top:0; z-index:100; border-bottom:3px solid var(--egat-yellow);
  }
  .brand-container{display:flex; align-items:center; gap:12px;}
  .brand-badge{width:36px; height:36px; background:var(--egat-yellow); color:#000; font-weight:700; border-radius:8px; display:flex; align-items:center; justify-content:center; font-family:'Space Grotesk',sans-serif;}
  .brand-title{font-size:18px; font-weight:700; letter-spacing:-0.01em;}
  .brand-subtitle{font-size:11px; color:#94A3B8; text-transform:uppercase; letter-spacing:0.05em;}

  .nav-controls{display:flex; align-items:center; gap:16px;}
  .lang-btn{
    background:var(--navy-light); color:#fff; border:1px solid #334155; padding:6px 14px; border-radius:6px;
    font-size:12px; font-weight:600; cursor:pointer; transition:0.2s;
  }
  .lang-btn:hover{background:var(--egat-yellow); color:#000;}

  .platform-switch{display:flex; background:var(--navy-light); padding:3px; border-radius:8px;}
  .platform-btn{
    background:transparent; color:#94A3B8; border:none; padding:6px 16px; border-radius:6px;
    font-size:13px; font-weight:600; cursor:pointer; font-family:'Space Grotesk',sans-serif; transition:0.2s;
  }
  .platform-btn.active{background:var(--egat-yellow); color:#000;}

  /* --- Main Layout --- */
  .app-container{display:grid; grid-template-columns:240px 1fr; min-height:calc(100vh - 65px);}
  
  .sidebar{
    background:var(--bg-card); border-right:1px solid var(--border-color); padding:24px 16px;
    display:flex; flex-direction:column; gap:8px;
  }
  .nav-item{
    display:flex; align-items:center; gap:12px; padding:12px 16px; border-radius:8px; font-size:14px;
    color:var(--text-muted); font-weight:500; cursor:pointer; transition:0.2s;
  }
  .nav-item svg{width:18px; height:18px;}
  .nav-item.active{background:var(--yellow-bg); color:var(--text-main); font-weight:600; border-left:4px solid var(--egat-yellow);}
  .nav-item:hover:not(.active){background:var(--bg-main); color:var(--text-main);}

  .content-area{padding:32px 40px; overflow-y:auto;}
  .page-view{display:none;}
  .page-view.active{display:block; animation:fadeIn 0.25s ease;}
  @keyframes fadeIn{from{opacity:0; transform:translateY(4px);} to{opacity:1; transform:translateY(0);}}

  .page-header{display:flex; justify-content:space-between; align-items:flex-end; margin-bottom:24px;}
  .page-title{font-size:24px; font-weight:700;}
  .page-sub{font-size:13px; color:var(--text-muted); margin-top:4px;}

  /* --- UI Components --- */
  .kpi-grid{display:grid; grid-template-columns:repeat(4,1fr); gap:20px; margin-bottom:24px;}
  .kpi-card{background:var(--bg-card); border:1px solid var(--border-color); border-radius:12px; padding:20px;}
  .kpi-lbl{font-size:12px; color:var(--text-muted); font-weight:500;}
  .kpi-val{font-size:26px; font-weight:700; font-family:'IBM Plex Mono',monospace; margin-top:8px;}
  .kpi-unit{font-size:13px; font-weight:400; color:var(--text-muted); margin-left:4px; font-family:'IBM Plex Sans',sans-serif;}

  .card{background:var(--bg-card); border:1px solid var(--border-color); border-radius:12px; padding:24px; margin-bottom:24px;}
  .card-header{display:flex; justify-content:space-between; align-items:center; margin-bottom:16px;}
  .card-title{font-size:16px; font-weight:600;}

  .btn-export{
    display:inline-flex; align-items:center; gap:8px; background:var(--navy-dark); color:#fff; border:none;
    padding:8px 16px; border-radius:6px; font-size:13px; font-weight:600; cursor:pointer; transition:0.2s;
  }
  .btn-export:hover{background:var(--navy-light);}
  .btn-export svg{width:16px; height:16px;}

  table.data-table{width:100%; border-collapse:collapse; font-size:13.5px;}
  table.data-table th{text-align:left; color:var(--text-muted); font-weight:600; padding:12px 8px; border-bottom:2px solid var(--border-color);}
  table.data-table td{padding:14px 8px; border-bottom:1px solid var(--border-color);}
  table.data-table tr:hover td{background:#F8FAFC;}

  .badge{display:inline-block; padding:4px 10px; border-radius:6px; font-size:11.5px; font-weight:600;}
  .badge.success{background:var(--success-bg); color:var(--success);}
  .badge.danger{background:var(--danger-bg); color:var(--danger);}

  /* --- Mobile App Container --- */
  #mobilePlatform{display:none; justify-content:center; padding:32px; background:var(--bg-main); min-height:calc(100vh - 65px);}
  .mobile-shell{
    width:375px; height:780px; background:#fff; border-radius:40px; border:10px solid var(--navy-dark);
    box-shadow:0 20px 40px rgba(0,0,0,0.15); position:relative; overflow:hidden; display:flex; flex-direction:column;
  }
  .mob-header{background:var(--navy-dark); color:#fff; padding:20px; text-align:center;}
  .mob-body{flex:1; overflow-y:auto; padding:16px;}
  .mob-card{background:var(--bg-main); border:1px solid var(--border-color); border-radius:10px; padding:14px; margin-bottom:12px;}
  .mob-nav{display:flex; background:var(--navy-dark); padding:12px; border-top:1px solid #334155;}
  .mob-tab{flex:1; text-align:center; color:#94A3B8; font-size:11px; font-weight:600;}
  .mob-tab.active{color:var(--egat-yellow);}
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
    <div class="nav-item active" onclick="switchPage('dashboard', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg>
      <span data-i18n="nav_dash">แดชบอร์ดหลัก</span>
    </div>
    <div class="nav-item" onclick="switchPage('stations', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 20l-5.5-2V6L9 8m0 12l6-2m-6 2V8m6 10l5.5 2V8L15 6m0 12V6m0 0L9 8"/></svg>
      <span data-i18n="nav_stations">สถานีและเซนเซอร์</span>
    </div>
    <div class="nav-item" onclick="switchPage('history', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
      <span data-i18n="nav_history">ข้อมูลย้อนหลัง</span>
    </div>
    <div class="nav-item" onclick="switchPage('reports', this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/></svg>
      <span data-i18n="nav_reports">รายงานและ Export</span>
    </div>
  </aside>

  <main class="content-area">
    
    <div id="page-dashboard" class="page-view active">
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="p1_title">ภาพรวมระบบกู้คืนก๊าซ SF6</h1>
          <div class="page-sub" data-i18n="p1_sub">ข้อมูลเรียลไทม์จาก 24 สถานีไฟฟ้าแรงสูง ทั่วประเทศ</div>
        </div>
      </div>

      <div class="kpi-grid">
        <div class="kpi-card">
          <div class="kpi-lbl" data-i18n="kpi_1">SF6 กู้คืนสะสม</div>
          <div class="kpi-val">142.6<span class="kpi-unit">kg</span></div>
        </div>
        <div class="kpi-card">
          <div class="kpi-lbl" data-i18n="kpi_2">ลดคาร์บอนเทียบเท่า</div>
          <div class="kpi-val">3,351<span class="kpi-unit">tCO2e</span></div>
        </div>
        <div class="kpi-card">
          <div class="kpi-lbl" data-i18n="kpi_3">มูลค่าคาร์บอนเครดิต</div>
          <div class="kpi-val">335,050<span class="kpi-unit">THB</span></div>
        </div>
        <div class="kpi-card">
          <div class="kpi-lbl" data-i18n="kpi_4">สถานะเซนเซอร์ปกติ</div>
          <div class="kpi-val" style="color:var(--success);">309<span class="kpi-unit">/ 312</span></div>
        </div>
      </div>

      <div style="display:grid; grid-template-columns: 2fr 1fr; gap:24px;">
        <div class="card">
          <div class="card-header">
            <h3 class="card-title" data-i18n="chart_title">แนวโน้มการลดก๊าซเรือนกระจก (tCO2e)</h3>
          </div>
          <canvas id="mainChart" height="150"></canvas>
        </div>
        <div class="card">
          <div class="card-header">
            <h3 class="card-title" data-i18n="risk_title">จุดเสี่ยงที่ต้องเฝ้าระวัง</h3>
          </div>
          <div style="display:flex; flex-direction:column; gap:12px;">
            <div style="padding:12px; background:#FEF2F2; border-left:4px solid var(--danger); border-radius:6px;">
              <div style="font-weight:600; font-size:13.5px;">บางปะกง · GIS-B3</div>
              <div style="font-size:12px; color:var(--text-muted);">ความดันลดลง 4.1%/wk (วิกฤตใน 5 วัน)</div>
            </div>
            <div style="padding:12px; background:#FEF2F2; border-left:4px solid var(--danger); border-radius:6px;">
              <div style="font-weight:600; font-size:13.5px;">พระนครเหนือ · GIS-A1</div>
              <div style="font-size:12px; color:var(--text-muted);">ความดันลดลง 3.6%/wk (วิกฤตใน 7 วัน)</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div id="page-stations" class="page-view">
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="p2_title">สถานะสถานีไฟฟ้าและถัง SF6</h1>
          <div class="page-sub" data-i18n="p2_sub">รายละเอียดแรงดันและอุณหภูมิรายถัง</div>
        </div>
      </div>
      <div class="card">
        <table class="data-table">
          <thead>
            <tr>
              <th data-i18n="th_st">สถานีไฟฟ้า</th>
              <th data-i18n="th_bay">Bay / Tank ID</th>
              <th data-i18n="th_press">แรงดัน (kPa)</th>
              <th data-i18n="th_temp">อุณหภูมิ (°C)</th>
              <th data-i18n="th_stat">สถานะ</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>บางปะกง</td><td class="mono">GIS-B3 / TK-0231</td><td class="mono" style="color:var(--danger)">548</td><td class="mono">33.1</td><td><span class="badge danger" data-i18n="st_high">เสี่ยงสูง</span></td></tr>
            <tr><td>พระนครเหนือ</td><td class="mono">GIS-A1 / TK-0104</td><td class="mono" style="color:var(--danger)">561</td><td class="mono">31.8</td><td><span class="badge danger" data-i18n="st_high">เสี่ยงสูง</span></td></tr>
            <tr><td>สุราษฎร์ธานี</td><td class="mono">GIS-C2 / TK-0087</td><td class="mono">598</td><td class="mono">32.4</td><td><span class="badge success" data-i18n="st_norm">ปกติ</span></td></tr>
          </tbody>
        </table>
      </div>
    </div>

    <div id="page-history" class="page-view">
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="p3_title">ประวัติข้อมูลย้อนหลัง</h1>
          <div class="page-sub" data-i18n="p3_sub">เรียกดูข้อมูลเซนเซอร์และกิจกรรมการบำรุงรักษาย้อนหลัง</div>
        </div>
      </div>
      <div class="card">
        <div style="margin-bottom:16px; display:flex; gap:12px;">
          <input type="date" value="2026-09-01" style="padding:8px; border:1px solid var(--border-color); border-radius:6px;">
          <input type="date" value="2026-09-06" style="padding:8px; border:1px solid var(--border-color); border-radius:6px;">
          <button style="background:var(--navy-dark); color:#fff; border:none; padding:8px 16px; border-radius:6px; cursor:pointer;" data-i18n="btn_filter">กรองข้อมูล</button>
        </div>
        <table class="data-table">
          <thead>
            <tr>
              <th data-i18n="th_date">วันที่เวลา</th>
              <th data-i18n="th_loc">สถานที่</th>
              <th data-i18n="th_event">เหตุการณ์ / บันทึก</th>
              <th data-i18n="th_val">ค่าที่วัดได้</th>
            </tr>
          </thead>
          <tbody>
            <tr><td class="mono">05.09.2026 14:12</td><td>บางปะกง GIS-B3</td><td>แจ้งเตือนแรงดันตกต่ำกว่าเกณฑ์</td><td class="mono">552 kPa</td></tr>
            <tr><td class="mono">04.09.2026 10:30</td><td>บางปะกง GIS-B1</td><td>ดำเนินการ Regeneration สำเร็จ</td><td class="mono">6.2 kg</td></tr>
          </tbody>
        </table>
      </div>
    </div>

    <div id="page-reports" class="page-view">
      <div class="page-header">
        <div>
          <h1 class="page-title" data-i18n="p4_title">รายงานและการส่งออกข้อมูล (Export)</h1>
          <div class="page-sub" data-i18n="p4_sub">ดึงข้อมูลสถิติและ Audit Log ลงไฟล์ Excel สำหรับยื่น อบก.</div>
        </div>
        <button class="btn-export" onclick="exportToExcel()">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="12" y1="18" x2="12" y2="12"/><line x1="9" y1="15" x2="15" y2="15"/></svg>
          <span data-i18n="btn_excel">ดาวน์โหลด Excel (CSV)</span>
        </button>
      </div>

      <div class="card">
        <h3 class="card-title" style="margin-bottom:12px;" data-i18n="rep_sample">ตัวอย่างข้อมูลเตรียมส่งออก</h3>
        <table class="data-table" id="exportTable">
          <thead>
            <tr>
              <th data-i18n="th_date">วันที่</th>
              <th data-i18n="th_st">สถานี</th>
              <th data-i18n="th_rec">ก๊าซกู้คืน (kg)</th>
              <th data-i18n="th_tco2">ลดคาร์บอน (tCO2e)</th>
              <th data-i18n="th_stat">สถานะตรวจสอบ</th>
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

<div id="mobilePlatform" style="display:none;">
  <div class="mobile-shell">
    <div class="mob-header">
      <div style="font-weight:700; font-size:16px;">SF6 Field App</div>
      <div style="font-size:11px; color:#94A3B8;">EGAT Technician Mode</div>
    </div>
    <div class="mob-body">
      <div class="mob-card">
        <div style="font-size:11px; color:var(--danger); font-weight:700;" data-i18n="mob_urg">ความเร่งด่วน: สูงมาก</div>
        <div style="font-weight:700; font-size:15px; margin:4px 0;">บางปะกง · GIS-B3</div>
        <div style="font-size:12px; color:var(--text-muted);" data-i18n="mob_desc">ตรวจพบอัตราการรั่วไหลสูง ต้องเข้าทำ Regeneration ภายใน 5 วัน</div>
        <button style="width:100%; margin-top:12px; background:var(--navy-dark); color:#fff; border:none; padding:10px; border-radius:6px; font-weight:600;" data-i18n="mob_btn">สแกนถัง / บันทึกงาน</button>
      </div>
    </div>
    <div class="mob-nav">
      <div class="mob-tab active" data-i18n="m_tab1">งานของฉัน</div>
      <div class="mob-tab" data-i18n="m_tab2">สแกน QR</div>
      <div class="mob-tab" data-i18n="m_tab3">ประวัติ</div>
    </div>
  </div>
</div>

<script>
  // --- Platform Switcher (Web / Mobile) ---
  function switchPlatform(type) {
    document.querySelectorAll('.platform-btn').forEach(b => b.classList.remove('active'));
    if(type === 'web') {
      document.getElementById('webPlatform').style.display = 'grid';
      document.getElementById('mobilePlatform').style.display = 'none';
      event.target.classList.add('active');
    } else {
      document.getElementById('webPlatform').style.display = 'none';
      document.getElementById('mobilePlatform').style.display = 'flex';
      event.target.classList.add('active');
    }
  }

  // --- Page Switcher (Web Dashboard Sub-menus) ---
  function switchPage(pageId, el) {
    document.querySelectorAll('.sidebar .nav-item').forEach(i => i.classList.remove('active'));
    document.querySelectorAll('.page-view').forEach(p => p.classList.remove('active'));
    el.classList.add('active');
    document.getElementById('page-' + pageId).classList.add('active');
  }

  // --- Export Table to Excel (CSV) ---
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
    downloadLink.download = "SmartSF6_Audit_Report.csv";
    downloadLink.href = window.URL.createObjectURL(csvFile);
    downloadLink.style.display = "none";
    document.body.appendChild(downloadLink);
    downloadLink.click();
    document.body.removeChild(downloadLink);
  }

  // --- Language Toggle Dictionary ---
  const langData = {
    "th": {
      "sw_web": "Web Dashboard", "sw_mob": "Mobile App",
      "nav_dash": "แดชบอร์ดหลัก", "nav_stations": "สถานีและเซนเซอร์", "nav_history": "ข้อมูลย้อนหลัง", "nav_reports": "รายงานและ Export",
      "p1_title": "ภาพรวมระบบกู้คืนก๊าซ SF6", "p1_sub": "ข้อมูลเรียลไทม์จาก 24 สถานีไฟฟ้าแรงสูง ทั่วประเทศ",
      "kpi_1": "SF6 กู้คืนสะสม", "kpi_2": "ลดคาร์บอนเทียบเท่า", "kpi_3": "มูลค่าคาร์บอนเครดิต", "kpi_4": "สถานะเซนเซอร์ปกติ",
      "chart_title": "แนวโน้มการลดก๊าซเรือนกระจก (tCO2e)", "risk_title": "จุดเสี่ยงที่ต้องเฝ้าระวัง",
      "p2_title": "สถานะสถานีไฟฟ้าและถัง SF6", "p2_sub": "รายละเอียดแรงดันและอุณหภูมิรายถัง",
      "th_st": "สถานีไฟฟ้า", "th_bay": "Bay / Tank ID", "th_press": "แรงดัน (kPa)", "th_temp": "อุณหภูมิ (°C)", "th_stat": "สถานะ",
      "st_high": "เสี่ยงสูง", "st_norm": "ปกติ",
      "p3_title": "ประวัติข้อมูลย้อนหลัง", "p3_sub": "เรียกดูข้อมูลเซนเซอร์และกิจกรรมการบำรุงรักษาย้อนหลัง", "btn_filter": "กรองข้อมูล",
      "th_date": "วันที่เวลา", "th_loc": "สถานที่", "th_event": "เหตุการณ์ / บันทึก", "th_val": "ค่าที่วัดได้",
      "p4_title": "รายงานและการส่งออกข้อมูล (Export)", "p4_sub": "ดึงข้อมูลสถิติและ Audit Log ลงไฟล์ Excel สำหรับยื่น อบก.", "btn_excel": "ดาวน์โหลด Excel (CSV)",
      "rep_sample": "ตัวอย่างข้อมูลเตรียมส่งออก", "th_rec": "ก๊าซกู้คืน (kg)", "th_tco2": "ลดคาร์บอน (tCO2e)",
      "mob_urg": "ความเร่งด่วน: สูงมาก", "mob_desc": "ตรวจพบอัตราการรั่วไหลสูง ต้องเข้าทำ Regeneration ภายใน 5 วัน", "mob_btn": "สแกนถัง / บันทึกงาน",
      "m_tab1": "งานของฉัน", "m_tab2": "สแกน QR", "m_tab3": "ประวัติ"
    },
    "en": {
      "sw_web": "Web Dashboard", "sw_mob": "Mobile App",
      "nav_dash": "Main Dashboard", "nav_stations": "Stations & Sensors", "nav_history": "Historical Data", "nav_reports": "Reports & Export",
      "p1_title": "SF6 Gas Recovery Overview", "p1_sub": "Real-time data from 24 substations nationwide",
      "kpi_1": "Total Recovered SF6", "kpi_2": "Carbon Offset Equivalent", "kpi_3": "Carbon Credit Value", "kpi_4": "Sensors Online",
      "chart_title": "GHG Reduction Trend (tCO2e)", "risk_title": "Predictive Risk Alerts",
      "p2_title": "Substation & Tank Status", "p2_sub": "Detailed pressure and temperature per tank",
      "th_st": "Substation", "th_bay": "Bay / Tank ID", "th_press": "Pressure (kPa)", "th_temp": "Temp (°C)", "th_stat": "Status",
      "st_high": "High Risk", "st_norm": "Normal",
      "p3_title": "Historical Data Logs", "p3_sub": "Review past sensor readings and maintenance activities", "btn_filter": "Filter",
      "th_date": "Timestamp", "th_loc": "Location", "th_event": "Event / Description", "th_val": "Recorded Value",
      "p4_title": "Reports & Data Export", "p4_sub": "Export statistics and audit logs to Excel for T-VER verification", "btn_excel": "Download Excel (CSV)",
      "rep_sample": "Export Data Preview", "th_rec": "Recovered (kg)", "th_tco2": "Offset (tCO2e)",
      "mob_urg": "Priority: Critical", "mob_desc": "High leakage rate detected. Regeneration required within 5 days.", "mob_btn": "Scan Tank / Log Task",
      "m_tab1": "My Tasks", "m_tab2": "Scan QR", "m_tab3": "History"
    }
  };

  let currentLang = "th";
  function toggleLanguage() {
    currentLang = currentLang === "th" ? "en" : "th";
    document.getElementById('langToggle').innerText = currentLang === "th" ? "EN" : "TH";
    document.querySelectorAll('[data-i18n]').forEach(el => {
      let key = el.getAttribute('data-i18n');
      if(langData[currentLang][key]) {
        el.innerHTML = langData[currentLang][key];
      }
    });
  }

  // --- Chart Initialization ---
  document.addEventListener("DOMContentLoaded", function() {
    const ctx = document.getElementById('mainChart');
    if(ctx) {
      new Chart(ctx, {
        type:'line',
        data:{
          labels:['Apr','May','Jun','Jul','Aug','Sep'],
          datasets:[
            { label:'Actual', data:[2410,2680,2790,3020,3095,3351], borderColor:'#0A192F', backgroundColor:'rgba(245,158,11,0.1)', fill:true, tension:0.4, borderWidth:2.5 },
            { label:'Target', data:[2500,2700,2900,3100,3300,3500], borderColor:'#94A3B8', borderDash:[5,5], fill:false, tension:0.4, borderWidth:2 }
          ]
        },
        options:{ responsive:true, plugins:{ legend:{display:false} }, scales:{ x:{ grid:{display:false} }, y:{ grid:{color:'#E2E8F0'} } } }
      });
    }
  });
</script>
</body>
</html>

