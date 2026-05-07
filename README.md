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

/* SUMMARY STRIP */
.summary-strip{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.scard{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px}
.scard-label{font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);margin-bottom:10px;font-weight:500}
.scard-val{font-size:2.4rem;font-weight:800;line-height:1;letter-spacing:-.03em}
.scard-sub{font-size:.72rem;color:var(--text3);margin-top:6px}

/* CHARTS */
.chart-card{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:18px 20px}
.chart-card h3{font-size:.72rem;font-weight:600;color:var(--text);margin-bottom:12px}
.clegend{display:flex;gap:12px;flex-wrap:wrap;margin-top:10px}
.cleg{display:flex;align-items:center;gap:5px;font-size:.65rem;color:var(--text2)}
.cleg-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0}

/* GOAL STATUS WIDGET */
.gs-row{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.gs-pill{display:flex;flex-direction:column;gap:12px;background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px 24px;position:relative;overflow:hidden}
.gs-pill-top{display:flex;align-items:center;justify-content:space-between}
.gs-pill-name{font-size:.65rem;text-transform:uppercase;letter-spacing:.1em;color:var(--text3);font-weight:500}
.gs-pill-pct{font-size:2rem;font-weight:800;line-height:1;letter-spacing:-.03em}
.gs-pill-track{background:var(--border);border-radius:4px;height:7px;overflow:hidden}
.gs-pill-fill{height:100%;border-radius:4px;transition:width .9s ease;width:0}
.gs-pill-val{font-size:.72rem;color:var(--text3)}
.gs-pill-dot{display:none}

/* TWO-CHART ROW */
.two-charts{display:grid;grid-template-columns:1fr 1fr;gap:14px}

/* FOOTER */
.footer{display:flex;justify-content:space-between;flex-wrap:wrap;gap:6px;padding:10px 28px;font-size:.64rem;color:var(--text3)}

/* RESPONSIVE */
@media(max-width:900px){
  .two-charts{grid-template-columns:1fr}
  .summary-strip{grid-template-columns:1fr 1fr}
}
@media(max-width:768px){
  .header{padding:12px 16px;gap:8px}
  .header-left h1{font-size:1.1rem}
  .dash{padding:14px 16px;gap:16px}
  .summary-strip{grid-template-columns:1fr 1fr}
  .scard{padding:16px 18px}
  .scard-val{font-size:2rem}
  .gs-row{grid-template-columns:1fr 1fr 1fr}
  .gs-pill{padding:14px 16px}
  .gs-pill-pct{font-size:1.6rem}
  .two-charts{grid-template-columns:1fr}
  .rev-wrap{padding:14px 16px}
  .footer{padding:8px 16px}
}
@media(max-width:480px){
  html{font-size:13px}
  .header{padding:10px 12px}
  .header-left h1{font-size:1rem}
  .header-right .refresh-btn span{display:none}
  .header-right .live-pill{font-size:.65rem;padding:4px 9px}
  .dash{padding:10px 12px;gap:14px}
  .summary-strip{grid-template-columns:1fr !important}
  .scard{padding:14px 16px}
  .scard-val{font-size:2.2rem}
  .gs-row{grid-template-columns:1fr !important}
  .gs-pill{padding:14px 16px;gap:10px}
  .gs-pill-pct{font-size:1.6rem}
  .two-charts{grid-template-columns:1fr !important}
  .chart-card{padding:14px}
  .chart-card h3{font-size:.7rem}
  .rev-wrap{padding:12px 14px}
  .footer{padding:8px 12px;font-size:.6rem}
}
@media(max-width:360px){
  .scard-val{font-size:1.9rem}
  .gs-pill-pct{font-size:1.4rem}
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
        <div class="scard-label">Avg Daily · Roman</div>
        <div class="scard-val" id="s-avg">—</div>
        <div class="scard-sub" id="s-avg-prev" style="margin-top:4px">—</div>
        <div class="scard-sub" id="s-avg-sub" style="margin-top:2px">encounters / day</div>
      </div>
      <div class="scard">
        <div class="scard-label">Total Encounters · Last 30 Days</div>
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
    <div class="sec-hd"><span class="sec-label">Today vs Target</span><span class="sec-note" id="gs-days-note">—</span></div>
    <div class="gs-row">
      <div class="gs-pill">
        <div class="gs-pill-top">
          <span class="gs-pill-name">Roman</span>
          <span class="gs-pill-pct" id="gs-rh-pct" style="color:var(--rh)">—</span>
        </div>
        <div class="gs-pill-track"><div class="gs-pill-fill" id="gs-rh-fill"></div></div>
        <span class="gs-pill-val" id="gs-rh-val">—</span>
      </div>
      <div class="gs-pill">
        <div class="gs-pill-top">
          <span class="gs-pill-name">Teladoc</span>
          <span class="gs-pill-pct" id="gs-td-pct" style="color:var(--td)">—</span>
        </div>
        <div class="gs-pill-track"><div class="gs-pill-fill" id="gs-td-fill"></div></div>
        <span class="gs-pill-val" id="gs-td-val">—</span>
      </div>
      <div class="gs-pill">
        <div class="gs-pill-top">
          <span class="gs-pill-name">MDLive</span>
          <span class="gs-pill-pct" id="gs-mdl-pct" style="color:var(--mdl)">—</span>
        </div>
        <div class="gs-pill-track"><div class="gs-pill-fill" id="gs-mdl-fill"></div></div>
        <span class="gs-pill-val" id="gs-mdl-val">—</span>
      </div>
    </div>
  </div>

  <!-- DAILY REVENUE -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Daily Revenue</span>
      <span class="sec-note" id="today-line">—</span>
    </div>
    <div class="rev-wrap">
      <div class="rev-hd"><span class="rev-title">Today vs Goal</span><span class="rev-goal">Goal <strong style="color:var(--text2)">$1,500</strong></span></div>
      <div class="rev-rail">
        <div class="rev-bg"><div class="rev-fill" id="t-rev-fill"></div></div>
        <div class="rev-badge" id="t-rev-badge" style="left:2%;color:var(--rev);border-color:var(--rev)">$0 · 0%</div>
      </div>
      <div class="rev-ends"><span>$0</span><span>$1,500</span></div>
    </div>
  </div>

  <!-- CHARTS -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Charts</span>
      <span class="sec-note">RH $12 · TD phone $23 / video $28 · MDL phone $25 / video $28</span>
    </div>
    <div class="two-charts">
      <div class="chart-card">
        <h3>Revenue — Last 30 Days</h3>
        <div style="position:relative;height:260px"><canvas id="revChart"></canvas></div>
        <div class="clegend">
          <div class="cleg"><div class="cleg-dot" style="background:#2dd4a0"></div>Roman</div>
          <div class="cleg"><div class="cleg-dot" style="background:#7b9ef0"></div>Teladoc</div>
          <div class="cleg"><div class="cleg-dot" style="background:#f5a623"></div>MDLive</div>
        </div>
      </div>
      <div class="chart-card">
        <h3>Monthly Revenue — Jan 2025+ <span id="proj-note" style="font-weight:400;color:var(--text3)"></span></h3>
        <div style="position:relative;height:260px"><canvas id="histChart"></canvas></div>
        <div class="clegend">
          <div class="cleg"><div class="cleg-dot" style="background:#2dd4a0"></div>Roman</div>
          <div class="cleg"><div class="cleg-dot" style="background:#7b9ef0"></div>Teladoc</div>
          <div class="cleg"><div class="cleg-dot" style="background:#f5a623"></div>MDLive</div>
          <div class="cleg" id="proj-legend" style="display:none"><div class="cleg-dot" style="background:rgba(255,255,255,.2);border:1px solid rgba(255,255,255,.3)"></div>Projected</div>
        </div>
      </div>
    </div>
  </div>

  <!-- 30-DAY REVENUE -->
  <div>
    <div class="sec-hd">
      <span class="sec-label">Rolling 30-Day Revenue</span>
      <span class="sec-note" id="goals-note">last 30 days</span>
    </div>
    <div class="rev-wrap">
      <div class="rev-hd"><span class="rev-title">30-Day vs Goal</span><span class="rev-goal">Goal <strong style="color:var(--text2)">$45,000</strong></span></div>
      <div class="rev-rail">
        <div class="rev-bg"><div class="rev-fill" id="g-rev-fill"></div></div>
        <div class="rev-badge" id="g-rev-badge" style="left:2%;color:var(--rev);border-color:var(--rev)">$0 · 0%</div>
      </div>
      <div class="rev-ends"><span>$0</span><span>$45,000</span></div>
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
const GOALS={rh:100,td:5,mdl:5,dayRev:1500,moRh:3750,moTd:150,moMdl:150,moRev:45000};
let ALL_DATA={};
let revChart=null,histChart=null;

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
  const cutoff=new Date();cutoff.setDate(cutoff.getDate()-30);cutoff.setHours(0,0,0,0);
  const collect=(minDate)=>{
    const all=[];
    Object.entries(ALL_DATA).forEach(([,d])=>{
      d.labels.forEach((label,i)=>{
        const p=label.split('/');if(p.length<2)return;
        const date=new Date(d.year,+p[0]-1,+p[1]);
        if(minDate&&date<minDate)return;
        all.push({date,label,rh:d.rh[i]||0,td:d.td[i]||0,mdl:d.mdl[i]||0,
          rhRev:(d.rhRev||[])[i]||0,tdRev:(d.tdRev||[])[i]||0,mdlRev:(d.mdlRev||[])[i]||0});
      });
    });
    all.sort((a,b)=>a.date-b.date);return all;
  };
  const recent=collect(cutoff);
  return recent.length?recent:collect(null).slice(-30);
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
function getPrev30(){
  const all=[];
  const cutoff=new Date();cutoff.setDate(cutoff.getDate()-30);cutoff.setHours(0,0,0,0);
  const prevCutoff=new Date();prevCutoff.setDate(prevCutoff.getDate()-60);prevCutoff.setHours(0,0,0,0);
  Object.entries(ALL_DATA).forEach(([,d])=>{
    d.labels.forEach((label,i)=>{
      const p=label.split('/');if(p.length<2)return;
      const date=new Date(d.year,+p[0]-1,+p[1]);
      if(date>=prevCutoff&&date<cutoff)
        all.push({date,rh:d.rh[i]||0,td:d.td[i]||0,mdl:d.mdl[i]||0,
          rhRev:(d.rhRev||[])[i]||0,tdRev:(d.tdRev||[])[i]||0,mdlRev:(d.mdlRev||[])[i]||0});
    });
  });
  return all;
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
  const now=new Date();
  const pt=now.toLocaleString('en-US',{timeZone:'America/Los_Angeles',weekday:'short',month:'short',day:'numeric',hour:'numeric',minute:'2-digit'});
  set('today-line',pt+' PT');
  const day=getLatestDay();
  const dayRev=(day?.rhRev||0)+(day?.tdRev||0)+(day?.mdlRev||0);
  const rp=Math.min(Math.round(dayRev/GOALS.dayRev*100),100),rc=rp>=100?'#22c55e':rp>=75?'#f5a623':'#f87171';
  st('t-rev-fill','width',rp+'%');
  const rb=document.getElementById('t-rev-badge');
  if(rb){rb.style.left=Math.max(rp,3)+'%';rb.style.color=rc;rb.style.borderColor=rc;rb.textContent='$'+Math.round(dayRev).toLocaleString()+' · '+rp+'%';}
}

function updateGoalStatus(){
  const day=getLatestDay();if(!day)return;
  const now=new Date();
  const pt=now.toLocaleString('en-US',{timeZone:'America/Los_Angeles',hour:'numeric',minute:'2-digit'});
  set('gs-days-note',pt+' PT');
  function fill(k,val,target){
    const pct=Math.round(val/target*100);
    const c=pct>=100?'#22c55e':pct>=75?'#f5a623':'#f87171';
    set('gs-'+k+'-val',val+' / '+target);
    set('gs-'+k+'-pct',pct+'%');
    const f=document.getElementById('gs-'+k+'-fill');
    if(f){f.style.width=Math.min(pct,100)+'%';f.style.background=c;}
    const p=document.getElementById('gs-'+k+'-pct');
    if(p)p.style.color=c;
  }
  fill('rh',day.rh||0,GOALS.rh);
  fill('td',day.td||0,GOALS.td);
  fill('mdl',day.mdl||0,GOALS.mdl);
}

function updateGoals(){
  const days=getRolling30();if(!days.length)return;
  const tRev=days.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev,0);
  set('goals-note','last '+days.length+' days');
  const mp=Math.min(Math.round(tRev/GOALS.moRev*100),100),mc=mp>=100?'#22c55e':mp>=75?'#f5a623':'#f87171';
  st('g-rev-fill','width',mp+'%');
  const gb=document.getElementById('g-rev-badge');
  if(gb){gb.style.left=Math.max(mp,3)+'%';gb.style.color=mc;gb.style.borderColor=mc;gb.textContent='$'+Math.round(tRev).toLocaleString()+' · '+mp+'%';}
}

function updateSummary(){
  const days=getRolling30();if(!days.length)return;
  const prev=getPrev30();
  const tRH=days.reduce((a,d)=>a+d.rh,0),tTD=days.reduce((a,d)=>a+d.td,0),tMDL=days.reduce((a,d)=>a+d.mdl,0);
  const tEnc=tRH+tTD+tMDL,tRev=days.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev,0),n=days.length||1;
  const pRH=prev.reduce((a,d)=>a+d.rh,0),pEnc=prev.reduce((a,d)=>a+d.rh+d.td+d.mdl,0);
  const pRev=prev.reduce((a,d)=>a+d.rhRev+d.tdRev+d.mdlRev,0),pN=prev.length||1;

  const avgRH=Math.round(tRH/n),prevAvgRH=Math.round(pRH/pN);
  const rhUp=avgRH>prevAvgRH,rhSame=avgRH===prevAvgRH;
  const rhColor=rhUp?'#22c55e':rhSame?'var(--text2)':'#f87171';
  const rhArrow=rhUp?'↑':rhSame?'→':'↓';
  const avgEl=document.getElementById('s-avg');
  if(avgEl){avgEl.textContent=avgRH.toLocaleString();avgEl.style.color=rhColor;}
  set('s-avg-prev',rhArrow+' '+prevAvgRH+' prev · '+rhArrow+' '+Math.abs(avgRH-prevAvgRH)+(rhUp?' more':rhSame?'':' less')+'/day');
  const avgPrevEl=document.getElementById('s-avg-prev');if(avgPrevEl)avgPrevEl.style.color=rhColor;

  const encUp=tEnc>pEnc,encSame=tEnc===pEnc;
  const encColor=encUp?'#22c55e':encSame?'var(--text2)':'#f87171';
  const encArrow=encUp?'↑':encSame?'→':'↓';
  const encEl=document.getElementById('s-enc');
  if(encEl){encEl.textContent=tEnc.toLocaleString();encEl.style.color=encColor;}
  set('s-enc-prev',encArrow+' '+pEnc.toLocaleString()+' prev · '+encArrow+' '+Math.abs(tEnc-pEnc).toLocaleString()+(encUp?' more':encSame?'':' less'));
  const encPrevEl=document.getElementById('s-enc-prev');if(encPrevEl)encPrevEl.style.color=encColor;
  set('s-enc-sub','RH '+tRH.toLocaleString()+' · TD '+tTD+' · MDL '+tMDL);

  const avgRev=Math.round(tRev/n),prevAvgRev=Math.round(pRev/pN);
  const revUp=avgRev>prevAvgRev,revSame=avgRev===prevAvgRev;
  const revColor=revUp?'#22c55e':revSame?'var(--text2)':'#f87171';
  const revArrow=revUp?'↑':revSame?'→':'↓';
  const revEl=document.getElementById('s-rev');
  if(revEl){revEl.textContent='$'+avgRev.toLocaleString();revEl.style.color=revColor;}
  set('s-rev-prev',revArrow+' $'+prevAvgRev.toLocaleString()+' prev · '+revArrow+' $'+Math.abs(avgRev-prevAvgRev).toLocaleString()+(revUp?' more':revSame?'':' less')+'/day');
  const revPrevEl=document.getElementById('s-rev-prev');if(revPrevEl)revPrevEl.style.color=revColor;
}

function updateCharts(){
  const days=getRolling30();if(!days.length)return;
  const early=isBeforeThreePST(),lastI=days.length-1;
  const nullLast=(v,i)=>i===lastI&&early?null:v;
  const labels=days.map(d=>d.label);
  if(revChart)revChart.destroy();
  revChart=new Chart(document.getElementById('revChart').getContext('2d'),{type:'line',
    data:{labels,datasets:[
      {label:'Roman',  data:days.map((d,i)=>nullLast(d.rhRev,i)),borderColor:'#2dd4a0',backgroundColor:'rgba(45,212,160,.07)',fill:true,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2.5,spanGaps:false},
      {label:'Teladoc',data:days.map((d,i)=>nullLast(d.tdRev,i)),borderColor:'#7b9ef0',backgroundColor:'transparent',fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2,spanGaps:false},
      {label:'MDLive', data:days.map((d,i)=>nullLast(d.mdlRev,i)),borderColor:'#f5a623',backgroundColor:'transparent',fill:false,tension:.4,pointRadius:0,pointHoverRadius:4,borderWidth:2,spanGaps:false},
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{display:false},tooltip:{...TT,callbacks:{
        label:i=>' '+i.dataset.label+': $'+(i.raw||0).toLocaleString(),
        footer:items=>'Total: $'+items.reduce((a,i)=>a+(i.raw||0),0).toLocaleString()
      }}},
      scales:{x:{grid:{color:GRID},ticks:{maxRotation:45,autoSkip:true,maxTicksLimit:8}},
              y:{grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?Math.round(v/1000)+'k':v)}}}}
  });
}

function updateHistChart(){
  const moN=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const sorted=Object.entries(ALL_DATA)
    .filter(([,d])=>d.year>2025||(d.year===2025&&d.month>=1))
    .sort(([,a],[,b])=>a.year!==b.year?a.year-b.year:a.month-b.month);
  if(!sorted.length)return;

  const now=new Date();
  const curMonth=now.getMonth()+1,curYear=now.getFullYear();
  const daysInMonth=new Date(curYear,curMonth,0).getDate();
  const dayOfMonth=now.getDate();

  const labels=[],rhR=[],tdR=[],mdlR=[],projR=[];
  sorted.forEach(([,d])=>{
    labels.push(moN[d.month-1]+" '"+String(d.year).slice(2));
    const aRH=Math.round(d.rh.reduce((a,b)=>a+b,0)*RATES.rh);
    const aTD=Math.round((d.td1||[]).reduce((a,b)=>a+b,0)*RATES.td1+(d.td2||[]).reduce((a,b)=>a+b,0)*RATES.td2);
    const aMDL=Math.round((d.mdl1||[]).reduce((a,b)=>a+b,0)*RATES.mdl1+(d.mdl2||[]).reduce((a,b)=>a+b,0)*RATES.mdl2);
    rhR.push(aRH);tdR.push(aTD);mdlR.push(aMDL);
    const isCur=d.month===curMonth&&d.year===curYear;
    if(isCur&&dayOfMonth>0&&dayOfMonth<daysInMonth){
      const rate=(aRH+aTD+aMDL)/dayOfMonth;
      projR.push(Math.round(rate*(daysInMonth-dayOfMonth)));
    } else {
      projR.push(null);
    }
  });

  const hasProj=projR.some(v=>v!==null);
  const projLegend=document.getElementById('proj-legend');
  const projNote=document.getElementById('proj-note');
  if(projLegend)projLegend.style.display=hasProj?'flex':'none';
  if(projNote&&hasProj){
    const dLeft=daysInMonth-dayOfMonth;
    projNote.textContent='· '+dLeft+' days projected';
  }

  if(histChart)histChart.destroy();
  histChart=new Chart(document.getElementById('histChart').getContext('2d'),{type:'bar',
    data:{labels:[...labels],datasets:[
      {label:'Roman',   data:rhR,  backgroundColor:'rgba(45,212,160,.85)',borderWidth:0,borderRadius:0,stack:'s'},
      {label:'Teladoc', data:tdR,  backgroundColor:'rgba(123,158,240,.85)',borderWidth:0,borderRadius:0,stack:'s'},
      {label:'MDLive',  data:mdlR, backgroundColor:'rgba(245,166,35,.85)',borderWidth:0,borderRadius:0,stack:'s'},
      ...(hasProj?[{label:'Projected',data:projR,
        backgroundColor:'rgba(255,255,255,.1)',
        borderColor:'rgba(255,255,255,.25)',
        borderWidth:1,borderRadius:3,stack:'s'}]:[]),
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{
        legend:{display:false},
        tooltip:{...TT,callbacks:{
          label:i=>{
            if(i.raw===null||i.raw===0)return null;
            if(i.dataset.label==='Projected'){
              const act=i.chart.data.datasets.slice(0,3).reduce((a,ds)=>a+(ds.data[i.dataIndex]||0),0);
              return ' Est. month total: $'+(act+i.raw).toLocaleString()+' (+'+'$'+i.raw.toLocaleString()+' proj)';
            }
            return ' '+i.dataset.label+': $'+i.raw.toLocaleString();
          },
          footer:items=>{
            const act=items.filter(i=>i.dataset.label!=='Projected').reduce((a,i)=>a+(i.raw||0),0);
            const proj=items.find(i=>i.dataset.label==='Projected'&&i.raw);
            if(proj){const dLeft=daysInMonth-dayOfMonth;return 'Actual: $'+act.toLocaleString()+' | Est. total: $'+(act+proj.raw).toLocaleString()+' ('+dLeft+' days left)';}
            return 'Total: $'+act.toLocaleString();
          }
        }}
      },
      scales:{
        x:{stacked:true,grid:{color:GRID},ticks:{maxRotation:45}},
        y:{stacked:true,grid:{color:GRID},beginAtZero:true,ticks:{callback:v=>'$'+(v>=1000?(v/1000).toFixed(0)+'k':v)}}
      }
    }
  });
}

function updateHeader(){
  const sh=Object.values(ALL_DATA).sort((a,b)=>a.year!==b.year?a.year-b.year:a.month-b.month);
  const moN=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  if(sh.length){const f=sh[0];set('date-range',moN[f.month-1]+' '+f.year+' – Present');}
}

function updateAll(){
  updateToday();updateGoalStatus();updateGoals();updateSummary();updateCharts();updateHistChart();updateHeader();
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
(()=>{
  const now=new Date();
  const pt=now.toLocaleString('en-US',{timeZone:'America/Los_Angeles',weekday:'short',month:'short',day:'numeric',hour:'numeric',minute:'2-digit'});
  set('today-line',isBeforeThreePST()?pt+' PT · data locks at 3 PM':pt+' PT');
  set('date-range','Connecting…');
})();
loadSheet();
setInterval(()=>loadSheet(true),60000);

function manualRefresh(btn){const o=btn.innerHTML;btn.innerHTML='↻ Refreshing…';btn.disabled=true;loadSheet(false).finally(()=>{btn.innerHTML=o;btn.disabled=false;});}
</script>
</body>
</html>
