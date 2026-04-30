

<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Mones TeleDash</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.2/dist/chart.umd.min.js"></script>
<style>
:root {
  --bg:        #0d1117;
  --surface:   #161b27;
  --surface2:  #1c2333;
  --border:    #242d3f;
  --border2:   #2d3a50;
  --rh:        #2dd4a0;
  --td-c:      #7b9ef0;
  --mdl:       #f5a623;
  --revenue:   #f87171;
  --text:      #e2e8f0;
  --text2:     #94a3b8;
  --text3:     #64748b;
  --pill-active: #2563eb;
  --active-bg:   #1e3a5f;
  --active-border:#3b82f6;
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{font-size:14px}
body{
  background:var(--bg);color:var(--text);
  font-family:'DM Sans',sans-serif;
  min-height:100vh;padding:0 0 40px;
}
.topbar{
  display:flex;align-items:center;justify-content:space-between;
  padding:18px 28px 14px;border-bottom:1px solid var(--border);
  flex-wrap:wrap;gap:12px;
}
.topbar-left h1{font-size:1.45rem;font-weight:700;letter-spacing:-.01em;}
.topbar-left .sub{
  font-size:.73rem;color:var(--text3);margin-top:5px;
  display:flex;align-items:center;gap:6px;flex-wrap:wrap;
}
.ldot{width:8px;height:8px;border-radius:2px;display:inline-block;}
.feed-badge{
  display:flex;align-items:center;gap:7px;
  background:var(--surface2);border:1px solid var(--border2);
  border-radius:20px;padding:6px 14px;
  font-size:.72rem;color:var(--text2);white-space:nowrap;
}
.feed-dot{width:8px;height:8px;border-radius:50%;background:#ef4444;animation:blink 2s infinite;}
.feed-dot.live{background:#22c55e;}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.statrow{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;padding:20px 28px;}
.statcard{
  background:var(--surface);border:1px solid var(--border);
  border-radius:10px;padding:16px 18px;
}
.sclabel{font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:12px;}
.scrow{display:flex;gap:24px;align-items:flex-end;flex-wrap:wrap;}
.metric{display:flex;flex-direction:column;gap:3px;}
.mname{font-size:.72rem;color:var(--text2);}
.mval{font-size:1.85rem;font-weight:700;line-height:1;letter-spacing:-.02em;}
.rh-c {color:var(--rh);}
.rev-c{color:var(--revenue);}
.td-c2{color:var(--td-c);}
.mdl-c{color:var(--mdl);}
.tabnav{
  display:flex;gap:4px;flex-wrap:wrap;
  padding:14px 28px 0;border-bottom:1px solid var(--border);
}
.tab{
  padding:9px 18px;border-radius:8px 8px 0 0;
  border:1px solid transparent;border-bottom:none;
  background:transparent;color:var(--text2);font-size:.8rem;
  cursor:pointer;transition:.15s;font-family:'DM Sans',sans-serif;
  position:relative;bottom:-1px;
}
.tab:hover{color:var(--text);background:var(--surface);}
.tab.active{
  background:var(--surface2);border-color:var(--border);
  border-bottom-color:var(--surface2);color:var(--text);font-weight:600;
}
.tab.live-tab.active{
  background:var(--active-bg);
  border-color:#f5a623;
  border-bottom-color:var(--active-bg);
  color:#60a5fa;
  box-shadow: 0 0 0 1px #f5a623;
}
.panel{
  display:none;background:var(--surface2);
  border:1px solid var(--border);border-top:none;
  margin:0 28px;border-radius:0 0 12px 12px;
  padding:24px;animation:fadeIn .2s ease;
}
.panel.active{display:block;}
@keyframes fadeIn{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:none}}
.ph2{
  display:flex;align-items:center;gap:12px;flex-wrap:wrap;
  font-size:1.1rem;font-weight:700;margin-bottom:4px;
}
.live-badge{
  display:none;align-items:center;gap:5px;
  font-size:.72rem;color:#22c55e;font-weight:500;
}
.live-badge .dot{width:7px;height:7px;border-radius:50%;background:#22c55e;animation:blink 2s infinite;}
.psub{font-size:.72rem;color:var(--text3);margin-bottom:18px;}
.mtd-row{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:24px;}
.mtd-card{
  background:var(--surface);border:1px solid var(--border);
  border-radius:8px;padding:14px 16px;
}
.mclabel{font-size:.62rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:6px;}
.mcval{font-size:1.6rem;font-weight:700;letter-spacing:-.02em;line-height:1;}
.chart-sec{margin-bottom:28px;}
.chart-sec h3{font-size:.8rem;font-weight:600;color:var(--text);margin-bottom:12px;}
.chart-wrap{position:relative;height:260px;}
.clegend{display:flex;gap:18px;flex-wrap:wrap;margin-top:10px;}
.cleg{display:flex;align-items:center;gap:6px;font-size:.72rem;color:var(--text2);}
.cleg-dot{width:10px;height:10px;border-radius:2px;}
.footer-row{
  display:flex;justify-content:space-between;flex-wrap:wrap;gap:6px;
  padding:12px 28px 0;font-size:.68rem;color:var(--text3);
}
.loading-pulse {
  display:inline-block;background:var(--border2);border-radius:4px;
  color:transparent!important;animation:skeleton-pulse 1.4s ease infinite;
  min-width:50px;user-select:none;
}
@keyframes skeleton-pulse{0%,100%{opacity:.35}50%{opacity:.75}}
.tab-filters {
  display:flex;gap:10px 24px;align-items:center;flex-wrap:wrap;
  padding:12px 20px;margin-bottom:16px;
  background:var(--surface2);border-radius:10px;border:1px solid var(--border);
}
.tf-group{display:flex;align-items:center;gap:6px;flex-wrap:wrap;}
.tf-label{font-size:.6rem;text-transform:uppercase;letter-spacing:.08em;color:var(--text3);white-space:nowrap;}
.tf-select{
  background:var(--surface2);border:1px solid var(--border2);color:var(--text2);
  border-radius:8px;padding:5px 12px;font-size:.75rem;font-family:'DM Sans',sans-serif;
  cursor:pointer;outline:none;transition:.12s;
}
.tf-select:hover{border-color:var(--active-border);color:var(--text);}
.tf-select:focus{border-color:var(--active-border);}
@media(max-width:700px){
  .statrow{grid-template-columns:1fr 1fr;}
  .mtd-row{grid-template-columns:1fr;}
  .topbar{flex-direction:column;align-items:flex-start;}
}
</style>
</head>
<body>

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-left">
    <h1>Mones TeleDash</h1>
    <div class="sub">
      <span id="date-range">Last 30 Days</span> &nbsp;·&nbsp;
      <span class="ldot" style="background:var(--rh)"></span> Roman &nbsp;·&nbsp;
      <span class="ldot" style="background:var(--mdl)"></span> MDLive &nbsp;·&nbsp;
      <span class="ldot" style="background:var(--td-c)"></span> Teladoc
    </div>
  </div>
  <div style="display:flex;align-items:center;gap:8px;">
    <div class="feed-badge">
      <span class="feed-dot" id="feed-dot"></span>
      <span id="feed-label">Connecting…</span>
    </div>
    <button onclick="manualRefresh(this)" title="Refresh now" style="
      background:var(--surface2);border:1px solid var(--border2);
      border-radius:20px;padding:6px 12px;color:var(--text2);
      font-size:.72rem;cursor:pointer;font-family:'DM Sans',sans-serif;
      transition:.15s;display:flex;align-items:center;gap:5px;
    " onmouseover="this.style.borderColor='#60a5fa';this.style.color='#60a5fa'"
       onmouseout="this.style.borderColor='var(--border2)';this.style.color='var(--text2)'">
      ↻ Refresh
    </button>
  </div>
</div>

<!-- GOAL CARDS — Rolling 30 Days -->
<div class="statrow" style="grid-template-columns:repeat(4,1fr);">

  <div class="statcard" style="position:relative;overflow:hidden;">
    <div style="position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,var(--rh),var(--td-c),var(--mdl));"></div>
    <div class="sclabel" id="sc-total-label">Last 30 Days — Combined</div>
    <div style="display:flex;align-items:flex-end;gap:8px;margin-bottom:10px;">
      <span style="font-size:2rem;font-weight:800;color:var(--text);line-height:1;" id="sc-total-enc">—</span>
      <span style="font-size:.8rem;color:var(--text3);margin-bottom:4px;">/ 2,820</span>
    </div>
    <div style="background:var(--border);border-radius:4px;height:6px;overflow:hidden;margin-bottom:6px;">
      <div id="sc-total-bar" style="height:100%;border-radius:4px;background:linear-gradient(90deg,var(--rh),var(--td-c));transition:width .8s;width:0%;"></div>
    </div>
    <div style="display:flex;justify-content:space-between;font-size:.7rem;">
      <span id="sc-total-pct" style="font-weight:700;">—%</span>
      <span id="sc-total-status" style="color:var(--text3);">of goal</span>
    </div>
  </div>

  <div class="statcard" style="position:relative;overflow:hidden;">
    <div style="position:absolute;top:0;left:0;right:0;height:2px;background:var(--rh);"></div>
    <div class="sclabel">Roman — 30d</div>
    <div style="display:flex;align-items:flex-end;gap:8px;margin-bottom:10px;">
      <span style="font-size:2rem;font-weight:800;color:var(--rh);line-height:1;" id="sc-rh-enc">—</span>
      <span style="font-size:.8rem;color:var(--text3);margin-bottom:4px;">/ 2,520</span>
    </div>
    <div style="background:var(--border);border-radius:4px;height:6px;overflow:hidden;margin-bottom:6px;">
      <div id="sc-rh-bar" style="height:100%;border-radius:4px;background:var(--rh);transition:width .8s;width:0%;"></div>
    </div>
    <div style="display:flex;justify-content:space-between;font-size:.7rem;">
      <span id="sc-rh-pct" style="font-weight:700;color:var(--rh);">—%</span>
      <span id="sc-rh-status" style="color:var(--text3);">of goal</span>
    </div>
  </div>

  <div class="statcard" style="position:relative;overflow:hidden;">
    <div style="position:absolute;top:0;left:0;right:0;height:2px;background:var(--td-c);"></div>
    <div class="sclabel">Teladoc — 30d</div>
    <div style="display:flex;align-items:flex-end;gap:8px;margin-bottom:10px;">
      <span style="font-size:2rem;font-weight:800;color:var(--td-c);line-height:1;" id="sc-td-enc">—</span>
      <span style="font-size:.8rem;color:var(--text3);margin-bottom:4px;">/ 200</span>
    </div>
    <div style="background:var(--border);border-radius:4px;height:6px;overflow:hidden;margin-bottom:6px;">
      <div id="sc-td-bar" style="height:100%;border-radius:4px;background:var(--td-c);transition:width .8s;width:0%;"></div>
    </div>
    <div style="display:flex;justify-content:space-between;font-size:.7rem;">
      <span id="sc-td-pct" style="font-weight:700;color:var(--td-c);">—%</span>
      <span id="sc-td-status" style="color:var(--text3);">of goal</span>
    </div>
  </div>

  <div class="statcard" style="position:relative;overflow:hidden;">
    <div style="position:absolute;top:0;left:0;right:0;height:2px;background:var(--mdl);"></div>
    <div class="sclabel">MDLive — 30d</div>
    <div style="display:flex;align-items:flex-end;gap:8px;margin-bottom:10px;">
      <span style="font-size:2rem;font-weight:800;color:var(--mdl);line-height:1;" id="sc-mdl-enc">—</span>
      <span style="font-size:.8rem;color:var(--text3);margin-bottom:4px;">/ 100</span>
    </div>
    <div style="background:var(--border);border-radius:4px;height:6px;overflow:hidden;margin-bottom:6px;">
      <div id="sc-mdl-bar" style="height:100%;border-radius:4px;background:var(--mdl);transition:width .8s;width:0%;"></div>
    </div>
    <div style="display:flex;justify-content:space-between;font-size:.7rem;">
      <span id="sc-mdl-pct" style="font-weight:700;color:var(--mdl);">—%</span>
      <span id="sc-mdl-status" style="color:var(--text3);">of goal</span>
    </div>
  </div>

</div>

<!-- SETUP NOTICE -->
<div id="setup-notice" style="display:none;
  margin:16px 28px 0;background:#1a1f00;border:1px solid #3a4500;
  border-left:3px solid #a3e635;border-radius:8px;padding:16px 20px;
  font-size:.78rem;color:#bef264;line-height:1.8;font-family:'DM Sans',sans-serif;">
  <strong style="font-size:.85rem;color:#d9f99d;">⚡ One-time setup to enable the live feed (takes 2 minutes):</strong><br/>
  <span style="color:#94a3b8;">
  1. Open your Google Sheet → click <strong style="color:#d9f99d;">Extensions → Apps Script</strong><br/>
  2. Delete any existing code and paste this in:
  </span>
  <pre style="background:#0d1117;border:1px solid #1e2a3a;border-radius:6px;padding:12px 14px;margin:10px 0 10px;color:#a5f3fc;font-size:.75rem;overflow-x:auto;line-height:1.6;">function doGet() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheets = ss.getSheets();
  var result = {};
  sheets.forEach(function(sheet) {
    var name = sheet.getName();
    if (!/^[A-Za-z]{3}\s*\d{2,4}$/.test(name.trim())) return;
    var lastRow = Math.min(sheet.getLastRow(), 8);
    var lastCol = Math.min(sheet.getLastColumn(), 35);
    if (lastRow < 2 || lastCol < 2) return;
    var data = sheet.getRange(1, 1, lastRow, lastCol).getValues();
    var csv = data.map(function(row) {
      return row.map(function(cell) {
        var s;
        if (cell instanceof Date) {
          s = (cell.getMonth()+1) + "-" + cell.getDate() + "-" + cell.getFullYear();
        } else { s = String(cell).trim(); }
        return s.indexOf(",") > -1 ? '"' + s + '"' : s;
      }).join(",");
    }).join("\n");
    result[name] = csv;
  });
  var output = ContentService.createTextOutput(JSON.stringify(result));
  output.setMimeType(ContentService.MimeType.TEXT);
  return output;
}</pre>
  <span style="color:#94a3b8;">
  3. Click <strong style="color:#d9f99d;">Deploy → New deployment</strong> → Type: <strong style="color:#d9f99d;">Web app</strong><br/>
  4. Set <strong style="color:#d9f99d;">"Who has access" → Anyone</strong> → click <strong style="color:#d9f99d;">Deploy</strong><br/>
  5. Copy the <strong style="color:#d9f99d;">Web app URL</strong> → find <code style="background:#1e2a3a;padding:1px 5px;border-radius:3px">APPS_SCRIPT_URL = ""</code> → paste URL → save
  </span>
</div>

<!-- TAB NAV -->
<div class="tabnav">
  <button class="tab" onclick="switchTab('volume',this)">Volume</button>
  <button class="tab" onclick="switchTab('revenue',this)">Revenue</button>
  <button class="tab" onclick="switchTab('daily',this)">Last 30 Days</button>
  <button class="tab live-tab active" onclick="switchTab('today',this)">Today</button>
</div>

<!-- REVENUE PANEL -->
<div id="panel-revenue" class="panel">
  <div class="tab-filters">
    <div class="tf-group">
      <span class="tf-label">Window:</span>
      <select class="tf-select" onchange="setWindowCombined(this.value)" id="window-select-rev">
        <optgroup label="Year">
          <option value="year-2026">2026</option>
          <option value="year-2025">2025</option>
          <option value="year-2024">2024</option>
        </optgroup>
        <optgroup label="Time Window">
          <option value="365" selected>Last 12 Months</option>
          <option value="0">All Time</option>
          <option value="-1">Year to Date</option>
          <option value="180">6 Months</option>
          <option value="90">90 Days</option>
          <option value="60">60 Days</option>
          <option value="30">30 Days</option>
          <option value="7">7 Days</option>
        </optgroup>
      </select>
    </div>
    <div class="tf-group">
      <span class="tf-label">Month:</span>
      <select class="tf-select" onchange="setRevMonth(this.value)" id="month-select-rev">
        <option value="all">All Months</option>
        <option value="1">January</option><option value="2">February</option>
        <option value="3">March</option><option value="4">April</option>
        <option value="5">May</option><option value="6">June</option>
        <option value="7">July</option><option value="8">August</option>
        <option value="9">September</option><option value="10">October</option>
        <option value="11">November</option><option value="12">December</option>
      </select>
    </div>
    <div class="tf-group">
      <span class="tf-label">Platform:</span>
      <select class="tf-select" onchange="setPlatformDd(this.value)" id="platform-select-rev">
        <option value="all">All Platforms</option>
        <option value="rh">Roman</option>
        <option value="td">Teladoc</option>
        <option value="mdl">MDLive</option>
      </select>
    </div>
  </div>
  <div class="ph2" style="margin-bottom:4px;">Revenue by Platform</div>
  <div class="psub">Estimated daily revenue · RH $12/visit · TD Phone $23 / Video $28 · MDL Phone $25 / Video $28</div>

  <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:22px 28px;margin-bottom:20px;position:relative;overflow:hidden;">
    <div style="position:absolute;top:0;left:0;right:0;height:3px;background:linear-gradient(90deg,var(--rh),var(--td-c),var(--mdl));"></div>
    <div style="display:grid;grid-template-columns:2fr 1fr 1fr 1fr;gap:24px;align-items:center;">
      <div>
        <div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:6px;" id="rev-total-label">Total Revenue — All Time</div>
        <div style="font-size:3rem;font-weight:800;color:var(--revenue);line-height:1;" id="rev-grand-total">—</div>
        <div style="font-size:.72rem;color:var(--text3);margin-top:6px;" id="rev-total-sub">across all providers</div>
      </div>
      <div style="border-left:1px solid var(--border);padding-left:24px;">
        <div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:6px;">Roman</div>
        <div style="font-size:1.6rem;font-weight:700;color:var(--rh);line-height:1;" id="rev-total-rh">—</div>
        <div style="font-size:.65rem;color:var(--text3);margin-top:4px;" id="rev-pct-rh">—% of total</div>
      </div>
      <div style="border-left:1px solid var(--border);padding-left:24px;">
        <div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:6px;">Teladoc</div>
        <div style="font-size:1.6rem;font-weight:700;color:var(--td-c);line-height:1;" id="rev-total-td">—</div>
        <div style="font-size:.65rem;color:var(--text3);margin-top:4px;" id="rev-pct-td">—% of total</div>
      </div>
      <div style="border-left:1px solid var(--border);padding-left:24px;">
        <div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:6px;">MDLive</div>
        <div style="font-size:1.6rem;font-weight:700;color:var(--mdl);line-height:1;" id="rev-total-mdl">—</div>
        <div style="font-size:.65rem;color:var(--text3);margin-top:4px;" id="rev-pct-mdl">—% of total</div>
      </div>
    </div>
  </div>

  <div class="chart-sec">
    <h3 id="rev-hist-title" style="margin-bottom:12px;">Monthly Revenue — Last 12 Months</h3>
    <div class="chart-wrap" style="height:360px;"><canvas id="revHistChart"></canvas></div>
    <div class="clegend" style="margin-top:10px;" id="rev-hist-legend"></div>
  </div>
</div>

<!-- VOLUME PANEL -->
<div id="panel-volume" class="panel">
  <div class="tab-filters">
    <div class="tf-group">
      <span class="tf-label">Window:</span>
      <select class="tf-select" onchange="setWindowCombined(this.value)" id="window-select-vol">
        <optgroup label="Year">
          <option value="year-2026">2026</option>
          <option value="year-2025">2025</option>
          <option value="year-2024">2024</option>
        </optgroup>
        <optgroup label="Time Window">
          <option value="365" selected>Last 12 Months</option>
          <option value="0">All Time</option>
          <option value="-1">Year to Date</option>
          <option value="180">6 Months</option>
          <option value="90">90 Days</option>
          <option value="60">60 Days</option>
          <option value="30">30 Days</option>
          <option value="7">7 Days</option>
        </optgroup>
      </select>
    </div>
    <div class="tf-group">
      <span class="tf-label">Month:</span>
      <select class="tf-select" onchange="setMonthDd(this.value)" id="month-select-vol">
        <option value="all">All Months</option>
        <option value="1">January</option><option value="2">February</option>
        <option value="3">March</option><option value="4">April</option>
        <option value="5">May</option><option value="6">June</option>
        <option value="7">July</option><option value="8">August</option>
        <option value="9">September</option><option value="10">October</option>
        <option value="11">November</option><option value="12">December</option>
      </select>
    </div>
    <div class="tf-group">
      <span class="tf-label">Platform:</span>
      <select class="tf-select" onchange="setPlatformDd(this.value)" id="platform-select-vol">
        <option value="all">All Platforms</option>
        <option value="rh">Roman</option>
        <option value="td">Teladoc</option>
        <option value="mdl">MDLive</option>
      </select>
    </div>
  </div>
  <div class="chart-sec">
    <h3 id="vol-window-title">Daily Encounter Volume — All Time</h3>
    <div class="chart-wrap" style="height:300px;"><canvas id="volChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>
  <div class="mtd-row" style="margin-top:20px;">
    <div class="mtd-card"><div class="mclabel">RH Encounters</div><div class="mcval rh-c" id="vol-rh">—</div></div>
    <div class="mtd-card"><div class="mclabel">TD Encounters</div><div class="mcval td-c2" id="vol-td">—</div></div>
    <div class="mtd-card"><div class="mclabel">MDL Encounters</div><div class="mcval mdl-c" id="vol-mdl">—</div></div>
  </div>
</div>

<!-- LAST 30 DAYS PANEL — revenue line chart only -->
<div id="panel-daily" class="panel">
  <div class="ph2">
    Last 30 Days
    <span class="live-badge" id="live-badge" style="display:flex;">
      <span class="dot"></span>
      Live · <span id="days-rec">—</span>
    </span>
  </div>
  <div class="psub" id="psub">Loading from Google Sheet…</div>

  <div class="mtd-row" style="grid-template-columns:repeat(5,1fr);">
    <div class="mtd-card">
      <div class="mclabel">30d Encounters</div>
      <div class="mcval rh-c" id="mtd-enc">—</div>
    </div>
    <div class="mtd-card">
      <div class="mclabel">30d Revenue</div>
      <div class="mcval rev-c" id="mtd-rev">—</div>
    </div>
    <div class="mtd-card">
      <div class="mclabel">Avg Daily · Roman</div>
      <div class="mcval rh-c" id="avg-rh">—</div>
    </div>
    <div class="mtd-card">
      <div class="mclabel">Avg Daily · Teladoc</div>
      <div class="mcval td-c2" id="avg-td">—</div>
    </div>
    <div class="mtd-card">
      <div class="mclabel">Avg Daily · MDLive</div>
      <div class="mcval mdl-c" id="avg-mdl">—</div>
    </div>
  </div>

  <div class="chart-sec">
    <h3>Daily Revenue — Last 30 Days (estimated by provider)</h3>
    <div class="chart-wrap"><canvas id="revChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>
</div>

<!-- TODAY PANEL -->
<div id="panel-today" class="panel active">
  <div class="ph2" style="margin-bottom:4px;">
    Today
    <span class="live-badge" id="today-live-badge" style="display:flex;">
      <span class="dot"></span>
      <span id="today-date">—</span>
    </span>
  </div>
  <div class="psub" id="today-sub">Live progress toward daily &amp; monthly targets · auto-refreshes every minute</div>

  <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3);margin:20px 0 10px;">Daily Progress</div>

  <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:20px;margin-bottom:20px;">
    <!-- Roman -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--rh);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">Roman</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3.5rem;font-weight:800;color:var(--rh);line-height:1;" id="today-rh-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:8px;">/ 84</div>
      </div>
      <div style="background:var(--border);border-radius:6px;height:10px;overflow:hidden;margin-bottom:10px;">
        <div id="today-rh-bar" style="height:100%;border-radius:6px;background:var(--rh);transition:width .8s ease;width:0%;"></div>
      </div>
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <span id="today-rh-pct" style="font-size:1.1rem;font-weight:700;color:var(--rh);">—</span>
        <span id="today-rh-badge" style="font-size:.68rem;padding:3px 8px;border-radius:10px;background:var(--surface2);color:var(--text3);">of target</span>
      </div>
      <div style="margin-top:12px;font-size:.7rem;color:var(--text3);" id="today-rh-needed"></div>
    </div>
    <!-- Teladoc -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--td-c);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">Teladoc</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3.5rem;font-weight:800;color:var(--td-c);line-height:1;" id="today-td-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:8px;">/ 10</div>
      </div>
      <div style="background:var(--border);border-radius:6px;height:10px;overflow:hidden;margin-bottom:10px;">
        <div id="today-td-bar" style="height:100%;border-radius:6px;background:var(--td-c);transition:width .8s ease;width:0%;"></div>
      </div>
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <span id="today-td-pct" style="font-size:1.1rem;font-weight:700;color:var(--td-c);">—</span>
        <span id="today-td-badge" style="font-size:.68rem;padding:3px 8px;border-radius:10px;background:var(--surface2);color:var(--text3);">of target</span>
      </div>
      <div style="margin-top:12px;font-size:.7rem;color:var(--text3);" id="today-td-needed"></div>
    </div>
    <!-- MDLive -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--mdl);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">MDLive</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3.5rem;font-weight:800;color:var(--mdl);line-height:1;" id="today-mdl-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:8px;">/ 5</div>
      </div>
      <div style="background:var(--border);border-radius:6px;height:10px;overflow:hidden;margin-bottom:10px;">
        <div id="today-mdl-bar" style="height:100%;border-radius:6px;background:var(--mdl);transition:width .8s ease;width:0%;"></div>
      </div>
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <span id="today-mdl-pct" style="font-size:1.1rem;font-weight:700;color:var(--mdl);">—</span>
        <span id="today-mdl-badge" style="font-size:.68rem;padding:3px 8px;border-radius:10px;background:var(--surface2);color:var(--text3);">of target</span>
      </div>
      <div style="margin-top:12px;font-size:.7rem;color:var(--text3);" id="today-mdl-needed"></div>
    </div>
  </div>

  <!-- Daily Revenue Track -->
  <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px;margin-bottom:28px;">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;">
      <div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);">Daily Revenue</div>
      <div style="font-size:.63rem;color:var(--text3);">Goal: <span style="color:var(--text2);font-weight:600;">$1,380</span></div>
    </div>
    <div style="position:relative;height:32px;">
      <div style="position:absolute;top:50%;transform:translateY(-50%);left:0;right:0;height:12px;background:var(--border);border-radius:8px;overflow:hidden;">
        <div id="rev-track-fill" style="height:100%;width:0%;background:linear-gradient(90deg,#f87171,#f5a623,#22c55e);transition:width .8s ease;"></div>
      </div>
      <div id="rev-track-badge" style="position:absolute;top:-26px;left:0%;transform:translateX(-50%);background:var(--surface2);border:1px solid var(--border2);border-radius:20px;padding:2px 10px;font-size:.72rem;font-weight:700;white-space:nowrap;transition:left .8s ease;color:#f87171;">$0 · 0%</div>
    </div>
    <div style="display:flex;justify-content:space-between;margin-top:8px;font-size:.6rem;color:var(--text3);">
      <span>$0</span><span>$1,380</span>
    </div>
  </div>

  <!-- Rolling 30-Day Progress -->
  <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3);margin-bottom:10px;">Rolling 30-Day Progress — <span id="today-month-label">Last 30 Days</span></div>

  <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:20px;margin-bottom:20px;">
    <!-- Roman 30d -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--rh);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">Roman</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3rem;font-weight:800;color:var(--rh);line-height:1;" id="mgoal-rh-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:6px;">/ 2,520</div>
      </div>
      <div style="background:var(--border);border-radius:6px;height:10px;overflow:hidden;margin-bottom:10px;">
        <div id="mgoal-rh-bar" style="height:100%;border-radius:6px;background:var(--rh);transition:width .8s ease;width:0%;"></div>
      </div>
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <span id="mgoal-rh-pct" style="font-size:1.1rem;font-weight:700;color:var(--rh);">—</span>
        <span id="mgoal-rh-badge" style="font-size:.68rem;padding:3px 8px;border-radius:10px;background:var(--surface2);color:var(--text3);">of target</span>
      </div>
      <div style="margin-top:10px;font-size:.7rem;color:var(--text3);" id="mgoal-rh-needed"></div>
    </div>
    <!-- Teladoc 30d -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--td-c);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">Teladoc</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3rem;font-weight:800;color:var(--td-c);line-height:1;" id="mgoal-td-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:6px;">/ 200</div>
      </div>
      <div style="background:var(--border);border-radius:6px;height:10px;overflow:hidden;margin-bottom:10px;">
        <div id="mgoal-td-bar" style="height:100%;border-radius:6px;background:var(--td-c);transition:width .8s ease;width:0%;"></div>
      </div>
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <span id="mgoal-td-pct" style="font-size:1.1rem;font-weight:700;color:var(--td-c);">—</span>
        <span id="mgoal-td-badge" style="font-size:.68rem;padding:3px 8px;border-radius:10px;background:var(--surface2);color:var(--text3);">of target</span>
      </div>
      <div style="margin-top:10px;font-size:.7rem;color:var(--text3);" id="mgoal-td-needed"></div>
    </div>
    <!-- MDLive 30d -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--mdl);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">MDLive</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3rem;font-weight:800;color:var(--mdl);line-height:1;" id="mgoal-mdl-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:6px;">/ 100</div>
      </div>
      <div style="background:var(--border);border-radius:6px;height:10px;overflow:hidden;margin-bottom:10px;">
        <div id="mgoal-mdl-bar" style="height:100%;border-radius:6px;background:var(--mdl);transition:width .8s ease;width:0%;"></div>
      </div>
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <span id="mgoal-mdl-pct" style="font-size:1.1rem;font-weight:700;color:var(--mdl);">—</span>
        <span id="mgoal-mdl-badge" style="font-size:.68rem;padding:3px 8px;border-radius:10px;background:var(--surface2);color:var(--text3);">of target</span>
      </div>
      <div style="margin-top:10px;font-size:.7rem;color:var(--text3);" id="mgoal-mdl-needed"></div>
    </div>
  </div>

  <!-- 30-Day Revenue Track -->
  <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px;">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;">
      <div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);">Rolling 30-Day Revenue</div>
      <div style="font-size:.63rem;color:var(--text3);">Goal: <span style="color:var(--text2);font-weight:600;">$41,400</span></div>
    </div>
    <div style="position:relative;height:32px;">
      <div style="position:absolute;top:50%;transform:translateY(-50%);left:0;right:0;height:12px;background:var(--border);border-radius:8px;overflow:hidden;">
        <div id="mrev-track-fill" style="height:100%;width:0%;background:linear-gradient(90deg,#f87171,#f5a623,#22c55e);transition:width .8s ease;"></div>
      </div>
      <div id="mrev-track-badge" style="position:absolute;top:-26px;left:0%;transform:translateX(-50%);background:var(--surface2);border:1px solid var(--border2);border-radius:20px;padding:2px 10px;font-size:.72rem;font-weight:700;white-space:nowrap;transition:left .8s ease;color:#f87171;">$0 · 0%</div>
    </div>
    <div style="display:flex;justify-content:space-between;margin-top:8px;font-size:.6rem;color:var(--text3);">
      <span>$0</span><span>$41,400</span>
    </div>
  </div>
</div>

<!-- FOOTER -->
<div class="footer-row">
  <span>Last refreshed: <span id="last-updated">—</span></span>
  <span>Auto-refreshes every minute</span>
</div>

<script>
const APPS_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbwVT-Xukwe3rxiPAnaMGK8ywEVf0Tfnm9U6cTQo9Ts6DX3DOoGQNcgvHj2pnCAO0_N3/exec";
const SHEET_CSV_URL   = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSc3am4UpZjqNJx9i5asqyEn-AI51cqKSPKvjV89ujSi_4FnDDgxw5gbc6dk2hnX4HK3o52aqm6DUDW/pub?output=csv";

const RATES = {
  rh:   12.00,
  td1:  23.00,
  td2:  28.00,
  mdl1: 25.00,
  mdl2: 28.00,
};

let ALL_DATA   = {};
let ACTIVE_YEAR  = 'all';
let ACTIVE_MONTH = 'all';
let ACTIVE_TYPE  = 'all';
let REV_WINDOW   = 365;
let REV_MONTH    = 'all';
let revChart = null;
let revTabChart = null, volChart = null;
window._tabChartsBuilt = false;

// ── CSV row parser ──
function parseCSVRow(r) {
  const cols=[]; let cur=''; let inQ=false;
  for(const ch of r){
    if(ch==='"'){inQ=!inQ;continue;}
    if(ch===','&&!inQ){cols.push(cur.trim());cur='';}
    else cur+=ch;
  }
  cols.push(cur.trim());
  return cols;
}

function parseDateCol(h) {
  let m;
  m = h.match(/^(\d{1,2})-(\d{1,2})-(\d{4})$/);
  if (m) return {month:+m[1],day:+m[2],year:+m[3]};
  m = h.match(/([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})/);
  if (m) {
    const mo={jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12}[m[1].toLowerCase()];
    if (mo) return {month:mo,day:+m[2],year:+m[3]};
  }
  m = h.match(/^(\d{1,2})-([A-Za-z]{3})$/i);
  if (m) {
    const mo={jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12}[m[2].toLowerCase()];
    if (mo) return {month:mo,day:+m[1],year:2026};
  }
  m = h.match(/^(\d{1,2})\/(\d{1,2})(?:\/(\d{4}))?$/);
  if (m) return {month:+m[1],day:+m[2],year:m[3]?+m[3]:2026};
  return null;
}

function monthYearFromTabName(name) {
  const moMap = {jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12};
  const m = name.trim().match(/^([A-Za-z]{3})[\s-]*(\d{2,4})$/i);
  if (m) {
    const mo = moMap[m[1].toLowerCase()];
    if (mo) {
      let yr = +m[2];
      if (yr < 100) yr += yr < 50 ? 2000 : 1900;
      return { month: mo, year: yr };
    }
  }
  return null;
}

function parseSheetCSV(text, tabName) {
  const rows = text.trim().split('\n').map(parseCSVRow);
  if (rows.length < 2) return null;
  const headers = rows[0];
  const nameDate = tabName ? monthYearFromTabName(tabName) : null;
  const allDateCols = [];
  headers.forEach((h,i) => {
    if (i===0) return;
    const d = parseDateCol(h.trim());
    if (!d) return;
    if (nameDate) { if (d.month === nameDate.month) allDateCols.push({i,...d}); }
    else allDateCols.push({i,...d});
  });
  const dateCols = allDateCols.length > 0 ? allDateCols : (() => {
    const all = [];
    headers.forEach((h,i) => {
      if (i===0) return;
      const d = parseDateCol(h.trim());
      if (d) all.push({i,...d});
    });
    return all;
  })();
  if (!dateCols.length) return null;
  dateCols.sort((a,b) => a.day - b.day);
  const n = dateCols.length;
  const labels = dateCols.map(d => d.month+'/'+d.day);
  const rh=new Array(n).fill(0);
  const td1=new Array(n).fill(0), td2=new Array(n).fill(0);
  const mdl1=new Array(n).fill(0), mdl2=new Array(n).fill(0);
  const rev=new Array(n).fill(0);
  let tdCount=0, mdlCount=0;
  for (const row of rows.slice(1)) {
    const name = (row[0]||'').trim().toUpperCase().replace(/\s+/g,'');
    if (!name) continue;
    dateCols.forEach((col,j) => {
      const raw = (row[col.i]||'').toString().replace(/#[A-Z!]+/g,'').replace(/[$, ]/g,'');
      const v = parseFloat(raw)||0;
      if (name==='RO'||name==='RO+') { rh[j] += v; }
      else if (name==='TOT'||name==='TOTAL'||name==='TOTAL,') { rev[j] += v; }
    });
    const isTD  = name.startsWith('TD');
    const isMDL = name==='MD'||name==='MDL'||name==='ML'||name.startsWith('MLV')||name.startsWith('MDL');
    if (isTD) {
      tdCount++;
      dateCols.forEach((col,j) => {
        const v = parseFloat((row[col.i]||'').replace(/[$, ]/g,''))||0;
        if (tdCount===1) td1[j]+=v; else td2[j]+=v;
      });
    }
    if (isMDL) {
      mdlCount++;
      dateCols.forEach((col,j) => {
        const v = parseFloat((row[col.i]||'').replace(/[$, ]/g,''))||0;
        if (mdlCount===1) mdl1[j]+=v; else mdl2[j]+=v;
      });
    }
  }
  const td  = td1.map((v,i)=>v+td2[i]);
  const mdl = mdl1.map((v,i)=>v+mdl2[i]);
  let lastIdx = -1;
  for (let j=0;j<n;j++) if(rh[j]>0||td[j]>0||mdl[j]>0||rev[j]>0) lastIdx=j;
  if (lastIdx<0) return null;
  const t = lastIdx+1;
  const sheetMonth = nameDate ? nameDate.month : dateCols[0].month;
  const sheetYear  = nameDate ? nameDate.year  : dateCols[0].year;
  return {
    labels: labels.slice(0,t),
    rh:   rh.slice(0,t),
    td:   td.slice(0,t),   td1: td1.slice(0,t),  td2: td2.slice(0,t),
    mdl:  mdl.slice(0,t),  mdl1:mdl1.slice(0,t), mdl2:mdl2.slice(0,t),
    rev:  rev.slice(0,t),
    rhRev:  rh.slice(0,t).map(v  => Math.round(v*RATES.rh)),
    tdRev:  td1.slice(0,t).map((v,i) => Math.round(v*RATES.td1 + td2[i]*RATES.td2)),
    mdlRev: mdl1.slice(0,t).map((v,i)=> Math.round(v*RATES.mdl1 + mdl2[i]*RATES.mdl2)),
    month: sheetMonth, year: sheetYear,
  };
}

function yearFromTabName(name) {
  let m;
  m = name.trim().match(/(\d{4})/);
  if (m) return +m[1];
  m = name.trim().match(/(\d{2})\s*$/);
  if (m) { const y=+m[1]; return y<50?2000+y:1900+y; }
  return null;
}

function aggregate(dataArr) {
  if (!dataArr.length) return null;
  const labels=[],rh=[],td=[],td1=[],td2=[],mdl=[],mdl1=[],mdl2=[],rev=[],rhRev=[],tdRev=[],mdlRev=[];
  dataArr.forEach(d => {
    d.labels.forEach((l,i) => {
      labels.push(l); rh.push(d.rh[i]); td.push(d.td[i]);
      td1.push((d.td1||[])[i]||0); td2.push((d.td2||[])[i]||0);
      mdl.push(d.mdl[i]); mdl1.push((d.mdl1||[])[i]||0); mdl2.push((d.mdl2||[])[i]||0);
      rev.push(d.rev[i]);
      rhRev.push((d.rhRev||[])[i]||0);
      tdRev.push((d.tdRev||[])[i]||0);
      mdlRev.push((d.mdlRev||[])[i]||0);
    });
  });
  return {labels,rh,td,td1,td2,mdl,mdl1,mdl2,rev,rhRev,tdRev,mdlRev};
}

function getFilteredData() {
  let sheets = Object.entries(ALL_DATA);
  if (!sheets.length) return null;
  if (ACTIVE_YEAR !== 'all') {
    const yr = +ACTIVE_YEAR;
    sheets = sheets.filter(([name,d]) => d.year===yr || yearFromTabName(name)===yr);
  }
  if (ACTIVE_MONTH !== 'all') {
    const mo = +ACTIVE_MONTH;
    sheets = sheets.filter(([,d]) => d.month===mo);
  }
  if (!sheets.length) return null;
  sheets.sort(([,a],[,b]) => a.year!==b.year ? a.year-b.year : a.month-b.month);
  let agg = aggregate(sheets.map(([,d])=>d));
  if (ACTIVE_TYPE !== 'all') {
    agg = {...agg};
    if (ACTIVE_TYPE !== 'rh')  { agg.rh=agg.rh.map(()=>0);  agg.rhRev=agg.rhRev.map(()=>0); }
    if (ACTIVE_TYPE !== 'td')  { agg.td=agg.td.map(()=>0);  agg.tdRev=agg.tdRev.map(()=>0); }
    if (ACTIVE_TYPE !== 'mdl') { agg.mdl=agg.mdl.map(()=>0); agg.mdlRev=agg.mdlRev.map(()=>0); }
  }
  return agg;
}

// ── Always returns last 30 calendar days across all sheets ──
function getRolling30Days() {
  const allDays = [];
  Object.entries(ALL_DATA).forEach(([,d]) => {
    d.labels.forEach((label,i) => {
      const parts = label.split('/');
      if (parts.length < 2) return;
      const date = new Date(d.year, +parts[0]-1, +parts[1]);
      allDays.push({
        date, label,
        rh:  d.rh[i]  || 0, td:  d.td[i]  || 0, mdl: d.mdl[i] || 0,
        rhRev:  (d.rhRev  || [])[i] || 0,
        tdRev:  (d.tdRev  || [])[i] || 0,
        mdlRev: (d.mdlRev || [])[i] || 0,
        td1: (d.td1  || [])[i] || 0, td2: (d.td2  || [])[i] || 0,
        mdl1:(d.mdl1 || [])[i] || 0, mdl2:(d.mdl2 || [])[i] || 0,
      });
    });
  });
  allDays.sort((a,b) => a.date - b.date);
  return allDays.slice(-30);
}

const GRID = '#1e2a3a';
Chart.defaults.color = '#64748b';
Chart.defaults.font.family = "'DM Sans',sans-serif";
Chart.defaults.font.size = 11;

function tooltipDefaults() {
  return {backgroundColor:'#1c2333',borderColor:'#2d3a50',borderWidth:1,titleColor:'#94a3b8',bodyColor:'#e2e8f0',padding:10};
}

// ── Build only the revenue line chart for Last 30 Days tab ──
function buildCharts(d) {
  const rc = document.getElementById('revChart').getContext('2d');
  revChart = new Chart(rc, {
    type:'line',
    data:{labels:d.labels, datasets:[
      {label:'Roman',   data:d.rhRev||[],  borderColor:'#2dd4a0',backgroundColor:'rgba(45,212,160,.08)', fill:true, tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5,spanGaps:false},
      {label:'Teladoc', data:d.tdRev||[],  borderColor:'#7b9ef0',backgroundColor:'transparent',          fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5,spanGaps:false},
      {label:'MDLive',  data:d.mdlRev||[], borderColor:'#f5a623',backgroundColor:'transparent',          fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5,spanGaps:false},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{label:i=>' $'+i.raw.toLocaleString()}}},
      scales:{x:{grid:{color:GRID},ticks:{maxRotation:45}},y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(1)+'k':v)}}}}
  });
}

function buildTabCharts(d) {
  volChart = new Chart(document.getElementById('volChart').getContext('2d'), {
    type:'bar',
    data:{labels:d.labels, datasets:[
      {label:'Roman',   data:d.rh, backgroundColor:'rgba(45,212,160,.82)',borderWidth:0,borderRadius:2,stack:'s'},
      {label:'Teladoc', data:d.td, backgroundColor:'rgba(123,158,240,.82)',borderWidth:0,borderRadius:2,stack:'s'},
      {label:'MDLive',  data:d.mdl,backgroundColor:'rgba(245,166,35,.82)', borderWidth:0,borderRadius:2,stack:'s'},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{footer:items=>'Total: '+items.reduce((a,i)=>a+i.raw,0)}}},
      scales:{x:{stacked:true,grid:{color:GRID},ticks:{maxRotation:45}},y:{stacked:true,grid:{color:GRID},beginAtZero:true}}}
  });
  updateTabStats(d);
}

function updateTabStats(d) {
  const tRH=d.rh.reduce((a,b)=>a+b,0),tTD=d.td.reduce((a,b)=>a+b,0),tMDL=d.mdl.reduce((a,b)=>a+b,0);
  const e = (id,v) => { const el=document.getElementById(id); if(el) el.textContent=v; };
  e('vol-rh',  tRH.toLocaleString());
  e('vol-td',  tTD.toLocaleString());
  e('vol-mdl', tMDL.toLocaleString());
}

// ── Windowed data for revenue/volume tabs ──
function getLast30DaysData() {
  const allDays = [];
  Object.entries(ALL_DATA).forEach(([,d]) => {
    if (ACTIVE_YEAR !== 'all' && d.year !== +ACTIVE_YEAR) return;
    d.labels.forEach((label,i) => {
      const parts = label.split('/');
      if (parts.length < 2) return;
      const date = new Date(d.year, +parts[0]-1, +parts[1]);
      allDays.push({
        date, label,
        rh: d.rh[i]||0, td: d.td[i]||0, mdl: d.mdl[i]||0,
        rhRev:(d.rhRev||[])[i]||0, tdRev:(d.tdRev||[])[i]||0, mdlRev:(d.mdlRev||[])[i]||0,
      });
    });
  });
  allDays.sort((a,b) => a.date - b.date);
  let filtered = allDays;
  if (REV_MONTH !== 'all') filtered = allDays.filter(d => d.date.getMonth()+1 === +REV_MONTH);
  if (ACTIVE_YEAR !== 'all') return filtered.filter(d => d.date.getFullYear() === +ACTIVE_YEAR);
  const window = +REV_WINDOW;
  if (window === 0) return filtered;
  if (window === -1) {
    const ytdStart = new Date(new Date().getFullYear(), 0, 1);
    return filtered.filter(d => d.date >= ytdStart);
  }
  if (window === 365) {
    const yearAgo = new Date(); yearAgo.setFullYear(yearAgo.getFullYear()-1);
    return filtered.filter(d => d.date >= yearAgo);
  }
  return filtered.slice(-window);
}

let revHistChart = null;
let rev30BarChart = null;

function buildRevTab30Day() {
  const days = getLast30DaysData();
  if (!days.length) return;
  const labels  = days.map(d=>d.label);
  const rhRevs  = days.map(d=>d.rhRev);
  const tdRevs  = days.map(d=>d.tdRev);
  const mdlRevs = days.map(d=>d.mdlRev);
  const totRev  = days.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev,0);
  const rhTot   = days.reduce((a,d)=>a+d.rhRev,0);
  const tdTot   = days.reduce((a,d)=>a+d.tdRev,0);
  const mdlTot  = days.reduce((a,d)=>a+d.mdlRev,0);
  const tot     = totRev||1;
  const s = (id,v) => { const el=document.getElementById(id); if(el) el.textContent=v; };
  s('rev-total-rh',  '$'+Math.round(rhTot).toLocaleString());
  s('rev-pct-rh',    Math.round(rhTot/tot*100)+'% of total');
  s('rev-total-td',  '$'+Math.round(tdTot).toLocaleString());
  s('rev-pct-td',    Math.round(tdTot/tot*100)+'% of total');
  s('rev-total-mdl', '$'+Math.round(mdlTot).toLocaleString());
  s('rev-pct-mdl',   Math.round(mdlTot/tot*100)+'% of total');
  const rt = document.getElementById('rev-30day-title');
  if (rt) {
    const tMap = {0:'All Time','-1':'Year to Date',7:'7 Days',30:'30 Days',60:'60 Days',90:'90 Days',180:'6 Months',365:'Last 12 Months'};
    const lbl2 = ACTIVE_YEAR!=='all' ? String(ACTIVE_YEAR) : (tMap[REV_WINDOW]||REV_WINDOW+' Days');
    rt.textContent = 'Daily Revenue — '+lbl2;
  }
  if (revTabChart) { revTabChart.destroy(); revTabChart = null; }
  const rc = document.getElementById('revTabChart');
  if (rc) {
    revTabChart = new Chart(rc.getContext('2d'), {
      type:'line',
      data:{labels, datasets:[
        {label:'Roman',   data:rhRevs,  borderColor:'#2dd4a0', backgroundColor:'rgba(45,212,160,.07)', fill:true,  tension:.45, pointRadius:0, pointHoverRadius:5, pointBackgroundColor:'#2dd4a0', borderWidth:2.5, spanGaps:false},
        {label:'Teladoc', data:tdRevs,  borderColor:'#7b9ef0', backgroundColor:'rgba(123,158,240,.06)',fill:false, tension:.45, pointRadius:0, pointHoverRadius:5, pointBackgroundColor:'#7b9ef0', borderWidth:2,   spanGaps:false},
        {label:'MDLive',  data:mdlRevs, borderColor:'#f5a623', backgroundColor:'rgba(245,166,35,.06)', fill:false, tension:.45, pointRadius:0, pointHoverRadius:5, pointBackgroundColor:'#f5a623', borderWidth:2,   spanGaps:false},
      ]},
      options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
        plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{
          label:i=>' '+i.dataset.label+': $'+i.raw.toLocaleString(),
          footer:items=>'Total: $'+items.reduce((a,i)=>a+i.raw,0).toLocaleString()
        }}},
        scales:{
          x:{grid:{color:GRID},ticks:{maxRotation:45}},
          y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(1)+'k':v)}}
        }}
    });
  }
}

function buildRevHistChart() {
  if (revHistChart) { revHistChart.destroy(); revHistChart = null; }
  const gc = document.getElementById('revHistChart');
  if (!gc) return;
  const moNames = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  // Apply rolling window filter if set
  const histCutoff = REV_WINDOW === 365
    ? new Date(new Date().setFullYear(new Date().getFullYear()-1))
    : REV_WINDOW > 0
      ? new Date(Date.now() - REV_WINDOW * 86400000)
      : null;

  const filteredSheets = Object.entries(ALL_DATA)
    .filter(([name,d]) => {
      const yearOk  = ACTIVE_YEAR==='all' || d.year===+ACTIVE_YEAR || yearFromTabName(name)===+ACTIVE_YEAR;
      const monthOk = ACTIVE_MONTH==='all' || d.month===+ACTIVE_MONTH;
      if (!yearOk || !monthOk) return false;
      if (histCutoff) {
        // Include sheet if its month/year is >= cutoff month
        const sheetDate = new Date(d.year, d.month-1, 1);
        const cutoffMonth = new Date(histCutoff.getFullYear(), histCutoff.getMonth(), 1);
        return sheetDate >= cutoffMonth;
      }
      return true;
    })
    .sort(([,a],[,b]) => a.year!==b.year ? a.year-b.year : a.month-b.month);
  if (!filteredSheets.length) return;
  const labels = filteredSheets.map(([,d]) => moNames[d.month-1]+' '+String(d.year).slice(2));
  const rhRevs=[],tdRevs=[],mdlRevs=[];
  filteredSheets.forEach(([,d]) => {
    rhRevs.push(Math.round(d.rh.reduce((a,b)=>a+b,0)*RATES.rh));
    const td1t=(d.td1||[]).reduce((a,b)=>a+b,0), td2t=(d.td2||[]).reduce((a,b)=>a+b,0);
    const mdl1t=(d.mdl1||[]).reduce((a,b)=>a+b,0), mdl2t=(d.mdl2||[]).reduce((a,b)=>a+b,0);
    tdRevs.push(Math.round(td1t*RATES.td1+td2t*RATES.td2));
    mdlRevs.push(Math.round(mdl1t*RATES.mdl1+mdl2t*RATES.mdl2));
  });
  const title = document.getElementById('rev-hist-title');
  const winLabelHist = {0:'All Time','-1':'Year to Date',365:'Last 12 Months',180:'6 Months',90:'90 Days'};
  const yr  = ACTIVE_YEAR!=='all' ? ACTIVE_YEAR : (winLabelHist[REV_WINDOW] || (ACTIVE_YEAR==='all' ? 'All Time' : ACTIVE_YEAR));
  const mo  = ACTIVE_MONTH==='all' ? '' : ' · '+moNames[+ACTIVE_MONTH-1];
  if (title) title.textContent = 'Monthly Revenue — '+yr+mo+' ('+filteredSheets.length+' months)';
  const legend = document.getElementById('rev-hist-legend');
  if (legend) legend.innerHTML = [
    '<div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>',
    '<div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>',
    '<div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>',
  ].join('');
  revHistChart = new Chart(gc.getContext('2d'), {
    type:'bar',
    data:{labels, datasets:[
      {label:'Roman',   data:rhRevs,  backgroundColor:'rgba(45,212,160,.85)',borderWidth:0,borderRadius:3,stack:'s'},
      {label:'Teladoc', data:tdRevs,  backgroundColor:'rgba(123,158,240,.85)',borderWidth:0,borderRadius:3,stack:'s'},
      {label:'MDLive',  data:mdlRevs, backgroundColor:'rgba(245,166,35,.85)', borderWidth:0,borderRadius:3,stack:'s'},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{
        label:i=>' '+i.dataset.label+': $'+i.raw.toLocaleString(),
        footer:items=>'Total: $'+items.reduce((a,i)=>a+i.raw,0).toLocaleString()
      }}},
      scales:{x:{stacked:true,grid:{color:GRID},ticks:{maxRotation:45}},
              y:{stacked:true,grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(0)+'k':v)}}}}
  });
}

function updateRevSummary() {
  const days = getLast30DaysData();
  if (!days.length) return;
  const totalRHRev  = days.reduce((a,d)=>a+d.rhRev, 0);
  const totalTDRev  = days.reduce((a,d)=>a+d.tdRev, 0);
  const totalMDLRev = days.reduce((a,d)=>a+d.mdlRev,0);
  const grandTotal  = totalRHRev + totalTDRev + totalMDLRev;
  const tot = grandTotal || 1;
  const fmt = v => '$'+Math.round(v).toLocaleString();
  const pct = v => Math.round(v/tot*100)+'%';
  const set = (id,val) => { const el=document.getElementById(id); if(el) el.textContent=val; };
  const winLabel = {0:'All Time','-1':'Year to Date',7:'7 Days',30:'30 Days',60:'60 Days',90:'90 Days',180:'6 Months',365:'Last 12 Months'};
  const summaryLabel = ACTIVE_YEAR!=='all' ? String(ACTIVE_YEAR) : (winLabel[REV_WINDOW]||REV_WINDOW+' Days');
  set('rev-total-label',  'Total Revenue — '+summaryLabel);
  set('rev-grand-total',  fmt(grandTotal));
  const ndays = days.length||1;
  set('rev-total-sub', ndays+' days · '+fmt(grandTotal/ndays)+'/day avg');
  set('rev-total-rh',  fmt(totalRHRev));
  set('rev-pct-rh',    pct(totalRHRev)+' of total');
  set('rev-total-td',  fmt(totalTDRev));
  set('rev-pct-td',    pct(totalTDRev)+' of total');
  set('rev-total-mdl', fmt(totalMDLRev));
  set('rev-pct-mdl',   pct(totalMDLRev)+' of total');
}

// ── Goal progress cards — always rolling 30 days ──
function updateGoalCards() {
  try {
    const days30 = getRolling30Days();
    if (!days30.length) return;
    const tRH  = days30.reduce((a,d)=>a+d.rh,  0);
    const tTD  = days30.reduce((a,d)=>a+d.td,  0);
    const tMDL = days30.reduce((a,d)=>a+d.mdl, 0);
    const tTot = tRH + tTD + tMDL;
    const first = days30[0], last = days30[days30.length-1];
    const lbl = document.getElementById('sc-total-label');
    if (lbl) lbl.textContent = 'Last 30 Days · '+days30.length+' days';

    function setCard(prefix, val, goal, color) {
      const pct  = Math.min(Math.round(val/goal*100), 100);
      const hit  = val >= goal;
      const near = pct >= 80;
      const c    = hit ? '#22c55e' : near ? '#f5a623' : color;
      const s  = (id,v) => { const el=document.getElementById(id); if(el) el.textContent=v; };
      const ss = (id,p,v) => { const el=document.getElementById(id); if(el) el.style[p]=v; };
      s(prefix+'-enc',    val.toLocaleString());
      s(prefix+'-pct',    pct+'%');
      s(prefix+'-status', hit ? '✓ Goal reached!' : (goal-val).toLocaleString()+' to go');
      ss(prefix+'-bar','width', pct+'%');
      ss(prefix+'-bar','background', c);
      ss(prefix+'-pct','color', c);
    }
    setCard('sc-total', tTot, 2820, 'var(--text)');
    setCard('sc-rh',    tRH,  2520, 'var(--rh)');
    setCard('sc-td',    tTD,  200,  'var(--td-c)');
    setCard('sc-mdl',   tMDL, 100,  'var(--mdl)');
    const tb = document.getElementById('sc-total-bar');
    if (tb) tb.style.background = 'linear-gradient(90deg,var(--rh),var(--td-c))';
  } catch(e) { console.warn('updateGoalCards:', e); }
}

function getCurrentMonthData() {
  const sheets = Object.values(ALL_DATA);
  if (!sheets.length) return FB_MAR26;
  sheets.sort((a,b) => a.year!==b.year ? a.year-b.year : a.month-b.month);
  const latest = sheets[sheets.length-1];
  if (ACTIVE_TYPE === 'all') return latest;
  const d = {...latest};
  if (ACTIVE_TYPE !== 'rh')  { d.rh=d.rh.map(()=>0);   d.rhRev=d.rhRev.map(()=>0); }
  if (ACTIVE_TYPE !== 'td')  { d.td=d.td.map(()=>0);   d.tdRev=d.tdRev.map(()=>0); }
  if (ACTIVE_TYPE !== 'mdl') { d.mdl=d.mdl.map(()=>0); d.mdlRev=d.mdlRev.map(()=>0); }
  return d;
}

// ── Update Last 30 Days stat cards ──
function updateStats(daily) {
  const n = daily.labels.length;
  const tRH=daily.rh.reduce((a,b)=>a+b,0), tTD=daily.td.reduce((a,b)=>a+b,0), tMDL=daily.mdl.reduce((a,b)=>a+b,0);
  const tEnc=tRH+tTD+tMDL;
  const tRev = (daily.rhRev||[]).reduce((a,b)=>a+b,0)
             + (daily.tdRev||[]).reduce((a,b)=>a+b,0)
             + (daily.mdlRev||[]).reduce((a,b)=>a+b,0);
  const e = (id,v) => { const el=document.getElementById(id); if(el) el.textContent=v; };
  e('mtd-enc', tEnc.toLocaleString());
  e('mtd-rev', '$'+Math.round(tRev).toLocaleString());
  e('avg-rh',  n?(tRH/n).toFixed(1):'—');
  e('avg-td',  n?(tTD/n).toFixed(1):'—');
  e('avg-mdl', n?(tMDL/n).toFixed(1):'—');
  e('days-rec', n+' days');
}

// ── Apply filters and update Last 30 Days chart ──
function applyFilters() {
  const days30 = getRolling30Days();
  const daily = {
    labels:  days30.map(d=>d.label),
    rh:      days30.map(d=>d.rh),
    td:      days30.map(d=>d.td),
    mdl:     days30.map(d=>d.mdl),
    rhRev:   days30.map(d=>d.rhRev),
    tdRev:   days30.map(d=>d.tdRev),
    mdlRev:  days30.map(d=>d.mdlRev),
  };
  if (revChart) {
    const now6pm = new Date(); now6pm.setHours(18,0,0,0);
    const isBeforeSix = new Date() < now6pm;
    const lastIdx = daily.labels.length - 1;
    revChart.data.labels = daily.labels;
    revChart.data.datasets[0].data = (daily.rhRev||[]).map((v,i)=>(i===lastIdx&&isBeforeSix)?null:v);
    revChart.data.datasets[1].data = (daily.tdRev||[]).map((v,i)=>(i===lastIdx&&isBeforeSix)?null:v);
    revChart.data.datasets[2].data = (daily.mdlRev||[]).map((v,i)=>(i===lastIdx&&isBeforeSix)?null:v);
    revChart.update('active');
  }
  updateStats(daily);
  updateRevSummary();
  if (document.getElementById('panel-revenue').classList.contains('active')) {
    buildRevTab30Day();
  }
  if (revHistChart || document.getElementById('panel-revenue').classList.contains('active')) {
    buildRevHistChart();
  }
  updateGoalCards();
  updateGoalsTab();
  const d = getFilteredData();
  if (!d) return;
  if (window._tabChartsBuilt) {
    volChart.data.labels = d.labels;
    volChart.data.datasets[0].data = d.rh;
    volChart.data.datasets[1].data = d.td;
    volChart.data.datasets[2].data = d.mdl;
    volChart.update('active');
    updateTabStats(d);
  }
}

// ── Today tab ──
function updateTodayTab() {
  try {
    const curData = getCurrentMonthData();
    if (!curData || !curData.labels.length) return;
    const lastIdx = curData.labels.length - 1;
    const rhVal  = curData.rh[lastIdx]  || 0;
    const tdVal  = curData.td[lastIdx]  || 0;
    const mdlVal = curData.mdl[lastIdx] || 0;
    const total  = rhVal + tdVal + mdlVal;
    const dateEl = document.getElementById('today-date');
    if (dateEl) {
      const now = new Date();
      const timeStr = now.toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'});
      dateEl.textContent = now.toLocaleDateString('en-US',{weekday:'long',month:'long',day:'numeric'})+' · refreshed '+timeStr;
    }
    function setHero(id, actual, target) {
      const pct  = Math.round(actual/target*100);
      const hit  = actual >= target;
      const near = pct >= 80;
      const barColor  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
      const badgeText = hit ? '✓ On target' : near ? 'Almost there' : 'Below target';
      const badgeBg   = hit ? 'rgba(34,197,94,.15)' : near ? 'rgba(245,166,35,.15)' : 'rgba(248,113,113,.15)';
      const badgeCol  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
      const needed    = hit ? (actual-target)+' above target 🎉' : (target-actual)+' more to hit target';
      const v=document.getElementById('today-'+id+'-val');     if(v) v.textContent=actual;
      const bar=document.getElementById('today-'+id+'-bar');   if(bar){bar.style.width=Math.min(pct,100)+'%';bar.style.background=barColor;}
      const pctEl=document.getElementById('today-'+id+'-pct'); if(pctEl){pctEl.textContent=pct+'%';pctEl.style.color=barColor;}
      const badge=document.getElementById('today-'+id+'-badge');if(badge){badge.textContent=badgeText;badge.style.background=badgeBg;badge.style.color=badgeCol;}
      const need=document.getElementById('today-'+id+'-needed');if(need) need.textContent=needed;
    }
    setHero('rh',  rhVal,  84);
    setHero('td',  tdVal,  10);
    setHero('mdl', mdlVal, 5);
    const DAILY_REV_TARGET = 1380;
    const todayRev = ((curData.rhRev||[])[lastIdx]||0)+((curData.tdRev||[])[lastIdx]||0)+((curData.mdlRev||[])[lastIdx]||0);
    const revPct   = Math.min(Math.round(todayRev/DAILY_REV_TARGET*100),100);
    const revColor = revPct>=100?'#22c55e':revPct>=75?'#f5a623':'#f87171';
    const trackFill  = document.getElementById('rev-track-fill');
    const trackBadge = document.getElementById('rev-track-badge');
    if (trackFill)  trackFill.style.width = revPct+'%';
    if (trackBadge) {
      trackBadge.style.left = Math.max(revPct,4)+'%';
      trackBadge.style.color = revColor;
      trackBadge.style.borderColor = revColor;
      trackBadge.textContent = '$'+Math.round(todayRev).toLocaleString()+' · '+revPct+'%';
    }
  } catch(e) { console.warn('updateTodayTab:', e); }
}

// ── Rolling 30-day progress cards (Today tab bottom section) ──
const MONTHLY_GOALS = { rh:2520, td:200, mdl:100, total:2820 };

function updateGoalsTab() {
  try {
    const days30 = getRolling30Days();
    if (!days30.length) return;
    const tRH  = days30.reduce((a,d)=>a+d.rh,  0);
    const tTD  = days30.reduce((a,d)=>a+d.td,  0);
    const tMDL = days30.reduce((a,d)=>a+d.mdl, 0);
    const tot  = tRH + tTD + tMDL;

    const mlEl = document.getElementById('today-month-label');
    if (mlEl) {
      const first = days30[0], last = days30[days30.length-1];
      mlEl.textContent = 'Last '+days30.length+' Days';
    }

    function setHero(id, actual, target) {
      const pct  = Math.round(actual/target*100);
      const hit  = actual >= target;
      const near = pct >= 80;
      const barColor  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
      const badgeText = hit ? '✓ Goal reached!' : near ? 'Almost there' : 'Below target';
      const badgeBg   = hit ? 'rgba(34,197,94,.15)' : near ? 'rgba(245,166,35,.15)' : 'rgba(248,113,113,.15)';
      const badgeCol  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
      const needed    = hit ? (actual-target).toLocaleString()+' above target 🎉' : (target-actual).toLocaleString()+' more to hit target';
      const v=document.getElementById('mgoal-'+id+'-val');     if(v) v.textContent=actual.toLocaleString();
      const bar=document.getElementById('mgoal-'+id+'-bar');   if(bar){bar.style.width=Math.min(pct,100)+'%';bar.style.background=barColor;}
      const pctEl=document.getElementById('mgoal-'+id+'-pct'); if(pctEl){pctEl.textContent=pct+'%';pctEl.style.color=barColor;}
      const badge=document.getElementById('mgoal-'+id+'-badge');if(badge){badge.textContent=badgeText;badge.style.background=badgeBg;badge.style.color=badgeCol;}
      const need=document.getElementById('mgoal-'+id+'-needed');if(need) need.textContent=needed;
    }
    setHero('rh',  tRH,  MONTHLY_GOALS.rh);
    setHero('td',  tTD,  MONTHLY_GOALS.td);
    setHero('mdl', tMDL, MONTHLY_GOALS.mdl);

    // 30-day revenue track
    const revTotal = days30.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev,0);
    const MONTHLY_REV_TARGET = 41400;
    const mrevPct  = Math.min(Math.round(revTotal/MONTHLY_REV_TARGET*100),100);
    const mrevColor= mrevPct>=100?'#22c55e':mrevPct>=75?'#f5a623':mrevPct>=40?'#fbbf24':'#f87171';
    const mFill  = document.getElementById('mrev-track-fill');
    const mBadge = document.getElementById('mrev-track-badge');
    if (mFill)  mFill.style.width = mrevPct+'%';
    if (mBadge) {
      mBadge.style.left = Math.max(mrevPct,4)+'%';
      mBadge.style.color = mrevColor;
      mBadge.style.borderColor = mrevColor;
      mBadge.textContent = '$'+Math.round(revTotal).toLocaleString()+' · '+mrevPct+'%';
    }
  } catch(e) { console.warn('updateGoalsTab:', e); }
}

// ── Fallback data ──
const FB_MAR26 = {
  labels:['3/1','3/2','3/3','3/4','3/5','3/6','3/7','3/8','3/9','3/10','3/11','3/12','3/13','3/14','3/15','3/16','3/17','3/18','3/19','3/20','3/21','3/22','3/23','3/24','3/25'],
  rh:    [84,63,84,66,65,78,84,84,82,84,81,115,98,115,84,85,85,70,115,90,70,141,51,67,22],
  td:    [0,6,0,7,9,3,0,0,0,0,5,0,4,0,0,0,0,9,0,2,7,0,2,0,10],
  mdl:   [0,4,0,2,0,0,0,0,1,0,0,0,4,0,0,0,0,1,0,1,0,0,0,0,0],
  rev:   [1008,1007,1008,1003,1002,1005,1008,1008,1009,1008,1107,1380,1381,1380,1008,1020,1020,1072,1380,1151,1016,1692,658,804,509],
  rhRev: [84,63,84,66,65,78,84,84,82,84,81,115,98,115,84,85,85,70,115,90,70,141,51,67,22].map(v=>Math.round(v*12)),
  tdRev: [0,6,0,7,9,3,0,0,0,0,5,0,4,0,0,0,0,9,0,2,7,0,2,0,10].map(v=>Math.round(v*24.33)),
  mdlRev:[0,4,0,2,0,0,0,0,1,0,0,0,4,0,0,0,0,1,0,1,0,0,0,0,0].map(v=>Math.round(v*25.46)),
  td1:[0,6,0,7,9,3,0,0,0,0,5,0,4,0,0,0,0,9,0,2,7,0,2,0,10],
  td2:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
  mdl1:[0,4,0,2,0,0,0,0,1,0,0,0,4,0,0,0,0,1,0,1,0,0,0,0,0],
  mdl2:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],
  month:3, year:2026,
};
ALL_DATA['Mar 26'] = FB_MAR26;
window._tabChartsBuilt = false;

const EMPTY = {labels:[],rh:[],td:[],mdl:[],rev:[],rhRev:[],tdRev:[],mdlRev:[],month:3,year:2026};
buildCharts(EMPTY);

['mtd-enc','mtd-rev','avg-rh','avg-mdl','avg-td'].forEach(id => {
  const el = document.getElementById(id);
  if(el){ el.textContent='·'; el.classList.add('loading-pulse'); }
});
document.getElementById('days-rec').textContent='Loading…';
document.getElementById('live-badge').style.display='flex';

// ── Cache ──
const CACHE_KEY    = 'tally_dashboard_cache';
const CACHE_TS_KEY = 'tally_dashboard_cache_ts';
function saveCache(text) {
  try { localStorage.setItem(CACHE_KEY,text); localStorage.setItem(CACHE_TS_KEY,Date.now().toString()); } catch(e){}
}
function loadCache() {
  try {
    const ts=localStorage.getItem(CACHE_TS_KEY), text=localStorage.getItem(CACHE_KEY);
    if(!text||!ts) return null;
    return {text, age:Math.round((Date.now()-+ts)/60000)};
  } catch(e){ return null; }
}

async function fetchCSV() {
  const endpoints = APPS_SCRIPT_URL
    ? [APPS_SCRIPT_URL+'?_='+Date.now()]
    : ['https://corsproxy.io/?url='+encodeURIComponent(SHEET_CSV_URL)];
  for (const url of endpoints) {
    try {
      const r = await fetch(url,{cache:'no-store',redirect:'follow'});
      if (!r.ok) throw new Error('HTTP '+r.status);
      const t = await r.text();
      if (!t||t.trim().length<10) throw new Error('empty');
      return t;
    } catch(e) {
      console.warn('Fetch failed:', e.message);
    }
  }
  throw new Error('Could not reach server');
}

async function loadSheet(background=false) {
  if (!background) {
    const cached = loadCache();
    if (cached) {
      try {
        const parsed = JSON.parse(cached.text);
        const allParsed = {};
        for (const [name,csv] of Object.entries(parsed)) {
          const d = parseSheetCSV(csv, name);
          if (d) { if(!d.year) d.year=yearFromTabName(name)||2026; allParsed[name]=d; }
        }
        if (Object.keys(allParsed).length) {
          ALL_DATA = allParsed;
          try { applyFilters(); } catch(e){}
          try { updateGoalCards(); } catch(e){}
          try { updateTodayTab(); } catch(e){}
          try { updateGoalsTab(); } catch(e){}
          document.getElementById('feed-dot').classList.remove('live');
          document.getElementById('feed-label').textContent = 'Cached · '+cached.age+'m ago · refreshing…';
          document.querySelectorAll('.loading-pulse').forEach(el=>el.classList.remove('loading-pulse'));
        }
      } catch(e){ console.warn('Cache parse failed:',e); }
    }
  }
  try {
    const t = await fetchCSV();
    let parsed = {};
    if (t.trim().startsWith('{')) {
      const json = JSON.parse(t);
      for (const [name,csv] of Object.entries(json)) {
        const d = parseSheetCSV(csv, name);
        if (d) { if(!d.year) d.year=yearFromTabName(name)||2026; parsed[name]=d; }
      }
    } else {
      const d = parseSheetCSV(t);
      if (d) parsed['Mar 26'] = d;
    }
    if (!Object.keys(parsed).length) throw new Error('No sheets parsed');
    saveCache(t);
    ALL_DATA = parsed;
    // Keep REV_WINDOW at its current value (default 365)
    try { applyFilters(); } catch(e){}
    try { updateGoalCards(); } catch(e){}
    try { updateTodayTab(); } catch(e){}
    try { updateGoalsTab(); } catch(e){}
    try { buildRevTab30Day(); updateRevSummary(); buildRevHistChart(); } catch(e){}
    document.querySelectorAll('.loading-pulse').forEach(el=>el.classList.remove('loading-pulse'));
    // Sync window dropdowns to current REV_WINDOW
    document.querySelectorAll('select[id^="window-select-"]').forEach(s=>{ if(s.value!==String(REV_WINDOW)) s.value=String(REV_WINDOW); });
    document.getElementById('feed-dot').classList.add('live');
    document.getElementById('feed-label').textContent = 'Live · last: '+new Date().toLocaleTimeString();
    try {
      const allSheetsSorted = Object.values(ALL_DATA).sort((a,b)=>a.year!==b.year?a.year-b.year:a.month-b.month);
      if (allSheetsSorted.length) {
        const moN=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
        const first = allSheetsSorted[0];
        const rangeEl = document.getElementById('date-range');
        if (rangeEl) rangeEl.textContent = moN[first.month-1]+' '+first.year+' – Present';
      }
    } catch(e){}
    const psubEl = document.getElementById('psub');
    if (psubEl) psubEl.textContent = 'Pulled from '+Object.keys(parsed).length+' sheet tabs · refreshes every minute';
    const luEl = document.getElementById('last-updated');
    if (luEl) luEl.textContent = new Date().toLocaleTimeString();
    const notice = document.getElementById('setup-notice');
    if (notice) notice.style.display = 'none';
  } catch(e) {
    document.getElementById('feed-dot').classList.remove('live');
    document.getElementById('feed-label').textContent = 'Feed unavailable · cached data';
    const psubEl = document.getElementById('psub');
    if (psubEl) psubEl.textContent = '⚠️ '+e.message;
    const luEl = document.getElementById('last-updated');
    if (luEl) luEl.textContent = 'Failed · '+new Date().toLocaleTimeString();
  }
}
loadSheet();
setInterval(()=>loadSheet(true), 60*1000);

// ── Tab switching ──
function switchTab(id, el) {
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('panel-'+id).classList.add('active');
  el.classList.add('active');
  setTimeout(() => {
    if (id === 'daily') { revChart && revChart.resize(); return; }
    const d = getFilteredData() || FB_MAR26;
    if (!window._tabChartsBuilt) {
      buildTabCharts(d);
      window._tabChartsBuilt = true;
    } else {
      if (id === 'revenue') {
        REV_MONTH = 'all'; ACTIVE_YEAR = 'all'; REV_WINDOW = 365;
        const rms = document.getElementById('month-select-rev');
        if (rms) rms.value = 'all';
        const wsr = document.getElementById('window-select-rev');
        if (wsr) wsr.value = '365';
        buildRevTab30Day(); updateRevSummary(); buildRevHistChart();
        setTimeout(()=>{ revTabChart&&revTabChart.resize(); revHistChart&&revHistChart.resize(); },50);
      }
      if (id === 'volume') {
        ACTIVE_YEAR = 'all'; REV_MONTH = 'all'; REV_WINDOW = 365;
        const wsd = document.getElementById('window-select-vol');
        if (wsd) wsd.value = '365';
        buildVolWindowChart();
        setTimeout(()=>volChart&&volChart.resize(), 50);
      }
      if (id === 'today') { updateTodayTab(); updateGoalsTab(); }
    }
  }, 80);
}

// ── Volume window chart ──
function buildVolWindowChart() {
  if (!volChart) return;
  const tMap = {0:'All Time','-1':'Year to Date',7:'7 Days',30:'30 Days',60:'60 Days',90:'90 Days',180:'6 Months',365:'Last 12 Months'};
  const label = tMap[REV_WINDOW]||REV_WINDOW+' Days';
  const vt = document.getElementById('vol-window-title');
  if (vt) {
    const volLabel = ACTIVE_YEAR!=='all' ? String(ACTIVE_YEAR) : label;
    vt.textContent = 'Daily Encounter Volume — '+volLabel;
  }
  const savedMonth = REV_MONTH; REV_MONTH = 'all';
  const days = getLast30DaysData();
  REV_MONTH = savedMonth;
  volChart.data.labels = days.map(d=>d.label);
  volChart.data.datasets[0].data = (ACTIVE_TYPE==='all'||ACTIVE_TYPE==='rh')  ? days.map(d=>d.rh)  : days.map(()=>0);
  volChart.data.datasets[1].data = (ACTIVE_TYPE==='all'||ACTIVE_TYPE==='td')  ? days.map(d=>d.td)  : days.map(()=>0);
  volChart.data.datasets[2].data = (ACTIVE_TYPE==='all'||ACTIVE_TYPE==='mdl') ? days.map(d=>d.mdl) : days.map(()=>0);
  volChart.update('active');
  const e = (id,v) => { const el=document.getElementById(id); if(el) el.textContent=v.toLocaleString(); };
  e('vol-rh',  days.reduce((a,d)=>a+d.rh, 0));
  e('vol-td',  days.reduce((a,d)=>a+d.td, 0));
  e('vol-mdl', days.reduce((a,d)=>a+d.mdl,0));
}

// ── Filter controls ──
function setMonthDd(value) {
  ACTIVE_MONTH = value==='all' ? 'all' : +value;
  document.querySelectorAll('select[id^="month-select-"]').forEach(s=>s.value=value);
  try { applyFilters(); } catch(e){}
  try { buildRevTab30Day(); updateRevSummary(); buildRevHistChart(); } catch(e){}
  try { buildVolWindowChart(); } catch(e){}
}

function setRevMonth(value) {
  REV_MONTH = value;
  document.querySelectorAll('select[id^="month-select-"]').forEach(s=>s.value=value);
  buildRevTab30Day(); updateRevSummary();
}

function setWindowCombined(value) {
  document.querySelectorAll('select[id^="window-select-"]').forEach(s=>s.value=value);
  if (value.startsWith('year-')) {
    ACTIVE_YEAR = value.replace('year-','');
    REV_WINDOW  = 0;
  } else {
    ACTIVE_YEAR = 'all';
    REV_WINDOW  = +value;
  }
  try { applyFilters(); } catch(e){}
  try { buildRevTab30Day(); updateRevSummary(); buildRevHistChart(); } catch(e){}
  try { buildVolWindowChart(); } catch(e){}
  const tMap = {0:'All Time','-1':'Year to Date',7:'7 Days',30:'30 Days',60:'60 Days',90:'90 Days',180:'6 Months',365:'Last 12 Months'};
  const lbl = ACTIVE_YEAR!=='all' ? ACTIVE_YEAR : (tMap[String(REV_WINDOW)]||REV_WINDOW+' Days');
  const rt = document.getElementById('rev-30day-title'); if(rt) rt.textContent='Daily Revenue — '+lbl;
  const vt = document.getElementById('vol-window-title'); if(vt) vt.textContent='Daily Encounter Volume — '+lbl;
}

function setPlatformDd(value) {
  ACTIVE_TYPE = value;
  document.querySelectorAll('select[id^="platform-select-"]').forEach(s=>s.value=value);
  try { applyFilters(); } catch(e){}
  try { buildRevTab30Day(); updateRevSummary(); } catch(e){}
  try { buildVolWindowChart(); } catch(e){}
}

function manualRefresh(btn) {
  const orig = btn.innerHTML;
  btn.innerHTML = '↻ Refreshing…';
  btn.style.color = '#60a5fa';
  btn.disabled = true;
  loadSheet(false).finally(()=>{
    btn.innerHTML = orig;
    btn.style.color = 'var(--text2)';
    btn.disabled = false;
  });
}
</script>
</body>
</html>
