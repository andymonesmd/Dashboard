
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
  <div class="psub" id="today-sub">Live progress toward daily &amp; monthly targets · updates on refresh</div>

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


</div>

<!-- FOOTER -->
