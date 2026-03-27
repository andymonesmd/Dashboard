
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

/* ── TOPBAR ── */
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

/* ── FILTERBAR ── */
.filterbar{
  display:flex;gap:16px 32px;align-items:center;flex-wrap:wrap;
  padding:14px 28px;background:var(--surface);border-bottom:1px solid var(--border);
}
.filter-group{display:flex;align-items:center;gap:8px;flex-wrap:wrap;}
.flabel{font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);white-space:nowrap;}
.fdivider{width:1px;height:22px;background:var(--border2);}
.pill{
  padding:5px 14px;border-radius:20px;border:1px solid var(--border2);
  background:transparent;color:var(--text2);font-size:.78rem;
  cursor:pointer;transition:.15s;font-family:'DM Sans',sans-serif;
}
.pill:hover{border-color:var(--text3);color:var(--text);}
.pill.active{background:var(--pill-active);border-color:var(--pill-active);color:#fff;font-weight:600;}

/* ── STAT CARDS ── */
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

/* ── TABS ── */
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

/* ── PANELS ── */
.panel{
  display:none;background:var(--surface2);
  border:1px solid var(--border);border-top:none;
  margin:0 28px;border-radius:0 0 12px 12px;
  padding:24px;animation:fadeIn .2s ease;
}
.panel.active{display:block;}
@keyframes fadeIn{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:none}}
.placeholder{text-align:center;padding:60px;color:var(--text3);font-size:.9rem;}

/* ── DAILY PANEL ── */
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

/* ── FOOTER ── */
.footer-row{
  display:flex;justify-content:space-between;flex-wrap:wrap;gap:6px;
  padding:12px 28px 0;font-size:.68rem;color:var(--text3);
}

@media(max-width:700px){
  .statrow{grid-template-columns:1fr 1fr;}
  .mtd-row{grid-template-columns:1fr;}
  .topbar,.filterbar{flex-direction:column;align-items:flex-start;}
}
.loading-pulse {
  display:inline-block;
  background:var(--border2);
  border-radius:4px;
  color:transparent!important;
  animation:skeleton-pulse 1.4s ease infinite;
  min-width:50px;
  user-select:none;
}
@keyframes skeleton-pulse {
  0%,100%{opacity:.35} 50%{opacity:.75}
}
</style>
</head>
<body>

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-left">
    <h1>Mones TeleDash</h1>
    <div class="sub">
      <span id="date-range">Jan 2024 – Present</span> &nbsp;·&nbsp;
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

<!-- FILTERBAR -->
<div class="filterbar">
  <div class="filter-group">
    <span class="flabel">Filter by year:</span>
    <button class="pill active" onclick="setYear(this,'all')">All</button>
    <button class="pill" onclick="setYear(this,'2024')">2024</button>
    <button class="pill" onclick="setYear(this,'2025')">2025</button>
    <button class="pill" onclick="setYear(this,'2026')">2026</button>
  </div>
  <div class="fdivider"></div>
  <div class="filter-group">
    <span class="flabel">Filter by month:</span>
    <button class="pill active" onclick="setMonth(this,'all')">All</button>
    <button class="pill" onclick="setMonth(this,1)">Jan</button>
    <button class="pill" onclick="setMonth(this,2)">Feb</button>
    <button class="pill" onclick="setMonth(this,3)">Mar</button>
    <button class="pill" onclick="setMonth(this,4)">Apr</button>
    <button class="pill" onclick="setMonth(this,5)">May</button>
    <button class="pill" onclick="setMonth(this,6)">Jun</button>
    <button class="pill" onclick="setMonth(this,7)">Jul</button>
    <button class="pill" onclick="setMonth(this,8)">Aug</button>
    <button class="pill" onclick="setMonth(this,9)">Sep</button>
    <button class="pill" onclick="setMonth(this,10)">Oct</button>
    <button class="pill" onclick="setMonth(this,11)">Nov</button>
    <button class="pill" onclick="setMonth(this,12)">Dec</button>
  </div>
  <div class="fdivider"></div>
  <div class="filter-group" id="rev-window-group">
    <span class="flabel">Revenue window:</span>
    <button class="pill active" onclick="setRevWindow(this,0)">All Time</button>
    <button class="pill" onclick="setRevWindow(this,365)">Last Year</button>
    <button class="pill" onclick="setRevWindow(this,180)">Last 6mo</button>
    <button class="pill" onclick="setRevWindow(this,90)">Last 90d</button>
    <button class="pill" onclick="setRevWindow(this,60)">Last 60d</button>
    <button class="pill" onclick="setRevWindow(this,30)">Last 30d</button>
  </div>
</div>

<!-- STAT CARDS -->
<div class="statrow" style="grid-template-columns:repeat(4,1fr);">
  <div class="statcard">
    <div class="sclabel" id="s-period-label">Avg Per Month</div>
    <div class="scrow">
      <div class="metric"><span class="mname">Encounters</span><span class="mval rh-c" id="s-enc">—</span></div>
      <div class="metric"><span class="mname">Revenue</span><span class="mval rev-c" id="s-rev">—</span></div>
    </div>
  </div>
  <div class="statcard">
    <div class="sclabel" id="s-visits-label">Avg Visits / Month</div>
    <div class="scrow">
      <div class="metric"><span class="mname">Roman</span><span class="mval rh-c" id="s-rh-mo">—</span></div>
      <div class="metric"><span class="mname">Teladoc</span><span class="mval td-c2" id="s-td-mo">—</span></div>
      <div class="metric"><span class="mname">MDLive</span><span class="mval mdl-c" id="s-mdl-mo">—</span></div>
    </div>
  </div>
  <div class="statcard">
    <div class="sclabel">Avg Visits / Day</div>
    <div class="scrow">
      <div class="metric"><span class="mname">Roman</span><span class="mval rh-c" id="s-rh-day">—</span></div>
      <div class="metric"><span class="mname">Teladoc</span><span class="mval td-c2" id="s-td-day">—</span></div>
      <div class="metric"><span class="mname">MDLive</span><span class="mval mdl-c" id="s-mdl-day">—</span></div>
    </div>
  </div>
  <div class="statcard">
    <div class="sclabel" id="s-rev-label">Avg Revenue / Day</div>
    <div class="scrow">
      <div class="metric"><span class="mname">Roman</span><span class="mval rh-c" id="s-rh-rev-day">—</span></div>
      <div class="metric"><span class="mname">Teladoc</span><span class="mval td-c2" id="s-td-rev-day">—</span></div>
      <div class="metric"><span class="mname">MDLive</span><span class="mval mdl-c" id="s-mdl-rev-day">—</span></div>
    </div>
  </div>
</div>

<!-- SETUP NOTICE -->
<div id="setup-notice" style="display:none;
  margin:16px 28px 0;
  background:#1a1f00;
  border:1px solid #3a4500;
  border-left:3px solid #a3e635;
  border-radius:8px;
  padding:16px 20px;
  font-size:.78rem;
  color:#bef264;
  line-height:1.8;
  font-family:'DM Sans',sans-serif;
">
  <strong style="font-size:.85rem;color:#d9f99d;">⚡ One-time setup to enable the live feed (takes 2 minutes):</strong><br/>
  <span style="color:#94a3b8;">
  1. Open your Google Sheet → click <strong style="color:#d9f99d;">Extensions → Apps Script</strong><br/>
  2. Delete any existing code and paste this in:
  </span>
  <pre style="
    background:#0d1117;border:1px solid #1e2a3a;border-radius:6px;
    padding:12px 14px;margin:10px 0 10px;color:#a5f3fc;
    font-size:.75rem;overflow-x:auto;line-height:1.6;
  ">function doGet() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheets = ss.getSheets();
  var result = {};

  sheets.forEach(function(sheet) {
    var name = sheet.getName();
    // Skip any sheet that doesn't look like a month tab (e.g. "Jan 24", "Mar 26")
    if (!/^[A-Za-z]{3}\s*\d{2,4}$/.test(name.trim())) return;

    // Only read first 8 rows (header + 6 data rows + total) and first 35 cols (31 days + extras)
    var lastRow = Math.min(sheet.getLastRow(), 8);
    var lastCol = Math.min(sheet.getLastColumn(), 35);
    if (lastRow < 2 || lastCol < 2) return;

    var data = sheet.getRange(1, 1, lastRow, lastCol).getValues();
    var csv = data.map(function(row) {
      return row.map(function(cell) {
        var s;
        if (cell instanceof Date) {
          s = (cell.getMonth()+1) + "-" + cell.getDate() + "-" + cell.getFullYear();
        } else {
          s = String(cell).trim();
        }
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
  5. Copy the <strong style="color:#d9f99d;">Web app URL</strong> (ends in <code style="background:#1e2a3a;padding:1px 5px;border-radius:3px">/exec</code>)<br/>
  6. Open this HTML file in a text editor → find <code style="background:#1e2a3a;padding:1px 5px;border-radius:3px">APPS_SCRIPT_URL = ""</code> → paste the URL between the quotes → save
  </span>
</div>

<!-- TAB NAV -->
<div class="tabnav">
  <button class="tab" onclick="switchTab('volume',this)">Encounter Volume</button>
  <button class="tab" onclick="switchTab('revenue',this)">Revenue</button>
  <button class="tab" onclick="switchTab('breakdown',this)">Visit Breakdown</button>
  <button class="tab" onclick="switchTab('daily',this)">Current Month</button>
  <button class="tab live-tab active" onclick="switchTab('today',this)">Today</button>
</div>

<!-- PANELS -->
<div id="panel-revenue" class="panel">
  <div class="ph2" style="margin-bottom:4px;" id="rev-title">Revenue by Platform</div>
  <div class="psub">Estimated daily revenue based on per-visit rates · RH $12/visit · TD $24.33/visit · MDL $25.46/visit</div>

  <!-- Total revenue summary card -->
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
    <h3 id="rev-30day-title">Daily Revenue — All Time</h3>
    <div class="chart-wrap" style="height:280px;"><canvas id="revTabChart"></canvas></div>
    <div class="clegend" style="margin-top:10px;">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>

  <!-- Month-to-month historical chart -->
  <div class="chart-sec" style="margin-top:28px;">
    <h3 id="rev-hist-title" style="margin-bottom:12px;">Monthly Revenue — All Time</h3>
    <div class="chart-wrap" style="height:300px;"><canvas id="revHistChart"></canvas></div>
    <div class="clegend" style="margin-top:10px;" id="rev-hist-legend"></div>
  </div>
</div>

<div id="panel-volume" class="panel">
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

<div id="panel-breakdown" class="panel">
  <div class="ph2" style="margin-bottom:4px;" id="breakdown-title">Visit Breakdown</div>
  <div class="psub" id="breakdown-sub">Encounters by provider and visit type</div>

  <!-- Top: horizontal stacked bar chart -->
  <div class="chart-sec" style="margin-bottom:24px;">
    <h3>Phone vs Video Encounters — Teladoc &amp; MDLive</h3>
    <div class="chart-wrap" style="height:200px;"><canvas id="breakdownChart"></canvas></div>
    <div class="clegend" style="margin-top:10px;">
      
      <div class="cleg"><div class="cleg-dot" style="background:#7b9ef0"></div>TD Phone</div>
      <div class="cleg"><div class="cleg-dot" style="background:#4f6dd9"></div>TD Video</div>
      <div class="cleg"><div class="cleg-dot" style="background:#f5a623"></div>MDL Phone</div>
      <div class="cleg"><div class="cleg-dot" style="background:#e07b00"></div>MDL Video</div>
    </div>
  </div>

  <!-- Bottom: stat cards per provider with phone/video split -->
  <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:14px;">

    <!-- Roman -->
    <div class="mtd-card">
      <div class="mclabel" style="color:var(--rh);">Roman</div>
      <div style="font-size:1.8rem;font-weight:700;color:var(--rh);margin:6px 0 2px;" id="bd-rh-tot">—</div>
      <div style="font-size:.7rem;color:var(--text3);" id="bd-rh-pct">— of total</div>
      <div style="margin-top:10px;font-size:.68rem;color:var(--text2);">
        <span style="background:rgba(45,212,160,.15);padding:2px 8px;border-radius:8px;">Async only</span>
      </div>
    </div>

    <!-- Teladoc -->
    <div class="mtd-card">
      <div class="mclabel" style="color:var(--td-c);">Teladoc</div>
      <div style="font-size:1.8rem;font-weight:700;color:var(--td-c);margin:6px 0 2px;" id="bd-td-tot">—</div>
      <div style="font-size:.7rem;color:var(--text3);" id="bd-td-pct">— of total</div>
      <div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;">
        <div style="font-size:.68rem;">
          <span style="color:#7b9ef0;">📞 Phone:</span>
          <span style="color:var(--text);font-weight:600;" id="bd-td1-val">—</span>
        </div>
        <div style="font-size:.68rem;">
          <span style="color:#4f6dd9;">🎥 Video:</span>
          <span style="color:var(--text);font-weight:600;" id="bd-td2-val">—</span>
        </div>
      </div>
    </div>

    <!-- MDLive -->
    <div class="mtd-card">
      <div class="mclabel" style="color:var(--mdl);">MDLive</div>
      <div style="font-size:1.8rem;font-weight:700;color:var(--mdl);margin:6px 0 2px;" id="bd-mdl-tot">—</div>
      <div style="font-size:.7rem;color:var(--text3);" id="bd-mdl-pct">— of total</div>
      <div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;">
        <div style="font-size:.68rem;">
          <span style="color:#f5a623;">📞 Phone:</span>
          <span style="color:var(--text);font-weight:600;" id="bd-mdl1-val">—</span>
        </div>
        <div style="font-size:.68rem;">
          <span style="color:#e07b00;">🎥 Video:</span>
          <span style="color:var(--text);font-weight:600;" id="bd-mdl2-val">—</span>
        </div>
      </div>
    </div>

  </div>

  <!-- Day of Week Volume Chart -->
  <div class="chart-sec" style="margin-top:28px;">
    <h3>Encounters by Day of Week</h3>
    <div class="psub" style="margin-bottom:12px;">Average daily encounters per weekday — based on current filter selection</div>
    <div class="chart-wrap" style="height:280px;"><canvas id="dowChart"></canvas></div>
    <div class="clegend" style="margin-top:10px;">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>
</div>



<div id="panel-today" class="panel active">
  <div class="ph2" style="margin-bottom:4px;">
    Today
    <span class="live-badge" id="today-live-badge" style="display:flex;">
      <span class="dot"></span>
      <span id="today-date">—</span>
    </span>
  </div>
  <div class="psub" id="today-sub">Live progress toward daily &amp; monthly targets · auto-refreshes every minute</div>

  <!-- Section label -->
  <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3);margin:20px 0 10px;">Daily Progress</div>

  <!-- Daily hero cards -->
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

  <!-- Daily combined total -->
  <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:18px 24px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:16px;margin-bottom:28px;">
    <div><div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px;">Total Today</div><div style="font-size:2.2rem;font-weight:800;color:var(--text);line-height:1;" id="today-total">—</div></div>
    <div><div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px;">Daily Target</div><div style="font-size:2.2rem;font-weight:800;color:var(--text2);line-height:1;">99</div></div>
    <div><div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px;">Progress</div><div style="font-size:2.2rem;font-weight:800;line-height:1;" id="today-overall-pct">—</div></div>
    <div style="flex:1;min-width:200px;"><div style="background:var(--border);border-radius:6px;height:12px;overflow:hidden;"><div id="today-total-bar" style="height:100%;border-radius:6px;background:linear-gradient(90deg,var(--rh),var(--td-c));transition:width .8s ease;width:0%;"></div></div></div>
  </div>

  <!-- Section label -->
  <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3);margin-bottom:10px;">Month-to-Date Progress — <span id="today-month-label">Mar 2026</span></div>

  <!-- Monthly hero cards (no revenue) -->
  <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:20px;margin-bottom:20px;">
    <!-- Roman monthly -->
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
    <!-- Teladoc monthly -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--td-c);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">Teladoc</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3rem;font-weight:800;color:var(--td-c);line-height:1;" id="mgoal-td-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:6px;">/ 300</div>
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
    <!-- MDLive monthly -->
    <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:24px;position:relative;overflow:hidden;">
      <div style="position:absolute;top:0;left:0;right:0;height:3px;background:var(--mdl);"></div>
      <div style="font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:8px;">MDLive</div>
      <div style="display:flex;align-items:flex-end;gap:10px;margin-bottom:16px;">
        <div style="font-size:3rem;font-weight:800;color:var(--mdl);line-height:1;" id="mgoal-mdl-val">—</div>
        <div style="font-size:1rem;color:var(--text3);margin-bottom:6px;">/ 150</div>
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

  <!-- Monthly combined total -->
  <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:18px 24px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:16px;">
    <div><div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px;">Total This Month</div><div style="font-size:2.2rem;font-weight:800;color:var(--text);line-height:1;" id="mgoal-total">—</div></div>
    <div><div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px;">Monthly Target</div><div style="font-size:2.2rem;font-weight:800;color:var(--text2);line-height:1;">2,970</div></div>
    <div><div style="font-size:.63rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px;">Overall Progress</div><div style="font-size:2.2rem;font-weight:800;line-height:1;" id="mgoal-overall-pct">—</div></div>
    <div style="flex:1;min-width:200px;"><div style="background:var(--border);border-radius:6px;height:12px;overflow:hidden;"><div id="mgoal-total-bar" style="height:100%;border-radius:6px;background:linear-gradient(90deg,var(--rh),var(--td-c));transition:width .8s ease;width:0%;"></div></div></div>
  </div>

</div>

<div id="panel-daily" class="panel">
  <div class="ph2">
    Current Month Feed
    <span class="live-badge" id="live-badge">
      <span class="dot"></span>
      Live · <span id="days-rec">—</span>
    </span>
  </div>
  <div class="psub" id="psub">Loading from Google Sheet…</div>

  <div class="mtd-row" style="grid-template-columns:repeat(5,1fr);">
    <div class="mtd-card">
      <div class="mclabel">MTD Encounters</div>
      <div class="mcval rh-c" id="mtd-enc">—</div>
    </div>
    <div class="mtd-card">
      <div class="mclabel">MTD Revenue</div>
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
    <h3>Daily Encounters</h3>
    <div class="chart-wrap"><canvas id="encChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>

  <div class="chart-sec">
    <h3>Daily Revenue (estimated by provider)</h3>
    <div class="chart-wrap"><canvas id="revChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
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

// ── Revenue rates per visit ──
// Per-visit reimbursement rates (verified against CSV totals)
const RATES = {
  rh:    12.00,  // Roman
  td1:   23.00,  // Teladoc phone (row 2)
  td2:   28.00,  // Teladoc video (row 4)
  mdl1:  25.00,  // MDLive phone  (row 3)
  mdl2:  28.00,  // MDLive video  (row 5)
};

// ── Global state ──
let ALL_DATA     = {};   // { "Jan 24": {labels,rh,td,mdl,rev,...}, ... }
let ACTIVE_YEAR   = 'all';
let ACTIVE_MONTH  = 'all';
let ACTIVE_TYPE   = 'all';
let REV_WINDOW    = 0;  // 0 = All Time (default)
let encChart=null, revChart=null;
let revTabChart=null, volChart=null, pieChart=null, cmpChart=null;
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

// ── Parse a date header into {month,day,year} ──
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

// ── Parse month/year from a tab name like "Jan 25", "Feb 2024", "Mar 26" ──
function monthYearFromTabName(name) {
  const moMap = {jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12};
  // "Jan 25", "Feb 2024", "Mar 26"
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

// ── Parse one sheet's CSV text into a data object ──
function parseSheetCSV(text, tabName) {
  const rows = text.trim().split('\n').map(parseCSVRow);
  if (rows.length < 2) return null;
  const headers = rows[0];

  // Derive canonical month/year from the TAB NAME first (most reliable)
  const nameDate = tabName ? monthYearFromTabName(tabName) : null;

  const allDateCols = [];
  headers.forEach((h,i) => {
    if (i===0) return;
    const d = parseDateCol(h.trim());
    if (!d) return;
    // If we know the sheet's month from its name, only include columns that match
    if (nameDate) {
      if (d.month === nameDate.month) allDateCols.push({i,...d});
    } else {
      allDateCols.push({i,...d});
    }
  });

  // Fallback: if name-filtered gave nothing, use all date cols
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
    // Track TD and MDL rows in order of appearance
    // TD variants: TD, TD1, TD2
    // MDL variants: MD, MDL, MDL1, MDL2, MLV (MDLive video)
    // RO variants: RO, RO+
    const isTD  = name.startsWith('TD');
    const isMDL = name==='MD'||name==='MDL'||name==='ML'||name.startsWith('MLV')||name.startsWith('MDL');
    const isRO  = name==='RO'||name==='RO+';

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
  // Combined td/mdl for chart display
  const td  = td1.map((v,i)=>v+td2[i]);
  const mdl = mdl1.map((v,i)=>v+mdl2[i]);

  let lastIdx = -1;
  for (let j=0;j<n;j++) if(rh[j]>0||td[j]>0||mdl[j]>0||rev[j]>0) lastIdx=j;
  if (lastIdx<0) return null;
  const t = lastIdx+1;

  // Use tab name for month/year if available, else first date col
  const sheetMonth = nameDate ? nameDate.month : dateCols[0].month;
  const sheetYear  = nameDate ? nameDate.year  : dateCols[0].year;

  return {
    labels: labels.slice(0,t),
    rh:   rh.slice(0,t),
    td:   td.slice(0,t),   td1: td1.slice(0,t),  td2: td2.slice(0,t),
    mdl:  mdl.slice(0,t),  mdl1:mdl1.slice(0,t), mdl2:mdl2.slice(0,t),
    rev:  rev.slice(0,t),
    // Revenue = visits × correct per-visit rate per sub-type
    rhRev:  rh.slice(0,t).map(v  => Math.round(v*RATES.rh)),
    tdRev:  td1.slice(0,t).map((v,i) => Math.round(v*RATES.td1 + td2[i]*RATES.td2)),
    mdlRev: mdl1.slice(0,t).map((v,i)=> Math.round(v*RATES.mdl1 + mdl2[i]*RATES.mdl2)),
    month: sheetMonth,
    year:  sheetYear,
  };
}

// ── Derive a year string from a sheet tab name ──
// "Jan 24" → 2024, "Mar 26" → 2026, "Jan 2024" → 2024
function yearFromTabName(name) {
  let m;
  m = name.trim().match(/(\d{4})/);
  if (m) return +m[1];
  m = name.trim().match(/(\d{2})\s*$/);
  if (m) { const y=+m[1]; return y<50?2000+y:1900+y; }
  return null;
}

// ── Aggregate multiple sheet data objects into one combined dataset ──
function aggregate(dataArr) {
  if (!dataArr.length) return null;
  const labels=[], rh=[], td=[], td1=[], td2=[], mdl=[], mdl1=[], mdl2=[],
        rev=[], rhRev=[], tdRev=[], mdlRev=[];
  dataArr.forEach(d => {
    d.labels.forEach((l,i) => {
      labels.push(l);
      rh.push(d.rh[i]);
      td.push(d.td[i]);   td1.push((d.td1||[])[i]||0); td2.push((d.td2||[])[i]||0);
      mdl.push(d.mdl[i]); mdl1.push((d.mdl1||[])[i]||0); mdl2.push((d.mdl2||[])[i]||0);
      rev.push(d.rev[i]);
      rhRev.push((d.rhRev||[])[i]||0);
      tdRev.push((d.tdRev||[])[i]||0);
      mdlRev.push((d.mdlRev||[])[i]||0);
    });
  });
  return {labels,rh,td,td1,td2,mdl,mdl1,mdl2,rev,rhRev,tdRev,mdlRev};
}

// ── Get filtered dataset based on ACTIVE_YEAR / ACTIVE_MONTH / ACTIVE_TYPE ──
function getFilteredData() {
  let sheets = Object.entries(ALL_DATA);
  if (!sheets.length) return null;

  // Year filter
  if (ACTIVE_YEAR !== 'all') {
    const yr = +ACTIVE_YEAR;
    sheets = sheets.filter(([name, d]) => d.year === yr || yearFromTabName(name) === yr);
  }

  // Month filter
  if (ACTIVE_MONTH !== 'all') {
    const mo = +ACTIVE_MONTH;
    sheets = sheets.filter(([, d]) => d.month === mo);
  }

  if (!sheets.length) return null;

  // Sort by year then month
  sheets.sort(([,a],[,b]) => a.year !== b.year ? a.year - b.year : a.month - b.month);

  let agg = aggregate(sheets.map(([,d])=>d));

  // Type filter — zero out other series
  if (ACTIVE_TYPE !== 'all') {
    agg = {...agg};
    if (ACTIVE_TYPE !== 'rh')  { agg.rh=agg.rh.map(()=>0);  agg.rhRev=agg.rhRev.map(()=>0); }
    if (ACTIVE_TYPE !== 'td')  { agg.td=agg.td.map(()=>0);  agg.tdRev=agg.tdRev.map(()=>0); }
    if (ACTIVE_TYPE !== 'mdl') { agg.mdl=agg.mdl.map(()=>0); agg.mdlRev=agg.mdlRev.map(()=>0); }
  }
  return agg;
}

// ── Chart grid color ──
const GRID='#1e2a3a';
Chart.defaults.color='#64748b';
Chart.defaults.font.family="'DM Sans',sans-serif";
Chart.defaults.font.size=11;

function tooltipDefaults() {
  return {backgroundColor:'#1c2333',borderColor:'#2d3a50',borderWidth:1,titleColor:'#94a3b8',bodyColor:'#e2e8f0',padding:10};
}

// ── Build all charts ──
function buildCharts(d) {
  // Daily encounters bar
  const ec = document.getElementById('encChart').getContext('2d');
  encChart = new Chart(ec,{
    type:'bar', data:{labels:d.labels, datasets:[
      {label:'Roman',data:d.rh, backgroundColor:'rgba(45,212,160,.82)',borderWidth:0,borderRadius:2,stack:'s'},
      {label:'Teladoc',     data:d.td, backgroundColor:'rgba(123,158,240,.82)',borderWidth:0,borderRadius:2,stack:'s'},
      {label:'MDLive',      data:d.mdl,backgroundColor:'rgba(245,166,35,.82)', borderWidth:0,borderRadius:2,stack:'s'},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{footer:items=>'Total: '+items.reduce((a,i)=>a+i.raw,0)}}},
      scales:{x:{stacked:true,grid:{color:GRID},ticks:{maxRotation:45}},y:{stacked:true,grid:{color:GRID},beginAtZero:true}}}
  });

  // Daily revenue lines
  const rc = document.getElementById('revChart').getContext('2d');
  revChart = new Chart(rc,{
    type:'line', data:{labels:d.labels, datasets:[
      {label:'Roman',data:d.rhRev||[], borderColor:'#2dd4a0',backgroundColor:'rgba(45,212,160,.08)',fill:true, tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5},
      {label:'Teladoc', data:d.tdRev||[], borderColor:'#7b9ef0',backgroundColor:'transparent',fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5},
      {label:'MDLive',  data:d.mdlRev||[],borderColor:'#f5a623',backgroundColor:'transparent',fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{label:i=>' $'+i.raw.toLocaleString()}}},
      scales:{x:{grid:{color:GRID},ticks:{maxRotation:45}},y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(1)+'k':v)}}}}
  });
}

function buildTabCharts(d) {
  // Revenue tab line chart
  const mkLine = (id,datasets) => new Chart(document.getElementById(id).getContext('2d'),{
    type:'line', data:{labels:d.labels,datasets},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{label:i=>' $'+i.raw.toLocaleString()}}},
      scales:{x:{grid:{color:GRID},ticks:{maxRotation:45}},y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(1)+'k':v)}}}}
  });
  const mkBar=(id,datasets,stacked=true)=>new Chart(document.getElementById(id).getContext('2d'),{
    type:'bar', data:{labels:d.labels,datasets},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{footer:stacked?items=>'Total: '+items.reduce((a,i)=>a+i.raw,0):undefined}}},
      scales:{x:{stacked,grid:{color:GRID},ticks:{maxRotation:45}},y:{stacked,grid:{color:GRID},beginAtZero:true}}}
  });

  revTabChart = new Chart(document.getElementById('revTabChart').getContext('2d'), {
    type:'line',
    data:{ labels:d.labels, datasets:[{
      label:'Total Revenue',
      data: (d.rhRev||[]).map((v,i)=>(v||0)+((d.tdRev||[])[i]||0)+((d.mdlRev||[])[i]||0)),
      borderColor:'#f87171',
      backgroundColor:'rgba(248,113,113,.08)',
      fill:true, tension:.4, pointRadius:0, borderWidth:2.5,
    }]},
    options:{
      responsive:true, maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false}, tooltip:{...tooltipDefaults(),callbacks:{label:i=>' $'+i.raw.toLocaleString()}}},
      scales:{
        x:{grid:{color:GRID},ticks:{maxRotation:45}},
        y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(1)+'k':v)}}
      }
    }
  });
  volChart = mkBar('volChart',[
    {label:'Roman',data:d.rh, backgroundColor:'rgba(45,212,160,.82)',borderWidth:0,borderRadius:2,stack:'s'},
    {label:'Teladoc',     data:d.td, backgroundColor:'rgba(123,158,240,.82)',borderWidth:0,borderRadius:2,stack:'s'},
    {label:'MDLive',      data:d.mdl,backgroundColor:'rgba(245,166,35,.82)', borderWidth:0,borderRadius:2,stack:'s'},
  ]);
  const tRH=d.rh.reduce((a,b)=>a+b,0);
  const tTD1=(d.td1||[]).reduce((a,b)=>a+b,0), tTD2=(d.td2||[]).reduce((a,b)=>a+b,0);
  const tMDL1=(d.mdl1||[]).reduce((a,b)=>a+b,0), tMDL2=(d.mdl2||[]).reduce((a,b)=>a+b,0);
  const tTD=tTD1+tTD2, tMDL=tMDL1+tMDL2, tot=(tRH+tTD+tMDL)||1;

  pieChart = new Chart(document.getElementById('breakdownChart').getContext('2d'),{
    type:'bar',
    data:{
      labels:['Encounters'],
      datasets:[
        {label:'TD Phone',  data:[tTD1], backgroundColor:'rgba(123,158,240,.85)',borderColor:'#7b9ef0', borderWidth:1, borderRadius:3, stack:'s'},
        {label:'TD Video',  data:[tTD2], backgroundColor:'rgba(79,109,217,.85)', borderColor:'#4f6dd9', borderWidth:1, borderRadius:3, stack:'s'},
        {label:'MDL Phone', data:[tMDL1],backgroundColor:'rgba(245,166,35,.85)', borderColor:'#f5a623', borderWidth:1, borderRadius:3, stack:'s'},
        {label:'MDL Video', data:[tMDL2],backgroundColor:'rgba(224,123,0,.85)',  borderColor:'#e07b00', borderWidth:1, borderRadius:3, stack:'s'},
      ]
    },
    options:{
      indexAxis:'y',
      responsive:true, maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{...tooltipDefaults(), callbacks:{
          label: i => ' '+i.dataset.label+': '+i.raw.toLocaleString()+' ('+Math.round(i.raw/tot*100)+'%)'
        }}
      },
      scales:{
        x:{stacked:true,grid:{color:GRID},ticks:{callback:v=>v.toLocaleString()}},
        y:{stacked:true,grid:{color:'transparent'},ticks:{display:false}},
      }
    }
  });
  updateBreakdownStats(tRH,tTD,tTD1,tTD2,tMDL,tMDL1,tMDL2,tot);
  // % share line chart
  const cmpTotals = d.rh.map((_,i) => (d.rh[i]||0)+(d.td[i]||0)+(d.mdl[i]||0)||1);
  const rhPct  = d.rh.map((v,i)  => +((v/cmpTotals[i])*100).toFixed(1));
  const tdPct  = d.td.map((v,i)  => +((v/cmpTotals[i])*100).toFixed(1));
  const mdlPct = d.mdl.map((v,i) => +((v/cmpTotals[i])*100).toFixed(1));
  cmpChart = new Chart(document.getElementById('cmpChart').getContext('2d'), {
    type: 'line',
    data: { labels: d.labels, datasets: [
      { label:'Roman', data:rhPct,  borderColor:'#2dd4a0', backgroundColor:'rgba(45,212,160,.08)',  fill:true, tension:.35, pointRadius:2, borderWidth:2 },
      { label:'Teladoc',      data:tdPct,  borderColor:'#7b9ef0', backgroundColor:'rgba(123,158,240,.08)', fill:true, tension:.35, pointRadius:2, borderWidth:2 },
      { label:'MDLive',       data:mdlPct, borderColor:'#f5a623', backgroundColor:'rgba(245,166,35,.08)',  fill:true, tension:.35, pointRadius:2, borderWidth:2 },
    ]},
    options: {
      responsive:true, maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{...tooltipDefaults(), callbacks:{label: i => ' '+i.dataset.label+': '+i.raw+'%'}}
      },
      scales:{
        x:{grid:{color:GRID},ticks:{maxRotation:45}},
        y:{grid:{color:GRID},beginAtZero:true,max:100,ticks:{callback:v=>v+'%'}},
      }
    }
  });
  updateTabStats(d);
}

// ── Update all charts with new data ──
function refreshAllCharts() {
  applyFilters(); // applyFilters now handles daily vs tab split
}

// ── Update stat cards ──
function updateStats(daily) {
  // daily = current month data (for MTD cards)
  const n = daily.labels.length;
  const tRH=daily.rh.reduce((a,b)=>a+b,0), tTD=daily.td.reduce((a,b)=>a+b,0), tMDL=daily.mdl.reduce((a,b)=>a+b,0);
  const tEnc=tRH+tTD+tMDL, tRev=daily.rev.reduce((a,b)=>a+b,0);

  // MTD cards (always current month)
  document.getElementById('mtd-enc').textContent=tEnc.toLocaleString();
  document.getElementById('mtd-rev').textContent='$'+tRev.toLocaleString();
  document.getElementById('avg-rh').textContent=n?(tRH/n).toFixed(1):'—';
  const avgMdlEl=document.getElementById('avg-mdl'); if(avgMdlEl) avgMdlEl.textContent=n?(tMDL/n).toFixed(1):'—';
  const avgTdEl=document.getElementById('avg-td');   if(avgTdEl)  avgTdEl.textContent=n?(tTD/n).toFixed(1):'—';
  document.getElementById('days-rec').textContent=n+' days recorded';

  // ── Use FILTERED data for all three stat cards ──
  const filtered = getFilteredData();
  if (!filtered) return;

  // Count distinct months in current filter selection
  const filteredSheets = Object.entries(ALL_DATA).filter(([name, sd]) => {
    const yearOk = ACTIVE_YEAR==='all' || sd.year===+ACTIVE_YEAR || yearFromTabName(name)===+ACTIVE_YEAR;
    const monthOk = ACTIVE_MONTH==='all' || sd.month===+ACTIVE_MONTH;
    return yearOk && monthOk;
  });
  const numMonths = Math.max(filteredSheets.length, 1);

  // Total days across filtered months (for avg/day)
  const totalDays = filteredSheets.reduce((sum, [,sd]) => sum + sd.labels.length, 0) || 1;

  const fRH  = filtered.rh.reduce((a,b)=>a+b,0);
  const fTD  = filtered.td.reduce((a,b)=>a+b,0);
  const fMDL = filtered.mdl.reduce((a,b)=>a+b,0);
  const fEnc = fRH+fTD+fMDL;
  const fRev = filtered.rev.reduce((a,b)=>a+b,0);

  // Avg per month card
  // Per-provider revenue using correct sub-type rates
  const fRHRev  = fRH * RATES.rh;
  // For TD/MDL we need td1/td2 and mdl1/mdl2 from filtered data
  const fTD1  = filtered.td1  ? filtered.td1.reduce((a,b)=>a+b,0)  : 0;
  const fTD2  = filtered.td2  ? filtered.td2.reduce((a,b)=>a+b,0)  : 0;
  const fMDL1 = filtered.mdl1 ? filtered.mdl1.reduce((a,b)=>a+b,0) : 0;
  const fMDL2 = filtered.mdl2 ? filtered.mdl2.reduce((a,b)=>a+b,0) : 0;
  const fTDRev  = Math.round(fTD1*RATES.td1  + fTD2*RATES.td2);
  const fMDLRev = Math.round(fMDL1*RATES.mdl1 + fMDL2*RATES.mdl2);
  const fRevTotal = fRHRev + fTDRev + fMDLRev;

  document.getElementById('s-enc').textContent=Math.round(fEnc/numMonths).toLocaleString();
  document.getElementById('s-rev').textContent='$'+Math.round(fRevTotal/numMonths).toLocaleString();
  document.getElementById('s-rh-mo').textContent=Math.round(fRH/numMonths).toLocaleString();
  document.getElementById('s-mdl-mo').textContent=Math.round(fMDL/numMonths).toLocaleString();
  document.getElementById('s-td-mo').textContent=Math.round(fTD/numMonths).toLocaleString();

  // Avg per day — visits
  document.getElementById('s-rh-day').textContent=(fRH/totalDays).toFixed(1);
  document.getElementById('s-mdl-day').textContent=(fMDL/totalDays).toFixed(1);
  document.getElementById('s-td-day').textContent=(fTD/totalDays).toFixed(1);

  // Avg per day — revenue (new card)
  document.getElementById('s-rh-rev-day').textContent='$'+Math.round(fRHRev/totalDays).toLocaleString();
  document.getElementById('s-mdl-rev-day').textContent='$'+Math.round(fMDLRev/totalDays).toLocaleString();
  document.getElementById('s-td-rev-day').textContent='$'+Math.round(fTDRev/totalDays).toLocaleString();

  // Dynamic labels
  const moNames = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const yr  = ACTIVE_YEAR==='all'  ? 'All' : ACTIVE_YEAR;
  const mo  = ACTIVE_MONTH==='all' ? ''    : ' · '+moNames[+ACTIVE_MONTH-1];
  const ctx = yr+mo+' · '+numMonths+' mo';
  const periodEl = document.getElementById('s-period-label');
  if (periodEl) periodEl.textContent = numMonths===1 ? 'This Month' : 'Avg / Month ('+ctx+')';
  const visitsEl = document.getElementById('s-visits-label');
  if (visitsEl)  visitsEl.textContent  = numMonths===1 ? 'Visits This Month' : 'Avg Visits / Month';
}

// ── Day of Week chart ──
let dowChart = null;

function buildDowChart() {
  if (dowChart) { dowChart.destroy(); dowChart = null; }
  const gc = document.getElementById('dowChart');
  if (!gc) return;

  const DAYS = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
  const rhByDow  = new Array(7).fill(0);
  const tdByDow  = new Array(7).fill(0);
  const mdlByDow = new Array(7).fill(0);
  const countByDow = new Array(7).fill(0);

  // Get filtered sheets
  const filteredSheets = Object.entries(ALL_DATA).filter(([name, d]) => {
    const yearOk  = ACTIVE_YEAR==='all'  || d.year===+ACTIVE_YEAR  || yearFromTabName(name)===+ACTIVE_YEAR;
    const monthOk = ACTIVE_MONTH==='all' || d.month===+ACTIVE_MONTH;
    return yearOk && monthOk;
  });

  filteredSheets.forEach(([, d]) => {
    d.labels.forEach((label, i) => {
      // label format: "M/D" e.g. "3/26"
      const parts = label.split('/');
      if (parts.length < 2) return;
      const mo  = parseInt(parts[0]);
      const day = parseInt(parts[1]);
      const yr  = d.year || 2026;
      const date = new Date(yr, mo - 1, day);
      const dow  = date.getDay(); // 0=Sun, 6=Sat
      rhByDow[dow]  += d.rh[i]  || 0;
      tdByDow[dow]  += d.td[i]  || 0;
      mdlByDow[dow] += d.mdl[i] || 0;
      countByDow[dow]++;
    });
  });

  // Average per occurrence of that weekday
  const rhAvg  = rhByDow.map((v,i)  => countByDow[i] ? +(v/countByDow[i]).toFixed(1) : 0);
  const tdAvg  = tdByDow.map((v,i)  => countByDow[i] ? +(v/countByDow[i]).toFixed(1) : 0);
  const mdlAvg = mdlByDow.map((v,i) => countByDow[i] ? +(v/countByDow[i]).toFixed(1) : 0);

  dowChart = new Chart(gc.getContext('2d'), {
    type: 'bar',
    data: {
      labels: DAYS,
      datasets: [
        { label:'Roman',   data:rhAvg,  backgroundColor:'rgba(45,212,160,.85)', borderWidth:0, borderRadius:4, stack:'s' },
        { label:'Teladoc', data:tdAvg,  backgroundColor:'rgba(123,158,240,.85)',borderWidth:0, borderRadius:4, stack:'s' },
        { label:'MDLive',  data:mdlAvg, backgroundColor:'rgba(245,166,35,.85)', borderWidth:0, borderRadius:4, stack:'s' },
      ]
    },
    options: {
      responsive:true, maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{
          ...tooltipDefaults(),
          callbacks:{
            label: i => ' '+i.dataset.label+': '+i.raw,
            footer: items => 'Total avg: '+items.reduce((a,i)=>a+(+i.raw),0).toFixed(1)
          }
        }
      },
      scales:{
        x:{ stacked:true, grid:{color:GRID},
          ticks:{ font:{size:13, weight:'600'} }
        },
        y:{ stacked:true, grid:{color:GRID}, beginAtZero:true,
          title:{display:true, text:'Avg Daily Encounters', color:'#64748b', font:{size:10}}
        }
      }
    }
  });
}

function updateBreakdownStats(tRH,tTD,tTD1,tTD2,tMDL,tMDL1,tMDL2,tot) {
  const pct = v => Math.round(v/tot*100)+'% of total';
  const el = (id,val) => { const e=document.getElementById(id); if(e) e.textContent=val; };
  el('bd-rh-tot',  tRH.toLocaleString());
  el('bd-rh-pct',  pct(tRH));
  el('bd-td-tot',  tTD.toLocaleString());
  el('bd-td-pct',  pct(tTD));
  el('bd-td1-val', tTD1.toLocaleString());
  el('bd-td2-val', tTD2.toLocaleString());
  el('bd-mdl-tot', tMDL.toLocaleString());
  el('bd-mdl-pct', pct(tMDL));
  el('bd-mdl1-val',tMDL1.toLocaleString());
  el('bd-mdl2-val',tMDL2.toLocaleString());
}

// ── Revenue historical chart ──
let revHistChart = null;

// ── Get last 30 calendar days of data across all sheets ──
function getLast30DaysData() {
  const allDays = [];
  Object.entries(ALL_DATA).forEach(([, d]) => {
    // If year filter is active, only include that year's data
    if (ACTIVE_YEAR !== 'all' && d.year !== +ACTIVE_YEAR) return;
    d.labels.forEach((label, i) => {
      const parts = label.split('/');
      if (parts.length < 2) return;
      const date = new Date(d.year, +parts[0]-1, +parts[1]);
      allDays.push({
        date, label,
        rh:  d.rh[i]  || 0,
        td:  d.td[i]  || 0,
        mdl: d.mdl[i] || 0,
        rhRev:  (d.rhRev  || [])[i] || 0,
        tdRev:  (d.tdRev  || [])[i] || 0,
        mdlRev: (d.mdlRev || [])[i] || 0,
      });
    });
  });
  allDays.sort((a,b) => a.date - b.date);
  // Year filter trumps window — show all days in that year
  if (ACTIVE_YEAR !== 'all') return allDays;
  const window = +REV_WINDOW;
  return window === 0 ? allDays : allDays.slice(-window);
}

let rev30BarChart = null;

function buildRevTab30Day() {
  const days = getLast30DaysData();
  if (!days.length) return;

  const labels   = days.map(d => d.label);
  const rhRevs   = days.map(d => d.rhRev);
  const tdRevs   = days.map(d => d.tdRev);
  const mdlRevs  = days.map(d => d.mdlRev);
  const rhEnc    = days.map(d => d.rh);
  const tdEnc    = days.map(d => d.td);
  const mdlEnc   = days.map(d => d.mdl);
  const totRev   = days.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev, 0);

  // Update 30-day total summary card
  const rl = document.getElementById('rev-total-label');
  if (rl) rl.textContent = 'Total Revenue — Last 30 Days';
  const rv = document.getElementById('rev-grand-total');
  if (rv) rv.textContent = '$'+Math.round(totRev).toLocaleString();
  const rs = document.getElementById('rev-total-sub');
  if (rs) rs.textContent = '30 days · $'+Math.round(totRev/30).toLocaleString()+'/day avg';
  const rhTot = days.reduce((a,d)=>a+d.rhRev,0);
  const tdTot = days.reduce((a,d)=>a+d.tdRev,0);
  const mdlTot= days.reduce((a,d)=>a+d.mdlRev,0);
  const tot   = totRev||1;
  const s = (id,v) => { const e=document.getElementById(id); if(e) e.textContent=v; };
  s('rev-total-rh',  '$'+Math.round(rhTot).toLocaleString());
  s('rev-pct-rh',    Math.round(rhTot/tot*100)+'% of total');
  s('rev-total-td',  '$'+Math.round(tdTot).toLocaleString());
  s('rev-pct-td',    Math.round(tdTot/tot*100)+'% of total');
  s('rev-total-mdl', '$'+Math.round(mdlTot).toLocaleString());
  s('rev-pct-mdl',   Math.round(mdlTot/tot*100)+'% of total');

  // ── Top: 3 separate revenue lines, no dots ──
  if (revTabChart) { revTabChart.destroy(); revTabChart = null; }
  // Update chart title
  const rtitle = document.getElementById('rev-30day-title');
  if (rtitle) {
    const tMap = {0:'All Time',30:'Last 30 Days',60:'Last 60 Days',90:'Last 90 Days',180:'Last 6 Months',365:'Last Year'};
    const winLabel2 = ACTIVE_YEAR !== 'all' ? String(ACTIVE_YEAR) : (tMap[REV_WINDOW] || REV_WINDOW+' Days');
    rtitle.textContent = 'Daily Revenue — '+winLabel2;
  }
  const rc = document.getElementById('revTabChart');
  if (rc) {
    revTabChart = new Chart(rc.getContext('2d'), {
      type:'bar',
      data:{ labels, datasets:[
        { label:'Roman',   data:rhRevs,  backgroundColor:'rgba(45,212,160,.82)', borderWidth:0, borderRadius:2, stack:'s' },
        { label:'Teladoc', data:tdRevs,  backgroundColor:'rgba(123,158,240,.82)',borderWidth:0, borderRadius:2, stack:'s' },
        { label:'MDLive',  data:mdlRevs, backgroundColor:'rgba(245,166,35,.82)', borderWidth:0, borderRadius:2, stack:'s' },
      ]},
      options:{
        responsive:true, maintainAspectRatio:false,
        interaction:{mode:'index',intersect:false},
        plugins:{
          legend:{display:false},
          tooltip:{...tooltipDefaults(), callbacks:{
            label: i => ' '+i.dataset.label+': $'+i.raw.toLocaleString(),
            footer: items => 'Total: $'+items.reduce((a,i)=>a+i.raw,0).toLocaleString()
          }}
        },
        scales:{
          x:{ stacked:true, grid:{color:GRID}, ticks:{maxRotation:45} },
          y:{ stacked:true, grid:{color:GRID}, beginAtZero:true,
              ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(0)+'k':v)} }
        }
      }
    });
  }

  // ── Bottom: stacked bar (encounters by day) ──
  // Update bar chart title to match window
  const barTitle = document.getElementById('rev-bar-title');
  if (barTitle) {
    const tMap2 = {0:'All Time',30:'Last 30 Days',60:'Last 60 Days',90:'Last 90 Days',180:'Last 6 Months',365:'Last Year'};
    const barWinLabel = ACTIVE_YEAR !== 'all' ? String(ACTIVE_YEAR) : (tMap2[REV_WINDOW]||REV_WINDOW+' Days');
    barTitle.textContent = 'Daily Revenue — '+barWinLabel+' (Stacked)';
  }

  if (rev30BarChart) { rev30BarChart.destroy(); rev30BarChart = null; }
  const bc = document.getElementById('rev30BarChart');
  if (bc) {
    rev30BarChart = new Chart(bc.getContext('2d'), {
      type:'bar',
      data:{ labels, datasets:[
        { label:'Roman',   data:rhRevs,  backgroundColor:'rgba(45,212,160,.82)', borderWidth:0, borderRadius:2, stack:'s' },
        { label:'Teladoc', data:tdRevs,  backgroundColor:'rgba(123,158,240,.82)',borderWidth:0, borderRadius:2, stack:'s' },
        { label:'MDLive',  data:mdlRevs, backgroundColor:'rgba(245,166,35,.82)', borderWidth:0, borderRadius:2, stack:'s' },
      ]},
      options:{
        responsive:true, maintainAspectRatio:false,
        interaction:{mode:'index',intersect:false},
        plugins:{ legend:{display:false}, tooltip:{...tooltipDefaults(), callbacks:{
          label: i => ' '+i.dataset.label+': $'+i.raw.toLocaleString(),
          footer: items => 'Total: $'+items.reduce((a,i)=>a+i.raw,0).toLocaleString()
        }}},
        scales:{
          x:{stacked:true, grid:{color:GRID}, ticks:{maxRotation:45}},
          y:{stacked:true, grid:{color:GRID}, beginAtZero:true,
             ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(1)+'k':v)}}
        }
      }
    });
  }
}

function buildRevHistChart() {
  if (revHistChart) { revHistChart.destroy(); revHistChart = null; }
  const gc = document.getElementById('revHistChart');
  if (!gc) return;

  const moNames = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];

  // Get filtered sheets sorted chronologically
  const filteredSheets = Object.entries(ALL_DATA)
    .filter(([name, d]) => {
      const yearOk  = ACTIVE_YEAR==='all'  || d.year===+ACTIVE_YEAR  || yearFromTabName(name)===+ACTIVE_YEAR;
      const monthOk = ACTIVE_MONTH==='all' || d.month===+ACTIVE_MONTH;
      return yearOk && monthOk;
    })
    .sort(([,a],[,b]) => a.year!==b.year ? a.year-b.year : a.month-b.month);

  if (!filteredSheets.length) return;

  // Build per-month revenue totals
  const labels  = filteredSheets.map(([,d]) => moNames[d.month-1]+' '+String(d.year).slice(2));
  const rhRevs  = [], tdRevs  = [], mdlRevs = [];

  filteredSheets.forEach(([,d]) => {
    const rhR   = Math.round(d.rh.reduce((a,b)=>a+b,0) * RATES.rh);
    const td1t  = (d.td1||[]).reduce((a,b)=>a+b,0);
    const td2t  = (d.td2||[]).reduce((a,b)=>a+b,0);
    const mdl1t = (d.mdl1||[]).reduce((a,b)=>a+b,0);
    const mdl2t = (d.mdl2||[]).reduce((a,b)=>a+b,0);
    rhRevs.push(rhR);
    tdRevs.push(Math.round(td1t*RATES.td1 + td2t*RATES.td2));
    mdlRevs.push(Math.round(mdl1t*RATES.mdl1 + mdl2t*RATES.mdl2));
  });

  // Update title
  const title = document.getElementById('rev-hist-title');
  const yr  = ACTIVE_YEAR==='all'  ? 'All Time' : ACTIVE_YEAR;
  const mo  = ACTIVE_MONTH==='all' ? '' : ' · '+moNames[+ACTIVE_MONTH-1];
  if (title) title.textContent = 'Monthly Revenue — '+yr+mo+' ('+filteredSheets.length+' months)';

  // Legend
  const legend = document.getElementById('rev-hist-legend');
  if (legend) legend.innerHTML = [
    '<div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman</div>',
    '<div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>',
    '<div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>',
  ].join('');

  revHistChart = new Chart(gc.getContext('2d'), {
    type:'bar',
    data:{ labels, datasets:[
      {label:'Roman',   data:rhRevs,  backgroundColor:'rgba(45,212,160,.85)', borderWidth:0, borderRadius:3, stack:'s'},
      {label:'Teladoc', data:tdRevs,  backgroundColor:'rgba(123,158,240,.85)',borderWidth:0, borderRadius:3, stack:'s'},
      {label:'MDLive',  data:mdlRevs, backgroundColor:'rgba(245,166,35,.85)', borderWidth:0, borderRadius:3, stack:'s'},
    ]},
    options:{
      responsive:true, maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{...tooltipDefaults(), callbacks:{
          label: i => ' '+i.dataset.label+': $'+i.raw.toLocaleString(),
          footer: items => 'Total: $'+items.reduce((a,i)=>a+i.raw,0).toLocaleString()
        }}
      },
      scales:{
        x:{ stacked:true, grid:{color:GRID}, ticks:{maxRotation:45} },
        y:{ stacked:true, grid:{color:GRID}, beginAtZero:true,
            ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(0)+'k':v)} }
      }
    }
  });
}

function updateRevSummary() {
  // Always show last 30 days in the summary card
  const days = getLast30DaysData();
  if (!days.length) return;

  const totalRHRev  = days.reduce((a,d)=>a+d.rhRev,  0);
  const totalTDRev  = days.reduce((a,d)=>a+d.tdRev,  0);
  const totalMDLRev = days.reduce((a,d)=>a+d.mdlRev, 0);
  const grandTotal  = totalRHRev + totalTDRev + totalMDLRev;
  const tot = grandTotal || 1;
  const fmt = v => '$'+Math.round(v).toLocaleString();
  const pct = v => Math.round(v/tot*100)+'%';
  const set = (id,val) => { const e=document.getElementById(id); if(e) e.textContent=val; };

  const winLabel = {0:'All Time',30:'Last 30 Days',60:'Last 60 Days',90:'Last 90 Days',180:'Last 6 Months',365:'Last Year'};
  const summaryLabel = ACTIVE_YEAR !== 'all' ? String(ACTIVE_YEAR) : (winLabel[REV_WINDOW]||REV_WINDOW+' Days');
  set('rev-total-label',  'Total Revenue — '+summaryLabel);
  set('rev-grand-total',  fmt(grandTotal));
  const days30 = getLast30DaysData().length || 1;
  set('rev-total-sub', REV_WINDOW===0
    ? days30+' days · '+fmt(grandTotal/days30)+'/day avg'
    : REV_WINDOW+' days · '+fmt(grandTotal/REV_WINDOW)+'/day avg');
  set('rev-total-rh',     fmt(totalRHRev));
  set('rev-pct-rh',       pct(totalRHRev)+' of total');
  set('rev-total-td',     fmt(totalTDRev));
  set('rev-pct-td',       pct(totalTDRev)+' of total');
  set('rev-total-mdl',    fmt(totalMDLRev));
  set('rev-pct-mdl',      pct(totalMDLRev)+' of total');
}

function updateTabStats(d) {
  const tRH=d.rh.reduce((a,b)=>a+b,0), tTD=d.td.reduce((a,b)=>a+b,0), tMDL=d.mdl.reduce((a,b)=>a+b,0), tot=(tRH+tTD+tMDL)||1;
  const tRHR=(d.rhRev||[]).reduce((a,b)=>a+b,0), tTDR=(d.tdRev||[]).reduce((a,b)=>a+b,0), tMLR=(d.mdlRev||[]).reduce((a,b)=>a+b,0);
  document.getElementById('rev-rh-mtd').textContent='$'+tRHR.toLocaleString();
  document.getElementById('rev-td-mtd').textContent='$'+tTDR.toLocaleString();
  document.getElementById('rev-mdl-mtd').textContent='$'+tMLR.toLocaleString();
  document.getElementById('vol-rh').textContent=tRH.toLocaleString();
  document.getElementById('vol-td').textContent=tTD.toLocaleString();
  document.getElementById('vol-mdl').textContent=tMDL.toLocaleString();
  // Breakdown stats (phone/video split)
  const tTD1b=(d.td1||[]).reduce((a,b)=>a+b,0), tTD2b=(d.td2||[]).reduce((a,b)=>a+b,0);
  const tMDL1b=(d.mdl1||[]).reduce((a,b)=>a+b,0), tMDL2b=(d.mdl2||[]).reduce((a,b)=>a+b,0);
  updateBreakdownStats(tRH,tTD,tTD1b,tTD2b,tMDL,tMDL1b,tMDL2b,tot);
  const cmpTot3 = (tRH+tTD+tMDL)||1;
  document.getElementById('cmp-rh').textContent=Math.round(tRH/cmpTot3*100)+'%';
  document.getElementById('cmp-tdmdl').textContent=Math.round(tTD/cmpTot3*100)+'%';
  document.getElementById('cmp-mdl') && (document.getElementById('cmp-mdl').textContent=Math.round(tMDL/cmpTot3*100)+'%');
}

// ── Always get the most recent month's data (for Daily tab) ──
function getCurrentMonthData() {
  const sheets = Object.values(ALL_DATA);
  if (!sheets.length) return FB_MAR26;
  // Sort all sheets by year then month, pick the last one
  sheets.sort((a,b) => a.year!==b.year ? a.year-b.year : a.month-b.month);
  const latest = sheets[sheets.length-1];
  // Apply type filter only (not year filter) for daily tab
  if (ACTIVE_TYPE === 'all') return latest;
  const d = {...latest};
  if (ACTIVE_TYPE !== 'rh')  { d.rh=d.rh.map(()=>0);   d.rhRev=d.rhRev.map(()=>0); }
  if (ACTIVE_TYPE !== 'td')  { d.td=d.td.map(()=>0);   d.tdRev=d.tdRev.map(()=>0); }
  if (ACTIVE_TYPE !== 'mdl') { d.mdl=d.mdl.map(()=>0); d.mdlRev=d.mdlRev.map(()=>0); }
  return d;
}

// ── Apply current filters and redraw ──
function applyFilters() {
  // Daily tab always shows current month only
  const daily = getCurrentMonthData();
  encChart.data.labels=daily.labels;
  encChart.data.datasets[0].data=daily.rh;
  encChart.data.datasets[1].data=daily.td;
  encChart.data.datasets[2].data=daily.mdl;
  encChart.update('active');
  revChart.data.labels=daily.labels;
  revChart.data.datasets[0].data=daily.rhRev||[];
  revChart.data.datasets[1].data=daily.tdRev||[];
  revChart.data.datasets[2].data=daily.mdlRev||[];
  revChart.update('active');
  updateStats(daily);

  // Dynamic titles based on active filters
  const moNames2 = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const filterLabel = ACTIVE_MONTH !== 'all'
    ? moNames2[+ACTIVE_MONTH-1] + (ACTIVE_YEAR !== 'all' ? ' ' + ACTIVE_YEAR : ' — All Years')
    : (ACTIVE_YEAR !== 'all' ? 'All of ' + ACTIVE_YEAR : 'All Time');

  const volTitle = document.getElementById('vol-title');
  if (volTitle) volTitle.textContent = 'Encounter Volume — ' + filterLabel;
  // rev-title stays as "Revenue by Platform" — window label shown in chart heading
  const bdTitle = document.getElementById('breakdown-title');
  if (bdTitle) bdTitle.textContent = 'Visit Breakdown — ' + filterLabel;
  const bdSub = document.getElementById('breakdown-sub');
  if (bdSub) bdSub.textContent = 'Encounters by provider and visit type — ' + filterLabel;
  // Rebuild day-of-week chart when filters change


  // Revenue tab — only rebuild if panel is visible; it manages its own window
  updateRevSummary();
  if (document.getElementById('panel-revenue').classList.contains('active')) {
    buildRevTab30Day();
  }
  if (revHistChart || document.getElementById('panel-revenue').classList.contains('active')) buildRevHistChart();

  // Monthly targets — always update when filters change
  updateGoalsTab(getFilteredData());

  // Other tabs use full filtered data
  const d = getFilteredData();
  if (!d) return;
  // Always rebuild revTabChart with filtered data (not gated by _tabChartsBuilt)
  // revTabChart is rebuilt by buildRevTab30Day — no manual update needed here

  // Always update breakdown stats & charts regardless of tab state
  const tRH=d.rh.reduce((a,b)=>a+b,0),tTD=d.td.reduce((a,b)=>a+b,0),tMDL=d.mdl.reduce((a,b)=>a+b,0),tot=(tRH+tTD+tMDL)||1;
  const tTD1r=(d.td1||[]).reduce((a,b)=>a+b,0), tTD2r=(d.td2||[]).reduce((a,b)=>a+b,0);
  const tMDL1r=(d.mdl1||[]).reduce((a,b)=>a+b,0), tMDL2r=(d.mdl2||[]).reduce((a,b)=>a+b,0);
  updateBreakdownStats(tRH,tTD,tTD1r,tTD2r,tMDL,tMDL1r,tMDL2r,tot);
  buildDowChart();

  // Always rebuild the horizontal phone/video bar with filtered data
  if (pieChart) { pieChart.destroy(); pieChart = null; }
  const bdCtx = document.getElementById('breakdownChart');
  if (bdCtx) {
    pieChart = new Chart(bdCtx.getContext('2d'), {
      type:'bar',
      data:{ labels:['Encounters'], datasets:[
        {label:'TD Phone', data:[tTD1r], backgroundColor:'rgba(123,158,240,.85)',borderColor:'#7b9ef0',borderWidth:1,borderRadius:3,stack:'s'},
        {label:'TD Video', data:[tTD2r], backgroundColor:'rgba(79,109,217,.85)', borderColor:'#4f6dd9',borderWidth:1,borderRadius:3,stack:'s'},
        {label:'MDL Phone',data:[tMDL1r],backgroundColor:'rgba(245,166,35,.85)', borderColor:'#f5a623',borderWidth:1,borderRadius:3,stack:'s'},
        {label:'MDL Video',data:[tMDL2r],backgroundColor:'rgba(224,123,0,.85)',  borderColor:'#e07b00',borderWidth:1,borderRadius:3,stack:'s'},
      ]},
      options:{
        indexAxis:'y', responsive:true, maintainAspectRatio:false,
        interaction:{mode:'index',intersect:false},
        plugins:{legend:{display:false}, tooltip:{...tooltipDefaults(), callbacks:{
          label: i => ' '+i.dataset.label+': '+i.raw.toLocaleString()+' ('+Math.round(i.raw/tot*100)+'%)'
        }}},
        scales:{
          x:{stacked:true, grid:{color:GRID}, ticks:{callback:v=>v.toLocaleString()}},
          y:{stacked:true, grid:{color:'transparent'}, ticks:{display:false}},
        }
      }
    });
  }

  if (window._tabChartsBuilt) {
    volChart.data.labels=d.labels;
    volChart.data.datasets[0].data=d.rh; volChart.data.datasets[1].data=d.td; volChart.data.datasets[2].data=d.mdl;
    volChart.update('active');
    const cmpTot2 = d.rh.map((_,i)=>(d.rh[i]||0)+(d.td[i]||0)+(d.mdl[i]||0)||1);
    cmpChart.data.labels=d.labels;
    cmpChart.data.datasets[0].data=d.rh.map((v,i)=>+((v/cmpTot2[i])*100).toFixed(1));
    cmpChart.data.datasets[1].data=d.td.map((v,i)=>+((v/cmpTot2[i])*100).toFixed(1));
    cmpChart.data.datasets[2].data=d.mdl.map((v,i)=>+((v/cmpTot2[i])*100).toFixed(1));
    cmpChart.update('active');
    updateTabStats(d);
  }
}

// ── Init with Mar 26 fallback ──
const FB_MAR26 = {
  labels:['3/1','3/2','3/3','3/4','3/5','3/6','3/7','3/8','3/9','3/10','3/11','3/12','3/13','3/14','3/15','3/16','3/17','3/18','3/19','3/20','3/21','3/22','3/23','3/24','3/25'],
  rh:    [84,63,84,66,65,78,84,84,82,84,81,115,98,115,84,85,85,70,115,90,70,141,51,67,22],
  td:    [0,6,0,7,9,3,0,0,0,0,5,0,4,0,0,0,0,9,0,2,7,0,2,0,10],
  mdl:   [0,4,0,2,0,0,0,0,1,0,0,0,4,0,0,0,0,1,0,1,0,0,0,0,0],
  rev:   [1008,1007,1008,1003,1002,1005,1008,1008,1009,1008,1107,1380,1381,1380,1008,1020,1020,1072,1380,1151,1016,1692,658,804,509],
  rhRev: [84,63,84,66,65,78,84,84,82,84,81,115,98,115,84,85,85,70,115,90,70,141,51,67,22].map(v=>Math.round(v*12)),
  tdRev: [0,6,0,7,9,3,0,0,0,0,5,0,4,0,0,0,0,9,0,2,7,0,2,0,10].map(v=>Math.round(v*24.33)),
  mdlRev:[0,4,0,2,0,0,0,0,1,0,0,0,4,0,0,0,0,1,0,1,0,0,0,0,0].map(v=>Math.round(v*25.46)),
  month:3, year:2026,
};
ALL_DATA['Mar 26'] = FB_MAR26;
window._lastData   = FB_MAR26;
window._tabChartsBuilt = false;

// Start with empty charts — no flash of stale data
const EMPTY = {
  labels:[], rh:[], td:[], mdl:[], rev:[],
  rhRev:[], tdRev:[], mdlRev:[], month:3, year:2026
};
buildCharts(EMPTY);
// Show dashes in all stat cards until live data arrives
['mtd-enc','mtd-rev','avg-rh','avg-mdl','avg-td','s-enc','s-rev',
 's-rh-mo','s-mdl-mo','s-td-mo','s-rh-day','s-mdl-day','s-td-day',
 's-rh-rev-day','s-mdl-rev-day','s-td-rev-day']
  .forEach(id => {
    const el=document.getElementById(id);
    if(el){ el.textContent='·'; el.classList.add('loading-pulse'); }
  });
document.getElementById('days-rec').textContent='Loading…';
document.getElementById('live-badge').style.display='flex';

// ── Fetch all sheets from Apps Script ──
const CACHE_KEY = 'tally_dashboard_cache';
const CACHE_TS_KEY = 'tally_dashboard_cache_ts';

function saveCache(text) {
  try {
    localStorage.setItem(CACHE_KEY, text);
    localStorage.setItem(CACHE_TS_KEY, Date.now().toString());
  } catch(e) { /* storage full or private mode */ }
}

function loadCache() {
  try {
    const ts = localStorage.getItem(CACHE_TS_KEY);
    const text = localStorage.getItem(CACHE_KEY);
    if (!text || !ts) return null;
    return { text, age: Math.round((Date.now() - +ts) / 60000) }; // age in minutes
  } catch(e) { return null; }
}

async function fetchCSV() {
  const endpoints = APPS_SCRIPT_URL
    ? [APPS_SCRIPT_URL+'?_='+Date.now()]
    : ['https://corsproxy.io/?url='+encodeURIComponent(SHEET_CSV_URL)];
  for (const url of endpoints) {
    try {
      const controller = new AbortController();
      const timer = setTimeout(() => controller.abort(), 15000);
      const r = await fetch(url, { cache:'no-store', redirect:'follow', signal: controller.signal });
      clearTimeout(timer);
      if (!r.ok) throw new Error('HTTP '+r.status);
      const t = await r.text();
      if (!t||t.trim().length<10) throw new Error('empty');
      return t;
    } catch(e) {
      const msg = e.name === 'AbortError' ? 'timeout (15s)' : e.message;
      console.warn('Fetch failed:', msg);
    }
  }
  throw new Error('Could not reach server');
}

async function loadSheet(background=false) {
  // ── Show cache instantly if available ──
  if (!background) {
    const cached = loadCache();
    if (cached) {
      try {
        const parsed = JSON.parse(cached.text);
        const allParsed = {};
        for (const [name, csv] of Object.entries(parsed)) {
          const d = parseSheetCSV(csv, name);
          if (d) { if (!d.year) d.year = yearFromTabName(name)||2026; allParsed[name] = d; }
        }
        if (Object.keys(allParsed).length) {
          ALL_DATA = allParsed;
          REV_WINDOW = 30; // always default to 30 days on load
          const rwg2 = document.getElementById('rev-window-group');
          if (rwg2) rwg2.querySelectorAll('.pill').forEach((p,i) => p.classList.toggle('active', i===0));
          applyFilters();
          updateStats(getCurrentMonthData()||FB_MAR26);
          document.getElementById('feed-dot').classList.remove('live');
          document.getElementById('feed-label').textContent = 'Cached · '+cached.age+'m ago · refreshing…';
          document.querySelectorAll('.loading-pulse').forEach(el=>el.classList.remove('loading-pulse'));
        }
      } catch(e) { console.warn('Cache parse failed:', e); }
    }
  }

  try {
    const t = await fetchCSV();
    let parsed = {};

    // New format: JSON object of {sheetName: csvText}
    if (t.trim().startsWith('{')) {
      const json = JSON.parse(t);
      for (const [name, csv] of Object.entries(json)) {
        const d = parseSheetCSV(csv, name);
        if (d) {
          // Ensure year is set (tab name takes priority, already handled in parseSheetCSV)
          if (!d.year) d.year = yearFromTabName(name)||2026;
          parsed[name] = d;
        }
      }
    } else {
      // Fallback: single sheet CSV
      const d = parseSheetCSV(t);
      if (d) parsed['Mar 26'] = d;
    }

    if (!Object.keys(parsed).length) throw new Error('No sheets parsed');
    saveCache(t);  // Cache raw response for next load
    ALL_DATA = parsed;
    window._lastData = getCurrentMonthData()||FB_MAR26;
    REV_WINDOW = 0; // All Time default
    // Reset revenue window pill to Last 30d
    const rwg = document.getElementById('rev-window-group');
    if (rwg) {
      rwg.querySelectorAll('.pill').forEach((p,i) => p.classList.toggle('active', i===0));
    }
    buildRevTab30Day();
    updateRevSummary();
    buildRevHistChart();
    buildDowChart();
    const todayPanel = document.getElementById('panel-today');
    if (todayPanel && todayPanel.classList.contains('active')) {
      updateTodayTab();
      updateGoalsTab(getFilteredData());
    }
    // Refresh goals tab if open
    const goalsPanel = document.getElementById('panel-compare');
    updateGoalsTab(getFilteredData()); // always refresh monthly targets
  updateTodayTab(); // keep today in sync

    // Daily badge shows current month day count
    const curMonth = getCurrentMonthData();
    const mo = curMonth.month, yr = curMonth.year;
    const moName = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'][mo-1];
    document.getElementById('days-rec').textContent = curMonth.labels.length+' days recorded';
    // Update panel header label
    const ph = document.querySelector('.ph2');
    if (ph) ph.childNodes[0].textContent = 'Daily Breakdown — '+moName+' \''+String(yr).slice(2)+' ';

    applyFilters();

    // Remove loading skeletons
    document.querySelectorAll('.loading-pulse').forEach(el => el.classList.remove('loading-pulse'));
    document.getElementById('feed-dot').classList.add('live');
    document.getElementById('feed-label').textContent='Live · last: '+new Date().toLocaleTimeString();
    // Update subtitle with actual date range from data
    const allSheetsSorted = Object.values(ALL_DATA).sort((a,b)=>a.year!==b.year?a.year-b.year:a.month-b.month);
    if (allSheetsSorted.length) {
      const mo = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
      const first = allSheetsSorted[0];
      const rangeEl = document.getElementById('date-range');
      if (rangeEl) rangeEl.textContent = mo[first.month-1]+' '+first.year+' – Present';
    }
    document.getElementById('psub').textContent='Pulled from '+Object.keys(parsed).length+' sheet tabs · refreshes every minute';
    document.getElementById('last-updated').textContent=new Date().toLocaleTimeString();
    const notice=document.getElementById('setup-notice');
    if(notice) notice.style.display='none';

  } catch(e) {
    document.getElementById('feed-dot').classList.remove('live');
    document.getElementById('feed-label').textContent='Feed unavailable · cached data';
    document.getElementById('psub').textContent='⚠️ '+e.message;
    document.getElementById('last-updated').textContent='Failed · '+new Date().toLocaleTimeString();
    console.warn('loadSheet failed:',e);
  }
}
loadSheet();
setInterval(() => loadSheet(true), 60*1000); // refresh every minute

// ── Tab switching ──
function switchTab(id,el){
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('panel-'+id).classList.add('active');
  el.classList.add('active');
  setTimeout(()=>{

    if(id==='daily'){ encChart&&encChart.resize(); revChart&&revChart.resize(); return; }
    const d = getFilteredData()||FB_MAR26;
    if (!window._tabChartsBuilt) {
      buildTabCharts(d);
      window._tabChartsBuilt = true;
    } else {
      if(id==='revenue'){
        buildRevTab30Day();
        updateRevSummary();
        buildRevHistChart();
        setTimeout(()=>{ revTabChart&&revTabChart.resize(); rev30BarChart&&rev30BarChart.resize(); revHistChart&&revHistChart.resize(); }, 50);
      }
      if(id==='volume'){
        if (!window._tabChartsBuilt) {
          buildTabCharts(getFilteredData()||FB_MAR26);
          window._tabChartsBuilt = true;
        }
        buildVolWindowChart();
        setTimeout(()=>volChart&&volChart.resize(), 50);
      }
      if(id==='breakdown'){ pieChart&&pieChart.resize(); buildDowChart(); }
      if(id==='today'){ updateTodayTab(); updateGoalsTab(getFilteredData()); }
    }
  },80);
}

// ── YoY chart instance ──
let yoyChart = null;

function renderYoYView() {
  const stackView = document.getElementById('cmp-stack-view');
  const yoyView   = document.getElementById('cmp-yoy-view');
  const title     = document.getElementById('cmp-title');
  const sub       = document.getElementById('cmp-sub');
  const mo        = +ACTIVE_MONTH;
  const moName    = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'][mo-1];

  // Get all years that have data for this month
  const sheets = Object.entries(ALL_DATA)
    .filter(([, d]) => d.month === mo)
    .sort(([,a],[,b]) => a.year - b.year);

  if (!sheets.length) return;

  const yearLabels = sheets.map(([, d]) => String(d.year));

  // Apply type filter
  const showRH  = ACTIVE_TYPE === 'all' || ACTIVE_TYPE === 'rh';
  const showTD  = ACTIVE_TYPE === 'all' || ACTIVE_TYPE === 'td';
  const showMDL = ACTIVE_TYPE === 'all' || ACTIVE_TYPE === 'mdl';

  const rhTotals  = sheets.map(([,d]) => showRH  ? d.rh.reduce((a,b)=>a+b,0)  : 0);
  const tdTotals  = sheets.map(([,d]) => showTD  ? d.td.reduce((a,b)=>a+b,0)  : 0);
  const mdlTotals = sheets.map(([,d]) => showMDL ? d.mdl.reduce((a,b)=>a+b,0) : 0);

  // Destroy and rebuild
  if (yoyChart) { yoyChart.destroy(); yoyChart = null; }

  const ctx = document.getElementById('yoyChart');
  if (!ctx) return;

  yoyChart = new Chart(ctx.getContext('2d'), {
    type: 'bar',
    data: {
      labels: yearLabels,
      datasets: [
        { label:'Roman', data:rhTotals,  backgroundColor:'rgba(45,212,160,.85)', borderColor:'#2dd4a0', borderWidth:1, borderRadius:4 },
        { label:'Teladoc',      data:tdTotals,  backgroundColor:'rgba(123,158,240,.85)',borderColor:'#7b9ef0', borderWidth:1, borderRadius:4 },
        { label:'MDLive',       data:mdlTotals, backgroundColor:'rgba(245,166,35,.85)', borderColor:'#f5a623', borderWidth:1, borderRadius:4 },
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      interaction: { mode:'index', intersect:false },
      plugins: {
        legend: { display:false },
        tooltip: {
          ...tooltipDefaults(),
          callbacks: {
            footer: items => 'Total: ' + items.reduce((a,i)=>a+i.raw,0).toLocaleString()
          }
        }
      },
      scales: {
        x: { grid:{color:GRID}, ticks:{font:{size:13,weight:'600'}} },
        y: { grid:{color:GRID}, beginAtZero:true,
             title:{display:true,text:'Total Encounters',color:'#64748b',font:{size:10}} }
      }
    }
  });

  // Stat cards per year
  const row = document.getElementById('yoy-stat-row');
  if (row) {
    const YR_BORDER = { 2024:'#f87171', 2025:'#a78bfa', 2026:'#34d399' };
    row.innerHTML = sheets.map(([, d]) => {
      const tRH  = d.rh.reduce((a,b)=>a+b,0);
      const tTD  = d.td.reduce((a,b)=>a+b,0);
      const tMDL = d.mdl.reduce((a,b)=>a+b,0);
      const tot  = tRH+tTD+tMDL;
      const col  = YR_BORDER[d.year]||'#94a3b8';
      return '<div class="mtd-card" style="border-top:2px solid '+col+'">'
        +'<div class="mclabel">'+moName+' '+d.year+'</div>'
        +'<div style="font-size:1.4rem;font-weight:700;color:'+col+';line-height:1.2;">'+tot.toLocaleString()+'</div>'
        +'<div style="font-size:.68rem;color:var(--text3);margin-top:6px;">'
        +'RH <span style="color:var(--rh)">'+tRH+'</span>'
        +' &nbsp;·&nbsp; TD <span style="color:var(--td-c)">'+tTD+'</span>'
        +' &nbsp;·&nbsp; MDL <span style="color:var(--mdl)">'+tMDL+'</span>'
        +'</div></div>';
    }).join('');
  }

  // Update headings
  const chartTitle = document.getElementById('yoy-chart-title');
  if (chartTitle) chartTitle.textContent = moName+' Encounters — Year over Year';
  title.textContent = moName+' — Year over Year';
  sub.textContent   = 'Total monthly encounters per year · RH, Teladoc, MDLive side by side';
  stackView.style.display = 'none';
  yoyView.style.display   = 'block';
}

function renderStackView() {
  const stackView = document.getElementById('cmp-stack-view');
  const yoyView   = document.getElementById('cmp-yoy-view');
  const title     = document.getElementById('cmp-title');
  const sub       = document.getElementById('cmp-sub');
  const moNames3 = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const fl2 = ACTIVE_MONTH !== 'all'
    ? moNames3[+ACTIVE_MONTH-1] + (ACTIVE_YEAR !== 'all' ? ' '+ACTIVE_YEAR : ' — All Years')
    : (ACTIVE_YEAR !== 'all' ? 'All of '+ACTIVE_YEAR : 'All Time');
  title.textContent = 'Provider Mix — ' + fl2;
  sub.textContent   = 'What % of total daily encounters does each provider represent?';
  stackView.style.display = 'block';
  yoyView.style.display   = 'none';
  setTimeout(() => { cmpChart && cmpChart.resize(); }, 50);
}

function updateCompareTab() {
  if (ACTIVE_MONTH !== 'all') {
    renderYoYView();
  } else {
    renderStackView();
  }
}

// ── Today's Performance ──
function updateTodayTab() {
  try {
    const curData = getCurrentMonthData();
    if (!curData || !curData.labels.length) return;

    const lastIdx = curData.labels.length - 1;
    const todayLabel = curData.labels[lastIdx];
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
    const sub = document.getElementById('today-sub');
    if (sub) sub.textContent = 'Last data point from your Google Sheet · auto-refreshes every minute';

    function setHero(id, actual, target) {
      const pct  = Math.round(actual / target * 100);
      const hit  = actual >= target;
      const near = pct >= 80;
      const barColor  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
      const badgeText = hit ? '✓ On target' : near ? 'Almost there' : 'Below target';
      const badgeBg   = hit ? 'rgba(34,197,94,.15)' : near ? 'rgba(245,166,35,.15)' : 'rgba(248,113,113,.15)';
      const badgeCol  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
      const needed    = hit ? (actual-target)+' above target 🎉' : (target-actual)+' more to hit target';
      const v = document.getElementById('today-'+id+'-val');     if(v) v.textContent=actual;
      const bar=document.getElementById('today-'+id+'-bar');     if(bar){bar.style.width=Math.min(pct,100)+'%';bar.style.background=barColor;}
      const pctEl=document.getElementById('today-'+id+'-pct');   if(pctEl){pctEl.textContent=pct+'%';pctEl.style.color=barColor;}
      const badge=document.getElementById('today-'+id+'-badge'); if(badge){badge.textContent=badgeText;badge.style.background=badgeBg;badge.style.color=badgeCol;}
      const need=document.getElementById('today-'+id+'-needed'); if(need) need.textContent=needed;
    }

    setHero('rh',  rhVal,  84);
    setHero('td',  tdVal,  10);
    setHero('mdl', mdlVal, 5);

    const totalTarget = 99;
    const totalPct = Math.round(total / totalTarget * 100);
    const totalColor = totalPct>=100?'#22c55e':totalPct>=80?'#f5a623':'#f87171';
    const totEl=document.getElementById('today-total');        if(totEl) totEl.textContent=total;
    const oEl=document.getElementById('today-overall-pct');    if(oEl){oEl.textContent=totalPct+'%';oEl.style.color=totalColor;}
    const totBar=document.getElementById('today-total-bar');   if(totBar) totBar.style.width=Math.min(totalPct,100)+'%';
  } catch(e) { console.warn('updateTodayTab error:', e); }
}

// ── Monthly Targets ──
const GOALS = { rh: 84, td: 10, mdl: 5 };
const MONTHLY_GOALS = { rh: 2520, td: 300, mdl: 150, total: 2970 };
const MONTHLY_REV_GOALS = {
  rh:    2520 * RATES.rh,                          // $30,240
  td:    Math.round(300  * ((RATES.td1+RATES.td2)/2)),  // $7,650
  mdl:   Math.round(150  * ((RATES.mdl1+RATES.mdl2)/2)),// $3,975
  get total() { return this.rh + this.td + this.mdl; }  // ~$45,690
};
let mgoalsChart = null;

function updateGoalsTab(d) {
  try {
  // Fallback chain to always find data
  if (!d || !d.labels || !d.labels.length) d = getFilteredData();
  if (!d || !d.labels || !d.labels.length) d = getCurrentMonthData();
  if (!d || !d.labels || !d.labels.length) {
    const sheets = Object.values(ALL_DATA);
    if (sheets.length) d = sheets[sheets.length-1];
  }
  if (!d || !d.labels || !d.labels.length) return;

  // Use current month totals (MTD)
  const curMonth = getCurrentMonthData() || d;
  const tRH  = curMonth.rh.reduce((a,b)=>a+b,0);
  const tTD  = curMonth.td.reduce((a,b)=>a+b,0);
  const tMDL = curMonth.mdl.reduce((a,b)=>a+b,0);
  const tot  = tRH + tTD + tMDL;

  // Update sub label
  const now = new Date();
  const moName = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'][now.getMonth()];
  const sub = document.getElementById('goals-sub');
  if (sub) sub.textContent = 'MTD totals vs monthly target — '+moName+' '+now.getFullYear()+' ('+curMonth.labels.length+' days recorded)';
  const mlEl = document.getElementById('today-month-label');
  if (mlEl) mlEl.textContent = moName+' '+now.getFullYear();

  // Hero card helper
  function setHero(id, actual, target, color) {
    const pct  = Math.round(actual / target * 100);
    const hit  = actual >= target;
    const near = pct >= 80;
    const barColor  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
    const badgeText = hit ? '✓ Goal reached!' : near ? 'Almost there' : 'Below target';
    const badgeBg   = hit ? 'rgba(34,197,94,.15)' : near ? 'rgba(245,166,35,.15)' : 'rgba(248,113,113,.15)';
    const badgeCol  = hit ? '#22c55e' : near ? '#f5a623' : '#f87171';
    const needed    = hit
      ? (actual - target).toLocaleString() + ' above target 🎉'
      : (target - actual).toLocaleString() + ' more to hit target';

    const v = document.getElementById('mgoal-'+id+'-val');
    if (v) v.textContent = actual.toLocaleString();
    const bar = document.getElementById('mgoal-'+id+'-bar');
    if (bar) { bar.style.width = Math.min(pct,100)+'%'; bar.style.background = barColor; }
    const pctEl = document.getElementById('mgoal-'+id+'-pct');
    if (pctEl) { pctEl.textContent = pct+'%'; pctEl.style.color = barColor; }
    const badge = document.getElementById('mgoal-'+id+'-badge');
    if (badge) { badge.textContent = badgeText; badge.style.background = badgeBg; badge.style.color = badgeCol; }
    const need = document.getElementById('mgoal-'+id+'-needed');
    if (need) need.textContent = needed;
  }

  setHero('rh',  tRH,  MONTHLY_GOALS.rh);
  setHero('td',  tTD,  MONTHLY_GOALS.td);
  setHero('mdl', tMDL, MONTHLY_GOALS.mdl);

  // Revenue card — use correct sub-type rates
  const tTD1m  = (curMonth.td1  || []).reduce((a,b)=>a+b,0);
  const tTD2m  = (curMonth.td2  || []).reduce((a,b)=>a+b,0);
  const tMDL1m = (curMonth.mdl1 || []).reduce((a,b)=>a+b,0);
  const tMDL2m = (curMonth.mdl2 || []).reduce((a,b)=>a+b,0);
  const actRHRev  = Math.round(tRH  * RATES.rh);
  const actTDRev  = Math.round(tTD1m * RATES.td1  + tTD2m  * RATES.td2);
  const actMDLRev = Math.round(tMDL1m* RATES.mdl1 + tMDL2m * RATES.mdl2);
  const actRevTot = actRHRev + actTDRev + actMDLRev;
  const revPct    = Math.round(actRevTot / MONTHLY_REV_GOALS.total * 100);
  const revColor  = revPct>=100?'#22c55e':revPct>=80?'#f5a623':'#f87171';

  const fmt = v => '$'+Math.round(v).toLocaleString();
  const el2 = (id,val) => { const e=document.getElementById(id); if(e) e.textContent=val; };
  el2('mgoal-rev-val',        fmt(actRevTot));
  el2('mgoal-rev-target',     fmt(MONTHLY_REV_GOALS.total));
  el2('mgoal-rev-rh',         fmt(actRHRev));
  el2('mgoal-rev-rh-target',  fmt(MONTHLY_REV_GOALS.rh));
  el2('mgoal-rev-td',         fmt(actTDRev));
  el2('mgoal-rev-td-target',  fmt(MONTHLY_REV_GOALS.td));
  el2('mgoal-rev-mdl',        fmt(actMDLRev));
  el2('mgoal-rev-mdl-target', fmt(MONTHLY_REV_GOALS.mdl));
  el2('mgoal-rev-pct',        revPct+'%');
  const revBar = document.getElementById('mgoal-rev-bar');
  if (revBar) { revBar.style.width=Math.min(revPct,100)+'%'; revBar.style.background=revColor; }
  const revPctEl = document.getElementById('mgoal-rev-pct');
  if (revPctEl) revPctEl.style.color = revColor;
  const revStatus = document.getElementById('mgoal-rev-status');
  if (revStatus) revStatus.textContent = revPct>=100 ? '✓ Revenue goal reached! 🎉' : (MONTHLY_REV_GOALS.total-actRevTot).toLocaleString()+' away from revenue target';

  // Combined total row
  const totalPct   = Math.round(tot / MONTHLY_GOALS.total * 100);
  const totalColor = totalPct >= 100 ? '#22c55e' : totalPct >= 80 ? '#f5a623' : '#f87171';
  const totEl = document.getElementById('mgoal-total');
  if (totEl) totEl.textContent = tot.toLocaleString();
  const overallEl = document.getElementById('mgoal-overall-pct');
  if (overallEl) { overallEl.textContent = totalPct+'%'; overallEl.style.color = totalColor; }
  const totBar = document.getElementById('mgoal-total-bar');
  if (totBar) totBar.style.width = Math.min(totalPct,100)+'%';

  // ── Monthly totals chart — one grouped bar per month ──
  if (mgoalsChart) { mgoalsChart.destroy(); mgoalsChart = null; }
  const gc = document.getElementById('mgoalsChart');
  if (!gc) return;

  // Build month-by-month totals from ALL_DATA
  const allSheets = Object.entries(ALL_DATA)
    .sort(([,a],[,b]) => a.year!==b.year ? a.year-b.year : a.month-b.month);

  const moNames = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const mLabels = allSheets.map(([,s]) => moNames[s.month-1]+' '+String(s.year).slice(2));
  const mRH  = allSheets.map(([,s]) => s.rh.reduce((a,b)=>a+b,0));
  const mTD  = allSheets.map(([,s]) => s.td.reduce((a,b)=>a+b,0));
  const mMDL = allSheets.map(([,s]) => s.mdl.reduce((a,b)=>a+b,0));
  const mTarget = allSheets.map(() => MONTHLY_GOALS.total);

  mgoalsChart = new Chart(gc.getContext('2d'), {
    data: {
      labels: mLabels,
      datasets: [
        { type:'bar',  label:'Roman', data:mRH,     backgroundColor:'rgba(45,212,160,.82)',  borderWidth:0, borderRadius:3, stack:'s' },
        { type:'bar',  label:'Teladoc',      data:mTD,     backgroundColor:'rgba(123,158,240,.82)', borderWidth:0, borderRadius:3, stack:'s' },
        { type:'bar',  label:'MDLive',       data:mMDL,    backgroundColor:'rgba(245,166,35,.82)',  borderWidth:0, borderRadius:3, stack:'s' },
        { type:'line', label:'Target',       data:mTarget, borderColor:'rgba(255,255,255,.4)', borderDash:[6,4],
          pointRadius:0, borderWidth:2, fill:false },
      ]
    },
    options:{
      responsive:true, maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{...tooltipDefaults(), callbacks:{
          footer: items => {
            const total = items.filter(i=>i.dataset.type==='bar').reduce((a,i)=>a+i.raw,0);
            return 'Total: '+total.toLocaleString()+' / '+MONTHLY_GOALS.total.toLocaleString();
          }
        }}
      },
      scales:{
        x:{ stacked:true, grid:{color:GRID}, ticks:{maxRotation:45} },
        y:{ stacked:true, grid:{color:GRID}, beginAtZero:true },
      }
    }
  });
  } catch(e) { console.warn('updateGoalsTab error:', e); }
}

// ── Manual refresh ──
function manualRefresh(btn) {
  const orig = btn.innerHTML;
  btn.innerHTML = '↻ Refreshing…';
  btn.style.color = '#60a5fa';
  btn.disabled = true;
  loadSheet(false).finally(() => {
    btn.innerHTML = orig;
    btn.style.color = 'var(--text2)';
    btn.disabled = false;
  });
}

// ── Revenue window filter ──
function setRevWindow(el, days) {
  document.getElementById('rev-window-group')
    .querySelectorAll('.pill').forEach(p => p.classList.remove('active'));
  el.classList.add('active');
  REV_WINDOW = days;
  buildRevTab30Day();
  updateRevSummary();
  buildVolWindowChart();
  const tMap = { 0:'All Time', 30:'Last 30 Days', 60:'Last 60 Days', 90:'Last 90 Days', 180:'Last 6 Months', 365:'Last Year' };
  const label = tMap[days] || days+' Days';
  const title = document.getElementById('rev-30day-title');
  if (title) title.textContent = 'Daily Revenue — '+label;
}

function buildVolWindowChart() {
  if (!volChart) return;
  const tMap = {0:'All Time',30:'Last 30 Days',60:'Last 60 Days',90:'Last 90 Days',180:'Last 6 Months',365:'Last Year'};
  const label = tMap[REV_WINDOW] || REV_WINDOW+' Days';

  // Update title
  const vt = document.getElementById('vol-window-title');
  if (vt) {
    const volLabel = ACTIVE_YEAR !== 'all' ? String(ACTIVE_YEAR) : label;
    vt.textContent = 'Daily Encounter Volume — '+volLabel;
  }

  // Get windowed days
  const days = getLast30DaysData(); // uses REV_WINDOW
  const labels  = days.map(d => d.label);
  const rhEnc   = days.map(d => d.rh);
  const tdEnc   = days.map(d => d.td);
  const mdlEnc  = days.map(d => d.mdl);

  volChart.data.labels = labels;
  volChart.data.datasets[0].data = rhEnc;
  volChart.data.datasets[1].data = tdEnc;
  volChart.data.datasets[2].data = mdlEnc;
  volChart.update('active');

  // Update stat cards
  const s = (id,v) => { const e=document.getElementById(id); if(e) e.textContent=v.toLocaleString(); };
  s('vol-rh',  rhEnc.reduce((a,b)=>a+b,0));
  s('vol-td',  tdEnc.reduce((a,b)=>a+b,0));
  s('vol-mdl', mdlEnc.reduce((a,b)=>a+b,0));
}

// ── Year filter ──
function setYear(el, year){
  document.querySelectorAll('.filterbar .filter-group')[0]
    .querySelectorAll('.pill').forEach(p=>p.classList.remove('active'));
  el.classList.add('active');
  ACTIVE_YEAR = year;
  applyFilters();
  // Rebuild revenue + volume windowed charts (year trumps window)
  buildRevTab30Day();
  updateRevSummary();
  buildVolWindowChart();
  if (revHistChart) buildRevHistChart();
  const cur = getCurrentMonthData();
  if (cur) document.getElementById('days-rec').textContent = cur.labels.length+' days recorded';
}

// ── Month filter ──
function setMonth(el, month){
  document.querySelectorAll('.filterbar .filter-group')[1]
    .querySelectorAll('.pill').forEach(p=>p.classList.remove('active'));
  el.classList.add('active');
  ACTIVE_MONTH = month;
  applyFilters();
}

</script>
</body>
</html>
