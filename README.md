<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Telemedicine Activity Dashboard</title>
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
  .statrow,.mtd-row{grid-template-columns:1fr;}
  .topbar,.filterbar{flex-direction:column;align-items:flex-start;}
}
</style>
</head>
<body>

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-left">
    <h1>Telemedicine Activity Dashboard</h1>
    <div class="sub">
      July 2024 – Present &nbsp;·&nbsp;
      <span class="ldot" style="background:var(--rh)"></span> Roman Health &nbsp;·&nbsp;
      <span class="ldot" style="background:var(--mdl)"></span> MDLive &nbsp;·&nbsp;
      <span class="ldot" style="background:var(--td-c)"></span> Teladoc
    </div>
  </div>
  <div class="feed-badge">
    <span class="feed-dot" id="feed-dot"></span>
    <span id="feed-label">Connecting…</span>
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
    <span class="flabel">Filter by type:</span>
    <button class="pill active" onclick="setType(this,'all')">All</button>
    <button class="pill" onclick="setType(this,'rh')">Roman Health</button>
    <button class="pill" onclick="setType(this,'mdl')">MDLive</button>
    <button class="pill" onclick="setType(this,'td')">Teladoc</button>
  </div>
</div>

<!-- STAT CARDS -->
<div class="statrow">
  <div class="statcard">
    <div class="sclabel">Avg Per Month</div>
    <div class="scrow">
      <div class="metric"><span class="mname">Encounters</span><span class="mval rh-c" id="s-enc">—</span></div>
      <div class="metric"><span class="mname">Revenue</span><span class="mval rev-c" id="s-rev">—</span></div>
    </div>
  </div>
  <div class="statcard">
    <div class="sclabel">Avg Visits / Month</div>
    <div class="scrow">
      <div class="metric"><span class="mname">Roman Health</span><span class="mval rh-c" id="s-rh-mo">—</span></div>
      <div class="metric"><span class="mname">MDLive + Teladoc</span><span class="mval td-c2" id="s-tdmdl-mo">—</span></div>
    </div>
  </div>
  <div class="statcard">
    <div class="sclabel">Avg Visits / Day</div>
    <div class="scrow">
      <div class="metric"><span class="mname">Roman Health</span><span class="mval rh-c" id="s-rh-day">—</span></div>
      <div class="metric"><span class="mname">MDLive</span><span class="mval mdl-c" id="s-mdl-day">—</span></div>
      <div class="metric"><span class="mname">Teladoc</span><span class="mval td-c2" id="s-td-day">—</span></div>
    </div>
  </div>
</div>

<!-- SETUP NOTICE -->
<div id="setup-notice" style="
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
    var data = sheet.getDataRange().getValues();
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
  return ContentService.createTextOutput(JSON.stringify(result))
    .setMimeType(ContentService.MimeType.TEXT);
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
  <button class="tab" onclick="switchTab('revenue',this)">Revenue</button>
  <button class="tab" onclick="switchTab('volume',this)">Encounter Volume</button>
  <button class="tab" onclick="switchTab('breakdown',this)">Visit Breakdown</button>
  <button class="tab" onclick="switchTab('compare',this)">Roman Health vs Teladoc+MDLive</button>
  <button class="tab live-tab active" onclick="switchTab('daily',this)">Daily (Live)</button>
</div>

<!-- PANELS -->
<div id="panel-revenue" class="panel">
  <div class="ph2" style="margin-bottom:4px;">Revenue by Provider — Mar '26</div>
  <div class="psub">Estimated daily revenue based on per-visit rates · RH $12/visit · TD $24.33/visit · MDL $25.46/visit</div>
  <div class="chart-sec">
    <h3>Daily Revenue ($)</h3>
    <div class="chart-wrap" style="height:300px;"><canvas id="revTabChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman Health</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>
  <div class="mtd-row" style="margin-top:20px;">
    <div class="mtd-card"><div class="mclabel">RH Revenue MTD</div><div class="mcval rh-c" id="rev-rh-mtd">—</div></div>
    <div class="mtd-card"><div class="mclabel">TD Revenue MTD</div><div class="mcval td-c2" id="rev-td-mtd">—</div></div>
    <div class="mtd-card"><div class="mclabel">MDL Revenue MTD</div><div class="mcval mdl-c" id="rev-mdl-mtd">—</div></div>
  </div>
</div>

<div id="panel-volume" class="panel">
  <div class="ph2" style="margin-bottom:4px;">Encounter Volume — Mar '26</div>
  <div class="psub">Total daily encounters across all providers</div>
  <div class="chart-sec">
    <h3>Daily Encounter Volume</h3>
    <div class="chart-wrap" style="height:300px;"><canvas id="volChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman Health</div>
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
  <div class="ph2" style="margin-bottom:4px;">Visit Breakdown — Mar '26</div>
  <div class="psub">Share of total encounters by provider</div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;align-items:center;">
    <div class="chart-wrap" style="height:300px;"><canvas id="pieChart"></canvas></div>
    <div>
      <div class="mtd-card" style="margin-bottom:10px;">
        <div class="mclabel">Roman Health</div>
        <div class="mcval rh-c" id="pie-rh-pct">—</div>
        <div style="font-size:.7rem;color:var(--text3);margin-top:4px;" id="pie-rh-abs">— visits</div>
      </div>
      <div class="mtd-card" style="margin-bottom:10px;">
        <div class="mclabel">Teladoc</div>
        <div class="mcval td-c2" id="pie-td-pct">—</div>
        <div style="font-size:.7rem;color:var(--text3);margin-top:4px;" id="pie-td-abs">— visits</div>
      </div>
      <div class="mtd-card">
        <div class="mclabel">MDLive</div>
        <div class="mcval mdl-c" id="pie-mdl-pct">—</div>
        <div style="font-size:.7rem;color:var(--text3);margin-top:4px;" id="pie-mdl-abs">— visits</div>
      </div>
    </div>
  </div>
</div>

<div id="panel-compare" class="panel">
  <div class="ph2" style="margin-bottom:4px;">Roman Health vs Teladoc + MDLive</div>
  <div class="psub">Daily encounter comparison — RH (large volume) vs combined telehealth providers</div>
  <div class="chart-sec">
    <h3>Daily Encounters</h3>
    <div class="chart-wrap" style="height:300px;"><canvas id="cmpChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman Health</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc + MDLive</div>
    </div>
  </div>
  <div class="mtd-row" style="margin-top:20px;">
    <div class="mtd-card"><div class="mclabel">RH Total</div><div class="mcval rh-c" id="cmp-rh">—</div></div>
    <div class="mtd-card"><div class="mclabel">TD + MDL Total</div><div class="mcval td-c2" id="cmp-tdmdl">—</div></div>
    <div class="mtd-card"><div class="mclabel">RH Share</div><div class="mcval rh-c" id="cmp-pct">—</div></div>
  </div>
</div>

<div id="panel-daily" class="panel active">
  <div class="ph2">
    Daily Breakdown — Mar '26
    <span class="live-badge" id="live-badge">
      <span class="dot"></span>
      Live · <span id="days-rec">—</span>
    </span>
  </div>
  <div class="psub" id="psub">Loading from Google Sheet…</div>

  <div class="mtd-row">
    <div class="mtd-card">
      <div class="mclabel">MTD Encounters</div>
      <div class="mcval rh-c" id="mtd-enc">—</div>
    </div>
    <div class="mtd-card">
      <div class="mclabel">MTD Revenue</div>
      <div class="mcval rev-c" id="mtd-rev">—</div>
    </div>
    <div class="mtd-card">
      <div class="mclabel">Avg Daily (Roman Health)</div>
      <div class="mcval rh-c" id="avg-rh">—</div>
    </div>
  </div>

  <div class="chart-sec">
    <h3>Daily Encounters</h3>
    <div class="chart-wrap"><canvas id="encChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman Health</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>

  <div class="chart-sec">
    <h3>Daily Revenue (estimated by provider)</h3>
    <div class="chart-wrap"><canvas id="revChart"></canvas></div>
    <div class="clegend">
      <div class="cleg"><div class="cleg-dot" style="background:var(--rh)"></div>Roman Health</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--td-c)"></div>Teladoc</div>
      <div class="cleg"><div class="cleg-dot" style="background:var(--mdl)"></div>MDLive</div>
    </div>
  </div>
</div>

<!-- FOOTER -->
<div class="footer-row">
  <span>Last refreshed: <span id="last-updated">—</span></span>
  <span>Auto-refreshes every 5 minutes</span>
</div>

<script>
const APPS_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbyWtcu9TVuuwIfokKmIS2lbD6yM8li74UkfHLooUb3-QPJjkRLhMG0rM_ej6UVjCKNI/exec";
const SHEET_CSV_URL   = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSc3am4UpZjqNJx9i5asqyEn-AI51cqKSPKvjV89ujSi_4FnDDgxw5gbc6dk2hnX4HK3o52aqm6DUDW/pub?output=csv";

// ── Revenue rates per visit ──
const RATES = { rh: 12.00, td: 24.33, mdl: 25.46 };

// ── Global state ──
let ALL_DATA     = {};   // { "Jan 24": {labels,rh,td,mdl,rev,...}, ... }
let ACTIVE_YEAR  = 'all';
let ACTIVE_TYPE  = 'all';
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

// ── Parse one sheet's CSV text into a data object ──
function parseSheetCSV(text) {
  const rows = text.trim().split('\n').map(parseCSVRow);
  if (rows.length < 2) return null;
  const headers = rows[0];
  const allDateCols = [];
  headers.forEach((h,i) => {
    if (i===0) return;
    const d = parseDateCol(h.trim());
    if (d) allDateCols.push({i,...d});
  });
  if (!allDateCols.length) return null;

  allDateCols.sort((a,b) => a.day - b.day);
  const n = allDateCols.length;
  const labels = allDateCols.map(d => d.month+'/'+d.day);
  const rh=new Array(n).fill(0), td=new Array(n).fill(0),
        mdl=new Array(n).fill(0), rev=new Array(n).fill(0);

  for (const row of rows.slice(1)) {
    const name = (row[0]||'').trim().toUpperCase().replace(/\s+/g,'');
    if (!name) continue;
    allDateCols.forEach((col,j) => {
      const v = parseFloat((row[col.i]||'').replace(/[$, ]/g,''))||0;
      if      (name==='RO')                         rh[j]  += v;
      else if (name.startsWith('TD'))               td[j]  += v;
      else if (name==='MDL'||name==='MD')           mdl[j] += v;
      else if (name==='TOT'||name==='TOTAL')        rev[j] += v;
    });
  }

  let lastIdx = -1;
  for (let j=0;j<n;j++) if(rh[j]>0||td[j]>0||mdl[j]>0||rev[j]>0) lastIdx=j;
  if (lastIdx<0) return null;
  const t = lastIdx+1;

  return {
    labels: labels.slice(0,t),
    rh:  rh.slice(0,t),  td:  td.slice(0,t),  mdl: mdl.slice(0,t), rev: rev.slice(0,t),
    rhRev:  rh.slice(0,t).map(v=>Math.round(v*RATES.rh)),
    tdRev:  td.slice(0,t).map(v=>Math.round(v*RATES.td)),
    mdlRev: mdl.slice(0,t).map(v=>Math.round(v*RATES.mdl)),
    month: allDateCols[0].month,
    year:  allDateCols[0].year,
  };
}

// ── Derive a year string from a sheet tab name ──
// "Jan 24" → 2024, "Mar 26" → 2026, "Jan 2024" → 2024
function yearFromTabName(name) {
  let m;
  m = name.match(/(\d{4})/);
  if (m) return +m[1];
  m = name.match(/(\d{2})$/);
  if (m) { const y=+m[1]; return y<50?2000+y:1900+y; }
  return null;
}

// ── Aggregate multiple sheet data objects into one combined dataset ──
function aggregate(dataArr) {
  if (!dataArr.length) return null;
  const labels=[], rh=[], td=[], mdl=[], rev=[], rhRev=[], tdRev=[], mdlRev=[];
  dataArr.forEach(d => {
    d.labels.forEach((l,i) => {
      labels.push(l);
      rh.push(d.rh[i]);   td.push(d.td[i]);   mdl.push(d.mdl[i]); rev.push(d.rev[i]);
      rhRev.push(d.rhRev[i]); tdRev.push(d.tdRev[i]); mdlRev.push(d.mdlRev[i]);
    });
  });
  return {labels,rh,td,mdl,rev,rhRev,tdRev,mdlRev};
}

// ── Get filtered dataset based on ACTIVE_YEAR / ACTIVE_TYPE ──
function getFilteredData() {
  let sheets = Object.entries(ALL_DATA); // [[name, data], ...]
  if (!sheets.length) return null;

  // Year filter
  if (ACTIVE_YEAR !== 'all') {
    const yr = +ACTIVE_YEAR;
    sheets = sheets.filter(([name, d]) => d.year === yr || yearFromTabName(name) === yr);
  }
  if (!sheets.length) return null;

  // Sort by date
  sheets.sort(([,a],[,b]) => {
    if (a.year !== b.year) return a.year - b.year;
    return a.month - b.month;
  });

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
      {label:'Roman Health',data:d.rh, backgroundColor:'rgba(45,212,160,.82)',borderWidth:0,borderRadius:2,stack:'s'},
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
      {label:'Roman Health',data:d.rhRev||[], borderColor:'#2dd4a0',backgroundColor:'rgba(45,212,160,.06)',fill:true,tension:.35,pointRadius:3,pointBackgroundColor:'#2dd4a0',pointBorderColor:'#0d1117',pointBorderWidth:1.5,borderWidth:2},
      {label:'Teladoc',     data:d.tdRev||[], borderColor:'#7b9ef0',backgroundColor:'rgba(123,158,240,.06)',fill:true,tension:.35,pointRadius:3,pointBackgroundColor:'#7b9ef0',pointBorderColor:'#0d1117',pointBorderWidth:1.5,borderWidth:2},
      {label:'MDLive',      data:d.mdlRev||[],borderColor:'#f5a623',backgroundColor:'rgba(245,166,35,.06)', fill:true,tension:.35,pointRadius:3,pointBackgroundColor:'#f5a623',pointBorderColor:'#0d1117',pointBorderWidth:1.5,borderWidth:2},
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

  revTabChart = mkLine('revTabChart',[
    {label:'Roman Health',data:d.rhRev||[], borderColor:'#2dd4a0',backgroundColor:'rgba(45,212,160,.08)',fill:true,tension:.35,pointRadius:3,pointBackgroundColor:'#2dd4a0',pointBorderColor:'#0d1117',pointBorderWidth:1.5,borderWidth:2},
    {label:'Teladoc',     data:d.tdRev||[], borderColor:'#7b9ef0',backgroundColor:'rgba(123,158,240,.08)',fill:true,tension:.35,pointRadius:3,pointBackgroundColor:'#7b9ef0',pointBorderColor:'#0d1117',pointBorderWidth:1.5,borderWidth:2},
    {label:'MDLive',      data:d.mdlRev||[],borderColor:'#f5a623',backgroundColor:'rgba(245,166,35,.08)', fill:true,tension:.35,pointRadius:3,pointBackgroundColor:'#f5a623',pointBorderColor:'#0d1117',pointBorderWidth:1.5,borderWidth:2},
  ]);
  volChart = mkBar('volChart',[
    {label:'Roman Health',data:d.rh, backgroundColor:'rgba(45,212,160,.82)',borderWidth:0,borderRadius:2,stack:'s'},
    {label:'Teladoc',     data:d.td, backgroundColor:'rgba(123,158,240,.82)',borderWidth:0,borderRadius:2,stack:'s'},
    {label:'MDLive',      data:d.mdl,backgroundColor:'rgba(245,166,35,.82)', borderWidth:0,borderRadius:2,stack:'s'},
  ]);
  const tRH=d.rh.reduce((a,b)=>a+b,0), tTD=d.td.reduce((a,b)=>a+b,0), tMDL=d.mdl.reduce((a,b)=>a+b,0), tot=(tRH+tTD+tMDL)||1;
  pieChart = new Chart(document.getElementById('pieChart').getContext('2d'),{
    type:'doughnut', data:{labels:['Roman Health','Teladoc','MDLive'],datasets:[{data:[tRH,tTD,tMDL],backgroundColor:['rgba(45,212,160,.85)','rgba(123,158,240,.85)','rgba(245,166,35,.85)'],borderColor:['#2dd4a0','#7b9ef0','#f5a623'],borderWidth:2,hoverOffset:6}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:'65%',plugins:{legend:{display:false},tooltip:{...tooltipDefaults(),callbacks:{label:i=>i.label+': '+i.raw+' ('+Math.round(i.raw/tot*100)+'%)'}}}}
  });
  const tdmdl=d.td.map((v,i)=>v+(d.mdl[i]||0));
  cmpChart = mkBar('cmpChart',[
    {label:'Roman Health',   data:d.rh,  backgroundColor:'rgba(45,212,160,.82)',borderWidth:0,borderRadius:2},
    {label:'Teladoc+MDLive', data:tdmdl, backgroundColor:'rgba(123,158,240,.82)',borderWidth:0,borderRadius:2},
  ], false);
  updateTabStats(d);
}

// ── Update all charts with new data ──
function refreshAllCharts() {
  applyFilters(); // applyFilters now handles daily vs tab split
}

// ── Update stat cards ──
function updateStats(d) {
  const n=d.labels.length;
  const tRH=d.rh.reduce((a,b)=>a+b,0), tTD=d.td.reduce((a,b)=>a+b,0), tMDL=d.mdl.reduce((a,b)=>a+b,0);
  const tEnc=tRH+tTD+tMDL, tRev=d.rev.reduce((a,b)=>a+b,0);
  document.getElementById('mtd-enc').textContent=tEnc.toLocaleString();
  document.getElementById('mtd-rev').textContent='$'+tRev.toLocaleString();
  document.getElementById('avg-rh').textContent=n?(tRH/n).toFixed(0):'—';
  document.getElementById('days-rec').textContent=n+' days recorded';
  document.getElementById('s-enc').textContent=tEnc.toLocaleString();
  document.getElementById('s-rev').textContent='$'+tRev.toLocaleString();
  document.getElementById('s-rh-mo').textContent=tRH.toLocaleString();
  document.getElementById('s-tdmdl-mo').textContent=(tTD+tMDL).toLocaleString();
  document.getElementById('s-rh-day').textContent=n?(tRH/n).toFixed(1):'—';
  document.getElementById('s-mdl-day').textContent=n?(tMDL/n).toFixed(1):'—';
  document.getElementById('s-td-day').textContent=n?(tTD/n).toFixed(1):'—';
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
  document.getElementById('pie-rh-pct').textContent=Math.round(tRH/tot*100)+'%';
  document.getElementById('pie-td-pct').textContent=Math.round(tTD/tot*100)+'%';
  document.getElementById('pie-mdl-pct').textContent=Math.round(tMDL/tot*100)+'%';
  document.getElementById('pie-rh-abs').textContent=tRH.toLocaleString()+' visits';
  document.getElementById('pie-td-abs').textContent=tTD.toLocaleString()+' visits';
  document.getElementById('pie-mdl-abs').textContent=tMDL.toLocaleString()+' visits';
  document.getElementById('cmp-rh').textContent=tRH.toLocaleString();
  document.getElementById('cmp-tdmdl').textContent=(tTD+tMDL).toLocaleString();
  document.getElementById('cmp-pct').textContent=Math.round(tRH/tot*100)+'%';
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

  // Other tabs use full filtered data
  const d = getFilteredData();
  if (!d) return;
  if (window._tabChartsBuilt) {
    revTabChart.data.labels=volChart.data.labels=d.labels;
    revTabChart.data.datasets[0].data=d.rhRev||[]; revTabChart.data.datasets[1].data=d.tdRev||[]; revTabChart.data.datasets[2].data=d.mdlRev||[];
    revTabChart.update('active');
    volChart.data.datasets[0].data=d.rh; volChart.data.datasets[1].data=d.td; volChart.data.datasets[2].data=d.mdl;
    volChart.update('active');
    const tRH=d.rh.reduce((a,b)=>a+b,0),tTD=d.td.reduce((a,b)=>a+b,0),tMDL=d.mdl.reduce((a,b)=>a+b,0),tot=(tRH+tTD+tMDL)||1;
    pieChart.data.datasets[0].data=[tRH,tTD,tMDL]; pieChart.update('active');
    const tdmdl=d.td.map((v,i)=>v+(d.mdl[i]||0));
    cmpChart.data.labels=d.labels; cmpChart.data.datasets[0].data=d.rh; cmpChart.data.datasets[1].data=tdmdl; cmpChart.update('active');
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

buildCharts(FB_MAR26);
updateStats(FB_MAR26);
document.getElementById('days-rec').textContent = FB_MAR26.labels.length+' days recorded';
document.getElementById('live-badge').style.display='flex';

// ── Fetch all sheets from Apps Script ──
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
    } catch(e) { console.warn('Fetch failed:',e.message); }
  }
  throw new Error('All endpoints failed');
}

async function loadSheet() {
  try {
    const t = await fetchCSV();
    let parsed = {};

    // New format: JSON object of {sheetName: csvText}
    if (t.trim().startsWith('{')) {
      const json = JSON.parse(t);
      for (const [name, csv] of Object.entries(json)) {
        const d = parseSheetCSV(csv);
        if (d) {
          // Derive year from tab name if not detected from dates
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
    ALL_DATA = parsed;
    window._lastData = getCurrentMonthData()||FB_MAR26;

    // Daily badge shows current month day count
    const curMonth = getCurrentMonthData();
    const mo = curMonth.month, yr = curMonth.year;
    const moName = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'][mo-1];
    document.getElementById('days-rec').textContent = curMonth.labels.length+' days recorded';
    // Update panel header label
    const ph = document.querySelector('.ph2');
    if (ph) ph.childNodes[0].textContent = 'Daily Breakdown — '+moName+' \''+String(yr).slice(2)+' ';

    applyFilters();

    document.getElementById('feed-dot').classList.add('live');
    document.getElementById('feed-label').textContent='Live · '+Object.keys(parsed).length+' sheets loaded';
    document.getElementById('psub').textContent='Pulled from '+Object.keys(parsed).length+' sheet tabs · refreshes every 5 min';
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
setInterval(loadSheet, 5*60*1000);

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
      if(id==='revenue')   revTabChart&&revTabChart.resize();
      if(id==='volume')    volChart&&volChart.resize();
      if(id==='breakdown') pieChart&&pieChart.resize();
      if(id==='compare')   cmpChart&&cmpChart.resize();
    }
  },80);
}

// ── Year filter ──
function setYear(el, year){
  document.querySelectorAll('.filterbar .filter-group')[0]
    .querySelectorAll('.pill').forEach(p=>p.classList.remove('active'));
  el.classList.add('active');
  ACTIVE_YEAR = year;
  applyFilters();
  // Update header label
  // Daily tab always shows current month — days-rec never changes with year filter
  const cur = getCurrentMonthData();
  if (cur) document.getElementById('days-rec').textContent = cur.labels.length+' days recorded';
}

// ── Type filter ──
function setType(el, type){
  document.querySelectorAll('.filterbar .filter-group')[1]
    .querySelectorAll('.pill').forEach(p=>p.classList.remove('active'));
  el.classList.add('active');
  ACTIVE_TYPE = type;
  applyFilters();
}
</script>
</body>
</html>
