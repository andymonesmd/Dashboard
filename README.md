<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Telemedicine Activity Dashboard</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&display=swap');
  *, *::before, *::after { box-sizing:border-box; margin:0; padding:0; }
  :root {
    --roman:#4ade80; --mdlive:#f97316; --teladoc:#60a5fa;
    --bg:#0f1623; --card:#162032; --border:#1e3048;
    --muted:#4a6080; --text:#e2eaf4; --sub:#7a9ab8;
  }
  body { background:var(--bg); color:var(--text); font-family:'DM Mono','Courier New',monospace; min-height:100vh; padding:28px 24px; }
  .header { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:6px; flex-wrap:wrap; gap:12px; }
  h1 { font-size:26px; font-weight:700; letter-spacing:-0.5px; color:#e8f0fc; }
  .legend { display:flex; gap:14px; align-items:center; margin-top:7px; font-size:12px; color:var(--sub); flex-wrap:wrap; }
  .dot { width:9px; height:9px; border-radius:2px; display:inline-block; margin-right:4px; }
  .header-right { display:flex; align-items:center; gap:8px; flex-shrink:0; }
  .badge { background:#1a2a3a; border:1px solid var(--border); border-radius:20px; padding:6px 14px; font-size:12px; color:var(--sub); display:flex; align-items:center; gap:7px; white-space:nowrap; }
  .sdot { width:9px; height:9px; border-radius:50%; display:inline-block; transition:all .4s; }
  .sdot.loading { background:#f97316; }
  .sdot.live    { background:var(--roman); box-shadow:0 0 7px var(--roman); }
  .sdot.error   { background:#ef4444; }
  .btn { background:transparent; border:1px solid var(--border); border-radius:20px; padding:5px 14px; cursor:pointer; font-family:inherit; font-size:11px; color:var(--sub); transition:all .15s; }
  .btn:hover { border-color:var(--sub); color:var(--text); }
  .last-up { font-size:11px; color:var(--muted); margin-top:5px; min-height:16px; }
  .filter-bar { background:var(--card); border:1px solid var(--border); border-radius:12px; padding:14px 20px; margin-top:18px; margin-bottom:20px; display:flex; align-items:center; gap:10px; flex-wrap:wrap; }
  .filter-label { font-size:10px; font-weight:700; color:var(--muted); letter-spacing:1px; min-width:110px; }
  .pill { background:transparent; color:var(--sub); border:none; border-radius:20px; padding:5px 16px; cursor:pointer; font-family:inherit; font-size:12px; outline:none; transition:all .15s; }
  .pill.active { background:var(--teladoc); color:#fff; font-weight:700; }
  .kpi-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:16px; margin-bottom:20px; }
  @media(max-width:700px){ .kpi-grid{grid-template-columns:1fr;} }
  .kpi-card { background:var(--card); border:1px solid var(--border); border-radius:12px; padding:18px 22px; }
  .kpi-title { font-size:10px; font-weight:700; color:var(--muted); letter-spacing:1px; margin-bottom:13px; }
  .kpi-row { display:flex; gap:28px; flex-wrap:wrap; }
  .kpi-sub { font-size:11px; color:var(--sub); margin-bottom:5px; }
  .kpi-val { font-size:28px; font-weight:700; }
  .tabs { display:flex; gap:8px; flex-wrap:wrap; margin-bottom:20px; }
  .tab { background:transparent; color:var(--sub); border:1px solid var(--border); border-radius:8px; padding:7px 16px; cursor:pointer; font-family:inherit; font-size:12px; outline:none; white-space:nowrap; transition:all .15s; }
  .tab.active { background:var(--teladoc); color:#fff; border-color:var(--teladoc); font-weight:700; }
  .chart-panel { background:var(--card); border:1px solid var(--border); border-radius:14px; padding:24px 20px 18px; }
  .chart-hdr { display:flex; justify-content:space-between; align-items:center; margin-bottom:20px; flex-wrap:wrap; gap:8px; }
  .chart-title { font-size:15px; font-weight:700; color:#e8f0fc; }
  .live-pill { font-size:11px; color:var(--roman); margin-left:8px; }
  .chart-meta { font-size:11px; color:var(--sub); }
  .chart-footer { display:flex; align-items:center; justify-content:flex-end; gap:6px; margin-top:10px; font-size:11px; color:var(--muted); }
  .daily-kpis { display:grid; grid-template-columns:repeat(4,1fr); gap:12px; margin-bottom:22px; }
  @media(max-width:600px){ .daily-kpis{grid-template-columns:repeat(2,1fr);} }
  .dk { background:#0d1928; border-radius:10px; padding:14px 16px; border:1px solid var(--border); }
  .dk-label { font-size:10px; color:var(--muted); letter-spacing:1px; margin-bottom:5px; }
  .dk-val { font-size:22px; font-weight:700; }
  .dk-sub { font-size:10px; color:var(--muted); margin-top:3px; }
  .sec-label { font-size:12px; color:var(--sub); margin-bottom:8px; }
  .mt20 { margin-top:20px; }
  .conn-err { display:flex; flex-direction:column; align-items:center; justify-content:center; min-height:220px; gap:10px; text-align:center; padding:20px; }
  .ce-icon { font-size:32px; }
  .ce-msg { font-size:14px; color:#ef4444; font-weight:700; }
  .ce-detail { font-size:11px; color:var(--muted); max-width:460px; line-height:1.7; background:#0d1928; border:1px solid var(--border); border-radius:8px; padding:12px 16px; margin-top:4px; }
  .btn-try { background:var(--teladoc); color:#fff; border:none; border-radius:20px; padding:9px 24px; cursor:pointer; font-family:inherit; font-size:12px; margin-top:8px; }
</style>
</head>
<body>

<div class="header">
  <div>
    <h1>Telemedicine Activity Dashboard</h1>
    <div class="legend">
      <span>July 2024 – Present</span>
      <span><span class="dot" style="background:var(--roman)"></span>Roman Health</span>
      <span><span class="dot" style="background:var(--mdlive)"></span>MDLive</span>
      <span><span class="dot" style="background:var(--teladoc)"></span>Teladoc</span>
    </div>
  </div>
  <div class="header-right">
    <button class="btn" onclick="loadData()">↻ Refresh</button>
    <div class="badge">
      <span class="sdot loading" id="sDot"></span>
      <span id="sTxt">Connecting…</span>
    </div>
  </div>
</div>
<div class="last-up" id="lastUp"></div>

<div class="filter-bar">
  <span class="filter-label">FILTER BY YEAR:</span>
  <div style="display:flex;gap:4px;flex-wrap:wrap">
    <button class="pill active" onclick="setYear('All',this)">All</button>
    <button class="pill" onclick="setYear('24',this)">2024</button>
    <button class="pill" onclick="setYear('25',this)">2025</button>
    <button class="pill" onclick="setYear('26',this)">2026</button>
  </div>
</div>

<div class="kpi-grid">
  <div class="kpi-card">
    <div class="kpi-title">AVG PER MONTH</div>
    <div class="kpi-row">
      <div><div class="kpi-sub">Encounters</div><div class="kpi-val" id="kEnc" style="color:var(--teladoc)">—</div></div>
      <div><div class="kpi-sub">Revenue</div><div class="kpi-val" id="kRev" style="color:var(--mdlive)">—</div></div>
    </div>
  </div>
  <div class="kpi-card">
    <div class="kpi-title">AVG VISITS / MONTH</div>
    <div class="kpi-row">
      <div><div class="kpi-sub">Roman Health</div><div class="kpi-val" id="kRo" style="color:var(--roman)">—</div></div>
      <div><div class="kpi-sub">MDLive + Teladoc</div><div class="kpi-val" id="kMT" style="color:var(--teladoc)">—</div></div>
    </div>
  </div>
  <div class="kpi-card">
    <div class="kpi-title">AVG VISITS / DAY</div>
    <div class="kpi-row">
      <div><div class="kpi-sub">Roman Health</div><div class="kpi-val" id="kRoD" style="color:var(--roman)">—</div></div>
      <div><div class="kpi-sub">MDLive</div><div class="kpi-val" id="kMdD" style="color:var(--mdlive)">—</div></div>
      <div><div class="kpi-sub">Teladoc</div><div class="kpi-val" id="kTlD" style="color:var(--teladoc)">—</div></div>
    </div>
  </div>
</div>

<div class="tabs">
  <button class="tab active" onclick="setTab('revenue',this)">Revenue</button>
  <button class="tab" onclick="setTab('encounters',this)">Encounter Volume</button>
  <button class="tab" onclick="setTab('breakdown',this)">Visit Breakdown</button>
  <button class="tab" onclick="setTab('compare',this)">Roman Health vs Teladoc+MDLive</button>
  <button class="tab" onclick="setTab('daily',this)">Daily (Live)</button>
</div>

<div class="chart-panel">
  <div class="chart-hdr">
    <div>
      <span class="chart-title" id="cTitle">Monthly Revenue</span>
      <span class="live-pill" id="livePill" style="display:none">● LIVE</span>
    </div>
    <div class="chart-meta" id="cMeta"></div>
  </div>

  <div id="pMonthly"><canvas id="mainChart" height="300"></canvas></div>

  <div id="pDaily" style="display:none">
    <div class="daily-kpis" id="dKpis"></div>
    <div class="sec-label" id="dRevLbl">Daily Revenue</div>
    <canvas id="dRevChart" height="130"></canvas>
    <div class="sec-label mt20" id="dVisLbl">Daily Visits by Provider</div>
    <canvas id="dVisChart" height="130"></canvas>
  </div>

  <div id="pError" style="display:none" class="conn-err">
    <div class="ce-icon">⚠️</div>
    <div class="ce-msg">Could not load live data</div>
    <div class="ce-detail" id="errDetail">—</div>
    <button class="btn-try" onclick="loadData()">Try again</button>
  </div>

  <div class="chart-footer" id="cFooter">
    <span class="sdot" id="fDot"></span>
    <span id="fTxt">Current month live · prior months historical</span>
  </div>
</div>

<script>
// ── Config ────────────────────────────────────────────────────────────────────
const CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSc3am4UpZjqNJx9i5asqyEn-AI51cqKSPKvjV89ujSi_4FnDDgxw5gbc6dk2hnX4HK3o52aqm6DUDW/pub?output=csv";
const POLL_MS = 30_000;

// ── Historical ────────────────────────────────────────────────────────────────
const HIST = [
  {month:"Jul '24",revenue:26200,roman:820, mdlive:22,teladoc:48},
  {month:"Aug '24",revenue:10100,roman:710, mdlive:18,teladoc:40},
  {month:"Sep '24",revenue:7200, roman:650, mdlive:14,teladoc:36},
  {month:"Oct '24",revenue:20800,roman:880, mdlive:20,teladoc:52},
  {month:"Nov '24",revenue:12200,roman:760, mdlive:15,teladoc:38},
  {month:"Dec '24",revenue:20400,roman:890, mdlive:21,teladoc:50},
  {month:"Jan '25",revenue:22100,roman:910, mdlive:24,teladoc:60},
  {month:"Feb '25",revenue:30400,roman:970, mdlive:28,teladoc:72},
  {month:"Mar '25",revenue:39100,roman:1050,mdlive:32,teladoc:80},
  {month:"Apr '25",revenue:20200,roman:880, mdlive:22,teladoc:54},
  {month:"May '25",revenue:7400, roman:600, mdlive:12,teladoc:28},
  {month:"Jun '25",revenue:7100, roman:590, mdlive:11,teladoc:26},
  {month:"Jul '25",revenue:9600, roman:680, mdlive:14,teladoc:32},
  {month:"Aug '25",revenue:3200, roman:480, mdlive:8, teladoc:18},
  {month:"Sep '25",revenue:3800, roman:500, mdlive:9, teladoc:20},
  {month:"Oct '25",revenue:3600, roman:490, mdlive:8, teladoc:18},
  {month:"Nov '25",revenue:17400,roman:820, mdlive:20,teladoc:48},
  {month:"Dec '25",revenue:15800,roman:800, mdlive:18,teladoc:44},
  {month:"Jan '26",revenue:15600,roman:790, mdlive:17,teladoc:42},
  {month:"Feb '26",revenue:20600,roman:870, mdlive:22,teladoc:54},
];

// ── State ─────────────────────────────────────────────────────────────────────
let liveData=null, allMonthly=[...HIST];
let yearFilter="All", activeTab="revenue";
let charts={}, status="loading", countdown=POLL_MS/1000;

// ── CSV parser ────────────────────────────────────────────────────────────────
function parseRow(line){
  const cells=[]; let cur="",inQ=false;
  for(const ch of line){
    if(ch==='"'){inQ=!inQ;continue;}
    if(ch===','&&!inQ){cells.push(cur.trim());cur="";}
    else cur+=ch;
  }
  cells.push(cur.trim()); return cells;
}
function parseCSV(text){
  const rows=text.trim().split(/\r?\n/).map(parseRow);
  if(rows.length<7) throw new Error("CSV has fewer than 7 rows");
  const [header,td1,mdl1,td2,mdl2,ro,totals]=rows;
  const ti=header.findIndex(h=>h.toLowerCase()==="total");
  if(ti<0) throw new Error("No 'Total' column found. Headers: "+header.slice(0,6).join(", "));
  const n=s=>parseFloat((s||"0").replace(/[$,]/g,""))||0;
  const dayLabels=header.slice(1,ti);
  const daily=dayLabels.map((lbl,i)=>{
    const c=i+1;
    return{day:lbl,teladoc:n(td1[c])+n(td2[c]),mdlive:n(mdl1[c])+n(mdl2[c]),roman:n(ro[c]),revenue:n(totals[c])};
  }).filter(d=>d.roman>0||d.revenue>0||d.teladoc>0||d.mdlive>0);
  const monthly={revenue:n(totals[ti]),roman:n(ro[ti]),teladoc:n(td1[ti])+n(td2[ti]),mdlive:n(mdl1[ti])+n(mdl2[ti])};
  const mm=dayLabels[0]?.match(/[A-Za-z]+/);
  const currentMonth=mm?mm[0]:"?";
  const daysLogged=daily.length;
  const avgDailyRev=daysLogged?Math.round(monthly.revenue/daysLogged):0;
  return{daily,monthly,currentMonth,daysLogged,avgDailyRev};
}

// ── Fetch ─────────────────────────────────────────────────────────────────────
async function loadData(){
  setStatus("loading");
  try{
    const res=await fetch(CSV_URL+"&_="+Date.now(),{cache:"no-store"});
    if(!res.ok) throw new Error("HTTP "+res.status+" from Google Sheets");
    const text=await res.text();
    if(text.trim().startsWith("<")) throw new Error("Got an HTML page instead of CSV — is the sheet published to web?");
    const parsed=parseCSV(text);
    liveData=parsed;
    allMonthly=[...HIST,{
      month:parsed.currentMonth+" '26",
      revenue:parsed.monthly.revenue,
      roman:parsed.monthly.roman,
      mdlive:parsed.monthly.mdlive,
      teladoc:parsed.monthly.teladoc,
      live:true,
    }];
    setStatus("live");
    document.getElementById("lastUp").textContent="Last updated: "+new Date().toLocaleTimeString();
    countdown=POLL_MS/1000;
  }catch(e){
    setStatus("error");
    document.getElementById("errDetail").innerHTML=
      "<strong>Error:</strong> "+e.message+
      "<br><br>"+
      "This usually means the dashboard is being opened from a local file (<code>file://</code>).<br>"+
      "To fix: drag and drop the HTML file onto <strong>netlify.com/drop</strong> for a free live URL,<br>"+
      "or run <code>python3 -m http.server 8080</code> in the same folder and open <code>localhost:8080</code>.";
  }
  render();
}

// ── Status ────────────────────────────────────────────────────────────────────
function setStatus(s){
  status=s;
  ["sDot","fDot"].forEach(id=>{
    const el=document.getElementById(id);
    el.className="sdot "+s;
  });
  document.getElementById("sTxt").textContent=
    s==="loading"?"Connecting…":
    s==="live"?"Live · refreshes in "+countdown+"s":
    "Connection error · retrying";
}
setInterval(()=>{
  if(status==="live"){
    countdown=Math.max(0,countdown-1);
    document.getElementById("sTxt").textContent="Live · refreshes in "+countdown+"s";
  }
},1000);
setInterval(loadData,POLL_MS);

// ── Interactions ──────────────────────────────────────────────────────────────
function setYear(y,btn){
  yearFilter=y;
  document.querySelectorAll(".filter-bar .pill").forEach(b=>b.classList.remove("active"));
  btn.classList.add("active"); render();
}
function setTab(t,btn){
  activeTab=t;
  document.querySelectorAll(".tab").forEach(b=>b.classList.remove("active"));
  btn.classList.add("active"); render();
}
function getFiltered(){
  return yearFilter==="All"?allMonthly:allMonthly.filter(d=>d.month.includes("'"+yearFilter.slice(-2)));
}

// ── KPIs ──────────────────────────────────────────────────────────────────────
function updateKpis(){
  const fd=getFiltered(); if(!fd.length)return;
  const avg=k=>Math.round(fd.reduce((s,d)=>s+(d[k]||0),0)/fd.length);
  const ro=avg("roman"),md=avg("mdlive"),tl=avg("teladoc"),rv=avg("revenue");
  document.getElementById("kEnc").textContent=(ro+md+tl).toLocaleString();
  document.getElementById("kRev").textContent="$"+rv.toLocaleString();
  document.getElementById("kRo").textContent=ro.toLocaleString();
  document.getElementById("kMT").textContent=(md+tl).toLocaleString();
  document.getElementById("kRoD").textContent=(ro/30).toFixed(1);
  document.getElementById("kMdD").textContent=(md/30).toFixed(1);
  document.getElementById("kTlD").textContent=(tl/30).toFixed(1);
}

// ── Charts ────────────────────────────────────────────────────────────────────
const GC="#1e3048";
const TC={color:"#7a9ab8",font:{family:"'DM Mono','Courier New',monospace",size:11}};
const fmt$=n=>n>=1000?"$"+Math.round(n/1000)+"k":"$"+n;
const fmtF$=n=>"$"+Number(n).toLocaleString();
function killCharts(){Object.values(charts).forEach(c=>{try{c.destroy();}catch(e){}});charts={};}

function opts({yFmt,stacked=false,max,rev=false}){
  return{
    responsive:true,maintainAspectRatio:false,
    plugins:{
      legend:{labels:{color:"#7a9ab8",font:{family:"'DM Mono','Courier New',monospace",size:12},boxWidth:12}},
      tooltip:{
        backgroundColor:"#0d1928",borderColor:"#1e3048",borderWidth:1,
        titleColor:"#7a9ab8",bodyColor:"#e2eaf4",
        titleFont:{family:"'DM Mono','Courier New',monospace",size:11},
        bodyFont:{family:"'DM Mono','Courier New',monospace",size:12},
        callbacks:{label:ctx=>" "+ctx.dataset.label+": "+(rev?fmtF$(ctx.parsed.y):ctx.parsed.y.toLocaleString())}
      }
    },
    scales:{
      x:{grid:{display:false},ticks:TC,border:{display:false}},
      y:{stacked,max,grid:{color:GC},ticks:{...TC,callback:yFmt||(v=>v.toLocaleString())},border:{display:false}}
    }
  };
}

function buildMonthly(){
  const fd=getFiltered();
  const labels=fd.map(d=>d.month);
  const ctx=document.getElementById("mainChart").getContext("2d");
  const g=ctx.createLinearGradient(0,0,0,300);
  g.addColorStop(0,"rgba(96,165,250,0.18)");g.addColorStop(1,"rgba(96,165,250,0)");
  let cfg;
  if(activeTab==="revenue"){
    cfg={type:"line",data:{labels,datasets:[{
      label:"Revenue",data:fd.map(d=>d.revenue),
      borderColor:"#60a5fa",backgroundColor:g,borderWidth:2.5,fill:true,tension:.35,
      pointRadius:fd.map(d=>d.live?7:4),
      pointBackgroundColor:fd.map(d=>d.live?"#4ade80":"#60a5fa"),
      pointBorderColor:fd.map(d=>d.live?"#0f1623":"transparent"),
      pointBorderWidth:fd.map(d=>d.live?2:0),
    }]},options:opts({yFmt:fmt$,rev:true})};
  }else if(activeTab==="encounters"){
    cfg={type:"bar",data:{labels,datasets:[
      {label:"Roman Health",data:fd.map(d=>d.roman),  backgroundColor:"#4ade80",borderRadius:3},
      {label:"MDLive",      data:fd.map(d=>d.mdlive), backgroundColor:"#f97316",borderRadius:3},
      {label:"Teladoc",     data:fd.map(d=>d.teladoc),backgroundColor:"#60a5fa",borderRadius:3},
    ]},options:opts({})};
  }else if(activeTab==="breakdown"){
    const tots=fd.map(d=>(d.roman+d.mdlive+d.teladoc)||1);
    cfg={type:"bar",data:{labels,datasets:[
      {label:"Roman Health",data:fd.map((d,i)=>+(d.roman/tots[i]*100).toFixed(1)), backgroundColor:"#4ade80"},
      {label:"MDLive",      data:fd.map((d,i)=>+(d.mdlive/tots[i]*100).toFixed(1)),backgroundColor:"#f97316"},
      {label:"Teladoc",     data:fd.map((d,i)=>+(d.teladoc/tots[i]*100).toFixed(1)),backgroundColor:"#60a5fa"},
    ]},options:opts({stacked:true,yFmt:v=>v+"%",max:100})};
  }else if(activeTab==="compare"){
    cfg={type:"line",data:{labels,datasets:[
      {label:"Roman Health",    data:fd.map(d=>d.roman),          borderColor:"#4ade80",backgroundColor:"transparent",borderWidth:2.5,pointRadius:4,pointBackgroundColor:"#4ade80",tension:.35},
      {label:"MDLive + Teladoc",data:fd.map(d=>d.mdlive+d.teladoc),borderColor:"#f97316",backgroundColor:"transparent",borderWidth:2.5,pointRadius:4,pointBackgroundColor:"#f97316",tension:.35},
    ]},options:opts({})};
  }
  if(cfg) charts.main=new Chart(ctx,cfg);
}

function buildDaily(){
  const{daily,monthly,currentMonth,daysLogged,avgDailyRev}=liveData;
  document.getElementById("cMeta").textContent=daysLogged+" days · MTD: "+fmtF$(monthly.revenue);
  document.getElementById("dRevLbl").textContent="Daily Revenue — "+currentMonth+" 2026";
  document.getElementById("dVisLbl").textContent="Daily Visits by Provider — "+currentMonth+" 2026";
  document.getElementById("dKpis").innerHTML=[
    ["MTD REVENUE",   fmtF$(monthly.revenue),"#60a5fa","~"+fmtF$(avgDailyRev)+"/day avg"],
    ["RO VISITS",     monthly.roman.toLocaleString(),"#4ade80","thru day "+daysLogged],
    ["TELADOC VISITS",monthly.teladoc,"#60a5fa","thru day "+daysLogged],
    ["MDLIVE VISITS", monthly.mdlive, "#f97316","thru day "+daysLogged],
  ].map(([l,v,c,s])=>`<div class="dk"><div class="dk-label">${l}</div><div class="dk-val" style="color:${c}">${v}</div><div class="dk-sub">${s}</div></div>`).join("");
  const labels=daily.map(d=>d.day);
  const ctxR=document.getElementById("dRevChart").getContext("2d");
  const gR=ctxR.createLinearGradient(0,0,0,130);
  gR.addColorStop(0,"rgba(96,165,250,0.2)");gR.addColorStop(1,"rgba(96,165,250,0)");
  charts.dRev=new Chart(ctxR,{type:"line",data:{labels,datasets:[{
    label:"Revenue",data:daily.map(d=>d.revenue),
    borderColor:"#60a5fa",backgroundColor:gR,borderWidth:2,pointRadius:3,pointBackgroundColor:"#60a5fa",tension:.35,fill:true,
  }]},options:opts({yFmt:fmt$,rev:true})});
  charts.dVis=new Chart(document.getElementById("dVisChart").getContext("2d"),{
    type:"bar",data:{labels,datasets:[
      {label:"Roman Health",data:daily.map(d=>d.roman),  backgroundColor:"#4ade80",stack:"a"},
      {label:"Teladoc",     data:daily.map(d=>d.teladoc),backgroundColor:"#60a5fa",stack:"a"},
      {label:"MDLive",      data:daily.map(d=>d.mdlive), backgroundColor:"#f97316",stack:"a",borderRadius:2},
    ]},options:opts({stacked:true})
  });
}

// ── Render ────────────────────────────────────────────────────────────────────
function render(){
  updateKpis(); killCharts();
  const isD=activeTab==="daily", hasL=!!liveData;
  document.getElementById("pMonthly").style.display=isD?"none":"block";
  document.getElementById("pDaily").style.display=(isD&&hasL)?"block":"none";
  document.getElementById("pError").style.display=(isD&&!hasL)?"flex":"none";
  document.getElementById("livePill").style.display=(isD&&hasL)?"inline":"none";
  document.getElementById("cFooter").style.display=isD?"none":"flex";
  const titles={revenue:"Monthly Revenue",encounters:"Monthly Encounter Volume",
    breakdown:"Visit Type Breakdown",compare:"Roman Health vs Teladoc + MDLive",
    daily:hasL?`Daily Activity — ${liveData.currentMonth} 2026`:"Daily Activity (Live)"};
  document.getElementById("cTitle").textContent=titles[activeTab];
  document.getElementById("fTxt").textContent=status==="live"?"Current month live · prior months historical":"Historical data";
  if(!isD) buildMonthly();
  else if(hasL) buildDaily();
}

render();
loadData();
</script>
</body>
</html>
