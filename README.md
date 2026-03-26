
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Telemedicine Dashboard</title>
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
  background:var(--active-bg);border-color:var(--active-border);
  border-bottom-color:var(--active-bg);color:#60a5fa;
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
    <button class="pill active" onclick="setPill(this,0)">All</button>
    <button class="pill" onclick="setPill(this,0)">2024</button>
    <button class="pill" onclick="setPill(this,0)">2025</button>
    <button class="pill" onclick="setPill(this,0)">2026</button>
  </div>
  <div class="fdivider"></div>
  <div class="filter-group">
    <span class="flabel">Filter by type:</span>
    <button class="pill active" onclick="setPill(this,1)">All</button>
    <button class="pill" onclick="setPill(this,1)">Roman Health</button>
    <button class="pill" onclick="setPill(this,1)">MDLive</button>
    <button class="pill" onclick="setPill(this,1)">Teladoc</button>
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
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = sheet.getDataRange().getDisplayValues();
  var csv = data.map(function(row) {
    return row.map(function(cell) {
      var s = String(cell).trim();
      return s.indexOf(",") > -1 ? '"' + s + '"' : s;
    }).join(",");
  }).join("\n");
  return ContentService.createTextOutput(csv)
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
<div id="panel-revenue"   class="panel"><div class="placeholder">📈 Revenue chart — coming soon</div></div>
<div id="panel-volume"    class="panel"><div class="placeholder">📊 Encounter Volume chart — coming soon</div></div>
<div id="panel-breakdown" class="panel"><div class="placeholder">🥧 Visit Breakdown — coming soon</div></div>
<div id="panel-compare"   class="panel"><div class="placeholder">⚖️ Comparison chart — coming soon</div></div>

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
    <h3>Daily Revenue</h3>
    <div class="chart-wrap"><canvas id="revChart"></canvas></div>
  </div>
</div>

<!-- FOOTER -->
<div class="footer-row">
  <span>Last refreshed: <span id="last-updated">—</span></span>
  <span>Auto-refreshes every 5 minutes</span>
</div>

<script>
// ═══════════════════════════════════════════════════════
// Paste your Google Apps Script Web App URL below
// after completing the setup steps shown in the dashboard
// ═══════════════════════════════════════════════════════
const APPS_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbxWMOlUA_Nzvn5zRiyj1PAh5hx-EnhqoEJC6EzoQMp_Wl90myaZAAIz5N5bZQC-ic7n/exec"; // ← paste your /exec URL here

const SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSc3am4UpZjqNJx9i5asqyEn-AI51cqKSPKvjV89ujSi_4FnDDgxw5gbc6dk2hnX4HK3o52aqm6DUDW/pub?output=csv";

// Fallback — RO=Roman Health, TD=Teladoc, MDL=MDLive
const FB = {
  labels:['3/1','3/2','3/3','3/4','3/5','3/6','3/7','3/8','3/9','3/10','3/11','3/12','3/13','3/14','3/15','3/16','3/17','3/18','3/19','3/20','3/21','3/22','3/23','3/24','3/25'],
  rh: [84,63,84,66,65,78,84,84,82,84,81,115,98,115,84,85,85,70,115,90,70,141,51,67,62],
  td: [0,6,0,7,9,3,0,0,0,0,5,0,4,0,0,0,0,9,0,2,7,0,2,0,10],
  mdl:[0,4,0,2,0,0,0,0,1,0,0,0,4,0,0,0,0,1,0,1,0,0,0,0,0],
  rev:[1008,1007,1008,1003,1002,1005,1008,1008,1009,1008,1107,1380,1381,1380,1008,1020,1020,1072,1380,1151,1016,1692,658,804,989],
};

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
  // Format from updated Apps Script: "3-26-2026" (month-day-year)
  let m = h.match(/^(\d{1,2})-(\d{1,2})-(\d{4})$/);
  if (m) return { month: parseInt(m[1]), day: parseInt(m[2]), year: parseInt(m[3]) };

  // Full JS Date string: "Mon Jan 01 2024 00:00:00 GMT-0800..."
  // or "Thu Mar 26 2026 00:00:00..."
  m = h.match(/([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})/);
  if (m) {
    const months = {jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12};
    const mo = months[m[1].toLowerCase()];
    if (mo) return { month: mo, day: parseInt(m[2]), year: parseInt(m[3]) };
  }

  // "1-Mar", "2-Mar"
  m = h.match(/^(\d{1,2})-Mar$/i);
  if (m) return { month: 3, day: parseInt(m[1]), year: 2026 };

  // "3/1", "3/26/2026"
  m = h.match(/^(\d{1,2})\/(\d{1,2})(?:\/(\d{4}))?$/);
  if (m) return { month: parseInt(m[1]), day: parseInt(m[2]), year: m[3] ? parseInt(m[3]) : 2026 };

  return null;
}

function parseSheet(text) {
  const rows = text.trim().split('\n').map(parseCSVRow);
  if (rows.length < 2) return null;

  const headers = rows[0];

  // Find all date columns, then filter to March 2026
  const allDateCols = [];
  headers.forEach((h, i) => {
    if (i === 0) return;
    const d = parseDateCol(h.trim());
    if (d) allDateCols.push({ i, ...d });
  });

  // Prefer March 2026; fall back to any March
  let marCols = allDateCols.filter(d => d.month === 3 && d.year === 2026);
  if (!marCols.length) marCols = allDateCols.filter(d => d.month === 3);
  if (!marCols.length && allDateCols.length) {
    // Just use whatever month has data in col 1
    const firstMonth = allDateCols[0].month;
    const firstYear  = allDateCols[0].year;
    marCols = allDateCols.filter(d => d.month === firstMonth && d.year === firstYear);
  }
  if (!marCols.length) { console.warn('No date cols found. Headers:', headers.slice(0,5)); return null; }

  marCols.sort((a,b) => a.day - b.day);
  const dayIdxs = marCols.map(d => d.i);
  const dayNums = marCols.map(d => d.day);
  const n = dayIdxs.length;
  const labels = dayNums.map(d => '3/' + d);

  const rh=new Array(n).fill(0), td=new Array(n).fill(0),
        mdl=new Array(n).fill(0), rev=new Array(n).fill(0);

  for (const row of rows.slice(1)) {
    const name = (row[0]||'').trim().toUpperCase().replace(/\s+/g,'');
    if (!name) continue;
    dayIdxs.forEach((ci, j) => {
      const v = parseFloat((row[ci]||'').replace(/[$, ]/g,'')) || 0;
      if (name === 'RO')                          rh[j]  += v;
      else if (name.startsWith('TD'))             td[j]  += v;
      else if (name.startsWith('MD') || name.startsWith('MDL')) mdl[j] += v;
      else if (name === 'TOT' || name === 'TOTAL') rev[j] += v;
    });
  }

  // Only keep days that have at least some data entered (non-zero across all series)
  let lastDataIdx = -1;
  for (let j = 0; j < n; j++) {
    if (rh[j] > 0 || td[j] > 0 || mdl[j] > 0 || rev[j] > 0) lastDataIdx = j;
  }
  // If nothing at all parsed, return null so fallback kicks in
  if (lastDataIdx < 0) return null;

  // Slice to only days with data
  const trim = lastDataIdx + 1;
  return {
    labels: labels.slice(0, trim),
    rh:     rh.slice(0, trim),
    td:     td.slice(0, trim),
    mdl:    mdl.slice(0, trim),
    rev:    rev.slice(0, trim),
  };
}

Chart.defaults.color='#64748b';
Chart.defaults.font.family="'DM Sans',sans-serif";
Chart.defaults.font.size=11;
const GRID='#1e2a3a';

let encChart=null, revChart=null;

function buildCharts(d){
  const ec=document.getElementById('encChart').getContext('2d');
  encChart=new Chart(ec,{
    type:'bar',
    data:{
      labels:d.labels,
      datasets:[
        {label:'Roman Health',data:d.rh,backgroundColor:'rgba(45,212,160,.82)',borderWidth:0,borderRadius:2,stack:'s'},
        {label:'Teladoc',     data:d.td,backgroundColor:'rgba(123,158,240,.82)',borderWidth:0,borderRadius:2,stack:'s'},
        {label:'MDLive',      data:d.mdl,backgroundColor:'rgba(245,166,35,.82)',borderWidth:0,borderRadius:2,stack:'s'},
      ],
    },
    options:{
      responsive:true,maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{
          backgroundColor:'#1c2333',borderColor:'#2d3a50',borderWidth:1,
          titleColor:'#94a3b8',bodyColor:'#e2e8f0',padding:10,
          callbacks:{footer:items=>'Total: '+items.reduce((a,i)=>a+i.raw,0)}
        }
      },
      scales:{
        x:{stacked:true,grid:{color:GRID},ticks:{maxRotation:45}},
        y:{stacked:true,grid:{color:GRID},beginAtZero:true},
      }
    }
  });
  const rc=document.getElementById('revChart').getContext('2d');
  revChart=new Chart(rc,{
    type:'line',
    data:{
      labels:d.labels,
      datasets:[{
        label:'Revenue',data:d.rev,
        borderColor:'#f87171',backgroundColor:'rgba(248,113,113,.08)',
        fill:true,tension:.35,pointRadius:3,
        pointBackgroundColor:'#f87171',pointBorderColor:'#0d1117',pointBorderWidth:1.5,
        borderWidth:2,
      }]
    },
    options:{
      responsive:true,maintainAspectRatio:false,
      interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{
          backgroundColor:'#1c2333',borderColor:'#2d3a50',borderWidth:1,
          titleColor:'#94a3b8',bodyColor:'#e2e8f0',padding:10,
          callbacks:{label:i=>' $'+i.raw.toLocaleString()}
        }
      },
      scales:{
        x:{grid:{color:GRID},ticks:{maxRotation:45}},
        y:{grid:{color:GRID},beginAtZero:false,ticks:{callback:v=>'$'+(v/1000).toFixed(1)+'k'}},
      }
    }
  });
}

function refreshCharts(d){
  encChart.data.labels=d.labels;
  encChart.data.datasets[0].data=d.rh;
  encChart.data.datasets[1].data=d.td;
  encChart.data.datasets[2].data=d.mdl;
  encChart.update('active');
  revChart.data.labels=d.labels;
  revChart.data.datasets[0].data=d.rev;
  revChart.update('active');
}

function updateStats(d){
  const n=d.labels.length;
  const tRH=d.rh.reduce((a,b)=>a+b,0);
  const tTD=d.td.reduce((a,b)=>a+b,0);
  const tMDL=d.mdl.reduce((a,b)=>a+b,0);
  const tEnc=tRH+tTD+tMDL;
  const tRev=d.rev.reduce((a,b)=>a+b,0);

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

// Init with fallback
buildCharts(FB);
updateStats(FB);
document.getElementById('days-rec').textContent=FB.labels.length+' days recorded';
document.getElementById('live-badge').style.display='flex';

async function fetchCSV() {
  // Prefer Apps Script proxy (works from file://, no CORS issues)
  const endpoints = APPS_SCRIPT_URL
    ? [APPS_SCRIPT_URL + '?_=' + Date.now()]
    : [
        'https://corsproxy.io/?url=' + encodeURIComponent(SHEET_CSV_URL),
        'https://api.allorigins.win/raw?url=' + encodeURIComponent(SHEET_CSV_URL),
      ];

  for (const url of endpoints) {
    try {
      const r = await fetch(url, { cache: 'no-store', redirect: 'follow' });
      if (!r.ok) throw new Error('HTTP ' + r.status);
      const t = await r.text();
      if (!t || t.trim().length < 10) throw new Error('empty response');
      // If Apps Script returned JSON error, surface it
      if (t.trim().startsWith('{')) {
        const j = JSON.parse(t);
        if (j.error) throw new Error('Apps Script: ' + j.error);
      }
      return t;
    } catch(e) {
      console.warn('Endpoint failed:', url, e.message);
    }
  }
  throw new Error(APPS_SCRIPT_URL
    ? 'Apps Script URL failed — check it is deployed as "Anyone" access'
    : 'All fallback proxies failed — set up the Apps Script for reliable access');
}

async function loadSheet(){
  if (!APPS_SCRIPT_URL && !SHEET_CSV_URL) return;
  try{
    const t = await fetchCSV();
    const d = parseSheet(t);
    if(!d||!d.labels.length) throw new Error('Parsed 0 rows — check row labels are RO / TD / MDL in column A');
    refreshCharts(d);
    updateStats(d);
    document.getElementById('feed-dot').classList.add('live');
    document.getElementById('feed-label').textContent='Live feed · auto-refreshes every 5 min';
    document.getElementById('live-badge').style.display='flex';
    document.getElementById('psub').textContent='Pulled directly from your Google Sheet · refreshes every 5 minutes';
    document.getElementById('last-updated').textContent=new Date().toLocaleTimeString();
    // Hide setup notice on success
    const notice = document.getElementById('setup-notice');
    if (notice) notice.style.display = 'none';
  }catch(e){
    const msg=e.message||String(e);
    document.getElementById('feed-dot').classList.remove('live');
    document.getElementById('feed-label').textContent='Feed unavailable · cached data';
    document.getElementById('psub').textContent='⚠️ ' + msg;
    document.getElementById('last-updated').textContent='Failed · '+new Date().toLocaleTimeString();
    console.warn('Sheet load failed:', msg);
  }
}
loadSheet();
setInterval(loadSheet,5*60*1000);

function switchTab(id,el){
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('panel-'+id).classList.add('active');
  el.classList.add('active');
  if(id==='daily'){setTimeout(()=>{encChart&&encChart.resize();revChart&&revChart.resize();},50);}
}
function setPill(el,grp){
  document.querySelectorAll('.filterbar .filter-group')[grp]
    .querySelectorAll('.pill').forEach(p=>p.classList.remove('active'));
  el.classList.add('active');
}
</script>
</body>
</html>
