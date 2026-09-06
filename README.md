# smart-sf6-prototype-
นี่คือโค้ดฉบับสมบูรณ์ที่รวม ธีมสีสว่าง-เหลือง กฟผ. (Light/Yellow) เข้ากับ โค้ดทั้งหมด (Dashboard + Mobile App แบบเต็ม) และใส่ ระบบเปลี่ยนภาษา (TH/EN) ไว้ที่เมนูหลัก หัวข้อ กราฟ และปุ่มกดต่างๆ เรียบร้อยแล้วครับ
คุณสามารถกดปุ่ม "Copy" (มุมขวาบนของกล่องโค้ด) แล้วนำไปวางทับในไฟล์ index.html บน GitHub ของคุณได้เลยครับ
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
    padding: 6px 14px; font-size: 13px; font-weight: 700; cursor: pointer; transition: 0.2s;
  }
  .lang-toggle:hover { background: #F2A900; color: #fff; }

  .tabs{display:flex; gap:4px; background:var(--dk-bg); border:1px solid var(--dk-border); border-radius:10px; padding:4px;}
  .tab-btn{
    font-family:'Space Grotesk',sans-serif; font-size:13px; font-weight:600; letter-spacing:.02em;
    color:var(--dk-muted); background:transparent; border:none; padding:8px 18px; border-radius:7px; cursor:pointer;
    transition:background .15s, color .15s;
  }
  .tab-btn.active{background:var(--dk-teal); color:#fff;}
  .tab-btn:not(.active):hover{color:var(--dk-text);}

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
  .brand-sub{font-size:11px; color:var(--dk-muted); padding-left:39px;}
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
  
  .station-select{margin-top:auto; padding:12px; background:var(--dk-surface2); border:1px solid var(--dk-border); border-radius:10px;}
  .station-select label{font-size:10px; color:var(--dk-muted); text-transform:uppercase; letter-spacing:.06em;}
  .station-select select{
    width:100%; margin-top:6px; background:transparent; border:none; color:var(--dk-text);
    font-size:13px; font-family:inherit; outline:none;
  }

  .dash-main{padding:24px 32px 48px; overflow-x:hidden;}
  .dash-panel{display:none;}
  .dash-panel.active{display:block; animation:fadein .2s ease;}
  .dash-header{display:flex; align-items:center; justify-content:space-between; margin-bottom:22px; flex-wrap:wrap; gap:12px;}
  .dash-header h1{font-size:22px; font-weight:600;}
  .dash-header .subtitle{font-size:13px; color:var(--dk-muted); margin-top:3px;}
  .range-controls{display:flex; gap:8px;}
  .chip{padding:7px 14px; border-radius:8px; border:1px solid var(--dk-border); background:var(--dk-surface); font-size:12.5px; color:var(--dk-muted); cursor:pointer;}
  .chip.active{background:var(--dk-teal); color:#fff; border-color:var(--dk-teal); font-weight:600;}

  .alert-banner{
    display:flex; align-items:center; gap:12px; background:rgba(229,72,77,.12); border:1px solid rgba(229,72,77,.35);
    color:#D64545; padding:12px 16px; border-radius:10px; font-size:13px; margin-bottom:22px;
  }
  .alert-banner b{color:#E5484D;}
  .alert-banner .go{margin-left:auto; font-size:12px; color:var(--dk-text); background:rgba(0,0,0,.05); padding:6px 12px; border-radius:6px; cursor:pointer; white-space:nowrap;}

  .kpi-row{display:grid; grid-template-columns:repeat(4,1fr); gap:16px; margin-bottom:22px;}
  .kpi-card{background:var(--dk-surface); border:1px solid var(--dk-border); border-radius:14px; padding:18px 18px 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.02);}
  .kpi-label{font-size:12px; color:var(--dk-muted); display:flex; align-items:center; justify-content:space-between;}
  .kpi-trend{font-size:11px; padding:2px 7px; border-radius:20px; font-weight:600;}
  .kpi-trend.up{background:rgba(242,169,0,.15); color:var(--dk-teal);}
  .kpi-trend.warn{background:rgba(240,169,62,.15); color:var(--dk-amber);}
  .kpi-value{font-size:26px; font-weight:600; margin-top:10px; font-family:'IBM Plex Mono',monospace;}
  .kpi-unit{font-size:13px; color:var(--dk-muted); font-weight:400; margin-left:4px;}
  .kpi-foot{font-size:11.5px; color:#5C6774; margin-top:8px;}

  .grid-2{display:grid; grid-template-columns:1.4fr 1fr; gap:16px; margin-bottom:22px;}
  .panel{background:var(--dk-surface); border:1px solid var(--dk-border); border-radius:14px; padding:20px; box-shadow: 0 4px 12px rgba(0,0,0,0.02);}
  .panel-head{display:flex; align-items:center; justify-content:space-between; margin-bottom:16px;}
  .panel-head h3{font-size:14.5px; font-weight:600;}
  .panel-head .tag{font-size:11px; color:var(--dk-muted);}
  .legend-dot{display:inline-block; width:8px; height:8px; border-radius:50%; margin-right:6px;}

  .risk-list{display:flex; flex-direction:column; gap:10px;}
  .risk-row{display:flex; align-items:center; gap:12px; padding:10px 12px; background:var(--dk-surface2); border-radius:10px; border:1px solid var(--dk-border);}
  .risk-badge{width:8px; height:32px; border-radius:4px; flex-shrink:0;}
  .risk-badge.high{background:var(--dk-red);}
  .risk-badge.mid{background:var(--dk-amber);}
  .risk-info{flex:1;}
  .risk-info .name{font-size:13px; font-weight:600;}
  .risk-info .meta{font-size:11.5px; color:var(--dk-muted); margin-top:2px;}
  .risk-days{font-family:'IBM Plex Mono',monospace; font-size:13px; font-weight:600; text-align:right;}
  .risk-days.high{color:#E5484D;}
  .risk-days.mid{color:var(--dk-amber);}
  .risk-days small{display:block; font-size:10px; color:var(--dk-muted); font-weight:400; font-family:'IBM Plex Sans',sans-serif;}

  table.data-table{width:100%; border-collapse:collapse; font-size:13px;}
  table.data-table th{
    text-align:left; font-size:11px; text-transform:uppercase; letter-spacing:.05em; color:var(--dk-muted);
    padding:0 12px 10px; font-weight:600; border-bottom:1px solid var(--dk-border);
  }
  table.data-table td{padding:12px; border-bottom:1px solid var(--dk-border);}
  table.data-table tr:last-child td{border-bottom:none;}
  .status-pill{font-size:11px; padding:3px 10px; border-radius:20px; font-weight:600;}
  .status-pill.verified{background:rgba(242,169,0,.15); color:var(--dk-teal);}
  .status-pill.pending{background:rgba(240,169,62,.15); color:var(--dk-amber);}
  .mono-cell{font-family:'IBM Plex Mono',monospace;}

  /* =========================================================
     MOBILE APP
     ========================================================= */
  #mobile{ background:var(--dk-bg); min-height:calc(100vh - 57px); display:flex; align-items:flex-start; justify-content:center; gap:48px; padding:48px 24px; flex-wrap:wrap;}
  .phone{
    width:375px; height:780px; background:var(--mb-bg); border-radius:44px; border:10px solid #DDE1DA;
    box-shadow:0 20px 40px rgba(0,0,0,.08); position:relative; overflow:hidden; flex-shrink:0;
  }
  .phone-notch{position:absolute; top:0; left:50%; transform:translateX(-50%); width:120px; height:22px; background:#DDE1DA; border-radius:0 0 14px 14px; z-index:20;}
  .phone-screen{height:100%; display:flex; flex-direction:column; font-family:'IBM Plex Sans','IBM Plex Sans Thai',sans-serif; color:var(--mb-text);}
  .status-bar{padding:14px 22px 4px; display:flex; justify-content:space-between; font-size:12px; font-weight:600; color:#333;}

  .app-header{padding:10px 20px 14px;}
  .app-header .greet{font-size:12px; color:var(--mb-muted);}
  .app-header h2{font-size:20px; font-weight:700; margin-top:2px;}
  .sync-pill{
    display:inline-flex; align-items:center; gap:6px; margin-top:10px; font-size:11px; font-weight:600;
    background:rgba(47,158,104,.12); color:var(--mb-green); padding:5px 10px; border-radius:20px;
  }
  .sync-pill .dot{width:6px;height:6px;border-radius:50%; background:var(--mb-green);}
  .sync-pill.off{background:rgba(107,117,128,.14); color:var(--mb-muted);}
  .sync-pill.off .dot{background:var(--mb-muted);}

  .app-body{flex:1; overflow-y:auto; padding:4px 20px 90px;}
  .section-title{font-size:12px; font-weight:700; text-transform:uppercase; letter-spacing:.04em; color:var(--mb-muted); margin:16px 0 10px;}

  .alert-card{
    display:flex; gap:12px; background:var(--mb-surface); border:1px solid var(--mb-border); border-radius:16px;
    padding:14px; margin-bottom:10px; box-shadow:0 1px 2px rgba(20,20,10,.04);
  }
  .alert-stripe{width:5px; border-radius:4px; flex-shrink:0;}
  .alert-stripe.red{background:var(--mb-red);}
  .alert-stripe.amber{background:var(--mb-amber);}
  .alert-stripe.green{background:var(--mb-green);}
  .alert-body{flex:1;}
  .alert-top{display:flex; justify-content:space-between; align-items:flex-start;}
  .alert-top .station{font-size:14px; font-weight:700;}
  .alert-days{font-size:11px; font-weight:700; padding:3px 9px; border-radius:20px; white-space:nowrap;}
  .alert-days.red{background:rgba(214,69,69,.12); color:var(--mb-red);}
  .alert-days.amber{background:rgba(226,169,59,.14); color:#A9781E;}
  .alert-meta{font-size:12px; color:var(--mb-muted); margin-top:4px;}
  .alert-actions{display:flex; gap:8px; margin-top:10px;}
  .btn-mini{font-size:11.5px; font-weight:600; padding:7px 12px; border-radius:8px; border:1px solid var(--mb-border); background:#F7F7F5; color:var(--mb-text); cursor:pointer;}
  .btn-mini.primary{background:var(--mb-orange); color:#fff; border-color:var(--mb-orange);}

  .qr-screen{display:flex; flex-direction:column; align-items:center; padding-top:6px;}
  .qr-frame{
    width:230px; height:230px; border-radius:24px; background:var(--mb-bg); position:relative; margin:14px 0 18px;
    display:flex; align-items:center; justify-content:center;
  }
  .qr-corner{position:absolute; width:28px; height:28px; border:3px solid var(--mb-orange);}
  .qr-corner.tl{top:14px; left:14px; border-right:none; border-bottom:none; border-radius:8px 0 0 0;}
  .qr-corner.tr{top:14px; right:14px; border-left:none; border-bottom:none; border-radius:0 8px 0 0;}
  .qr-corner.bl{bottom:14px; left:14px; border-right:none; border-top:none; border-radius:0 0 0 8px;}
  .qr-corner.br{bottom:14px; right:14px; border-left:none; border-top:none; border-radius:0 0 8px 0;}
  .qr-scan-line{position:absolute; left:20px; right:20px; height:2px; background:var(--mb-orange); box-shadow:0 0 8px var(--mb-orange); animation:scan 2.2s ease-in-out infinite;}
  @keyframes scan{0%{top:20%;}50%{top:78%;}100%{top:20%;}}
  .qr-hint{font-size:13px; color:var(--mb-muted); text-align:center; padding:0 30px;}

  .tank-card{background:var(--mb-surface); border:1px solid var(--mb-border); border-radius:16px; padding:16px; margin-top:16px;}
  .tank-card .id{font-size:11px; color:var(--mb-muted); font-family:'IBM Plex Mono',monospace;}
  .tank-card .name{font-size:15px; font-weight:700; margin-top:2px;}
  .tank-row{display:flex; justify-content:space-between; font-size:12.5px; padding:8px 0; border-top:1px solid var(--mb-border);}
  .tank-row span:first-child{color:var(--mb-muted);}
  .tank-row span:last-child{font-weight:600; font-family:'IBM Plex Mono',monospace;}

  .form-card{background:var(--mb-surface); border:1px solid var(--mb-border); border-radius:16px; padding:16px; margin-top:14px;}
  .field{margin-bottom:14px;}
  .field label{font-size:11.5px; font-weight:600; color:var(--mb-muted); display:block; margin-bottom:6px;}
  .field .input{
    background:#F6F6F3; border:1px solid var(--mb-border); border-radius:10px; padding:11px 12px; font-size:14px;
    font-family:'IBM Plex Mono',monospace; color:var(--mb-text); display:flex; justify-content:space-between;
  }
  .field .input span{color:var(--mb-muted); font-family:'IBM Plex Sans',sans-serif; font-size:12px;}
  .photo-row{display:flex; gap:8px; margin-top:4px;}
  .photo-slot{width:60px;height:60px;border-radius:10px; background:#F0F0EC; border:1.5px dashed #C9CDC4; display:flex; align-items:center; justify-content:center; font-size:20px; color:#A7ACA1;}
  .photo-slot.filled{background:#E4EAE3; border-style:solid; border-color:var(--mb-green); color:var(--mb-green); font-size:13px; font-weight:700;}

  .estimate-box{
    background:linear-gradient(135deg,#FFF7E0,#FDE08B); color:#1B2430; border-radius:14px; padding:16px; margin-top:14px;
  }
  .estimate-box .label{font-size:11px; color:#6B7580; text-transform:uppercase; letter-spacing:.05em;}
  .estimate-box .value{font-family:'IBM Plex Mono',monospace; font-size:22px; font-weight:600; margin-top:4px;}
  .estimate-box .sub{font-size:11.5px; color:#6B7580; margin-top:6px;}

  .btn-submit{
    width:100%; background:var(--mb-orange); color:#fff; border:none; border-radius:12px; padding:14px;
    font-size:14.5px; font-weight:700; margin-top:16px; font-family:'Space Grotesk',sans-serif; letter-spacing:.01em;
    cursor:pointer; transition:0.2s;
  }
  .btn-submit:hover{background:var(--mb-orange-dk);}

  .tabbar{
    position:absolute; bottom:0; left:0; right:0; background:rgba(255,255,255,.92); backdrop-filter:blur(8px);
    border-top:1px solid var(--mb-border); display:flex; padding:10px 8px 20px;
  }
  .tabbar-item{flex:1; display:flex; flex-direction:column; align-items:center; gap:4px; font-size:10px; color:#A9AEA5; font-weight:600; cursor:pointer;}
  .tabbar-item.active{color:var(--mb-orange-dk);}
  .tabbar-item svg{width:20px; height:20px;}

  .screen-picker{display:flex; flex-direction:column; gap:10px; width:180px; flex-shrink:0;}
  .screen-picker h4{font-size:12px; text-transform:uppercase; letter-spacing:.05em; color:#5C6774; margin-bottom:2px;}
  .screen-btn{
    text-align:left; background:var(--dk-surface); border:1px solid var(--dk-border); color:var(--dk-muted);
    padding:10px 12px; border-radius:10px; font-size:12.5px; cursor:pointer;
  }
  .screen-btn.active{background:var(--dk-teal); color:#fff; font-weight:600; border-color:var(--dk-teal);}

  @media (max-width: 900px){
    .dash-shell{grid-template-columns:1fr;}
    .sidebar{display:none;}
    .kpi-row{grid-template-columns:1fr 1fr;}
    .grid-2{grid-template-columns:1fr;}
  }
</style>
</head>
<body>

<div class="switcher-bar">
  <div class="switcher-title"><span class="dot"></span> <span data-i18n="app_title">Smart SF6 Leakage &amp; Regeneration — Prototype</span></div>
  <div class="top-actions">
    <button id="langToggleBtn" class="lang-toggle" onclick="toggleLanguage()">EN</button>
    <div class="tabs">
      <button class="tab-btn active" data-view="dashboard" data-i18n="tab_web">Web Dashboard</button>
      <button class="tab-btn" data-view="mobile" data-i18n="tab_mobile">Mobile App</button>
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
      <div class="brand-sub">EGAT · Monitoring Platform</div>
    </div>

    <div class="nav-group">
      <div class="nav-label" data-i18n="menu_overview_lbl">Overview</div>
      <div class="nav-item active" data-panel="overview">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg>
        <span data-i18n="menu_dashboard">Dashboard</span>
      </div>
      <div class="nav-item" data-panel="assets">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 20l-5.5-2V6L9 8m0 12l6-2m-6 2V8m6 10l5.5 2V8L15 6m0 12V6m0 0L9 8"/></svg>
        <span data-i18n="menu_assets">Asset / Station</span>
      </div>
      <div class="nav-item" data-panel="workorders">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 9v4m0 4h.01M10.3 3.9L2.6 17.5a1.5 1.5 0 001.3 2.3h16.2a1.5 1.5 0 001.3-2.3L13.7 3.9a1.5 1.5 0 00-2.6 0z"/></svg>
        <span data-i18n="menu_workorders">Alerts &amp; Work Orders</span>
      </div>
      <div class="nav-item" data-panel="reports">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 3v18h18M7 15l4-6 3 3 5-8"/></svg>
        <span data-i18n="menu_reports">Reports</span>
      </div>
    </div>

    <div class="nav-group">
      <div class="nav-label" data-i18n="menu_sys_lbl">System</div>
      <div class="nav-item" data-panel="devices">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="8"/><path d="M12 8v4l3 2"/></svg>
        <span data-i18n="menu_devices">Device Registry</span>
      </div>
      <div class="nav-item" data-panel="admin">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="4"/><path d="M4 21v-1a6 6 0 016-6h4a6 6 0 016 6v1"/></svg>
        <span data-i18n="menu_admin">Admin</span>
      </div>
    </div>

    <div class="station-select">
      <label data-i18n="lbl_station">สถานีที่แสดงผล</label>
      <select>
        <option data-i18n="opt_all">ทุกสถานี (24 GIS Bays)</option>
        <option data-i18n="opt_bpk">สถานีไฟฟ้าแรงสูงบางปะกง</option>
        <option data-i18n="opt_nma">สถานีไฟฟ้าแรงสูงพระนครเหนือ</option>
      </select>
    </div>
  </aside>

  <main class="dash-main">
  <div class="dash-panel active" data-panel="overview">
    <div class="dash-header">
      <div>
        <h1 data-i18n="dash_h1">ภาพรวมองค์กร — Carbon &amp; Predictive Insight</h1>
        <div class="subtitle" data-i18n="dash_sub">อัปเดตล่าสุด 06 ก.ย. 2569, 09:42 น. · ข้อมูลจาก 24 สถานี / 312 จุดวัด</div>
      </div>
      <div class="range-controls">
        <div class="chip" data-i18n="chip_wk">รายสัปดาห์</div>
        <div class="chip active" data-i18n="chip_mo">รายเดือน</div>
        <div class="chip" data-i18n="chip_yr">รายปี</div>
      </div>
    </div>

    <div class="alert-banner">
      ⚠️ <span data-i18n="alert_banner"><b>ความเสี่ยงสูง 2 จุด</b> คาดการณ์ความดันจะต่ำกว่าเกณฑ์ปลอดภัยภายใน 7 วัน — สถานีบางปะกง GIS-B3, สถานีพระนครเหนือ GIS-A1</span>
      <div class="go" onclick="document.querySelector('.nav-item[data-panel=workorders]').click()" data-i18n="alert_go">ดู Work Order →</div>
    </div>

    <div class="kpi-row">
      <div class="kpi-card">
        <div class="kpi-label"><span data-i18n="kpi1">SF6 กู้คืนสะสม</span> <span class="kpi-trend up">+8.2%</span></div>
        <div class="kpi-value">142.6<span class="kpi-unit">kg</span></div>
        <div class="kpi-foot" data-i18n="kpi1_f">เทียบเดือนก่อน 131.8 kg</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label"><span data-i18n="kpi2">เทียบเท่าคาร์บอน</span> <span class="kpi-trend up">+8.2%</span></div>
        <div class="kpi-value">3,351<span class="kpi-unit">tCO2e</span></div>
        <div class="kpi-foot" data-i18n="kpi2_f">GWP อ้างอิง 23,500</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label" data-i18n="kpi3">มูลค่าคาร์บอนเครดิต</div>
        <div class="kpi-value">335,050<span class="kpi-unit" data-i18n="kpi3_u">บาท</span></div>
        <div class="kpi-foot" data-i18n="kpi3_f">ราคาประเมิน 100 บาท/ตัน (T-VER)</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label"><span data-i18n="kpi4">ROI สะสม</span> <span class="kpi-trend warn" data-i18n="kpi4_t">คืนทุน 2.1y</span></div>
        <div class="kpi-value">18.4<span class="kpi-unit">%</span></div>
        <div class="kpi-foot" data-i18n="kpi4_f">เทียบต้นทุนติดตั้งระบบ</div>
      </div>
    </div>

    <div class="grid-2">
      <div class="panel">
        <div class="panel-head">
          <h3 data-i18n="chart_title">แนวโน้มการลดคาร์บอนเครดิต (tCO2e)</h3>
          <div class="tag"><span class="legend-dot" style="background:#F2A900"></span><span data-i18n="chart_l1">Actual</span> &nbsp;&nbsp;<span class="legend-dot" style="background:#8A97A6"></span><span data-i18n="chart_l2">Target</span></div>
        </div>
        <canvas id="carbonChart" height="150"></canvas>
      </div>
      <div class="panel">
        <div class="panel-head">
          <h3 data-i18n="risk_title">Risk Ranking (Predictive)</h3>
          <div class="tag" data-i18n="risk_tag">7–14 วันข้างหน้า</div>
        </div>
        <div class="risk-list">
          <div class="risk-row">
            <div class="risk-badge high"></div>
            <div class="risk-info"><div class="name" data-i18n="risk1_n">บางปะกง · GIS-B3</div><div class="meta" data-i18n="risk1_m">Pressure trend -4.1%/wk</div></div>
            <div class="risk-days high">5<small data-i18n="risk_day">วัน</small></div>
          </div>
          <div class="risk-row">
            <div class="risk-badge high"></div>
            <div class="risk-info"><div class="name" data-i18n="risk2_n">พระนครเหนือ · GIS-A1</div><div class="meta" data-i18n="risk2_m">Pressure trend -3.6%/wk</div></div>
            <div class="risk-days high">7<small data-i18n="risk_day">วัน</small></div>
          </div>
          <div class="risk-row">
            <div class="risk-badge mid"></div>
            <div class="risk-info"><div class="name" data-i18n="risk3_n">สุราษฎร์ธานี · GIS-C2</div><div class="meta" data-i18n="risk3_m">Pressure trend -1.8%/wk</div></div>
            <div class="risk-days mid">12<small data-i18n="risk_day">วัน</small></div>
          </div>
          <div class="risk-row">
            <div class="risk-badge mid"></div>
            <div class="risk-info"><div class="name" data-i18n="risk4_n">ชลบุรี 2 · GIS-A4</div><div class="meta" data-i18n="risk4_m">Pressure trend -1.2%/wk</div></div>
            <div class="risk-days mid">14<small data-i18n="risk_day">วัน</small></div>
          </div>
        </div>
      </div>
    </div>

    <div class="panel">
      <div class="panel-head">
        <h3 data-i18n="tbl_title">รายละเอียดการทำ Regeneration ล่าสุด</h3>
        <div class="tag" data-i18n="tbl_tag">แสดง 5 จาก 47 รายการ</div>
      </div>
      <table class="data-table">
        <thead><tr>
          <th data-i18n="th_date">วันที่</th>
          <th data-i18n="th_station">สถานี / ถัง</th>
          <th data-i18n="th_tech">ช่างผู้ปฏิบัติ</th>
          <th data-i18n="th_rec">ปริมาณกู้คืน</th>
          <th data-i18n="th_credit">คาร์บอนเครดิต</th>
          <th data-i18n="th_stat">สถานะ</th>
        </tr></thead>
        <tbody>
          <tr><td data-i18n="td_d1">04 ก.ย. 2569</td><td class="mono-cell" data-i18n="td_s1">บางปะกง / GIS-B1</td><td data-i18n="td_n1">สมชาย ใจดี</td><td class="mono-cell">6.2 kg</td><td class="mono-cell" data-i18n="td_c1">14,570 บาท</td><td><span class="status-pill verified">Verified</span></td></tr>
          <tr><td data-i18n="td_d2">02 ก.ย. 2569</td><td class="mono-cell" data-i18n="td_s2">พระนครเหนือ / GIS-A3</td><td data-i18n="td_n2">วิชัย ทองดี</td><td class="mono-cell">4.8 kg</td><td class="mono-cell" data-i18n="td_c2">11,280 บาท</td><td><span class="status-pill pending">Pending</span></td></tr>
          <tr><td data-i18n="td_d3">30 ส.ค. 2569</td><td class="mono-cell" data-i18n="td_s3">ชลบุรี 2 / GIS-A2</td><td data-i18n="td_n3">ประยุทธ แสงทอง</td><td class="mono-cell">3.1 kg</td><td class="mono-cell" data-i18n="td_c3">7,285 บาท</td><td><span class="status-pill verified">Verified</span></td></tr>
        </tbody>
      </table>
    </div>
  </div>

  <div class="dash-panel" data-panel="assets">
    <div class="dash-header">
      <div>
        <h1 data-i18n="ast_h1">Asset / Station Explorer</h1>
        <div class="subtitle" data-i18n="ast_sub">24 สถานี · 47 GIS Bay · 312 จุดวัด</div>
      </div>
    </div>
    <div class="kpi-row" style="grid-template-columns:repeat(3,1fr);">
      <div class="kpi-card"><div class="kpi-label" data-i18n="ast_k1">สถานีทั้งหมด</div><div class="kpi-value">24</div></div>
      <div class="kpi-card"><div class="kpi-label" data-i18n="ast_k2">ถัง SF6 ที่มอนิเตอร์</div><div class="kpi-value">312</div></div>
      <div class="kpi-card"><div class="kpi-label" data-i18n="ast_k3">อุปกรณ์ออฟไลน์</div><div class="kpi-value warn" style="color:#F0A93E">3</div></div>
    </div>
  </div>
  
  <div class="dash-panel" data-panel="workorders">
    <div class="dash-header">
      <div><h1 data-i18n="wo_h1">Alerts &amp; Work Order Management</h1></div>
    </div>
  </div>
  
  <div class="dash-panel" data-panel="reports">
    <div class="dash-header">
      <div><h1 data-i18n="rp_h1">Reports</h1></div>
    </div>
  </div>
  
  <div class="dash-panel" data-panel="devices">
    <div class="dash-header">
      <div><h1 data-i18n="dv_h1">Device Registry</h1></div>
    </div>
  </div>
  
  <div class="dash-panel" data-panel="admin">
    <div class="dash-header">
      <div><h1 data-i18n="ad_h1">Admin — ผู้ใช้งานและสิทธิ์</h1></div>
    </div>
  </div>
  </main>
</div>
</div>

<div class="view" id="mobile">
  <div class="screen-picker">
    <h4 data-i18n="mob_sp_h">Screen</h4>
    <button class="screen-btn active" data-screen="alerts" data-i18n="mob_sp1">1 · Smart Alert</button>
    <button class="screen-btn" data-screen="detail" data-i18n="mob_sp2">2 · รายละเอียดงาน</button>
    <button class="screen-btn" data-screen="qr" data-i18n="mob_sp3">3 · สแกน QR/RFID</button>
    <button class="screen-btn" data-screen="form" data-i18n="mob_sp4">4 · บันทึก Regeneration</button>
    <button class="screen-btn" data-screen="done" data-i18n="mob_sp5">5 · ปิดงาน/Certificate</button>
    <button class="screen-btn" data-screen="history" data-i18n="mob_sp6">6 · ประวัติงาน</button>
  </div>

  <div class="phone">
    <div class="phone-notch"></div>
    <div class="phone-screen">
      <div class="status-bar"><span>9:42</span><span>SF6 Field App</span><span>4G ▮▮▯</span></div>

      <div class="mobile-screen" data-screen="alerts">
        <div class="app-header">
          <div class="greet" data-i18n="mob_greet1">สวัสดี, ช่างสมชาย</div>
          <h2 data-i18n="mob_h2_1">งานของคุณวันนี้</h2>
          <div class="sync-pill"><span class="dot"></span> <span data-i18n="mob_sync1">ออนไลน์ · sync ล่าสุด 2 นาทีที่แล้ว</span></div>
        </div>
        <div class="app-body">
          <div class="section-title" data-i18n="mob_sec1">ความเสี่ยงสูง — ดำเนินการก่อน</div>
          <div class="alert-card">
            <div class="alert-stripe red"></div>
            <div class="alert-body">
              <div class="alert-top"><div class="station" data-i18n="mob_st1">บางปะกง · GIS-B3</div><div class="alert-days red" data-i18n="mob_dy1">5 วัน</div></div>
              <div class="alert-meta" data-i18n="mob_mt1">ความดันลดลงต่อเนื่อง 4.1%/สัปดาห์ · ถังหมายเลข TK-0231</div>
              <div class="alert-actions">
                <button class="btn-mini primary go-screen" data-target="detail" data-i18n="btn_acc">รับงาน</button>
                <button class="btn-mini go-screen" data-target="detail" data-i18n="btn_det">ดูรายละเอียด</button>
              </div>
            </div>
          </div>
          
          <div class="section-title" data-i18n="mob_sec2">ความเสี่ยงปานกลาง</div>
          <div class="alert-card">
            <div class="alert-stripe amber"></div>
            <div class="alert-body">
              <div class="alert-top"><div class="station" data-i18n="mob_st2">สุราษฎร์ธานี · GIS-C2</div><div class="alert-days amber" data-i18n="mob_dy2">12 วัน</div></div>
              <div class="alert-meta" data-i18n="mob_mt2">ความดันลดลงต่อเนื่อง 1.8%/สัปดาห์ · ถังหมายเลข TK-0087</div>
            </div>
          </div>
        </div>
      </div>

      <div class="mobile-screen" data-screen="detail" style="display:none;">
        <div class="app-header">
          <div class="greet" data-i18n="mob_dt_sub">WO-2609-041 · ความเร่งด่วนสูง</div>
          <h2 data-i18n="mob_st1">บางปะกง · GIS-B3</h2>
        </div>
        <div class="app-body">
          <div class="tank-card">
            <div class="id">TK-0231</div>
            <div class="name" data-i18n="mob_dt_n">รายละเอียดงาน</div>
            <div class="tank-row"><span data-i18n="mob_r1">ความดันปัจจุบัน</span><span style="color:#D64545">548 kPa</span></div>
            <div class="tank-row"><span data-i18n="mob_r2">คาดถึงเกณฑ์ต่ำสุดใน</span><span data-i18n="mob_dy1">5 วัน</span></div>
          </div>
          <button class="btn-submit go-screen" data-target="qr" data-i18n="btn_start">เริ่มงาน → สแกนถัง</button>
        </div>
      </div>

      <div class="mobile-screen" data-screen="qr" style="display:none;">
        <div class="app-header">
          <div class="greet" data-i18n="mob_qr_sub">งาน · บางปะกง GIS-B3</div>
          <h2 data-i18n="mob_qr_h">สแกนถัง SF6</h2>
        </div>
        <div class="app-body">
          <div class="qr-screen">
            <div class="qr-frame">
              <div class="qr-corner tl"></div><div class="qr-corner tr"></div>
              <div class="qr-corner bl"></div><div class="qr-corner br"></div>
              <div class="qr-scan-line"></div>
            </div>
            <div class="qr-hint" data-i18n="mob_qr_hint">วางกล้องให้ตรงกับ QR หรือ RFID บนถัง เพื่อดึงประวัติอุปกรณ์อัตโนมัติ</div>
            <button class="btn-submit go-screen" data-target="form" style="margin-top:26px;" data-i18n="btn_next">ดำเนินการต่อ</button>
          </div>
        </div>
      </div>

      <div class="mobile-screen" data-screen="form" style="display:none;">
        <div class="app-header">
          <div class="greet">TK-0231 · GIS-B3</div>
          <h2 data-i18n="mob_fm_h">บันทึก Regeneration</h2>
        </div>
        <div class="app-body">
          <div class="form-card">
            <div class="field"><label data-i18n="mob_f1">ปริมาณก๊าซที่กู้คืน</label><div class="input">6.20 <span>kg</span></div></div>
            <div class="field"><label data-i18n="mob_f2">ความดันหลังดำเนินการ</label><div class="input">618 <span>kPa</span></div></div>
          </div>
          <div class="estimate-box">
            <div class="label" data-i18n="mob_est_l">มูลค่าคาร์บอนเครดิตโดยประมาณ</div>
            <div class="value" data-i18n="mob_est_v">14,570 บาท</div>
          </div>
          <button class="btn-submit go-screen" data-target="done" data-i18n="btn_submit">ส่งบันทึก & ปิดงาน</button>
        </div>
      </div>

      <div class="mobile-screen" data-screen="done" style="display:none;">
        <div class="app-header">
          <div class="greet">TK-0231 · GIS-B3</div>
          <h2 data-i18n="mob_dn_h">ปิดงานสำเร็จ</h2>
        </div>
        <div class="app-body">
          <div style="display:flex;flex-direction:column;align-items:center;padding:18px 0 6px;">
            <div style="width:64px;height:64px;border-radius:50%;background:rgba(47,158,104,.12);display:flex;align-items:center;justify-content:center;font-size:30px;color:var(--mb-green);">✓</div>
            <div style="font-weight:700;font-size:16px;margin-top:12px;" data-i18n="mob_dn_m1">บันทึก Regeneration สำเร็จ</div>
          </div>
          <button class="btn-mini go-screen" data-target="alerts" style="width:100%;margin-top:16px;padding:12px;" data-i18n="btn_home">กลับไปหน้างานของฉัน</button>
        </div>
      </div>

      <div class="mobile-screen" data-screen="history" style="display:none;">
        <div class="app-header">
          <div class="greet" data-i18n="td_n1">สมชาย ใจดี</div>
          <h2 data-i18n="mob_hs_h">ประวัติงานของฉัน</h2>
        </div>
        <div class="app-body">
          <div class="section-title" data-i18n="mob_hs_s">เดือนนี้</div>
          <div class="tank-card">
            <div class="id">RG-2609-0231</div>
            <div class="name" data-i18n="mob_st1">บางปะกง · GIS-B3</div>
            <div class="tank-row"><span data-i18n="mob_f1">ปริมาณกู้คืน</span><span>6.20 kg</span></div>
          </div>
        </div>
      </div>

      <div class="tabbar">
        <div class="tabbar-item active go-screen" data-target="alerts">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 10l9-7 9 7v10a1 1 0 01-1 1h-5v-6H9v6H4a1 1 0 01-1-1z"/></svg>
          <span data-i18n="tab_w">งาน</span>
        </div>
        <div class="tabbar-item go-screen" data-target="qr">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="4" width="6" height="6"/><rect x="14" y="4" width="6" height="6"/><rect x="4" y="14" width="6" height="6"/><rect x="14" y="14" width="6" height="6"/></svg>
          <span data-i18n="tab_s">สแกน</span>
        </div>
        <div class="tabbar-item go-screen" data-target="history">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/></svg>
          <span data-i18n="tab_h">ประวัติ</span>
        </div>
        <div class="tabbar-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="4"/><path d="M4 21v-1a6 6 0 016-6h4a6 6 0 016 6v1"/></svg>
          <span data-i18n="tab_p">โปรไฟล์</span>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
  // View Switcher
  document.querySelectorAll('.tab-btn').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
      document.getElementById(btn.dataset.view).classList.add('active');
    });
  });

  // Mobile Screen Switcher
  function goToScreen(name){
    document.querySelectorAll('.screen-btn').forEach(b=>b.classList.toggle('active', b.dataset.screen === name));
    document.querySelectorAll('.mobile-screen').forEach(s=>{
      s.style.display = (s.dataset.screen === name) ? 'block' : 'none';
    });
    document.querySelectorAll('.tabbar-item[data-target]').forEach(t=>{
      t.classList.toggle('active', t.dataset.target === name || (name!=='qr' && name!=='history' && t.dataset.target==='alerts'));
    });
  }
  document.querySelectorAll('.screen-btn').forEach(btn => btn.addEventListener('click', ()=> goToScreen(btn.dataset.screen)));
  document.querySelectorAll('.go-screen').forEach(btn => btn.addEventListener('click', ()=> goToScreen(btn.dataset.target)));

  // Language Dictionary
  const translations = {
    "th": {
      "app_title": "Smart SF6 Leakage & Regeneration", "tab_web": "Web Dashboard", "tab_mobile": "Mobile App",
      "menu_overview_lbl": "Overview", "menu_dashboard": "Dashboard", "menu_assets": "Asset / Station",
      "menu_workorders": "Alerts & Work Orders", "menu_reports": "Reports", 
      "menu_sys_lbl": "System", "menu_devices": "Device Registry", "menu_admin": "Admin",
      "lbl_station": "สถานีที่แสดงผล", "opt_all": "ทุกสถานี (24 GIS Bays)", "opt_bpk": "บางปะกง", "opt_nma": "พระนครเหนือ",
      "dash_h1": "ภาพรวมองค์กร — Carbon & Predictive Insight", "dash_sub": "อัปเดตล่าสุด 06 ก.ย. 2569 · ข้อมูลจาก 24 สถานี",
      "chip_wk": "รายสัปดาห์", "chip_mo": "รายเดือน", "chip_yr": "รายปี",
      "alert_banner": "<b>ความเสี่ยงสูง 2 จุด</b> คาดการณ์ความดันจะต่ำกว่าเกณฑ์ปลอดภัยภายใน 7 วัน — บางปะกง, พระนครเหนือ", "alert_go": "ดู Work Order →",
      "kpi1": "SF6 กู้คืนสะสม", "kpi1_f": "เทียบเดือนก่อน 131.8 kg",
      "kpi2": "เทียบเท่าคาร์บอน", "kpi2_f": "GWP อ้างอิง 23,500",
      "kpi3": "มูลค่าคาร์บอนเครดิต", "kpi3_u": "บาท", "kpi3_f": "ราคาประเมิน 100 บาท/ตัน",
      "kpi4": "ROI สะสม", "kpi4_t": "คืนทุน 2.1y", "kpi4_f": "เทียบต้นทุนติดตั้งระบบ",
      "chart_title": "แนวโน้มการลดคาร์บอนเครดิต (tCO2e)", "chart_l1": "Actual", "chart_l2": "Target",
      "risk_title": "Risk Ranking (Predictive)", "risk_tag": "7–14 วันข้างหน้า", "risk_day": "วัน",
      "risk1_n": "บางปะกง · GIS-B3", "risk1_m": "Pressure trend -4.1%/wk", "risk2_n": "พระนครเหนือ · GIS-A1", "risk2_m": "Pressure trend -3.6%/wk",
      "risk3_n": "สุราษฎร์ธานี · GIS-C2", "risk3_m": "Pressure trend -1.8%/wk", "risk4_n": "ชลบุรี 2 · GIS-A4", "risk4_m": "Pressure trend -1.2%/wk",
      "tbl_title": "รายละเอียดการทำ Regeneration ล่าสุด", "tbl_tag": "แสดง 5 จาก 47 รายการ",
      "th_date": "วันที่", "th_station": "สถานี / ถัง", "th_tech": "ช่างผู้ปฏิบัติ", "th_rec": "ปริมาณกู้คืน", "th_credit": "คาร์บอนเครดิต", "th_stat": "สถานะ",
      "td_d1": "04 ก.ย. 2569", "td_s1": "บางปะกง / GIS-B1", "td_n1": "สมชาย ใจดี", "td_c1": "14,570 บาท",
      "td_d2": "02 ก.ย. 2569", "td_s2": "พระนครเหนือ / GIS-A3", "td_n2": "วิชัย ทองดี", "td_c2": "11,280 บาท",
      "td_d3": "30 ส.ค. 2569", "td_s3": "ชลบุรี 2 / GIS-A2", "td_n3": "ประยุทธ แสงทอง", "td_c3": "7,285 บาท",
      "ast_h1": "Asset / Station Explorer", "ast_sub": "24 สถานี · 47 GIS Bay · 312 จุดวัด",
      "ast_k1": "สถานีทั้งหมด", "ast_k2": "ถัง SF6 ที่มอนิเตอร์", "ast_k3": "อุปกรณ์ออฟไลน์",
      "wo_h1": "Alerts & Work Order Management", "rp_h1": "Reports", "dv_h1": "Device Registry", "ad_h1": "Admin — ผู้ใช้งานและสิทธิ์",
      
      "mob_sp_h": "Screen", "mob_sp1": "1 · Smart Alert", "mob_sp2": "2 · รายละเอียดงาน", "mob_sp3": "3 · สแกน QR/RFID", "mob_sp4": "4 · บันทึก Regeneration", "mob_sp5": "5 · ปิดงาน", "mob_sp6": "6 · ประวัติงาน",
      "mob_greet1": "สวัสดี, ช่างสมชาย", "mob_h2_1": "งานของคุณวันนี้", "mob_sync1": "ออนไลน์ · sync ล่าสุด 2 นาทีที่แล้ว",
      "mob_sec1": "ความเสี่ยงสูง — ดำเนินการก่อน", "mob_st1": "บางปะกง · GIS-B3", "mob_dy1": "5 วัน", "mob_mt1": "ความดันลดลงต่อเนื่อง 4.1%/สัปดาห์",
      "btn_acc": "รับงาน", "btn_det": "ดูรายละเอียด",
      "mob_sec2": "ความเสี่ยงปานกลาง", "mob_st2": "สุราษฎร์ธานี · GIS-C2", "mob_dy2": "12 วัน", "mob_mt2": "ความดันลดลงต่อเนื่อง 1.8%/สัปดาห์",
      "mob_dt_sub": "WO-2609-041 · ความเร่งด่วนสูง", "mob_dt_n": "รายละเอียดงาน", "mob_r1": "ความดันปัจจุบัน", "mob_r2": "คาดถึงเกณฑ์ต่ำสุดใน", "btn_start": "เริ่มงาน → สแกนถัง",
      "mob_qr_sub": "งาน · บางปะกง GIS-B3", "mob_qr_h": "สแกนถัง SF6", "mob_qr_hint": "วางกล้องให้ตรงกับ QR หรือ RFID บนถัง เพื่อดึงประวัติอุปกรณ์อัตโนมัติ", "btn_next": "ดำเนินการต่อ",
      "mob_fm_h": "บันทึก Regeneration", "mob_f1": "ปริมาณก๊าซที่กู้คืน", "mob_f2": "ความดันหลังดำเนินการ", "mob_est_l": "มูลค่าคาร์บอนเครดิตโดยประมาณ", "mob_est_v": "14,570 บาท", "btn_submit": "ส่งบันทึก & ปิดงาน",
      "mob_dn_h": "ปิดงานสำเร็จ", "mob_dn_m1": "บันทึก Regeneration สำเร็จ", "btn_home": "กลับไปหน้างานของฉัน",
      "mob_hs_h": "ประวัติงานของฉัน", "mob_hs_s": "เดือนนี้",
      "tab_w": "งาน", "tab_s": "สแกน", "tab_h": "ประวัติ", "tab_p": "โปรไฟล์"
    },
    "en": {
      "app_title": "Smart SF6 Leakage & Regeneration", "tab_web": "Web Dashboard", "tab_mobile": "Mobile App",
      "menu_overview_lbl": "Overview", "menu_dashboard": "Dashboard", "menu_assets": "Asset / Station",
      "menu_workorders": "Alerts & Work Orders", "menu_reports": "Reports", 
      "menu_sys_lbl": "System", "menu_devices": "Device Registry", "menu_admin": "Admin",
      "lbl_station": "Display Station", "opt_all": "All Stations (24 GIS Bays)", "opt_bpk": "Bang Pakong", "opt_nma": "North Bangkok",
      "dash_h1": "Corporate Overview — Carbon & Predictive Insights", "dash_sub": "Last updated Sep 06, 2026 · Data from 24 stations",
      "chip_wk": "Weekly", "chip_mo": "Monthly", "chip_yr": "Yearly",
      "alert_banner": "<b>2 High Risks Detected</b> Pressure expected to drop below safe threshold within 7 days", "alert_go": "View Work Order →",
      "kpi1": "Total SF6 Recovered", "kpi1_f": "vs last month 131.8 kg",
      "kpi2": "Carbon Equivalent", "kpi2_f": "Ref GWP 23,500",
      "kpi3": "Carbon Credit Value", "kpi3_u": "THB", "kpi3_f": "Eval 100 THB/ton (T-VER)",
      "kpi4": "Cumulative ROI", "kpi4_t": "Payback 2.1y", "kpi4_f": "vs system cost",
      "chart_title": "Carbon Credit Reduction Trend (tCO2e)", "chart_l1": "Actual", "chart_l2": "Target",
      "risk_title": "Risk Ranking (Predictive)", "risk_tag": "Next 7–14 Days", "risk_day": "Days",
      "risk1_n": "Bang Pakong · GIS-B3", "risk1_m": "Pressure trend -4.1%/wk", "risk2_n": "North Bangkok · GIS-A1", "risk2_m": "Pressure trend -3.6%/wk",
      "risk3_n": "Surat Thani · GIS-C2", "risk3_m": "Pressure trend -1.8%/wk", "risk4_n": "Chonburi 2 · GIS-A4", "risk4_m": "Pressure trend -1.2%/wk",
      "tbl_title": "Recent Regeneration Logs", "tbl_tag": "Showing 5 of 47 logs",
      "th_date": "Date", "th_station": "Station / Bay", "th_tech": "Technician", "th_rec": "Recovered", "th_credit": "Carbon Credit", "th_stat": "Status",
      "td_d1": "Sep 04, 2026", "td_s1": "Bang Pakong / GIS-B1", "td_n1": "Somchai Jaidee", "td_c1": "14,570 THB",
      "td_d2": "Sep 02, 2026", "td_s2": "North BKK / GIS-A3", "td_n2": "Wichai Thongdee", "td_c2": "11,280 THB",
      "td_d3": "Aug 30, 2026", "td_s3": "Chonburi 2 / GIS-A2", "td_n3": "Prayut Saengthong", "td_c3": "7,285 THB",
      "ast_h1": "Asset / Station Explorer", "ast_sub": "24 Stations · 47 GIS Bays · 312 Sensors",
      "ast_k1": "Total Stations", "ast_k2": "Monitored SF6 Tanks", "ast_k3": "Offline Devices",
      "wo_h1": "Alerts & Work Order Management", "rp_h1": "Reports", "dv_h1": "Device Registry", "ad_h1": "Admin — Users & Roles",
      
      "mob_sp_h": "Screen", "mob_sp1": "1 · Smart Alert", "mob_sp2": "2 · Task Details", "mob_sp3": "3 · Scan QR/RFID", "mob_sp4": "4 · Log Regeneration", "mob_sp5": "5 · Task Done", "mob_sp6": "6 · Work History",
      "mob_greet1": "Hello, Tech Somchai", "mob_h2_1": "Your Tasks Today", "mob_sync1": "Online · synced 2 mins ago",
      "mob_sec1": "High Risk — Action Required", "mob_st1": "Bang Pakong · GIS-B3", "mob_dy1": "5 Days", "mob_mt1": "Pressure trend -4.1%/week",
      "btn_acc": "Accept", "btn_det": "Details",
      "mob_sec2": "Medium Risk", "mob_st2": "Surat Thani · GIS-C2", "mob_dy2": "12 Days", "mob_mt2": "Pressure trend -1.8%/week",
      "mob_dt_sub": "WO-2609-041 · High Priority", "mob_dt_n": "Task Details", "mob_r1": "Current Pressure", "mob_r2": "Est. to Critical in", "btn_start": "Start → Scan Tank",
      "mob_qr_sub": "Task · Bang Pakong GIS-B3", "mob_qr_h": "Scan SF6 Tank", "mob_qr_hint": "Align camera with QR/RFID on the tank to auto-fetch device history", "btn_next": "Continue",
      "mob_fm_h": "Log Regeneration", "mob_f1": "Recovered Gas Volume", "mob_f2": "Pressure After Proc.", "mob_est_l": "Est. Carbon Credit Value", "mob_est_v": "14,570 THB", "btn_submit": "Submit & Close Task",
      "mob_dn_h": "Task Completed", "mob_dn_m1": "Regeneration Logged Successfully", "btn_home": "Back to My Tasks",
      "mob_hs_h": "My Work History", "mob_hs_s": "This Month",
      "tab_w": "Tasks", "tab_s": "Scan", "tab_h": "History", "tab_p": "Profile"
    }
  };

  let currentLang = "th";

  function toggleLanguage() {
    currentLang = currentLang === "th" ? "en" : "th";
    document.getElementById('langToggleBtn').innerText = currentLang === "th" ? "EN" : "TH";

    const elements = document.querySelectorAll('[data-i18n]');
    elements.forEach(el => {
      const key = el.getAttribute('data-i18n');
      if (translations[currentLang][key]) {
        el.innerHTML = translations[currentLang][key];
      }
    });
  }

  // Chart.js Init
  document.addEventListener("DOMContentLoaded", function() {
    const ctx = document.getElementById('carbonChart');
    if(ctx) {
      new Chart(ctx, {
        type:'line',
        data:{
          labels:['เม.ย.','พ.ค.','มิ.ย.','ก.ค.','ส.ค.','ก.ย.'],
          datasets:[
            { label:'Actual', data:[2410,2680,2790,3020,3095,3351], borderColor:'#F2A900', backgroundColor:'rgba(242,169,0,.15)', fill:true, tension:.35, pointRadius:3, pointBackgroundColor:'#F2A900', borderWidth:2.5 },
            { label:'Target', data:[2500,2700,2900,3100,3300,3500], borderColor:'#8A97A6', borderDash:[5,5], fill:false, tension:.35, pointRadius:0, borderWidth:2 }
          ]
        },
        options:{ responsive:true, plugins:{ legend:{display:false} }, scales:{ x:{ grid:{color:'#E4E7EB'}, ticks:{color:'#6B7580', font:{family:'IBM Plex Sans'}} }, y:{ grid:{color:'#E4E7EB'}, ticks:{color:'#6B7580', font:{family:'IBM Plex Mono'}} } } }
      });
    }
  });
</script>
</body>
</html>

