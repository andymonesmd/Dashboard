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
  border-radius:8px;
  cursor:default;transition:transform .12s,box-shadow .12s;
  position:relative;
  min-height:90px;
}
.hm-cell:hover{transform:scale(1.07);box-shadow:0 0 0 2.5px rgba(255,255,255,.5);z-index:10}
.hm-cell.today{box-shadow:0 0 0 2.5px #fff!important}
.hm-cell.empty{background:#111827!important;cursor:default}
.hm-cell.future{background:#0f172a!important;cursor:default;border:1px dashed rgba(255,255,255,.08)!important}
.hm-cell.future .hm-day{color:rgba(255,255,255,.18)!important}
.hm-day{font-size:clamp(.65rem,1vw,.78rem);color:rgba(255,255,255,.45);font-family:'JetBrains Mono',monospace;line-height:1;font-weight:500;align-self:center}
.hm-enc{font-size:clamp(1.4rem,2.2vw,1.9rem);font-weight:900;line-height:1;color:#fff;margin-top:4px;letter-spacing:-.03em}
.hm-rev{font-size:clamp(.62rem,.95vw,.76rem);font-weight:700;font-family:'JetBrains Mono',monospace;color:rgba(255,255,255,.65);margin-top:5px}
.hm-cell.today .hm-day{color:rgba(255,255,255,.9);font-weight:700}
#hm-floating-tip{
  display:none;position:fixed;
  background:#1c2333;border:1px solid #334155;border-radius:10px;padding:10px 14px;
  font-size:.7rem;color:#e2e8f0;white-space:nowrap;z-index:9999;pointer-events:none;
  line-height:1.8;box-shadow:0 8px 32px rgba(0,0,0,.7);
}

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

  <!-- TOP BAR: TODAY VS TARGET -->
  <div style="background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:14px 20px">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;flex-wrap:wrap;gap:6px">
      <span style="font-size:.58rem;text-transform:uppercase;letter-spacing:.13em;color:var(--text3);font-weight:600">Most Recent Day vs Target</span>
      <span style="font-size:.65rem;color:var(--text3)" id="gs-days-note">—</span>
    </div>
    <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:16px">
      <!-- Roman Health -->
      <div>
        <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px">
          <span style="font-size:.6rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3)">Roman Health</span>
          <span style="font-size:1.4rem;font-weight:800;letter-spacing:-.03em" id="gs-rh-pct">—</span>
        </div>
        <div style="background:var(--border);border-radius:4px;height:6px;overflow:hidden;margin-bottom:5px">
          <div id="gs-rh-fill" style="height:100%;border-radius:4px;transition:width .9s ease;width:0"></div>
        </div>
        <span style="font-size:.65rem;color:var(--text3)" id="gs-rh-val">—</span>
      </div>
      <!-- Teladoc -->
      <div style="border-left:1px solid var(--border);padding-left:16px">
        <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px">
          <span style="font-size:.6rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3)">Teladoc</span>
          <span style="font-size:1.4rem;font-weight:800;letter-spacing:-.03em" id="gs-td-pct">—</span>
        </div>
        <div style="background:var(--border);border-radius:4px;height:6px;overflow:hidden;margin-bottom:5px">
          <div id="gs-td-fill" style="height:100%;border-radius:4px;transition:width .9s ease;width:0"></div>
        </div>
        <span style="font-size:.65rem;color:var(--text3)" id="gs-td-val">—</span>
      </div>
      <!-- MDLive -->
      <div style="border-left:1px solid var(--border);padding-left:16px">
        <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px">
          <span style="font-size:.6rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3)">MDLive</span>
          <span style="font-size:1.4rem;font-weight:800;letter-spacing:-.03em" id="gs-mdl-pct">—</span>
        </div>
        <div style="background:var(--border);border-radius:4px;height:6px;overflow:hidden;margin-bottom:5px">
          <div id="gs-mdl-fill" style="height:100%;border-radius:4px;transition:width .9s ease;width:0"></div>
        </div>
        <span style="font-size:.65rem;color:var(--text3)" id="gs-mdl-val">—</span>
      </div>
    </div>
  </div>

  <!-- SUMMARY STRIP -->
  <div>
    <div class="sec-hd"><span class="sec-label">Rolling 30 Days — At a Glance</span></div>
    <div class="summary-strip" style="grid-template-columns:1fr 1fr">
      <div class="scard">
        <div class="scard-label">Avg Daily Encounters</div>
        <div class="scard-val" id="s-avg">—</div>
        <div class="scard-sub" id="s-avg-prev" style="margin-top:4px">—</div>
        <div class="scard-sub" id="s-avg-sub" style="margin-top:2px">all platforms · 30-day avg</div>
        <div style="margin-top:10px;padding-top:10px;border-top:1px solid rgba(255,255,255,.08)">
          <div style="font-size:.58rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px">7-Day Avg</div>
          <div style="display:flex;align-items:baseline;gap:6px">
            <span id="s-avg-7d" style="font-size:1.4rem;font-weight:800;letter-spacing:-.03em">—</span>
            <span id="s-avg-7d-trend" style="font-size:.68rem;color:var(--text3)">—</span>
          </div>
          <div id="s-avg-7d-sub" style="font-size:.62rem;color:var(--text3);margin-top:2px">—</div>
        </div>
      </div>
      <div class="scard">
        <div class="scard-label">Avg Daily Revenue</div>
        <div class="scard-val" id="s-rev">—</div>
        <div class="scard-sub" id="s-rev-prev" style="margin-top:4px">—</div>
        <div class="scard-sub" id="s-rev-sub" style="margin-top:2px">30-day avg</div>
        <div style="margin-top:10px;padding-top:10px;border-top:1px solid rgba(255,255,255,.08)">
          <div style="font-size:.58rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:4px">7-Day Avg</div>
          <div style="display:flex;align-items:baseline;gap:6px">
            <span id="s-rev-7d" style="font-size:1.4rem;font-weight:800;letter-spacing:-.03em">—</span>
            <span id="s-rev-7d-trend" style="font-size:.68rem;color:var(--text3)">—</span>
          </div>
          <div id="s-rev-7d-sub" style="font-size:.62rem;color:var(--text3);margin-top:2px">—</div>
        </div>
      </div>
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

  <!-- COMBINED REVENUE CARD -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Revenue</span>
      <span class="sec-note" id="today-line">—</span>
    </div>
    <div class="rev-wrap">

      <!-- Daily row -->
      <div class="rev-hd" style="margin-bottom:8px">
        <div class="rev-hd-left">
          <span class="rev-title">Daily Revenue</span>
          <span class="rev-achieved" id="t-rev-amt" style="color:var(--rev)">$0</span>
        </div>
        <div class="rev-hd-right">
          <span class="rev-goal-label">Daily Goal</span>
          <span class="rev-goal-val">$1,370</span>
          <span class="rev-pct-pill" id="t-rev-pct" style="color:var(--rev);border-color:var(--rev)">0%</span>
        </div>
      </div>
      <div class="rev-track">
        <div class="rev-fill" id="t-rev-fill" style="width:0%"></div>
      </div>
      <div class="rev-ends" style="margin-bottom:20px"><span>$0</span><span>$1,370</span></div>

      <!-- Divider -->
      <div style="border-top:1px solid var(--border);margin-bottom:20px"></div>

      <!-- 30-day row -->
      <div class="rev-hd" style="margin-bottom:8px">
        <div class="rev-hd-left">
          <span class="rev-title">Rolling 30-Day Revenue</span>
          <span class="rev-achieved" id="g-rev-amt" style="color:var(--rev)">$0</span>
        </div>
        <div class="rev-hd-right">
          <span class="rev-goal-label">30-Day Goal</span>
          <span class="rev-goal-val">$40,000</span>
          <span class="rev-pct-pill" id="g-rev-pct" style="color:var(--rev);border-color:var(--rev)">0%</span>
        </div>
      </div>
      <div class="rev-track">
        <div class="rev-fill" id="g-rev-fill" style="width:0%"></div>
      </div>
      <div class="rev-ends"><span>$0</span><span>$40,000</span></div>
      <div style="font-size:.6rem;color:var(--text3);margin-top:4px;text-align:right" id="goals-note">last 30 days</div>

    </div>
  </div>

  <!-- CALENDAR HEATMAP — full width -->
  <div>
    <div class="sec-hd">
      <span class="sec-label" id="heatmap-label">Encounter Heatmap — Current Month</span>
      <span class="sec-note">green ≥$1.37k · orange $1k–$1.37k · red &lt;$1k &nbsp;·&nbsp; ⬤ Roman &nbsp;■ Teladoc &nbsp;◆ MDLive</span>
    </div>
    <div class="chart-card">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;flex-wrap:wrap;gap:8px">
        <h3 id="heatmap-title" style="margin:0">—</h3>
        <!-- color legend -->
        <div style="display:flex;align-items:center;gap:8px;font-size:.62rem;color:var(--text3)">
          <span>← Below $1,500</span>
          <div style="display:flex;gap:3px;align-items:center">
            <div title="Below $500" style="width:16px;height:16px;border-radius:3px;background:#450a0a;border:1px solid rgba(255,255,255,.06)"></div>
            <div title="$500–$999" style="width:16px;height:16px;border-radius:3px;background:#991b1b;border:1px solid rgba(255,255,255,.06)"></div>
            <div title="$1,000–$1,499" style="width:16px;height:16px;border-radius:3px;background:#dc2626;border:1px solid rgba(255,255,255,.06)"></div>
            <div style="width:1px;height:16px;background:rgba(255,255,255,.15);margin:0 2px"></div>
            <div title="$1,500–$1,999" style="width:16px;height:16px;border-radius:3px;background:#ea580c;border:1px solid rgba(255,255,255,.06)"></div>
            <div style="width:1px;height:16px;background:rgba(255,255,255,.15);margin:0 2px"></div>
            <div title="$2,000+ goal met" style="width:16px;height:16px;border-radius:3px;background:#22c55e;border:1px solid rgba(255,255,255,.06)"></div>
          </div>
          <span>✓ $1,370+</span>
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

<div id="hm-floating-tip"></div>
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
const GOALS = { rh:167, td:10, mdl:10, dayRev:1370, moRev:40000 };

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
    // Match TD, TLD, TDOC, TELADOC, TELADOCPHONE, TELADOCVIDEO, TLDVIDEO etc.
    const isTD    = rawLabel === 'TD' || rawLabel === 'TLD' ||
                    rawLabel.startsWith('TD') || rawLabel.startsWith('TLD') ||
                    rawLabel.includes('TELADOC');
    const isMDL   = !isTD && (rawLabel === 'MDL' || rawLabel === 'MD' ||
                    rawLabel.startsWith('MDL') || rawLabel.includes('MDLIVE'));

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
    dateCols:  dateCols.slice(0, t),
    td_phone:  td_phone.slice(0, t),
    td_video:  td_video.slice(0, t),
    mdl_phone: mdl_phone.slice(0, t),
    mdl_video: mdl_video.slice(0, t),
    mdl_async: mdl_async.slice(0, t),
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
    const trendCol = up ? '#22c55e' : same ? 'var(--text2)' : '#f87171';
    const arr  = up ? '↑' : same ? '→' : '↓';
    const diff = Math.abs(val - prevVal);
    const el = document.getElementById(elId);
    if (el) { el.textContent = fmt(val); }
    set(prevElId, arr + ' ' + fmt(prevVal) + ' prev · ' + arr + ' ' + fmt(diff) + (up?' more':same?'':' less'));
    const pe = document.getElementById(prevElId); if (pe) pe.style.color = trendCol;
    if (subElId && subText) set(subElId, subText);

    // Color the parent scard tile based on goal thresholds
    const card = el && el.closest('.scard');
    if (card) {
      const rawNum = typeof val === 'number' ? val : parseFloat(String(val).replace(/[$,]/g,''));
      let tileBg, tileText;
      if (elId === 's-avg') {
        tileBg   = rawNum >= 167  ? 'rgba(34,197,94,.15)'  : 'rgba(234,88,12,.15)';
        tileText = rawNum >= 167  ? '#22c55e'              : '#ea580c';
      } else if (elId === 's-rev') {
        tileBg   = rawNum >= 2000 ? 'rgba(34,197,94,.15)'  : 'rgba(234,88,12,.15)';
        tileText = rawNum >= 2000 ? '#22c55e'              : '#ea580c';
      }
      if (tileBg) {
        card.style.background   = tileBg;
        card.style.borderColor  = tileText.replace(')', ',.4)').replace('rgb','rgba');
        if (el) el.style.color  = tileText;
      }
    }
  }

  const fNum = v => Math.round(v).toLocaleString();
  const fDol = v => '$' + Math.round(v).toLocaleString();

  renderStat('s-avg',  's-avg-prev',  's-avg-sub',  Math.round((tRH+tTD+tMDL)/n), Math.round((pRH+pEnc-pRH)/pN + pRH/pN), fNum, 'RH ' + Math.round(tRH/n) + ' · TD ' + Math.round(tTD/n) + ' · MDL ' + Math.round(tMDL/n));

  renderStat('s-rev',  's-rev-prev',  's-rev-sub',  Math.round(tRev/n), Math.round(pRev/pN), fDol, 'avg daily revenue');

  // ── 7-day averages ────────────────────────────────────────────────────────
  const last7  = getRollingN(7,  true);
  const prev7  = getRollingN(14, true).slice(0, 7); // days 8–14 ago

  if (last7.length) {
    const n7   = last7.length;
    const enc7 = Math.round(last7.reduce((a,d) => a+d.rh+d.td+d.mdl, 0) / n7);
    const rev7 = Math.round(last7.reduce((a,d) => a+d.rhRev+d.tdRev+d.mdlRev, 0) / n7);
    const pEnc7= prev7.length ? Math.round(prev7.reduce((a,d) => a+d.rh+d.td+d.mdl, 0) / prev7.length) : null;
    const pRev7= prev7.length ? Math.round(prev7.reduce((a,d) => a+d.rhRev+d.tdRev+d.mdlRev, 0) / prev7.length) : null;

    // Encounters 7-day
    const encCol7 = enc7 >= 167 ? '#22c55e' : '#ea580c';
    const e7el = document.getElementById('s-avg-7d');
    if (e7el) { e7el.textContent = enc7.toLocaleString(); e7el.style.color = encCol7; }
    if (pEnc7 !== null) {
      const diff = enc7 - pEnc7;
      const arr  = diff > 0 ? '↑' : diff < 0 ? '↓' : '→';
      const tc   = diff > 0 ? '#22c55e' : diff < 0 ? '#f87171' : 'var(--text3)';
      const t7el = document.getElementById('s-avg-7d-trend');
      if (t7el) { t7el.textContent = arr + ' ' + Math.abs(diff) + ' vs prev 7d'; t7el.style.color = tc; }
    }
    const rh7  = Math.round(last7.reduce((a,d) => a+d.rh,  0) / n7);
    const td7  = Math.round(last7.reduce((a,d) => a+d.td,  0) / n7);
    const mdl7 = Math.round(last7.reduce((a,d) => a+d.mdl, 0) / n7);
    set('s-avg-7d-sub', 'RH ~' + rh7 + ' · TD ~' + td7 + ' · MDL ~' + mdl7);

    // Revenue 7-day
    const revCol7 = rev7 >= 2000 ? '#22c55e' : '#ea580c';
    const r7el = document.getElementById('s-rev-7d');
    if (r7el) { r7el.textContent = '$' + rev7.toLocaleString(); r7el.style.color = revCol7; }
    if (pRev7 !== null) {
      const diff = rev7 - pRev7;
      const arr  = diff > 0 ? '↑' : diff < 0 ? '↓' : '→';
      const tc   = diff > 0 ? '#22c55e' : diff < 0 ? '#f87171' : 'var(--text3)';
      const rt7el = document.getElementById('s-rev-7d-trend');
      if (rt7el) { rt7el.textContent = arr + ' $' + Math.abs(diff).toLocaleString() + ' vs prev 7d'; rt7el.style.color = tc; }
    }
    set('s-rev-7d-sub', 'last ' + n7 + ' days excl. today');
  }
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
function encToColor(rev) {
  if (rev === 0) return { bg:'#0d1117', textDark:false };
  if (rev >= 1370) return { bg:'#22c55e', textDark:false };   // ≥$1,370 — green
  if (rev >= 1000) return { bg:'#ea580c', textDark:false };   // $1,000–$1,369 — orange
  // Below $1,000 — red gradient
  const pct = rev / 1000;
  if (pct < 0.20) return { bg:'#450a0a', textDark:false };
  if (pct < 0.40) return { bg:'#7f1d1d', textDark:false };
  if (pct < 0.60) return { bg:'#991b1b', textDark:false };
  if (pct < 0.80) return { bg:'#b91c1c', textDark:false };
  return                 { bg:'#dc2626', textDark:false };
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
    const d      = +p[1];
    const rh     = cur.rh[i]  || 0;
    const td     = cur.td[i]  || 0;
    const mdl    = cur.mdl[i] || 0;
    const enc    = rh + td + mdl;
    const rev    = ((cur.rhRev||[])[i]||0) + ((cur.tdRev||[])[i]||0) + ((cur.mdlRev||[])[i]||0);
    const tdPh   = (cur.td_phone ||[])[i] || 0;
    const tdVid  = (cur.td_video ||[])[i] || 0;
    const mdlPh  = (cur.mdl_phone||[])[i] || 0;
    const mdlVid = (cur.mdl_video||[])[i] || 0;
    const mdlAsy = (cur.mdl_async||[])[i] || 0;
    if (enc > 0 || rev > 0) dayMap[d] = { enc, rh, td, mdl, rev, tdPh, tdVid, mdlPh, mdlVid, mdlAsy };
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
    const targetMet = data && data.rev >= 2000;

    const { bg, textDark } = data
      ? encToColor(data.rev)
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
    const encCol = '#fff';

    if (data) {
      const _tipRevColor = data.rev >= 2000 ? '#22c55e' : data.rev >= 1500 ? '#f5a623' : '#f87171';
            const revStatus = data.rev >= 1370
        ? `<span style="color:#22c55e">✓ $${(data.rev-1370).toLocaleString()} over $1,370 goal</span>`
        : data.rev >= 1000
          ? `<span style="color:#f5a623">$${(1370-data.rev).toLocaleString()} short of $1,370 goal</span>`
          : `<span style="color:#f87171">$${(1370-data.rev).toLocaleString()} below $1,370 goal</span>`;

      const tdLines = [
        data.tdPh  > 0 ? `&nbsp;&nbsp;📞 Phone: ${data.tdPh}` : '',
        data.tdVid > 0 ? `&nbsp;&nbsp;🎥 Video: ${data.tdVid}` : '',
      ].filter(Boolean).join('<br>');
      const mdlLines = [
        data.mdlPh  > 0 ? `&nbsp;&nbsp;📞 Phone: ${data.mdlPh}` : '',
        data.mdlVid > 0 ? `&nbsp;&nbsp;🎥 Video: ${data.mdlVid}` : '',
        data.mdlAsy > 0 ? `&nbsp;&nbsp;⚡ Async: ${data.mdlAsy}` : '',
      ].filter(Boolean).join('<br>');

      // Store tooltip content as data attribute for mouseover handler
      const tipHTML =
        `<strong style="color:#f1f5f9">${moShort[cur.month-1]} ${day}${isToday ? ' · Today' : ''}</strong>` +
        `<div style="border-top:1px solid rgba(255,255,255,.15);margin:5px 0 4px"></div>` +
        `<span style="color:#2dd4a0;font-weight:600">Roman Health: ${data.rh}</span><br>` +
        `<span style="color:#7b9ef0;font-weight:600">Teladoc: ${data.td}</span>` +
          (data.tdPh  > 0 ? `<br><span style="color:#7b9ef0;opacity:.75">&nbsp;&nbsp;Phone: ${data.tdPh}</span>` : '') +
          (data.tdVid > 0 ? `<br><span style="color:#7b9ef0;opacity:.75">&nbsp;&nbsp;Video: ${data.tdVid}</span>` : '') +
        `<br><span style="color:#f5a623;font-weight:600">MDLive: ${data.mdl}</span>` +
          (data.mdlPh  > 0 ? `<br><span style="color:#f5a623;opacity:.75">&nbsp;&nbsp;Phone: ${data.mdlPh}</span>` : '') +
          (data.mdlVid > 0 ? `<br><span style="color:#f5a623;opacity:.75">&nbsp;&nbsp;Video: ${data.mdlVid}</span>` : '') +
          (data.mdlAsy > 0 ? `<br><span style="color:#f5a623;opacity:.75">&nbsp;&nbsp;Async: ${data.mdlAsy}</span>` : '') +
        `<div style="border-top:1px solid rgba(255,255,255,.15);margin:5px 0 4px"></div>` +
        `<span style="color:#f1f5f9">Total: <strong>${data.enc}</strong> &nbsp;·&nbsp; <strong>$${data.rev.toLocaleString()}</strong></span><br>` +
        revStatus;
      const tip = '';  // unused, tooltip via mouseover
      cell.setAttribute('data-tip', tipHTML);

      // ── Cell layout ───────────────────────────────────────────────────────
      const sz = '20px';
      const mkIcon = (val, shape) => {
        if (!val || val === 0) return '';
        const fs = val >= 100 ? '.48rem' : val >= 10 ? '.54rem' : '.6rem';
        if (shape === 'circle')
          return `<div style="width:${sz};height:${sz};border-radius:50%;background:#fff;display:inline-flex;align-items:center;justify-content:center;flex-shrink:0"><span style="font-size:${fs};font-weight:900;color:#000;line-height:1">${val}</span></div>`;
        if (shape === 'square')
          return `<div style="width:${sz};height:${sz};border-radius:3px;background:#fff;display:inline-flex;align-items:center;justify-content:center;flex-shrink:0"><span style="font-size:${fs};font-weight:900;color:#000;line-height:1">${val}</span></div>`;
        if (shape === 'diamond')
          return `<div style="width:${sz};height:${sz};border-radius:2px;transform:rotate(45deg);background:#fff;display:inline-flex;align-items:center;justify-content:center;flex-shrink:0"><span style="transform:rotate(-45deg);font-size:${fs};font-weight:900;color:#000;line-height:1;display:block">${val}</span></div>`;
        return '';
      };

      const revColor  = data.rev >= 1370 ? 'rgba(255,255,255,.95)' : 'rgba(255,255,255,.7)';
      const overAmt   = data.rev - 1370;
      const overText  = overAmt >= 0
        ? `<span style="font-size:.55rem;color:rgba(255,255,255,.65)">+$${overAmt.toLocaleString()}</span>`
        : `<span style="font-size:.55rem;color:rgba(255,255,255,.5)">-$${Math.abs(overAmt).toLocaleString()}</span>`;

      const iconRow = [mkIcon(data.rh,'circle'), mkIcon(data.td,'square'), mkIcon(data.mdl,'diamond')]
        .filter(Boolean).join('');

      cell.innerHTML = `
        <div style="display:flex;flex-direction:column;height:100%;padding:5px 6px;box-sizing:border-box;min-height:90px">
          <div style="text-align:right;font-size:.65rem;font-weight:600;color:rgba(255,255,255,.75);line-height:1">${day}</div>
          <div style="text-align:center;font-size:clamp(1.2rem,2vw,1.6rem);font-weight:900;color:#fff;line-height:1.1;margin:3px 0 2px">${data.enc}</div>
          <div style="display:flex;gap:3px;justify-content:center;align-items:center;margin-bottom:3px">${iconRow}</div>
          <div style="text-align:center;line-height:1.3;margin-top:auto">
            <div style="font-size:.62rem;font-weight:700;color:${revColor}">$${data.rev.toLocaleString()}</div>
            ${overText}
          </div>
        </div>`;
    } else if (isFuture) {
      cell.innerHTML = `<div style="text-align:right;padding:5px 6px;font-size:.68rem;font-weight:600;color:rgba(255,255,255,.18)">${day}</div>`;
    } else {
      cell.innerHTML = `<div style="text-align:right;padding:5px 6px;font-size:.68rem;font-weight:600;color:rgba(255,255,255,.22)">${day}</div>`;
    }

    grid.appendChild(cell);
  }

  // Populate 7-day avg strip
  const last7h  = getRollingN(7,  true);
  const prev7h  = getRollingN(14, true).slice(0, 7);
  if (last7h.length) {
    const n7 = last7h.length;
    const enc7 = Math.round(last7h.reduce((a,d) => a+d.rh+d.td+d.mdl, 0) / n7);
    const rev7 = Math.round(last7h.reduce((a,d) => a+d.rhRev+d.tdRev+d.mdlRev, 0) / n7);
    const pEnc7 = prev7h.length ? Math.round(prev7h.reduce((a,d) => a+d.rh+d.td+d.mdl, 0) / prev7h.length) : null;
    const pRev7 = prev7h.length ? Math.round(prev7h.reduce((a,d) => a+d.rhRev+d.tdRev+d.mdlRev, 0) / prev7h.length) : null;
    const encCol7 = enc7 >= 167 ? '#22c55e' : '#ea580c';
    const revCol7 = rev7 >= 2000 ? '#22c55e' : '#ea580c';
    const eEl = document.getElementById('cal-7d-enc');
    if (eEl) { eEl.textContent = enc7.toLocaleString(); eEl.style.color = encCol7; }
    const rEl = document.getElementById('cal-7d-rev');
    if (rEl) { rEl.textContent = '$' + rev7.toLocaleString(); rEl.style.color = revCol7; }
    if (pEnc7 !== null) {
      const d = enc7 - pEnc7, arr = d > 0 ? '↑' : d < 0 ? '↓' : '→', tc = d > 0 ? '#22c55e' : d < 0 ? '#f87171' : 'rgba(255,255,255,.4)';
      const etEl = document.getElementById('cal-7d-enc-trend');
      if (etEl) { etEl.textContent = arr + ' ' + Math.abs(d) + ' vs prior 7d'; etEl.style.color = tc; }
      const rh7 = Math.round(last7h.reduce((a,d)=>a+d.rh,0)/n7);
      const td7 = Math.round(last7h.reduce((a,d)=>a+d.td,0)/n7);
      const ml7 = Math.round(last7h.reduce((a,d)=>a+d.mdl,0)/n7);
      set('cal-7d-enc-sub', 'RH ~' + rh7 + ' · TD ~' + td7 + ' · MDL ~' + ml7);
    }
    if (pRev7 !== null) {
      const d = rev7 - pRev7, arr = d > 0 ? '↑' : d < 0 ? '↓' : '→', tc = d > 0 ? '#22c55e' : d < 0 ? '#f87171' : 'rgba(255,255,255,.4)';
      const rtEl = document.getElementById('cal-7d-rev-trend');
      if (rtEl) { rtEl.textContent = arr + ' $' + Math.abs(d).toLocaleString() + ' vs prior 7d'; rtEl.style.color = tc; }
    }
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

async function loadSheet(bg=false) {
  if (!bg) {
    const c = loadCacheObj();
    if (c) { try { const p = parseAll(c.text); if (Object.keys(p).length) { ALL_DATA=p; updateAll(); set('live-label','Cached · '+c.age+'m ago'); }} catch(e){} }
  }
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
      return;
    } catch(e) {
      console.warn('fetch failed:', e.message);
    }
  }
  document.getElementById('live-dot').classList.remove('on');
  set('live-label', 'Unavailable · using cache');
  set('last-updated', 'Failed');
}

// ── Floating calendar tooltip ─────────────────────────────────────────────────
(function() {
  const ft = document.getElementById('hm-floating-tip');
  if (!ft) return;
  document.addEventListener('mouseover', e => {
    const cell = e.target.closest('[data-tip]');
    if (!cell) { ft.style.display = 'none'; return; }
    ft.innerHTML = cell.getAttribute('data-tip');
    ft.style.display = 'block';
  });
  document.addEventListener('mouseout', e => {
    if (!e.target.closest('[data-tip]')) ft.style.display = 'none';
  });
  document.addEventListener('mousemove', e => {
    if (ft.style.display === 'none') return;
    const x = e.clientX, y = e.clientY;
    const w = ft.offsetWidth, h = ft.offsetHeight;
    const vw = window.innerWidth, vh = window.innerHeight;
    ft.style.left = (x + 14 + w > vw ? x - w - 10 : x + 14) + 'px';
    ft.style.top  = (y - 10 + h > vh ? y - h + 10 : y - 10) + 'px';
  });
})();

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
