<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1"/>
<title>Mones Telehealth Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.2/dist/chart.umd.min.js"></script>
<style>
:root{
  --bg:#0d1117;--surface:#161b27;--surface2:#1c2333;
  --border:#242d3f;--border2:#2d3a50;
  --rh:#2dd4a0;--td:#7b9ef0;--mdl:#f5a623;--rev:#f87171;
  --text:#e2e8f0;--text2:#94a3b8;--text3:#64748b;
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{font-size:14px}
body{background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;min-height:100vh;padding-bottom:48px;overflow-x:hidden}

.header{display:flex;align-items:center;justify-content:space-between;padding:14px 28px;border-bottom:1px solid var(--border);flex-wrap:wrap;gap:8px}
.header-title{font-family:'JetBrains Mono',monospace;font-size:1rem;font-weight:700;letter-spacing:-.01em;color:var(--text)}
.header-left .sub{font-family:'JetBrains Mono',monospace;font-size:.62rem;color:var(--text3);margin-top:4px;display:flex;align-items:center;gap:6px;flex-wrap:wrap}
.ldot{width:7px;height:7px;border-radius:2px;display:inline-block}
.header-right{display:flex;align-items:center;gap:8px}
.live-pill{display:flex;align-items:center;gap:6px;background:var(--surface2);border:1px solid #22c55e;border-radius:4px;padding:4px 10px;font-family:'JetBrains Mono',monospace;font-size:.65rem;color:#22c55e;white-space:nowrap;text-transform:uppercase;letter-spacing:.06em}
.live-dot{width:7px;height:7px;border-radius:50%;background:#ef4444;animation:blink 2s infinite;flex-shrink:0}
.live-dot.on{background:#22c55e}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.refresh-btn{background:var(--surface2);border:1px solid var(--border2);border-radius:4px;padding:4px 10px;color:var(--text3);font-size:.65rem;font-family:'JetBrains Mono',monospace;cursor:pointer;transition:.15s;white-space:nowrap;text-transform:uppercase;letter-spacing:.06em}
.refresh-btn:hover{border-color:#22c55e;color:#22c55e}

/* stale-data warning banner */
.stale-banner{display:none;background:rgba(245,166,35,.1);border-bottom:1px solid rgba(245,166,35,.3);padding:6px 28px;font-size:.68rem;color:#f5a623;font-family:'JetBrains Mono',monospace}
.stale-banner.show{display:block}

.dash{padding:22px 28px;display:flex;flex-direction:column;gap:30px;max-width:100%;overflow-x:hidden}
.sec-hd{display:flex;align-items:baseline;justify-content:space-between;margin-bottom:14px;flex-wrap:wrap;gap:6px}
.sec-label{font-size:.58rem;text-transform:uppercase;letter-spacing:.13em;color:var(--text3);font-weight:600}
.sec-note{font-size:.65rem;color:var(--text3)}

.rev-wrap{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px}
.rev-hd{display:flex;justify-content:space-between;align-items:flex-end;margin-bottom:16px}
.rev-hd-left{display:flex;flex-direction:column;gap:4px}
.rev-title{font-size:.58rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3);font-weight:600}
.rev-achieved{font-size:1.6rem;font-weight:800;letter-spacing:-.03em;line-height:1}
.rev-hd-right{display:flex;flex-direction:column;align-items:flex-end;gap:3px}
.rev-goal-label{font-size:.58rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3)}
.rev-goal-val{font-size:1rem;font-weight:700;color:var(--text2)}
.rev-pct-pill{font-size:.72rem;font-weight:700;border-radius:20px;padding:3px 10px;border:1px solid;white-space:nowrap}
.rev-track{position:relative;height:14px;background:var(--border);border-radius:8px;overflow:visible}
.rev-fill{height:100%;border-radius:8px;background:linear-gradient(90deg,#f87171,#f5a623,#22c55e);transition:width .9s ease}
.rev-ends{display:flex;justify-content:space-between;margin-top:7px;font-size:.6rem;color:var(--text3)}

.summary-strip{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.scard{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px;min-width:0}
.scard-label{font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:10px;font-weight:500}
.scard-val{font-size:2.4rem;font-weight:800;line-height:1;letter-spacing:-.03em}
.scard-sub{font-size:.72rem;color:var(--text3);margin-top:6px}

.chart-card{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:18px 20px;min-width:0;overflow:hidden}
.chart-card h3{font-size:.72rem;font-weight:600;color:var(--text);margin-bottom:12px}
.clegend{display:flex;gap:12px;flex-wrap:wrap;margin-top:10px}
.cleg{display:flex;align-items:center;gap:5px;font-size:.65rem;color:var(--text2)}
.cleg-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0}

.gs-row{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.gs-pill{display:flex;flex-direction:column;gap:12px;background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px;position:relative;overflow:hidden;min-width:0}
.gs-pill-top{display:flex;align-items:center;justify-content:space-between}
.gs-pill-name{font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);font-weight:500}
.gs-pill-pct{font-size:2rem;font-weight:800;line-height:1;letter-spacing:-.03em}
.gs-pill-track{background:var(--border);border-radius:4px;height:7px;overflow:hidden}
.gs-pill-fill{height:100%;border-radius:4px;transition:width .9s ease;width:0}
.gs-pill-val{font-size:.72rem;color:var(--text3)}

.two-charts{display:grid;grid-template-columns:1fr 1fr;gap:14px;min-width:0}

/* removed daily table styles */

/* heatmap cells */
.hm-cell{
  border-radius:6px;display:flex;flex-direction:column;
  align-items:center;justify-content:center;
  cursor:default;transition:transform .15s,box-shadow .15s;
  position:relative;padding:6px 4px;
  min-height:72px;
}
.hm-cell:hover{transform:scale(1.05);box-shadow:0 0 0 1.5px rgba(255,255,255,.25);z-index:2}
.hm-cell.today{box-shadow:0 0 0 2.5px #22c55e!important}
.hm-cell.empty{background:#111827!important;cursor:default}
/* Future days — clearly grayed, dashed border, no data */
.hm-cell.future{
  background:#0f172a!important;cursor:default;
  border:1px dashed rgba(255,255,255,.1)!important;
}
.hm-cell.future .hm-day{color:rgba(255,255,255,.2)!important}

/* standard cell content */
.hm-day{font-size:clamp(.7rem,1.1vw,.9rem);color:rgba(255,255,255,.45);font-family:'JetBrains Mono',monospace;line-height:1;font-weight:500}
.hm-enc{font-size:clamp(1rem,1.6vw,1.4rem);font-weight:800;line-height:1.1;margin-top:3px}
.hm-cell.today .hm-day{color:#22c55e}

/* TARGET MET cell */
.hm-cell.target-met{border-radius:8px}
.hm-cell.target-met .hm-day{color:rgba(0,0,0,.5)!important}
.hm-cell.target-met .hm-enc{font-size:clamp(1rem,1.6vw,1.4rem);font-weight:900;color:#000}
.hm-cell.target-met .hm-check{font-size:clamp(.65rem,1vw,.82rem);font-weight:800;color:rgba(0,0,0,.72);margin-top:2px;line-height:1.2}
.hm-cell.target-met .hm-rev{font-size:clamp(.58rem,.9vw,.72rem);color:rgba(0,0,0,.58);margin-top:2px;font-family:'JetBrains Mono',monospace}

/* tooltip */
.hm-cell .hm-tip{
  display:none;position:absolute;bottom:calc(100% + 8px);left:50%;transform:translateX(-50%);
  background:#1c2333;border:1px solid #2d3a50;border-radius:8px;padding:8px 12px;
  font-size:.68rem;color:var(--text);white-space:nowrap;z-index:20;pointer-events:none;
  line-height:1.7;box-shadow:0 8px 24px rgba(0,0,0,.5);
}
.hm-cell:hover .hm-tip{display:block}
.hm-cell:hover .hm-tooltip{display:block}

.footer{display:flex;justify-content:space-between;flex-wrap:wrap;gap:6px;padding:10px 28px;font-size:.64rem;color:var(--text3)}

@media(max-width:900px){
  .two-charts{grid-template-columns:1fr}
  .summary-strip{grid-template-columns:1fr 1fr}
  .gs-row{grid-template-columns:repeat(3,1fr)}
  .dash{padding:16px 20px;gap:22px}
}
@media(max-width:768px){
  .header{padding:10px 16px}
  .dash{padding:14px 16px;gap:18px}
  .summary-strip{grid-template-columns:1fr 1fr}
  .scard{padding:14px 16px}
  .scard-val{font-size:2rem}
  .gs-row{grid-template-columns:repeat(3,1fr)}
  .gs-pill{padding:14px 16px}
  .gs-pill-pct{font-size:1.6rem}
}
@media(max-width:480px){
  html{font-size:13px}
  .header{padding:8px 12px}
  .dash{padding:10px 12px;gap:14px}
  .summary-strip{grid-template-columns:1fr}
  .scard{padding:12px 14px}
  .scard-val{font-size:1.9rem}
  .gs-row{grid-template-columns:1fr}
  .gs-pill{padding:12px 14px;gap:9px}
  .gs-pill-pct{font-size:1.5rem}
  .two-charts{grid-template-columns:1fr}
  .chart-card{padding:12px}
  .stale-banner{padding:6px 12px}
}
</style>
</head>
<body>

<!-- Stale data warning — shown when latest data is not from today -->
<div class="stale-banner" id="stale-banner">
  ⚠ Today's data not yet in sheet — showing most recent available day. Sheet updates after encounters are logged.
</div>

<div class="header">
  <div class="header-left">
    <div class="header-title">Mones Telehealth — Revenue Dashboard</div>
    <div class="sub">
      <span id="date-range">—</span>&nbsp;·&nbsp;
      <span class="ldot" style="background:var(--rh)"></span>Roman Health&nbsp;·&nbsp;
      <span class="ldot" style="background:var(--td)"></span>Teladoc&nbsp;·&nbsp;
      <span class="ldot" style="background:var(--mdl)"></span>MDLive
    </div>
  </div>
  <div class="header-right">
    <div class="live-pill"><span class="live-dot" id="live-dot"></span><span id="live-label">Connecting…</span></div>
    <button class="refresh-btn" onclick="manualRefresh(this)">↻ <span>Refresh</span></button>
  </div>
</div>

<div class="dash">

  <!-- SUMMARY STRIP -->
  <div>
    <div class="sec-hd"><span class="sec-label">Rolling 30 Days — At a Glance</span></div>
    <div class="summary-strip">
      <div class="scard">
        <div class="scard-label">Avg Daily · Roman Health</div>
        <div class="scard-val" id="s-avg">—</div>
        <div class="scard-sub" id="s-avg-prev" style="margin-top:4px">—</div>
        <div class="scard-sub" id="s-avg-sub" style="margin-top:2px">encounters / day</div>
      </div>
      <div class="scard">
        <div class="scard-label">Total Encounters · 30 Days</div>
        <div class="scard-val" id="s-enc">—</div>
        <div class="scard-sub" id="s-enc-prev" style="margin-top:4px">—</div>
        <div class="scard-sub" id="s-enc-sub" style="margin-top:2px">all platforms</div>
      </div>
      <div class="scard">
        <div class="scard-label">Avg Daily Revenue</div>
        <div class="scard-val" id="s-rev">—</div>
        <div class="scard-sub" id="s-rev-prev" style="margin-top:4px">—</div>
        <div class="scard-sub" id="s-rev-sub" style="margin-top:2px">last 30 days</div>
      </div>
    </div>
  </div>

  <!-- TODAY VS TARGET -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Most Recent Day vs Target</span>
      <span class="sec-note" id="gs-days-note">—</span>
    </div>
    <div class="gs-row">
      <div class="gs-pill">
        <div class="gs-pill-top">
          <span class="gs-pill-name">Roman Health</span>
          <span class="gs-pill-pct" id="gs-rh-pct" style="color:var(--rh)">—</span>
        </div>
        <div class="gs-pill-track"><div class="gs-pill-fill" id="gs-rh-fill" style="background:var(--rh)"></div></div>
        <span class="gs-pill-val" id="gs-rh-val">—</span>
      </div>
      <div class="gs-pill">
        <div class="gs-pill-top">
          <span class="gs-pill-name">Teladoc</span>
          <span class="gs-pill-pct" id="gs-td-pct" style="color:var(--td)">—</span>
        </div>
        <div class="gs-pill-track"><div class="gs-pill-fill" id="gs-td-fill" style="background:var(--td)"></div></div>
        <span class="gs-pill-val" id="gs-td-val">—</span>
      </div>
      <div class="gs-pill">
        <div class="gs-pill-top">
          <span class="gs-pill-name">MDLive</span>
          <span class="gs-pill-pct" id="gs-mdl-pct" style="color:var(--mdl)">—</span>
        </div>
        <div class="gs-pill-track"><div class="gs-pill-fill" id="gs-mdl-fill" style="background:var(--mdl)"></div></div>
        <span class="gs-pill-val" id="gs-mdl-val">—</span>
      </div>
    </div>
  </div>

  <!-- DAILY REVENUE BAR -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Most Recent Day Revenue</span>
      <span class="sec-note" id="today-line">—</span>
    </div>
    <div class="rev-wrap">
      <div class="rev-hd">
        <div class="rev-hd-left">
          <span class="rev-title">Daily Revenue</span>
          <span class="rev-achieved" id="t-rev-amt" style="color:var(--rev)">$0</span>
        </div>
        <div class="rev-hd-right">
          <span class="rev-goal-label">Daily Goal</span>
          <span class="rev-goal-val">$2,000</span>
          <span class="rev-pct-pill" id="t-rev-pct" style="color:var(--rev);border-color:var(--rev)">0%</span>
        </div>
      </div>
      <div class="rev-track">
        <div class="rev-fill" id="t-rev-fill" style="width:0%"></div>
      </div>
      <div class="rev-ends"><span>$0</span><span>$2,000</span></div>
    </div>
  </div>

  <!-- CHARTS ROW -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Charts</span>
      <span class="sec-note">RH $12 · TD phone $23 / video $28 · MDL phone $25 / video $28 / async $12.50</span>
    </div>
    <div class="two-charts">
      <div class="chart-card">
        <h3>Daily Revenue — Last 30 Days</h3>
        <div style="position:relative;height:260px"><canvas id="revChart"></canvas></div>
        <div class="clegend">
          <div class="cleg"><div class="cleg-dot" style="background:#2dd4a0"></div>Roman Health</div>
          <div class="cleg"><div class="cleg-dot" style="background:#7b9ef0"></div>Teladoc</div>
          <div class="cleg"><div class="cleg-dot" style="background:#f5a623"></div>MDLive</div>
          <div class="cleg"><div style="width:18px;height:2px;background:#fbbf24;border-radius:2px;border-top:2px dashed #fbbf24;margin-top:3px"></div>&nbsp;7-Day Avg</div>
        </div>
      </div>
      <div class="chart-card">
        <h3>Monthly Revenue — Jan 2025+ <span id="proj-note" style="font-weight:400;color:var(--text3)"></span></h3>
        <div style="position:relative;height:260px"><canvas id="histChart"></canvas></div>
        <div class="clegend">
          <div class="cleg"><div class="cleg-dot" style="background:#2dd4a0"></div>Roman Health</div>
          <div class="cleg"><div class="cleg-dot" style="background:#7b9ef0"></div>Teladoc</div>
          <div class="cleg"><div class="cleg-dot" style="background:#f5a623"></div>MDLive</div>
          <div class="cleg" id="proj-legend" style="display:none"><div class="cleg-dot" style="background:rgba(255,255,255,.2);border:1px solid rgba(255,255,255,.3)"></div>Projected</div>
        </div>
      </div>
    </div>
  </div>

  <!-- 30-DAY ROLLING REVENUE GOAL -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Rolling 30-Day Revenue vs Goal</span>
      <span class="sec-note" id="goals-note">last 30 days</span>
    </div>
    <div class="rev-wrap">
      <div class="rev-hd">
        <div class="rev-hd-left">
          <span class="rev-title">Rolling 30-Day Revenue</span>
          <span class="rev-achieved" id="g-rev-amt" style="color:var(--rev)">$0</span>
        </div>
        <div class="rev-hd-right">
          <span class="rev-goal-label">30-Day Goal</span>
          <span class="rev-goal-val">$60,000</span>
          <span class="rev-pct-pill" id="g-rev-pct" style="color:var(--rev);border-color:var(--rev)">0%</span>
        </div>
      </div>
      <div class="rev-track">
        <div class="rev-fill" id="g-rev-fill" style="width:0%"></div>
      </div>
      <div class="rev-ends"><span>$0</span><span>$60,000</span></div>
    </div>
  </div>

  <!-- CALENDAR HEATMAP — full width -->
  <div>
    <div class="sec-hd">
      <span class="sec-label" id="heatmap-label">Encounter Heatmap — Current Month</span>
      <span class="sec-note">Target met (RH ≥ 167 · $2,000+/day) shown in green with full detail</span>
    </div>
    <div class="chart-card">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;flex-wrap:wrap;gap:8px">
        <h3 id="heatmap-title" style="margin:0">—</h3>
        <!-- color legend -->
        <div style="display:flex;align-items:center;gap:8px;font-size:.62rem;color:var(--text3)">
          <span>Below target →</span>
          <div style="display:flex;gap:3px;align-items:center">
            <div title="Very low" style="width:16px;height:16px;border-radius:3px;background:#450a0a;border:1px solid rgba(255,255,255,.06)"></div>
            <div title="Low" style="width:16px;height:16px;border-radius:3px;background:#991b1b;border:1px solid rgba(255,255,255,.06)"></div>
            <div title="Mid" style="width:16px;height:16px;border-radius:3px;background:#c2410c;border:1px solid rgba(255,255,255,.06)"></div>
            <div title="Getting close" style="width:16px;height:16px;border-radius:3px;background:#ea580c;border:1px solid rgba(255,255,255,.06)"></div>
            <div title="Amber" style="width:16px;height:16px;border-radius:3px;background:#d97706;border:1px solid rgba(255,255,255,.06)"></div>
            <div title="Near target" style="width:16px;height:16px;border-radius:3px;background:#a16207;border:1px solid rgba(255,255,255,.06)"></div>
            <div style="width:1px;height:16px;background:rgba(255,255,255,.15);margin:0 2px"></div>
            <div title="Target met (RH ≥ 167)" style="width:16px;height:16px;border-radius:3px;background:#22c55e;border:1px solid rgba(255,255,255,.06)"></div>
          </div>
          <span>✓ Target</span>
        </div>
      </div>

      <!-- Day-of-week headers -->
      <div style="display:grid;grid-template-columns:repeat(7,1fr);gap:5px;margin-bottom:5px">
        <div style="text-align:center;font-size:clamp(.65rem,1vw,.82rem);color:var(--text3);padding:4px 0;font-weight:600;letter-spacing:.04em">Sun</div>
        <div style="text-align:center;font-size:clamp(.65rem,1vw,.82rem);color:var(--text3);padding:4px 0;font-weight:600;letter-spacing:.04em">Mon</div>
        <div style="text-align:center;font-size:clamp(.65rem,1vw,.82rem);color:var(--text3);padding:4px 0;font-weight:600;letter-spacing:.04em">Tue</div>
        <div style="text-align:center;font-size:clamp(.65rem,1vw,.82rem);color:var(--text3);padding:4px 0;font-weight:600;letter-spacing:.04em">Wed</div>
        <div style="text-align:center;font-size:clamp(.65rem,1vw,.82rem);color:var(--text3);padding:4px 0;font-weight:600;letter-spacing:.04em">Thu</div>
        <div style="text-align:center;font-size:clamp(.65rem,1vw,.82rem);color:var(--text3);padding:4px 0;font-weight:600;letter-spacing:.04em">Fri</div>
        <div style="text-align:center;font-size:clamp(.65rem,1vw,.82rem);color:var(--text3);padding:4px 0;font-weight:600;letter-spacing:.04em">Sat</div>
      </div>

      <!-- Grid cells -->
      <div id="heatmap-grid" style="display:grid;grid-template-columns:repeat(7,1fr);gap:5px"></div>
    </div>
  </div>



</div>

<div class="footer">
  <span>Last refreshed: <span id="last-updated">—</span></span>
  <span id="data-as-of">—</span>
  <span>Auto-refreshes every 60 s</span>
</div>

<script>
// ── CONFIG ────────────────────────────────────────────────────────────────────
// FIX: Use your Google Apps Script URL here. If using plain CSV, set CSV_URL instead.
const APPS_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbwVT-Xukwe3rxiPAnaMGK8ywEVf0Tfnm9U6cTQo9Ts6DX3DOoGQNcgvHj2pnCAO0_N3/exec";

// Fallback plain CSV (single-sheet published URL — only loads current sheet)
const CSV_FALLBACK_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vS0D1ReEdPb5-BI6rQUyZj5tEP8wrjWUc09JAZs7yJF8fQnDak7ChKmh3TIWIY92Q/pub?output=csv";

// ── RATES & GOALS ─────────────────────────────────────────────────────────────
const RATES = { rh:12, td_phone:23, td_video:28, mdl_phone:25, mdl_video:28, mdl_async:12.50 };
const GOALS = { rh:167, td:5, mdl:15, dayRev:2000, moRev:60000 };

let ALL_DATA = {};
let revChart = null, histChart = null;

// ── CSV PARSING ───────────────────────────────────────────────────────────────
function parseCSVRow(r) {
  const cols = []; let cur = '', inQ = false;
  for (const ch of r) {
    if (ch === '"') { inQ = !inQ; continue; }
    if (ch === ',' && !inQ) { cols.push(cur.trim()); cur = ''; }
    else cur += ch;
  }
  cols.push(cur.trim());
  return cols;
}

function parseDateCol(h) {
  let m;
  // MM/DD or MM/DD/YYYY
  m = h.match(/^(\d{1,2})\/(\d{1,2})(?:\/(\d{4}))?$/);
  if (m) return { month:+m[1], day:+m[2], year: m[3] ? +m[3] : null };
  // DD-Mon (e.g. 22-Mar)
  m = h.match(/^(\d{1,2})-([A-Za-z]{3})$/i);
  if (m) { const mo = monthNum(m[2]); if (mo) return { month:mo, day:+m[1], year:null }; }
  // Mon-DD (e.g. Mar-22)
  m = h.match(/^([A-Za-z]{3})-(\d{1,2})$/i);
  if (m) { const mo = monthNum(m[1]); if (mo) return { month:mo, day:+m[2], year:null }; }
  // MM-DD-YYYY
  m = h.match(/^(\d{1,2})-(\d{1,2})-(\d{4})$/);
  if (m) return { month:+m[1], day:+m[2], year:+m[3] };
  // "Mar 22 2026" style
  m = h.match(/([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})/);
  if (m) { const mo = monthNum(m[1]); if (mo) return { month:mo, day:+m[2], year:+m[3] }; }
  return null;
}

function monthNum(s) {
  return { jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12 }[s.toLowerCase().slice(0,3)];
}

function monthYearFromTabName(n) {
  const m = n.trim().match(/^([A-Za-z]{3})[\s_-]*(\d{2,4})$/i);
  if (m) {
    const mo = monthNum(m[1]);
    if (mo) { let yr = +m[2]; if (yr < 100) yr += yr < 50 ? 2000 : 1900; return { month:mo, year:yr }; }
  }
  return null;
}

function yearFromTabName(n) {
  const m = n.trim().match(/(\d{4})/);
  if (m) return +m[1];
  const m2 = n.trim().match(/(\d{2})\s*$/);
  if (m2) { const y = +m2[1]; return y < 50 ? 2000+y : 1900+y; }
  return null;
}

function parseSheetCSV(text, tabName) {
  const rows = text.trim().split('\n').map(parseCSVRow);
  if (rows.length < 2) return null;

  const headers = rows[0];
  const nameDate = tabName ? monthYearFromTabName(tabName) : null;

  // Find date columns
  const allDC = [];
  headers.forEach((h, i) => {
    if (i === 0) return;
    const d = parseDateCol(h.trim());
    if (!d) return;
    if (nameDate) { if (d.month === nameDate.month) allDC.push({ i, ...d }); }
    else allDC.push({ i, ...d });
  });

  const dateCols = allDC.length > 0 ? allDC : (() => {
    const all = [];
    headers.forEach((h, i) => {
      if (i === 0) return;
      const d = parseDateCol(h.trim()); if (d) all.push({ i, ...d });
    });
    return all;
  })();

  if (!dateCols.length) return null;

  // Assign year to each date column
  // FIX: carry year from tab name or infer from surrounding dates to handle Dec/Jan boundary
  const sheetYear = nameDate ? nameDate.year : (yearFromTabName(tabName || '') || new Date().getFullYear());
  dateCols.forEach(dc => { if (!dc.year) dc.year = sheetYear; });
  dateCols.sort((a, b) => a.day - b.day);

  const n = dateCols.length;
  const rh  = new Array(n).fill(0), rev = new Array(n).fill(0);

  // Name-based rate detection — works regardless of row order
  // Accumulate separate buckets per visit type
  const td_phone = new Array(n).fill(0);
  const td_video = new Array(n).fill(0);
  const mdl_phone= new Array(n).fill(0);
  const mdl_video= new Array(n).fill(0);
  const mdl_async= new Array(n).fill(0);

  for (const row of rows.slice(1)) {
    const rawLabel = (row[0] || '').trim().toUpperCase().replace(/\s+/g, '').replace(/[^A-Z0-9+]/g, '');
    if (!rawLabel) continue;

    const isRO    = rawLabel === 'RO' || rawLabel === 'RO+';
    const isTot   = rawLabel === 'TOT' || rawLabel === 'TOTAL';
    const isTD    = rawLabel.includes('TD') || rawLabel.includes('TELADOC');
    const isMDL   = !isTD && (rawLabel === 'MDL' || rawLabel === 'MD' || rawLabel.includes('MDL') || rawLabel.includes('MDLIVE'));

    const isPhone = rawLabel.includes('PHONE');
    const isVideo = rawLabel.includes('VIDEO');
    const isAsync = rawLabel.includes('ASYNC');

    if (!isRO && !isTot && !isTD && !isMDL) continue;

    dateCols.forEach((col, j) => {
      const cellRaw = (row[col.i] || '').toString().replace(/#[A-Z!]+/g, '').replace(/[$, ]/g, '');
      const v = parseFloat(cellRaw) || 0;
      if (!v) return;

      if (isRO)  { rh[j]  += v; return; }
      if (isTot) { rev[j] += v; return; }

      if (isTD) {
        if (isVideo)        td_video[j] += v;
        else                td_phone[j] += v;  // phone or unspecified TD
      }
      if (isMDL) {
        if (isAsync)        mdl_async[j]+= v;
        else if (isVideo)   mdl_video[j]+= v;
        else                mdl_phone[j]+= v;  // phone or unspecified MDL
      }
    });
  }

  // Combine for total encounter counts
  // aliases for backward compat with return value and histChart
  const td1 = td_phone, td2 = td_video;
  const mdl1= mdl_phone, mdl2 = mdl_video;
  const td  = td_phone.map((v, i) => v + td_video[i]);
  const mdl = mdl_phone.map((v, i) => v + mdl_video[i] + mdl_async[i]);
  const labels = dateCols.map(d => d.month + '/' + d.day);

  // Find last day with any data
  let lastIdx = -1;
  for (let j = 0; j < n; j++) if (rh[j] > 0 || td[j] > 0 || mdl[j] > 0) lastIdx = j;
  if (lastIdx < 0) return null;
  const t = lastIdx + 1;

  const sm = nameDate ? nameDate.month : dateCols[0].month;
  const sy = sheetYear;

  return {
    labels:  labels.slice(0, t),
    rh:   rh.slice(0, t),
    td:   td.slice(0, t),
    td1:  td1.slice(0, t),  td2:  td2.slice(0, t),
    mdl:  mdl.slice(0, t),  mdl1: mdl1.slice(0, t), mdl2: mdl2.slice(0, t),
    rev:  rev.slice(0, t),
    // Pre-calculated revenue arrays — name-based rates
    rhRev:  rh.slice(0,t).map(v => Math.round(v * RATES.rh)),
    tdRev:  td_phone.slice(0,t).map((v,i) => Math.round(v * RATES.td_phone + td_video[i] * RATES.td_video)),
    mdlRev: mdl_phone.slice(0,t).map((v,i) => Math.round(
      v * RATES.mdl_phone +
      mdl_video[i] * RATES.mdl_video +
      mdl_async[i] * RATES.mdl_async
    )),
    // Date info
    month: sm,
    year:  sy,
    dateCols: dateCols.slice(0, t),  // keep for accurate date comparisons
  };
}

// ── DATA HELPERS ──────────────────────────────────────────────────────────────

// FIX: use dateCols for accurate per-day dates instead of inferring from sheet-level year
function getDayDate(sheet, idx) {
  if (sheet.dateCols && sheet.dateCols[idx]) {
    const dc = sheet.dateCols[idx];
    return new Date(dc.year || sheet.year, dc.month - 1, dc.day);
  }
  const p = (sheet.labels[idx] || '').split('/');
  return new Date(sheet.year, +p[0] - 1, +p[1]);
}

function getRollingN(n, excludeToday) {
  const now = new Date();
  const todayStr = `${now.getFullYear()}-${now.getMonth()}-${now.getDate()}`;
  const cutoff = new Date(); cutoff.setDate(cutoff.getDate() - n); cutoff.setHours(0, 0, 0, 0);
  const all = [];
  Object.values(ALL_DATA).forEach(d => {
    d.labels.forEach((label, i) => {
      const date = getDayDate(d, i);
      if (date < cutoff) return;
      const dateStr = `${date.getFullYear()}-${date.getMonth()}-${date.getDate()}`;
      if (excludeToday && dateStr === todayStr) return;
      all.push({
        date, label,
        rh: d.rh[i]||0, td: d.td[i]||0, mdl: d.mdl[i]||0,
        rhRev: (d.rhRev||[])[i]||0, tdRev: (d.tdRev||[])[i]||0, mdlRev: (d.mdlRev||[])[i]||0,
      });
    });
  });
  all.sort((a, b) => a.date - b.date);
  if (all.length < 5) {
    const fallback = [];
    Object.values(ALL_DATA).forEach(d => {
      d.labels.forEach((label, i) => {
        fallback.push({
          date: getDayDate(d, i), label,
          rh: d.rh[i]||0, td: d.td[i]||0, mdl: d.mdl[i]||0,
          rhRev: (d.rhRev||[])[i]||0, tdRev: (d.tdRev||[])[i]||0, mdlRev: (d.mdlRev||[])[i]||0,
        });
      });
    });
    fallback.sort((a, b) => a.date - b.date);
    return fallback.slice(-n);
  }
  return all;
}

function getRolling30(excludeToday) {
  const now = new Date();
  const todayStr = `${now.getFullYear()}-${now.getMonth()}-${now.getDate()}`;
  const cutoff = new Date(); cutoff.setDate(cutoff.getDate() - 30); cutoff.setHours(0, 0, 0, 0);

  const all = [];
  Object.values(ALL_DATA).forEach(d => {
    d.labels.forEach((label, i) => {
      const date = getDayDate(d, i);
      if (date < cutoff) return;
      const dateStr = `${date.getFullYear()}-${date.getMonth()}-${date.getDate()}`;
      if (excludeToday && dateStr === todayStr) return;
      all.push({
        date, label,
        rh: d.rh[i]||0, td: d.td[i]||0, mdl: d.mdl[i]||0,
        rhRev: (d.rhRev||[])[i]||0, tdRev: (d.tdRev||[])[i]||0, mdlRev: (d.mdlRev||[])[i]||0,
      });
    });
  });

  all.sort((a, b) => a.date - b.date);
  // fallback: if fewer than 5 days within window, just use last 30 rows
  if (all.length < 5) {
    const fallback = [];
    Object.values(ALL_DATA).forEach(d => {
      d.labels.forEach((label, i) => {
        fallback.push({
          date: getDayDate(d, i), label,
          rh: d.rh[i]||0, td: d.td[i]||0, mdl: d.mdl[i]||0,
          rhRev: (d.rhRev||[])[i]||0, tdRev: (d.tdRev||[])[i]||0, mdlRev: (d.mdlRev||[])[i]||0,
        });
      });
    });
    fallback.sort((a, b) => a.date - b.date);
    return fallback.slice(-30);
  }
  return all;
}

function getPrev30() {
  const cutoff    = new Date(); cutoff.setDate(cutoff.getDate()-30);    cutoff.setHours(0,0,0,0);
  const prevCut   = new Date(); prevCut.setDate(prevCut.getDate()-60);  prevCut.setHours(0,0,0,0);
  const all = [];
  Object.values(ALL_DATA).forEach(d => {
    d.labels.forEach((label, i) => {
      const date = getDayDate(d, i);
      if (date >= prevCut && date < cutoff)
        all.push({ date, rh: d.rh[i]||0, td: d.td[i]||0, mdl: d.mdl[i]||0,
          rhRev:(d.rhRev||[])[i]||0, tdRev:(d.tdRev||[])[i]||0, mdlRev:(d.mdlRev||[])[i]||0 });
    });
  });
  return all;
}

// FIX: getLatestDay now returns the most recent calendar day AND flags if it's actually today
function getLatestDayInfo() {
  const sheets = Object.values(ALL_DATA);
  if (!sheets.length) return null;
  sheets.sort((a, b) => a.year !== b.year ? a.year - b.year : a.month - b.month);
  const cur = sheets[sheets.length - 1];
  const i = cur.labels.length - 1;
  const date = getDayDate(cur, i);
  const now = new Date();
  const isToday = date.getFullYear() === now.getFullYear() &&
                  date.getMonth()    === now.getMonth()    &&
                  date.getDate()     === now.getDate();
  return {
    rh: cur.rh[i]||0, td: cur.td[i]||0, mdl: cur.mdl[i]||0,
    rhRev:(cur.rhRev||[])[i]||0, tdRev:(cur.tdRev||[])[i]||0, mdlRev:(cur.mdlRev||[])[i]||0,
    date, isToday, label: cur.labels[i],
  };
}

// FIX: correct hour12:false — use numeric 24h comparison for 3 PM PST cutoff
function isBeforeThreePST() {
  try {
    const h = +new Date().toLocaleString('en-US', { timeZone:'America/Los_Angeles', hour:'2-digit', hour12:false });
    return h < 15;
  } catch(e) { return false; }
}

// ── DOM HELPERS ───────────────────────────────────────────────────────────────
function set(id, v) { const el = document.getElementById(id); if (el) el.textContent = v; }
function st(id, p, v) { const el = document.getElementById(id); if (el) el.style[p] = v; }

// ── CHART CONFIG ──────────────────────────────────────────────────────────────
const GRID = '#1e2a3a';
Chart.defaults.color = '#64748b';
Chart.defaults.font.family = "'DM Sans',sans-serif";
Chart.defaults.font.size = 11;
const TT = { backgroundColor:'#1c2333', borderColor:'#2d3a50', borderWidth:1, titleColor:'#94a3b8', bodyColor:'#e2e8f0', padding:10 };

// ── UPDATERS ──────────────────────────────────────────────────────────────────
function updateToday() {
  const info = getLatestDayInfo();
  if (!info) return;

  // Show stale banner if latest day is not today
  const banner = document.getElementById('stale-banner');
  if (banner) banner.classList.toggle('show', !info.isToday);

  // Label: show actual date of latest data
  const opts = { weekday:'short', month:'short', day:'numeric' };
  const dateStr = info.isToday
    ? 'Today · ' + info.date.toLocaleDateString('en-US', opts)
    : 'Latest: ' + info.date.toLocaleDateString('en-US', opts);
  set('today-line', dateStr);
  set('gs-days-note', dateStr);
  set('data-as-of', 'Data as of: ' + info.date.toLocaleDateString('en-US', { month:'short', day:'numeric', year:'numeric' }));

  const dayRev = (info.rhRev||0) + (info.tdRev||0) + (info.mdlRev||0);
  const rp = Math.min(Math.round(dayRev / GOALS.dayRev * 100), 100);
  const rc = rp >= 100 ? '#22c55e' : rp >= 75 ? '#f5a623' : '#f87171';
  st('t-rev-fill', 'width', Math.min(rp, 100) + '%');
  const tAmt = document.getElementById('t-rev-amt');
  const tPct = document.getElementById('t-rev-pct');
  if (tAmt) { tAmt.textContent = '$' + Math.round(dayRev).toLocaleString(); tAmt.style.color = rc; }
  if (tPct) { tPct.textContent = rp + '%'; tPct.style.color = rc; tPct.style.borderColor = rc;
    tPct.style.background = rp >= 100 ? 'rgba(34,197,94,.12)' : rp >= 75 ? 'rgba(245,166,35,.1)' : 'rgba(248,113,113,.1)'; }

  // Today vs target pills
  function fill(k, val, target, color) {
    const pct = Math.round(val / target * 100);
    const c = pct >= 100 ? '#22c55e' : pct >= 75 ? '#f5a623' : '#f87171';
    set('gs-'+k+'-val', val + ' encounters / target: ' + target);
    set('gs-'+k+'-pct', pct + '%');
    const f = document.getElementById('gs-'+k+'-fill');
    if (f) { f.style.width = Math.min(pct, 100) + '%'; f.style.background = c; }
    const pp = document.getElementById('gs-'+k+'-pct');
    if (pp) pp.style.color = c;
  }
  fill('rh',  info.rh  || 0, GOALS.rh);
  fill('td',  info.td  || 0, GOALS.td);
  fill('mdl', info.mdl || 0, GOALS.mdl);
}

function updateGoals() {
  const days = getRolling30(true);
  if (!days.length) return;
  const tRev = days.reduce((a, d) => a + d.rhRev + d.tdRev + d.mdlRev, 0);
  set('goals-note', 'last ' + days.length + ' days (excl. today)');
  const mp = Math.min(Math.round(tRev / GOALS.moRev * 100), 100);
  const mc = mp >= 100 ? '#22c55e' : mp >= 75 ? '#f5a623' : '#f87171';
  st('g-rev-fill', 'width', Math.min(mp, 100) + '%');
  const gAmt = document.getElementById('g-rev-amt');
  const gPct = document.getElementById('g-rev-pct');
  if (gAmt) { gAmt.textContent = '$' + Math.round(tRev).toLocaleString(); gAmt.style.color = mc; }
  if (gPct) { gPct.textContent = mp + '%'; gPct.style.color = mc; gPct.style.borderColor = mc;
    gPct.style.background = mp >= 100 ? 'rgba(34,197,94,.12)' : mp >= 75 ? 'rgba(245,166,35,.1)' : 'rgba(248,113,113,.1)'; }
}

function updateSummary() {
  const days = getRolling30(true);
  if (!days.length) return;
  const prev = getPrev30();
  const tRH  = days.reduce((a,d) => a+d.rh,  0);
  const tTD  = days.reduce((a,d) => a+d.td,  0);
  const tMDL = days.reduce((a,d) => a+d.mdl, 0);
  const tEnc = tRH + tTD + tMDL;
  const tRev = days.reduce((a,d) => a+d.rhRev+d.tdRev+d.mdlRev, 0);
  const n    = days.length || 1;
  const pRH  = prev.reduce((a,d) => a+d.rh,  0);
  const pEnc = prev.reduce((a,d) => a+d.rh+d.td+d.mdl, 0);
  const pRev = prev.reduce((a,d) => a+d.rhRev+d.tdRev+d.mdlRev, 0);
  const pN   = prev.length || 1;

  function renderStat(elId, prevElId, subElId, val, prevVal, fmt, subText) {
    const up   = val > prevVal, same = val === prevVal;
    const col  = up ? '#22c55e' : same ? 'var(--text2)' : '#f87171';
    const arr  = up ? '↑' : same ? '→' : '↓';
    const diff = Math.abs(val - prevVal);
    const el = document.getElementById(elId);
    if (el) { el.textContent = fmt(val); el.style.color = col; }
    set(prevElId, arr + ' ' + fmt(prevVal) + ' prev · ' + arr + ' ' + fmt(diff) + (up?' more':same?'':' less'));
    const pe = document.getElementById(prevElId); if (pe) pe.style.color = col;
    if (subElId && subText) set(subElId, subText);
  }

  const fNum = v => Math.round(v).toLocaleString();
  const fDol = v => '$' + Math.round(v).toLocaleString();

  renderStat('s-avg',  's-avg-prev',  's-avg-sub',  Math.round(tRH/n),  Math.round(pRH/pN),  fNum, 'encounters / day (Roman Health)');
  renderStat('s-enc',  's-enc-prev',  null,          tEnc,               pEnc,                fNum);
  set('s-enc-sub', 'RH ' + tRH.toLocaleString() + ' · TD ' + tTD + ' · MDL ' + tMDL);
  renderStat('s-rev',  's-rev-prev',  's-rev-sub',  Math.round(tRev/n), Math.round(pRev/pN), fDol, 'avg daily revenue');
}

function updateCharts() {
  const days = getRolling30(false);
  if (!days.length) return;
  const early = isBeforeThreePST();
  const lastI = days.length - 1;
  const nullLast = (v, i) => (i === lastI && early) ? null : v;
  const labels = days.map(d => d.label);

  if (revChart) revChart.destroy();
  // Fetch 37 days so we have 7 days of lookback for the full 30-day MA
  const extDays = getRollingN(37, false);
  const extRev  = extDays.map(d => d.rhRev + d.tdRev + d.mdlRev);

  // Compute MA7 across all 37 days, then slice to align with the 30-day display
  const ma7full = extRev.map((_, i) => {
    if (i < 6) return null;
    const slice = extRev.slice(i - 6, i + 1);
    return Math.round(slice.reduce((a, b) => a + b, 0) / 7);
  });
  // Align: take last `days.length` entries from ma7full
  const ma7raw = ma7full.slice(ma7full.length - days.length);
  const ma7 = ma7raw.map((v, i) => (i === lastI && early) ? null : v);

  revChart = new Chart(document.getElementById('revChart').getContext('2d'), {
    type: 'line',
    data: {
      labels,
      datasets: [
        {
          label: 'Roman Health',
          data: days.map((d,i) => nullLast(d.rhRev, i)),
          borderColor: '#2dd4a0',
          backgroundColor: 'rgba(45,212,160,.12)',
          fill: true,
          tension: 0.4,
          borderWidth: 2.5,
          pointRadius: 0,
          pointHoverRadius: 5,
          pointHoverBackgroundColor: '#2dd4a0',
          spanGaps: false,
          order: 2,
        },
        {
          label: 'Teladoc',
          data: days.map((d,i) => nullLast(d.tdRev, i)),
          borderColor: '#7b9ef0',
          backgroundColor: 'transparent',
          fill: false,
          tension: 0.4,
          borderWidth: 2,
          pointRadius: 0,
          pointHoverRadius: 4,
          pointHoverBackgroundColor: '#7b9ef0',
          spanGaps: false,
          order: 2,
        },
        {
          label: 'MDLive',
          data: days.map((d,i) => nullLast(d.mdlRev, i)),
          borderColor: '#f5a623',
          backgroundColor: 'transparent',
          fill: false,
          tension: 0.4,
          borderWidth: 2,
          pointRadius: 0,
          pointHoverRadius: 4,
          pointHoverBackgroundColor: '#f5a623',
          spanGaps: false,
          order: 2,
        },
        {
          label: '7-Day Avg',
          data: ma7,
          borderColor: '#fbbf24',
          backgroundColor: 'transparent',
          fill: false,
          borderWidth: 2.5,
          borderDash: [6, 3],
          pointRadius: 0,
          pointHoverRadius: 5,
          pointHoverBackgroundColor: '#fbbf24',
          tension: 0.4,
          spanGaps: false,
          order: 1,
        },
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      interaction: { mode:'index', intersect:false },
      plugins: {
        legend: { display:false },
        tooltip: { ...TT, callbacks: {
          label: i => {
            if (!i.raw && i.raw !== 0) return null;
            if (i.dataset.label === '7-Day Avg')
              return ' 7-Day Avg: $' + i.raw.toLocaleString();
            return ' ' + i.dataset.label + ': $' + i.raw.toLocaleString();
          },
          footer: items => {
            const tot = items
              .filter(i => i.dataset.label !== '7-Day Avg')
              .reduce((a,i) => a + (i.raw||0), 0);
            return tot ? 'Day Total: $' + tot.toLocaleString() : '';
          }
        }}
      },
      scales: {
        x: { grid:{color:GRID}, ticks:{maxRotation:45, autoSkip:true, maxTicksLimit:10} },
        y: { grid:{color:GRID}, beginAtZero:true, ticks:{callback: v => '$'+(v>=1000?Math.round(v/1000)+'k':v)} }
      }
    }
  });
}

function updateHistChart() {
  const moN = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const sorted = Object.entries(ALL_DATA)
    .filter(([,d]) => d.year > 2025 || (d.year === 2025 && d.month >= 1))
    .sort(([,a],[,b]) => a.year !== b.year ? a.year - b.year : a.month - b.month);
  if (!sorted.length) return;

  const now = new Date();
  const curMonth = now.getMonth() + 1, curYear = now.getFullYear();
  const daysInMonth = new Date(curYear, curMonth, 0).getDate();
  const dayOfMonth  = now.getDate();

  const labels=[], rhR=[], tdR=[], mdlR=[], projR=[];
  sorted.forEach(([,d]) => {
    labels.push(moN[d.month-1] + " '" + String(d.year).slice(2));
    const aRH  = Math.round(d.rh.reduce((a,b)=>a+b, 0) * RATES.rh);
    const aTD  = Math.round((d.td1||[]).reduce((a,b)=>a+b,0)*RATES.td_phone + (d.td2||[]).reduce((a,b)=>a+b,0)*RATES.td_video);
    const aMDL = Math.round(
      (d.mdl1||[]).reduce((a,b)=>a+b,0)*RATES.mdl_phone +
      (d.mdl2||[]).reduce((a,b)=>a+b,0)*RATES.mdl_video
    );  // mdl_async included in mdlRev per-day already
    rhR.push(aRH); tdR.push(aTD); mdlR.push(aMDL);
    const isCur = d.month === curMonth && d.year === curYear;
    if (isCur && dayOfMonth > 0 && dayOfMonth < daysInMonth) {
      const daysRecorded = d.labels.length;
      if (daysRecorded > 0) {
        const rate = (aRH + aTD + aMDL) / daysRecorded;  // FIX: use days recorded, not calendar day
        projR.push(Math.round(rate * (daysInMonth - daysRecorded)));
      } else projR.push(null);
    } else projR.push(null);
  });

  const hasProj = projR.some(v => v !== null);
  const pl = document.getElementById('proj-legend');
  const pn = document.getElementById('proj-note');
  if (pl) pl.style.display = hasProj ? 'flex' : 'none';
  if (pn && hasProj) {
    const cur = sorted[sorted.length-1][1];
    const dLeft = daysInMonth - cur.labels.length;
    pn.textContent = '· ' + dLeft + ' days left';
  }

  if (histChart) histChart.destroy();
  histChart = new Chart(document.getElementById('histChart').getContext('2d'), {
    type: 'bar',
    data: {
      labels: [...labels],
      datasets: [
        { label:'Roman Health', data:rhR,   backgroundColor:'rgba(45,212,160,.85)', borderWidth:0, borderRadius:0, stack:'s' },
        { label:'Teladoc',      data:tdR,   backgroundColor:'rgba(123,158,240,.85)',borderWidth:0, borderRadius:0, stack:'s' },
        { label:'MDLive',       data:mdlR,  backgroundColor:'rgba(245,166,35,.85)', borderWidth:0, borderRadius:0, stack:'s' },
        ...(hasProj ? [{ label:'Projected', data:projR,
          backgroundColor:'rgba(255,255,255,.1)', borderColor:'rgba(255,255,255,.25)',
          borderWidth:1, borderRadius:3, stack:'s' }] : []),
      ]
    },
    options: {
      responsive:true, maintainAspectRatio:false,
      interaction:{ mode:'index', intersect:false },
      plugins:{
        legend:{ display:false },
        tooltip:{ ...TT, callbacks:{
          label: i => {
            if (!i.raw) return null;
            if (i.dataset.label === 'Projected') {
              const act = i.chart.data.datasets.slice(0,3).reduce((a,ds) => a+(ds.data[i.dataIndex]||0), 0);
              return ' Est. month total: $' + (act + i.raw).toLocaleString() + ' (+$' + i.raw.toLocaleString() + ' proj)';
            }
            return ' ' + i.dataset.label + ': $' + i.raw.toLocaleString();
          },
          footer: items => {
            const act  = items.filter(i => i.dataset.label !== 'Projected').reduce((a,i) => a+(i.raw||0), 0);
            const proj = items.find(i => i.dataset.label === 'Projected' && i.raw);
            if (proj) return 'Actual: $' + act.toLocaleString() + ' | Est. total: $' + (act+proj.raw).toLocaleString();
            return 'Total: $' + act.toLocaleString();
          }
        }}
      },
      scales:{
        x:{ stacked:true, grid:{color:GRID}, ticks:{maxRotation:45} },
        y:{ stacked:true, grid:{color:GRID}, beginAtZero:true, ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(0)+'k':v)} }
      }
    }
  });
}

// ── CALENDAR HEATMAP ──────────────────────────────────────────────────────────
// Color scale based on % of RH target (167).
// Below target: red → orange → amber → yellow (traffic light approach).
// At/over target: graduated greens, brighter = further over.
function encToColor(rh) {
  if (rh === 0) return { bg:'#0d1117', textDark:false };

  const pct = rh / 167; // % of target

  // ── AT OR OVER TARGET — single bright green ─────────────────────────────
  if (pct >= 1.0) return { bg:'#22c55e', textDark:false };

  // ── BELOW TARGET — red → orange → amber → yellow ─────────────────────────
  if (pct < 0.15) return { bg:'#450a0a', textDark:false }; // 0–15%   deep red
  if (pct < 0.30) return { bg:'#7f1d1d', textDark:false }; // 15–30%  dark red
  if (pct < 0.45) return { bg:'#991b1b', textDark:false }; // 30–45%  red
  if (pct < 0.58) return { bg:'#c2410c', textDark:false }; // 45–58%  red-orange
  if (pct < 0.70) return { bg:'#ea580c', textDark:true  }; // 58–70%  orange
  if (pct < 0.82) return { bg:'#d97706', textDark:true  }; // 70–82%  amber
  if (pct < 0.92) return { bg:'#ca8a04', textDark:true  }; // 82–92%  golden
  return                 { bg:'#a16207', textDark:true  };  // 92–99%  dark gold (so close!)
}

function updateHeatmap() {
  const sheets = Object.values(ALL_DATA);
  if (!sheets.length) return;
  sheets.sort((a,b) => a.year !== b.year ? a.year-b.year : a.month-b.month);
  const cur = sheets[sheets.length-1];
  const moN = ['January','February','March','April','May','June','July','August','September','October','November','December'];
  const title = moN[cur.month-1] + ' ' + cur.year;
  set('heatmap-title', title);
  set('heatmap-label', 'Encounter Heatmap — ' + title);

  const now = new Date();
  const todayM = now.getMonth()+1, todayD = now.getDate(), todayY = now.getFullYear();
  const daysInMonth = new Date(cur.year, cur.month, 0).getDate();
  const firstDow = new Date(cur.year, cur.month-1, 1).getDay(); // 0=Sun

  // Build day→data map
  const dayMap = {};
  cur.labels.forEach((label, i) => {
    const p = label.split('/');
    if (p.length < 2) return;
    const d = +p[1];
    const enc = (cur.rh[i]||0) + (cur.td[i]||0) + (cur.mdl[i]||0);
    const rev = ((cur.rhRev||[])[i]||0) + ((cur.tdRev||[])[i]||0) + ((cur.mdlRev||[])[i]||0);
    if (enc > 0 || rev > 0) dayMap[d] = {
      enc, rev,
      rh:  cur.rh[i]||0,
      td:  cur.td[i]||0,
      mdl: cur.mdl[i]||0,
    };
  });

  const maxEnc = Math.max(...Object.values(dayMap).map(d => d.enc), 1);
  const moShort = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];

  const grid = document.getElementById('heatmap-grid');
  if (!grid) return;
  grid.innerHTML = '';

  // Blank offset cells
  for (let k = 0; k < firstDow; k++) {
    const blank = document.createElement('div');
    blank.className = 'hm-cell empty';
    grid.appendChild(blank);
  }

  for (let day = 1; day <= daysInMonth; day++) {
    const data     = dayMap[day];
    const isToday  = todayM === cur.month && todayY === cur.year && todayD === day;
    const isFuture = !data && new Date(cur.year, cur.month-1, day) > now;
    const targetMet = data && data.rh >= GOALS.rh;  // 167

    const { bg, textDark } = data
      ? encToColor(data.rh)
      : { bg: isFuture ? '#0d1117' : '#111827', textDark: false };

    const cell = document.createElement('div');
    cell.className = ['hm-cell',
      isToday    ? 'today'      : '',
      isFuture   ? 'future'     : '',
      !data && !isFuture ? 'empty' : '',
      targetMet  ? 'target-met' : '',
    ].filter(Boolean).join(' ');
    cell.style.background = bg;

    const dayCol = textDark ? 'rgba(0,0,0,.45)' : 'rgba(255,255,255,.38)';
    const encCol = textDark ? '#000' : '#fff';

    if (data) {
      const tip = `<div class="hm-tip">
        <strong style="color:var(--text)">${moShort[cur.month-1]} ${day}${isToday ? ' · Today' : ''}</strong><br>
        <span style="color:#2dd4a0">RH ${data.rh}</span> &nbsp;·&nbsp;
        <span style="color:#7b9ef0">TD ${data.td}</span> &nbsp;·&nbsp;
        <span style="color:#f5a623">MDL ${data.mdl}</span><br>
        Total: <strong>${data.enc}</strong> &nbsp;·&nbsp; $${data.rev.toLocaleString()}
        ${targetMet ? '<br><span style="color:#22c55e">✓ Daily target met (RH ≥ 167)</span>' : ''}
      </div>`;

      if (targetMet) {
        const over    = data.rh - 167;
        const overPct = Math.round(over / 167 * 100);
        const tdmdl   = (data.td||0) + (data.mdl||0);
        cell.innerHTML = `${tip}
          <div class="hm-day" style="color:${dayCol}">${day}</div>
          <div class="hm-enc" style="color:${encCol}">${data.rh}</div>
          <div class="hm-check">+${over} <span style="opacity:.7;font-size:.55rem">(+${overPct}%)</span></div>
          ${tdmdl > 0 ? `<div style="font-size:.58rem;color:rgba(0,0,0,.45);margin-top:1px">+${tdmdl} TD/MDL</div>` : ''}
          <div class="hm-rev">$${data.rev.toLocaleString()}</div>`;
      } else {
        const tdmdl = (data.td||0) + (data.mdl||0);
        cell.innerHTML = `${tip}
          <div class="hm-day" style="color:${dayCol}">${day}</div>
          <div class="hm-enc" style="color:${encCol}">${data.rh}</div>
          ${tdmdl > 0 ? `<div style="font-size:.58rem;color:${textDark?'rgba(0,0,0,.45)':'rgba(255,255,255,.35)'};margin-top:1px">+${tdmdl} TD/MDL</div>` : ''}`;
      }
    } else if (isFuture) {
      // Future day — dashed outline, day number only, clearly not-yet
      cell.innerHTML = `<div class="hm-day">${day}</div>`;
    } else {
      // Past day with no data logged
      cell.innerHTML = `<div class="hm-day" style="color:rgba(255,255,255,.22)">${day}</div>`;
    }

    grid.appendChild(cell);
  }
}

function updateHeader() {
  const sh = Object.values(ALL_DATA).sort((a,b) => a.year !== b.year ? a.year-b.year : a.month-b.month);
  const moN = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  if (sh.length) { const f = sh[0]; set('date-range', moN[f.month-1]+' '+f.year+' – Present'); }
}

function updateAll() {
  updateToday();
  updateGoals();
  updateSummary();
  updateCharts();
  updateHistChart();
  updateHeatmap();
  updateHeader();
}

// ── CACHE ─────────────────────────────────────────────────────────────────────
const CK='tmd_c', CKT='tmd_t';
function saveCache(t) { try { localStorage.setItem(CK, t); localStorage.setItem(CKT, Date.now().toString()); } catch(e){} }
function loadCacheObj() { try { const ts=localStorage.getItem(CKT), t=localStorage.getItem(CK); if(!t||!ts) return null; return { text:t, age:Math.round((Date.now()-+ts)/60000) }; } catch(e){ return null; } }

function parseAll(text) {
  const p = {};
  if (text.trim().startsWith('{')) {
    const json = JSON.parse(text);
    for (const [name, csv] of Object.entries(json)) {
      const d = parseSheetCSV(csv, name);
      if (d) { if (!d.year) d.year = yearFromTabName(name)||2026; p[name] = d; }
    }
  } else {
    const d = parseSheetCSV(text);
    if (d) p['Sheet'] = d;
  }
  return p;
}

// ── FETCH ─────────────────────────────────────────────────────────────────────
async function loadSheet(bg=false) {
  if (!bg) {
    const c = loadCacheObj();
    if (c) { try { const p = parseAll(c.text); if (Object.keys(p).length) { ALL_DATA=p; updateAll(); set('live-label','Cached · '+c.age+'m ago'); }} catch(e){} }
  }

  // Try Apps Script first, fall back to plain CSV
  const urls = [APPS_SCRIPT_URL + '?_=' + Date.now(), CSV_FALLBACK_URL + '&_=' + Date.now()];

  for (const url of urls) {
    try {
      const r = await fetch(url, { cache:'no-store', redirect:'follow' });
      if (!r.ok) throw new Error('HTTP ' + r.status);
      const t = await r.text();
      if (!t || t.trim().length < 10) throw new Error('empty response');
      const p = parseAll(t);
      if (!Object.keys(p).length) throw new Error('no sheets parsed');
      saveCache(t);
      ALL_DATA = p;
      updateAll();
      document.getElementById('live-dot').classList.add('on');
      set('live-label', 'Live · ' + new Date().toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'}));
      set('last-updated', new Date().toLocaleTimeString());
      return; // success — stop trying URLs
    } catch(e) {
      console.warn('fetch failed (' + url.slice(0,40) + '…):', e.message);
    }
  }

  // Both failed
  document.getElementById('live-dot').classList.remove('on');
  set('live-label', 'Unavailable · using cache');
  set('last-updated', 'Failed');
}

// ── INIT ──────────────────────────────────────────────────────────────────────
loadSheet();
setInterval(() => loadSheet(true), 60000);

function manualRefresh(btn) {
  const o = btn.innerHTML;
  btn.innerHTML = '↻ Refreshing…';
  btn.disabled = true;
  loadSheet(false).finally(() => { btn.innerHTML = o; btn.disabled = false; });
}
</script>
</body>
</html>
