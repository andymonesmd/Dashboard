
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1"/>
<title>Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet"/>
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
body{background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;min-height:100vh;padding-bottom:48px}

/* HEADER */
.header{display:flex;align-items:center;justify-content:space-between;padding:16px 28px;border-bottom:1px solid var(--border);flex-wrap:wrap;gap:10px;}
.header-left h1{font-size:1.3rem;font-weight:700;letter-spacing:-.01em}
.header-left .sub{font-size:.68rem;color:var(--text3);margin-top:3px;display:flex;align-items:center;gap:6px;flex-wrap:wrap}
.ldot{width:7px;height:7px;border-radius:2px;display:inline-block}
.header-right{display:flex;align-items:center;gap:8px}
.live-pill{display:flex;align-items:center;gap:6px;background:var(--surface2);border:1px solid var(--border2);border-radius:20px;padding:5px 12px;font-size:.7rem;color:var(--text2);white-space:nowrap}
.live-dot{width:7px;height:7px;border-radius:50%;background:#ef4444;animation:blink 2s infinite;flex-shrink:0}
.live-dot.on{background:#22c55e}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.refresh-btn{background:var(--surface2);border:1px solid var(--border2);border-radius:20px;padding:5px 12px;color:var(--text2);font-size:.7rem;cursor:pointer;font-family:'DM Sans',sans-serif;transition:.15s;white-space:nowrap}
.refresh-btn:hover{border-color:#60a5fa;color:#60a5fa}

/* LAYOUT */
.dash{padding:22px 28px;display:flex;flex-direction:column;gap:30px}
.sec-hd{display:flex;align-items:baseline;justify-content:space-between;margin-bottom:14px;flex-wrap:wrap;gap:6px}
.sec-label{font-size:.58rem;text-transform:uppercase;letter-spacing:.13em;color:var(--text3);font-weight:600}
.sec-note{font-size:.65rem;color:var(--text3)}

/* TODAY PROVIDER CARDS */
.today-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-bottom:14px}
.pcard{background:var(--surface);border:1px solid var(--border);border-radius:14px;padding:20px 22px;position:relative;overflow:hidden}
.pcard::before{content:'';position:absolute;top:0;left:0;right:0;height:3px}
.pcard.rh::before{background:var(--rh)}
.pcard.td::before{background:var(--td)}
.pcard.mdl::before{background:var(--mdl)}
.pcard-name{font-size:.58rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3);margin-bottom:10px}
.pcard-hero{display:flex;align-items:flex-end;gap:8px;margin-bottom:14px}
.pcard-val{font-size:3rem;font-weight:800;line-height:1;letter-spacing:-.02em}
.pcard-target{font-size:.85rem;color:var(--text3);margin-bottom:4px}
.prog{background:var(--border);border-radius:4px;height:8px;overflow:hidden;margin-bottom:8px}
.prog-fill{height:100%;border-radius:4px;transition:width .9s ease;width:0}
.pcard-foot{display:flex;justify-content:space-between;align-items:center;gap:6px}
.pcard-pct{font-size:1rem;font-weight:700}
.badge{font-size:.62rem;padding:3px 9px;border-radius:10px;white-space:nowrap}
.pcard-note{font-size:.63rem;color:var(--text3);margin-top:8px}

/* REV TRACK */
.rev-wrap{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:16px 20px}
.rev-hd{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px}
.rev-title{font-size:.58rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3)}
.rev-goal{font-size:.65rem;color:var(--text3)}
.rev-rail{position:relative;height:28px}
.rev-bg{position:absolute;top:50%;transform:translateY(-50%);left:0;right:0;height:10px;background:var(--border);border-radius:6px;overflow:hidden}
.rev-fill{height:100%;border-radius:6px;background:linear-gradient(90deg,#f87171,#f5a623,#22c55e);transition:width .9s ease}
.rev-badge{position:absolute;top:-22px;transform:translateX(-50%);background:var(--surface2);border:1px solid var(--border2);border-radius:20px;padding:2px 10px;font-size:.7rem;font-weight:700;white-space:nowrap;transition:left .9s ease}
.rev-ends{display:flex;justify-content:space-between;margin-top:6px;font-size:.6rem;color:var(--text3)}

/* GOALS */
.goals-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:14px}
.gcard{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:16px 18px}
.gcard-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
.gcard-name{font-size:.58rem;text-transform:uppercase;letter-spacing:.12em;color:var(--text3)}
.gcard-nums{display:flex;align-items:flex-end;gap:6px;margin-bottom:10px}
.gcard-val{font-size:1.9rem;font-weight:800;line-height:1;letter-spacing:-.02em}
.gcard-of{font-size:.75rem;color:var(--text3);margin-bottom:2px}
.gcard-note{font-size:.63rem;color:var(--text3);margin-top:6px}

/* SUMMARY STRIP */
.summary-strip{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
.scard{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px}
.scard-label{font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:10px;font-weight:500}
.scard-val{font-size:2.4rem;font-weight:800;line-height:1;letter-spacing:-.03em}
.scard-sub{font-size:.72rem;color:var(--text3);margin-top:6px}

/* CHARTS */
.charts-row{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.chart-card{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:18px 20px}
.chart-card h3{font-size:.72rem;font-weight:600;color:var(--text);margin-bottom:12px}
.clegend{display:flex;gap:12px;flex-wrap:wrap;margin-top:10px}
.cleg{display:flex;align-items:center;gap:5px;font-size:.65rem;color:var(--text2)}
.cleg-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0}

/* THREE-CHART ROW */
.three-charts{display:grid;grid-template-columns:repeat(3,1fr);gap:14px}

/* FOOTER */
.footer{display:flex;justify-content:space-between;flex-wrap:wrap;gap:6px;padding:10px 28px;font-size:.64rem;color:var(--text3)}

/* RESPONSIVE */
@media(max-width:900px){
  .three-charts{grid-template-columns:1fr 1fr}
  .summary-strip{grid-template-columns:1fr 1fr}
}
@media(max-width:700px){
  .header{padding:12px 16px}
  .header-left h1{font-size:1.15rem}
  .dash{padding:14px 16px;gap:20px}
  .today-grid,.goals-grid{grid-template-columns:1fr;gap:10px}
  .pcard-val{font-size:2.5rem}
  .three-charts{grid-template-columns:1fr}
  .summary-strip{grid-template-columns:1fr 1fr}
  .scard{padding:14px 16px}
  .scard-val{font-size:1.8rem}
  .footer{padding:8px 16px}
}
@media(max-width:480px){
  .header{padding:10px 12px}
  .dash{padding:12px 12px;gap:16px}
  .pcard{padding:16px}
  .pcard-val{font-size:2.2rem}
  .gcard-val{font-size:1.6rem}
  .summary-strip{grid-template-columns:1fr 1fr}
  .rev-wrap,.chart-card{padding:14px}
  .header-right .refresh-btn span{display:none}
}
@media(max-width:360px){
  .summary-strip{grid-template-columns:1fr}
}
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <h1>Dashboard</h1>
    <div class="sub">
      <span id="date-range">—</span>&nbsp;·&nbsp;
      <span class="ldot" style="background:var(--rh)"></span>Roman&nbsp;·&nbsp;
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
    <div class="sec-hd"><span class="sec-label">Last 30 Days — At a Glance</span></div>
    <div class="summary-strip">
      <div class="scard">
        <div class="scard-label">Total Encounters</div>
        <div class="scard-val" id="s-enc">—</div>
        <div class="scard-sub" id="s-enc-sub">all platforms</div>
      </div>
      <div class="scard">
        <div class="scard-label">Total Revenue</div>
        <div class="scard-val" style="color:var(--rev)" id="s-rev">—</div>
        <div class="scard-sub" id="s-rev-sub">estimated</div>
      </div>
      <div class="scard">
        <div class="scard-label">Avg Daily · Roman</div>
        <div class="scard-val" style="color:var(--rh)" id="s-avg">—</div>
        <div class="scard-sub">encounters / day</div>
      </div>
      <div class="scard">
        <div class="scard-label">Active Days</div>
        <div class="scard-val" id="s-days">—</div>
        <div class="scard-sub" id="s-range">—</div>
      </div>
    </div>
  </div>



<!-- CHARTS ROW -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Charts</span>
      <span class="sec-note">RH $12 · TD phone $23 / video $28 · MDL phone $25 / video $28</span>
    </div>
    <div class="three-charts">

      <!-- Revenue line -->
      <div class="chart-card">
        <h3>Revenue — Last 30 Days</h3>
        <div style="position:relative;height:260px"><canvas id="revChart" role="img" aria-label="Daily revenue last 30 days"></canvas></div>
        <div class="clegend">
          <div class="cleg"><div class="cleg-dot" style="background:#2dd4a0"></div>Roman</div>
          <div class="cleg"><div class="cleg-dot" style="background:#7b9ef0"></div>Teladoc</div>
          <div class="cleg"><div class="cleg-dot" style="background:#f5a623"></div>MDLive</div>
        </div>
      </div>

      <!-- State volume bar -->
      <div class="chart-card">
        <h3>Encounters by State — April</h3>
        <div style="position:relative;height:260px"><canvas id="stateChart" role="img" aria-label="April encounter volume by state horizontal bar chart"></canvas></div>
        <div class="clegend">
          <div class="cleg"><div class="cleg-dot" style="background:#534AB7"></div>Volume (sorted)</div>
          <div class="cleg" style="color:var(--text3)">PA active · pending</div>
        </div>
      </div>

      <!-- Monthly history -->
      <div class="chart-card">
        <h3>Monthly Revenue — Jan 2025+</h3>
        <div style="position:relative;height:260px"><canvas id="histChart" role="img" aria-label="Monthly revenue history from Jan 2025"></canvas></div>
        <div class="clegend">
          <div class="cleg"><div class="cleg-dot" style="background:#2dd4a0"></div>Roman</div>
          <div class="cleg"><div class="cleg-dot" style="background:#7b9ef0"></div>Teladoc</div>
          <div class="cleg"><div class="cleg-dot" style="background:#f5a623"></div>MDLive</div>
        </div>
      </div>

    </div>
  </div>

<!-- TODAY -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Today</span>
      <span class="sec-note" id="today-line">—</span>
    </div>
    <div class="today-grid">
      <div class="pcard rh">
        <div class="pcard-name">Roman</div>
        <div class="pcard-hero"><div class="pcard-val" style="color:var(--rh)" id="t-rh-v">—</div><div class="pcard-target">/ 100</div></div>
        <div class="prog"><div class="prog-fill" id="t-rh-bar" style="background:var(--rh)"></div></div>
        <div class="pcard-foot"><span class="pcard-pct" id="t-rh-pct" style="color:var(--rh)">—</span><span class="badge" id="t-rh-badge">of target</span></div>
        <div class="pcard-note" id="t-rh-note"></div>
      </div>
      <div class="pcard td">
        <div class="pcard-name">Teladoc</div>
        <div class="pcard-hero"><div class="pcard-val" style="color:var(--td)" id="t-td-v">—</div><div class="pcard-target">/ 5</div></div>
        <div class="prog"><div class="prog-fill" id="t-td-bar" style="background:var(--td)"></div></div>
        <div class="pcard-foot"><span class="pcard-pct" id="t-td-pct" style="color:var(--td)">—</span><span class="badge" id="t-td-badge">of target</span></div>
        <div class="pcard-note" id="t-td-note"></div>
      </div>
      <div class="pcard mdl">
        <div class="pcard-name">MDLive</div>
        <div class="pcard-hero"><div class="pcard-val" style="color:var(--mdl)" id="t-mdl-v">—</div><div class="pcard-target">/ 5</div></div>
        <div class="prog"><div class="prog-fill" id="t-mdl-bar" style="background:var(--mdl)"></div></div>
        <div class="pcard-foot"><span class="pcard-pct" id="t-mdl-pct" style="color:var(--mdl)">—</span><span class="badge" id="t-mdl-badge">of target</span></div>
        <div class="pcard-note" id="t-mdl-note"></div>
      </div>
    </div>
    <div class="rev-wrap">
      <div class="rev-hd"><span class="rev-title">Daily Revenue</span><span class="rev-goal">Goal <strong style="color:var(--text2)">$1,380</strong></span></div>
      <div class="rev-rail">
        <div class="rev-bg"><div class="rev-fill" id="t-rev-fill"></div></div>
        <div class="rev-badge" id="t-rev-badge" style="left:2%;color:var(--rev);border-color:var(--rev)">$0 · 0%</div>
      </div>
      <div class="rev-ends"><span>$0</span><span>$1,380</span></div>
    </div>
  </div>


<!-- 30-DAY GOALS -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Rolling 30-Day Goals</span>
      <span class="sec-note" id="goals-note">last 30 days</span>
    </div>
    <div class="goals-grid">
      <div class="gcard">
        <div class="gcard-top"><span class="gcard-name">Roman</span><span class="badge" id="g-rh-badge" style="background:rgba(45,212,160,.12);color:var(--rh)">—%</span></div>
        <div class="gcard-nums"><div class="gcard-val" style="color:var(--rh)" id="g-rh-v">—</div><div class="gcard-of">/ 3,750</div></div>
        <div class="prog"><div class="prog-fill" id="g-rh-bar" style="background:var(--rh)"></div></div>
        <div class="gcard-note" id="g-rh-note"></div>
      </div>
      <div class="gcard">
        <div class="gcard-top"><span class="gcard-name">Teladoc</span><span class="badge" id="g-td-badge" style="background:rgba(123,158,240,.12);color:var(--td)">—%</span></div>
        <div class="gcard-nums"><div class="gcard-val" style="color:var(--td)" id="g-td-v">—</div><div class="gcard-of">/ 150</div></div>
        <div class="prog"><div class="prog-fill" id="g-td-bar" style="background:var(--td)"></div></div>
        <div class="gcard-note" id="g-td-note"></div>
      </div>
      <div class="gcard">
        <div class="gcard-top"><span class="gcard-name">MDLive</span><span class="badge" id="g-mdl-badge" style="background:rgba(245,166,35,.12);color:var(--mdl)">—%</span></div>
        <div class="gcard-nums"><div class="gcard-val" style="color:var(--mdl)" id="g-mdl-v">—</div><div class="gcard-of">/ 150</div></div>
        <div class="prog"><div class="prog-fill" id="g-mdl-bar" style="background:var(--mdl)"></div></div>
        <div class="gcard-note" id="g-mdl-note"></div>
      </div>
    </div>
    <div class="rev-wrap">
      <div class="rev-hd"><span class="rev-title">Rolling 30-Day Revenue</span><span class="rev-goal">Goal <strong style="color:var(--text2)">$41,400</strong></span></div>
      <div class="rev-rail">
        <div class="rev-bg"><div class="rev-fill" id="g-rev-fill"></div></div>
        <div class="rev-badge" id="g-rev-badge" style="left:2%;color:var(--rev);border-color:var(--rev)">$0 · 0%</div>
      </div>
      <div class="rev-ends"><span>$0</span><span>$41,400</span></div>
    </div>
  </div>


</div>

<div class="footer">
  <span>Last refreshed: <span id="last-updated">—</span></span>
  <span>Auto-refreshes every 60 s</span>
</div>

<script>
const APPS_SCRIPT_URL="https://script.google.com/macros/s/AKfycbwVT-Xukwe3rxiPAnaMGK8ywEVf0Tfnm9U6cTQo9Ts6DX3DOoGQNcgvHj2pnCAO0_N3/exec";
const RATES={rh:12,td1:23,td2:28,mdl1:25,mdl2:28};
const GOALS={rh:100,td:5,mdl:5,dayRev:1380,moRh:3750,moTd:150,moMdl:150,moRev:41400};
let ALL_DATA={};
let revChart=null,volChart=null,stateChart=null,histChart=null;

/* ── CSV PARSING ── */
function parseCSVRow(r){const cols=[];let cur='',inQ=false;for(const ch of r){if(ch==='"'){inQ=!inQ;continue;}if(ch===','&&!inQ){cols.push(cur.trim());cur='';}else cur+=ch;}cols.push(cur.trim());return cols;}
function parseDateCol(h){
  let m;
  m=h.match(/^(\d{1,2})-(\d{1,2})-(\d{4})$/);if(m)return{month:+m[1],day:+m[2],year:+m[3]};
  m=h.match(/([A-Za-z]{3})\s+(\d{1,2})\s+(\d{4})/);if(m){const mo={jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12}[m[1].toLowerCase()];if(mo)return{month:mo,day:+m[2],year:+m[3]};}
  m=h.match(/^(\d{1,2})-([A-Za-z]{3})$/i);if(m){const mo={jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12}[m[2].toLowerCase()];if(mo)return{month:mo,day:+m[1],year:2026};}
  m=h.match(/^(\d{1,2})\/(\d{1,2})(?:\/(\d{4}))?$/);if(m)return{month:+m[1],day:+m[2],year:m[3]?+m[3]:2026};
  return null;
}
function monthYearFromTabName(n){const moMap={jan:1,feb:2,mar:3,apr:4,may:5,jun:6,jul:7,aug:8,sep:9,oct:10,nov:11,dec:12};const m=n.trim().match(/^([A-Za-z]{3})[\s-]*(\d{2,4})$/i);if(m){const mo=moMap[m[1].toLowerCase()];if(mo){let yr=+m[2];if(yr<100)yr+=yr<50?2000:1900;return{month:mo,year:yr};}}return null;}
function yearFromTabName(n){let m;m=n.trim().match(/(\d{4})/);if(m)return+m[1];m=n.trim().match(/(\d{2})\s*$/);if(m){const y=+m[1];return y<50?2000+y:1900+y;}return null;}
function parseSheetCSV(text,tabName){
  const rows=text.trim().split('\n').map(parseCSVRow);if(rows.length<2)return null;
  const headers=rows[0],nameDate=tabName?monthYearFromTabName(tabName):null,allDC=[];
  headers.forEach((h,i)=>{if(i===0)return;const d=parseDateCol(h.trim());if(!d)return;if(nameDate){if(d.month===nameDate.month)allDC.push({i,...d});}else allDC.push({i,...d});});
  const dateCols=allDC.length>0?allDC:(()=>{const all=[];headers.forEach((h,i)=>{if(i===0)return;const d=parseDateCol(h.trim());if(d)all.push({i,...d});});return all;})();
  if(!dateCols.length)return null;
  dateCols.sort((a,b)=>a.day-b.day);
  const n=dateCols.length,labels=dateCols.map(d=>d.month+'/'+d.day);
  const rh=new Array(n).fill(0),td1=new Array(n).fill(0),td2=new Array(n).fill(0),mdl1=new Array(n).fill(0),mdl2=new Array(n).fill(0),rev=new Array(n).fill(0);
  let tdc=0,mc=0;
  for(const row of rows.slice(1)){
    const nm=(row[0]||'').trim().toUpperCase().replace(/\s+/g,'');if(!nm)continue;
    dateCols.forEach((col,j)=>{const raw=(row[col.i]||'').toString().replace(/#[A-Z!]+/g,'').replace(/[$, ]/g,'');const v=parseFloat(raw)||0;if(nm==='RO'||nm==='RO+')rh[j]+=v;else if(nm==='TOT'||nm==='TOTAL')rev[j]+=v;});
    const isTD=nm.startsWith('TD'),isMDL=nm==='MD'||nm==='MDL'||nm==='ML'||nm.startsWith('MLV')||nm.startsWith('MDL');
    if(isTD){tdc++;dateCols.forEach((col,j)=>{const v=parseFloat((row[col.i]||'').replace(/[$, ]/g,''))||0;if(tdc===1)td1[j]+=v;else td2[j]+=v;});}
    if(isMDL){mc++;dateCols.forEach((col,j)=>{const v=parseFloat((row[col.i]||'').replace(/[$, ]/g,''))||0;if(mc===1)mdl1[j]+=v;else mdl2[j]+=v;});}
  }
  const td=td1.map((v,i)=>v+td2[i]),mdl=mdl1.map((v,i)=>v+mdl2[i]);
  let lastIdx=-1;for(let j=0;j<n;j++)if(rh[j]>0||td[j]>0||mdl[j]>0||rev[j]>0)lastIdx=j;
  if(lastIdx<0)return null;const t=lastIdx+1;
  const sm=nameDate?nameDate.month:dateCols[0].month,sy=nameDate?nameDate.year:dateCols[0].year;
  return{labels:labels.slice(0,t),rh:rh.slice(0,t),td:td.slice(0,t),td1:td1.slice(0,t),td2:td2.slice(0,t),mdl:mdl.slice(0,t),mdl1:mdl1.slice(0,t),mdl2:mdl2.slice(0,t),rev:rev.slice(0,t),rhRev:rh.slice(0,t).map(v=>Math.round(v*RATES.rh)),tdRev:td1.slice(0,t).map((v,i)=>Math.round(v*RATES.td1+td2[i]*RATES.td2)),mdlRev:mdl1.slice(0,t).map((v,i)=>Math.round(v*RATES.mdl1+mdl2[i]*RATES.mdl2)),month:sm,year:sy};
}

/* ── DATA HELPERS ── */
function getRolling30(){
  // Only include days within the last 30 calendar days from today
  const cutoff=new Date(); cutoff.setDate(cutoff.getDate()-30); cutoff.setHours(0,0,0,0);
  const collect=(minDate)=>{
    const all=[];
    Object.entries(ALL_DATA).forEach(([,d])=>{
      d.labels.forEach((label,i)=>{
        const p=label.split('/'); if(p.length<2)return;
        const date=new Date(d.year,+p[0]-1,+p[1]);
        if(minDate && date<minDate)return;
        all.push({date,label,rh:d.rh[i]||0,td:d.td[i]||0,mdl:d.mdl[i]||0,
          rhRev:(d.rhRev||[])[i]||0,tdRev:(d.tdRev||[])[i]||0,mdlRev:(d.mdlRev||[])[i]||0});
      });
    });
    all.sort((a,b)=>a.date-b.date);
    return all;
  };
  const recent=collect(cutoff);
  // If there's no data in the last 30 calendar days (e.g. sheet is behind),
  // fall back to the most recent 30 data points so the dashboard never goes blank
  return recent.length ? recent : collect(null).slice(-30);
}
function getLatestDay(){
  const sh=Object.values(ALL_DATA);if(!sh.length)return null;
  sh.sort((a,b)=>a.year!==b.year?a.year-b.year:a.month-b.month);
  const cur=sh[sh.length-1],i=cur.labels.length-1;
  return{rh:cur.rh[i]||0,td:cur.td[i]||0,mdl:cur.mdl[i]||0,rhRev:(cur.rhRev||[])[i]||0,tdRev:(cur.tdRev||[])[i]||0,mdlRev:(cur.mdlRev||[])[i]||0};
}
function isBeforeThreePST(){
  return +new Date().toLocaleString('en-US',{timeZone:'America/Los_Angeles',hour:'numeric',hour12:false})<15;
}

/* ── DOM HELPERS ── */
function set(id,v){const el=document.getElementById(id);if(el)el.textContent=v;}
function st(id,p,v){const el=document.getElementById(id);if(el)el.style[p]=v;}
function bStyle(pct){const hit=pct>=100,near=pct>=80;return{c:hit?'#22c55e':near?'#f5a623':'#f87171',bg:hit?'rgba(34,197,94,.13)':near?'rgba(245,166,35,.13)':'rgba(248,113,113,.13)',t:hit?'✓ On track':near?'Almost there':'Below target'};}

/* ── CHART CONFIG ── */
const GRID='#1e2a3a';
Chart.defaults.color='#64748b';Chart.defaults.font.family="'DM Sans',sans-serif";Chart.defaults.font.size=11;
const TT={backgroundColor:'#1c2333',borderColor:'#2d3a50',borderWidth:1,titleColor:'#94a3b8',bodyColor:'#e2e8f0',padding:10};

/* ── UPDATERS ── */
function updateToday(){
  const early=isBeforeThreePST();
  const now=new Date();
  const pt=now.toLocaleString('en-US',{timeZone:'America/Los_Angeles',weekday:'short',month:'short',day:'numeric',hour:'numeric',minute:'2-digit'});
  set('today-line',early?pt+' PT · data locks at 3 PM':pt+' PT');
  const day=getLatestDay();
  const rv=early?0:(day?.rh||0),tv=early?0:(day?.td||0),mv=early?0:(day?.mdl||0);
  const dayRev=early?0:((day?.rhRev||0)+(day?.tdRev||0)+(day?.mdlRev||0));
  function fillCard(k,val,tgt){
    const pct=Math.round(val/tgt*100),b=bStyle(pct);
    set('t-'+k+'-v',val);set('t-'+k+'-pct',pct+'%');
    st('t-'+k+'-pct','color',b.c);st('t-'+k+'-bar','width',Math.min(pct,100)+'%');st('t-'+k+'-bar','background',b.c);
    const bd=document.getElementById('t-'+k+'-badge');if(bd){bd.textContent=b.t;bd.style.background=b.bg;bd.style.color=b.c;}
    set('t-'+k+'-note',val>=tgt?(val-tgt)+' above target 🎉':(tgt-val)+' more to hit target');
  }
  fillCard('rh',rv,GOALS.rh);fillCard('td',tv,GOALS.td);fillCard('mdl',mv,GOALS.mdl);
  const rp=Math.min(Math.round(dayRev/GOALS.dayRev*100),100),rc=rp>=100?'#22c55e':rp>=75?'#f5a623':'#f87171';
  st('t-rev-fill','width',rp+'%');
  const rb=document.getElementById('t-rev-badge');
  if(rb){rb.style.left=Math.max(rp,3)+'%';rb.style.color=rc;rb.style.borderColor=rc;rb.textContent='$'+Math.round(dayRev).toLocaleString()+' · '+rp+'%';}
}

function updateGoals(){
  const days=getRolling30();if(!days.length)return;
  const tRH=days.reduce((a,d)=>a+d.rh,0),tTD=days.reduce((a,d)=>a+d.td,0),tMDL=days.reduce((a,d)=>a+d.mdl,0);
  const tRev=days.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev,0);
  set('goals-note','last '+days.length+' days');
  function fillGoal(k,val,tgt){
    const pct=Math.round(val/tgt*100),b=bStyle(pct);
    set('g-'+k+'-v',val.toLocaleString());st('g-'+k+'-bar','width',Math.min(pct,100)+'%');st('g-'+k+'-bar','background',b.c);
    const bd=document.getElementById('g-'+k+'-badge');if(bd){bd.textContent=pct+'%';bd.style.color=b.c;bd.style.background=b.bg;}
    set('g-'+k+'-note',val>=tgt?(val-tgt).toLocaleString()+' above target 🎉':(tgt-val).toLocaleString()+' to go');
  }
  fillGoal('rh',tRH,GOALS.moRh);fillGoal('td',tTD,GOALS.moTd);fillGoal('mdl',tMDL,GOALS.moMdl);
  const mp=Math.min(Math.round(tRev/GOALS.moRev*100),100),mc=mp>=100?'#22c55e':mp>=75?'#f5a623':'#f87171';
  st('g-rev-fill','width',mp+'%');
  const gb=document.getElementById('g-rev-badge');
  if(gb){gb.style.left=Math.max(mp,3)+'%';gb.style.color=mc;gb.style.borderColor=mc;gb.textContent='$'+Math.round(tRev).toLocaleString()+' · '+mp+'%';}
}

function updateSummary(){
  const days=getRolling30();if(!days.length)return;
  const tRH=days.reduce((a,d)=>a+d.rh,0),tTD=days.reduce((a,d)=>a+d.td,0),tMDL=days.reduce((a,d)=>a+d.mdl,0);
  const tEnc=tRH+tTD+tMDL,tRev=days.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev,0),n=days.length||1;
  set('s-enc',tEnc.toLocaleString());set('s-enc-sub','RH '+tRH.toLocaleString()+' · TD '+tTD+' · MDL '+tMDL);
  set('s-rev','$'+Math.round(tRev).toLocaleString());set('s-rev-sub','$'+Math.round(tRev/n).toLocaleString()+'/day avg');
  set('s-avg',(tRH/n).toFixed(1));set('s-days',n);
  const first=days[0],last=days[n-1];set('s-range',first.label+' – '+last.label);
}

function updateCharts(){
  const days=getRolling30();if(!days.length)return;
  const early=isBeforeThreePST(),lastI=days.length-1;
  const nullLast=(v,i)=>i===lastI&&early?null:v;
  const labels=days.map(d=>d.label);

  // Revenue line chart
  if(revChart)revChart.destroy();
  revChart=new Chart(document.getElementById('revChart').getContext('2d'),{type:'line',
    data:{labels,datasets:[
      {label:'Roman',  data:days.map((d,i)=>nullLast(d.rhRev,i)), borderColor:'#2dd4a0',backgroundColor:'rgba(45,212,160,.07)',fill:true, tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5,spanGaps:false},
      {label:'Teladoc',data:days.map((d,i)=>nullLast(d.tdRev,i)), borderColor:'#7b9ef0',backgroundColor:'transparent',              fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2,  spanGaps:false},
      {label:'MDLive', data:days.map((d,i)=>nullLast(d.mdlRev,i)),borderColor:'#f5a623',backgroundColor:'transparent',              fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2,  spanGaps:false},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...TT,callbacks:{
        label:i=>' '+i.dataset.label+': $'+(i.raw||0).toLocaleString(),
        footer:items=>'Total: $'+items.reduce((a,i)=>a+(i.raw||0),0).toLocaleString()
      }}},
      scales:{x:{grid:{color:GRID},ticks:{maxRotation:45,autoSkip:true,maxTicksLimit:8}},
              y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(1)+'k':v)}}}}
  });

  // State volume chart — static April data, only build once
  if(!stateChart) buildStateChart();
}

function buildStateChart(){
  const states=[
    {s:'TX',v:860},{s:'NC',v:573},{s:'AZ',v:432},{s:'OR',v:322},
    {s:'IN',v:244},{s:'TN',v:155},{s:'WA',v:87}, {s:'AL',v:56},
    {s:'OH',v:23}, {s:'IL',v:19}, {s:'MI',v:13}, {s:'PA',v:0},
  ];
  const max=860;
  // Purple ramp: lightest for lowest, darkest for highest
  const clr=v=>{
    if(v===0)return '#3C3489';
    const t=v/max;
    if(t>0.75)return '#534AB7';
    if(t>0.5) return '#7F77DD';
    if(t>0.25)return '#AFA9EC';
    return '#CECBF6';
  };
  const el=document.getElementById('stateChart');if(!el)return;
  stateChart=new Chart(el.getContext('2d'),{
    type:'bar',
    data:{
      labels:states.map(d=>d.s),
      datasets:[{
        data:states.map(d=>d.v),
        backgroundColor:states.map(d=>d.v===0?'#2d3a50':clr(d.v)),
        borderWidth:0,
        borderRadius:3,
      }]
    },
    options:{
      responsive:true,maintainAspectRatio:false,
      plugins:{
        legend:{display:false},
        tooltip:{...TT,callbacks:{
          label:i=>i.raw===0?' PA — active, not yet seeing members':' '+i.raw.toLocaleString()+' encounters',
          footer:i=>{const pct=Math.round(i[0].raw/2784*100);return i[0].raw>0?pct+'% of April total':''}
        }}
      },
      scales:{
        x:{grid:{display:false},ticks:{color:'#94a3b8',font:{size:11,weight:'500'},maxRotation:0}},
        y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>v>=1000?(v/1000).toFixed(1)+'k':v}}
      }
    }
  });
}

function updateHistChart(){
  const moN=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const sorted=Object.entries(ALL_DATA).filter(([,d])=>d.year>2025||(d.year===2025&&d.month>=1)).sort(([,a],[,b])=>a.year!==b.year?a.year-b.year:a.month-b.month);
  if(!sorted.length)return;
  const labels=sorted.map(([,d])=>moN[d.month-1]+' \''+String(d.year).slice(2));
  const rhR=[],tdR=[],mdlR=[];
  sorted.forEach(([,d])=>{
    rhR.push(Math.round(d.rh.reduce((a,b)=>a+b,0)*RATES.rh));
    tdR.push(Math.round((d.td1||[]).reduce((a,b)=>a+b,0)*RATES.td1+(d.td2||[]).reduce((a,b)=>a+b,0)*RATES.td2));
    mdlR.push(Math.round((d.mdl1||[]).reduce((a,b)=>a+b,0)*RATES.mdl1+(d.mdl2||[]).reduce((a,b)=>a+b,0)*RATES.mdl2));
  });
  // hist-note element removed; title is now in the card h3
  if(histChart)histChart.destroy();
  histChart=new Chart(document.getElementById('histChart').getContext('2d'),{type:'bar',
    data:{labels,datasets:[
      {label:'Roman',  data:rhR, backgroundColor:'rgba(45,212,160,.85)',borderWidth:0,borderRadius:3,stack:'s'},
      {label:'Teladoc',data:tdR, backgroundColor:'rgba(123,158,240,.85)',borderWidth:0,borderRadius:3,stack:'s'},
      {label:'MDLive', data:mdlR,backgroundColor:'rgba(245,166,35,.85)', borderWidth:0,borderRadius:3,stack:'s'},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...TT,callbacks:{label:i=>' '+i.dataset.label+': $'+i.raw.toLocaleString(),footer:items=>'Total: $'+items.reduce((a,i)=>a+i.raw,0).toLocaleString()}}},
      scales:{x:{stacked:true,grid:{color:GRID},ticks:{maxRotation:45}},y:{stacked:true,grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(0)+'k':v)}}}}
  });
}

function updateHeader(){
  const sh=Object.values(ALL_DATA).sort((a,b)=>a.year!==b.year?a.year-b.year:a.month-b.month);
  const moN=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  if(sh.length){const f=sh[0];set('date-range',moN[f.month-1]+' '+f.year+' – Present');}
}

function updateAll(){
  updateToday();updateGoals();updateSummary();updateCharts();updateHistChart();updateHeader();
}

/* ── CACHE ── */
const CK='tmd_c',CKT='tmd_t';
function saveCache(t){try{localStorage.setItem(CK,t);localStorage.setItem(CKT,Date.now().toString());}catch(e){}}
function loadCacheObj(){try{const ts=localStorage.getItem(CKT),t=localStorage.getItem(CK);if(!t||!ts)return null;return{text:t,age:Math.round((Date.now()-+ts)/60000)};}catch(e){return null;}}

function parseAll(text){
  const p={};
  if(text.trim().startsWith('{')){
    const json=JSON.parse(text);
    for(const[name,csv]of Object.entries(json)){const d=parseSheetCSV(csv,name);if(d){if(!d.year)d.year=yearFromTabName(name)||2026;p[name]=d;}}
  }else{const d=parseSheetCSV(text);if(d)p['Sheet']=d;}
  return p;
}

async function loadSheet(bg=false){
  if(!bg){
    const c=loadCacheObj();
    if(c){try{const p=parseAll(c.text);if(Object.keys(p).length){ALL_DATA=p;updateAll();set('live-label','Cached · '+c.age+'m ago');}}catch(e){}}
  }
  try{
    const r=await fetch(APPS_SCRIPT_URL+'?_='+Date.now(),{cache:'no-store',redirect:'follow'});
    if(!r.ok)throw new Error('HTTP '+r.status);
    const t=await r.text();if(!t||t.trim().length<10)throw new Error('empty');
    const p=parseAll(t);if(!Object.keys(p).length)throw new Error('No sheets parsed');
    saveCache(t);ALL_DATA=p;
    updateAll();
    document.getElementById('live-dot').classList.add('on');
    set('live-label','Live · '+new Date().toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'}));
    set('last-updated',new Date().toLocaleTimeString());
  }catch(e){
    document.getElementById('live-dot').classList.remove('on');
    set('live-label','Unavailable — '+e.message);set('last-updated','Failed');
    console.warn('loadSheet:',e.message);
  }
}

/* ── INIT ── */
// Show date/time immediately; cards populate once live data arrives
(()=>{
  const now=new Date();
  const pt=now.toLocaleString('en-US',{timeZone:'America/Los_Angeles',weekday:'short',month:'short',day:'numeric',hour:'numeric',minute:'2-digit'});
  const early=isBeforeThreePST();
  set('today-line', early ? pt+' PT · data locks at 3 PM' : pt+' PT');
  set('date-range','Connecting…');
})();
loadSheet();
setInterval(()=>loadSheet(true),60000);

function manualRefresh(btn){const o=btn.innerHTML;btn.innerHTML='↻ Refreshing…';btn.disabled=true;loadSheet(false).finally(()=>{btn.innerHTML=o;btn.disabled=false;});}
</script>
</body>
</html>
