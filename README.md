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
    width:375px; height:780px; background:var(--mb-bg); border-radius:

