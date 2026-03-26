import { useState, useMemo, useEffect, useCallback, useRef } from "react";
import {
  LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip,
  ResponsiveContainer, BarChart, Bar, Legend, ComposedChart, Area
} from "recharts";

// ── Config ────────────────────────────────────────────────────────────────────
const DEFAULT_SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSc3am4UpZjqNJx9i5asqyEn-AI51cqKSPKvjV89ujSi_4FnDDgxw5gbc6dk2hnX4HK3o52aqm6DUDW/pub?output=csv";
const DEFAULT_API_RANGE     = "A1:ZZ";
// localStorage keys
const LS_URL_KEY    = "dashboard_sheet_url";
const LS_MODE_KEY   = "dashboard_source_mode";   // "csv" | "api"
const LS_API_ID_KEY = "dashboard_api_sheet_id";
const LS_API_KEY    = "dashboard_api_key";
const LS_API_RANGE  = "dashboard_api_range";
// Try direct first, fall back to CORS proxies (CSV mode only)
const PROXIES = [
  url => url,
  url => `https://corsproxy.io/?${encodeURIComponent(url)}`,
  url => `https://api.allorigins.win/raw?url=${encodeURIComponent(url)}`,
];
const POLL_MS = 30_000;

// ── Fetch helpers ─────────────────────────────────────────────────────────────
async function fetchCSV(sheetUrl) {
  let lastErr;
  for (const proxy of PROXIES) {
    try {
      const res = await fetch(proxy(sheetUrl), { cache: "no-store" });
      if (!res.ok) throw new Error(`HTTP ${res.status}`);
      const text = await res.text();
      if (text.includes("<!DOCTYPE")) throw new Error("Got HTML, not CSV");
      return text;
    } catch (e) {
      lastErr = e;
    }
  }
  throw lastErr;
}

// Fetches rows directly from the Google Sheets API v4 (real-time, no caching).
// The spreadsheet must be shared as "Anyone with the link can view".
// Returns a 2-D array of cell strings, same shape as the parsed CSV rows.
async function fetchSheetsAPI(spreadsheetId, apiKey, range) {
  const url = `https://sheets.googleapis.com/v4/spreadsheets/${encodeURIComponent(spreadsheetId)}/values/${encodeURIComponent(range)}?key=${encodeURIComponent(apiKey)}`;
  const res = await fetch(url, { cache: "no-store" });
  if (!res.ok) {
    let msg = `Sheets API HTTP ${res.status}`;
    try { const j = await res.json(); msg = j?.error?.message || msg; } catch (_) {}
    throw new Error(msg);
  }
  const json = await res.json();
  if (json.error) throw new Error(json.error.message);
  return json.values || [];
}

// ── Sheet parser ──────────────────────────────────────────────────────────────
// Shared by both CSV and API paths.
// Sheet layout:
//   Row 0: header  ["", "1-Mar", "2-Mar", ..., "Total", "", "Annual"]
//   Row 1: TD row1   visit counts
//   Row 2: MDL row1
//   Row 3: TD row2
//   Row 4: MDL row2
//   Row 5: RO
//   Row 6: Total   daily revenue
function parseRows(rows) {
  if (rows.length < 7) return null;

  const [header, td1, mdl1, td2, mdl2, ro, totals] = rows;
  const totalIdx = header.findIndex(h => (h || "").toLowerCase() === "total");
  if (totalIdx < 0) return null;

  const n = s => parseFloat((s || "0").replace(/[$,]/g, "")) || 0;

  const dayHeaders = header.slice(1, totalIdx);
  const daily = dayHeaders.map((dayLabel, i) => {
    const col = i + 1;
    return {
      day:     dayLabel,
      teladoc: n(td1[col])  + n(td2[col]),
      mdlive:  n(mdl1[col]) + n(mdl2[col]),
      roman:   n(ro[col]),
      revenue: n(totals[col]),
    };
  }).filter(d => d.total > 0 || d.revenue > 0 || d.roman > 0);

  const monthly = {
    revenue: n(totals[totalIdx]),
    roman:   n(ro[totalIdx]),
    teladoc: n(td1[totalIdx])  + n(td2[totalIdx]),
    mdlive:  n(mdl1[totalIdx]) + n(mdl2[totalIdx]),
  };

  const monthMatch = dayHeaders[0]?.match(/[A-Za-z]+/);
  const currentMonth = monthMatch ? monthMatch[0] : "?";
  const daysLogged   = daily.length;
  const avgDailyRev  = daysLogged ? Math.round(monthly.revenue / daysLogged) : 0;

  return { daily, monthly, currentMonth, daysLogged, avgDailyRev };
}

function parseCSVText(text) {
  const parseRow = line => {
    const cells = []; let cur = "", inQ = false;
    for (const ch of line) {
      if (ch === '"') { inQ = !inQ; continue; }
      if (ch === ',' && !inQ) { cells.push(cur.trim()); cur = ""; }
      else cur += ch;
    }
    cells.push(cur.trim());
    return cells;
  };
  return parseRows(text.trim().split(/\r?\n/).map(parseRow));
}

// ── Historical data ───────────────────────────────────────────────────────────
const HISTORICAL = [
  { month:"Jul '24", revenue:26200, roman:820,  mdlive:22, teladoc:48 },
  { month:"Aug '24", revenue:10100, roman:710,  mdlive:18, teladoc:40 },
  { month:"Sep '24", revenue:7200,  roman:650,  mdlive:14, teladoc:36 },
  { month:"Oct '24", revenue:20800, roman:880,  mdlive:20, teladoc:52 },
  { month:"Nov '24", revenue:12200, roman:760,  mdlive:15, teladoc:38 },
  { month:"Dec '24", revenue:20400, roman:890,  mdlive:21, teladoc:50 },
  { month:"Jan '25", revenue:22100, roman:910,  mdlive:24, teladoc:60 },
  { month:"Feb '25", revenue:30400, roman:970,  mdlive:28, teladoc:72 },
  { month:"Mar '25", revenue:39100, roman:1050, mdlive:32, teladoc:80 },
  { month:"Apr '25", revenue:20200, roman:880,  mdlive:22, teladoc:54 },
  { month:"May '25", revenue:7400,  roman:600,  mdlive:12, teladoc:28 },
  { month:"Jun '25", revenue:7100,  roman:590,  mdlive:11, teladoc:26 },
  { month:"Jul '25", revenue:9600,  roman:680,  mdlive:14, teladoc:32 },
  { month:"Aug '25", revenue:3200,  roman:480,  mdlive:8,  teladoc:18 },
  { month:"Sep '25", revenue:3800,  roman:500,  mdlive:9,  teladoc:20 },
  { month:"Oct '25", revenue:3600,  roman:490,  mdlive:8,  teladoc:18 },
  { month:"Nov '25", revenue:17400, roman:820,  mdlive:20, teladoc:48 },
  { month:"Dec '25", revenue:15800, roman:800,  mdlive:18, teladoc:44 },
  { month:"Jan '26", revenue:15600, roman:790,  mdlive:17, teladoc:42 },
  { month:"Feb '26", revenue:20600, roman:870,  mdlive:22, teladoc:54 },
];

// ── Theme ─────────────────────────────────────────────────────────────────────
const C = {
  roman:"#4ade80", mdlive:"#f97316", teladoc:"#60a5fa",
  accent:"#60a5fa", bg:"#0f1623", card:"#162032",
  border:"#1e3048", muted:"#4a6080", text:"#e2eaf4", subtext:"#7a9ab8",
};
const FONT = "'DM Mono','Courier New',monospace";
const YEARS = ["All","2024","2025","2026"];
const TABS  = ["Revenue","Encounter Volume","Visit Breakdown","Roman Health vs Teladoc+MDLive","Daily (Live)"];
const fmt$  = n => n >= 1000 ? `$${(n/1000).toFixed(0)}k` : `$${n}`;
const fmtF$ = n => `$${Number(n).toLocaleString()}`;

// ── Small shared chart pieces ─────────────────────────────────────────────────
const GGrid  = () => <CartesianGrid strokeDasharray="3 3" stroke={C.border} vertical={false}/>;
const GX     = ({k="month"}) => <XAxis dataKey={k} tick={{fill:C.subtext,fontSize:11,fontFamily:FONT}} axisLine={false} tickLine={false}/>;
const GY     = ({fmt}) => <YAxis tickFormatter={fmt} tick={{fill:C.subtext,fontSize:11,fontFamily:FONT}} axisLine={false} tickLine={false}/>;
const GLeg   = () => <Legend wrapperStyle={{fontFamily:FONT,fontSize:12,color:C.subtext}}/>;
const TTip   = ({active,payload,label,rev}) => {
  if (!active||!payload?.length) return null;
  return (
    <div style={{background:"#0d1928",border:`1px solid ${C.border}`,borderRadius:10,
      padding:"10px 16px",fontFamily:FONT,fontSize:12,color:C.text,boxShadow:"0 8px 32px #000a"}}>
      <div style={{color:C.subtext,marginBottom:6}}>{label}</div>
      {payload.map((p,i)=>(
        <div key={i} style={{color:p.color,marginTop:2}}>
          {p.name}: <strong>{rev?fmtF$(p.value):Number(p.value).toLocaleString()}</strong>
        </div>
      ))}
    </div>
  );
};
const Pill = ({active,onClick,children}) => (
  <button onClick={onClick} style={{
    background:active?C.accent:"transparent", color:active?"#fff":C.subtext,
    border:"none", borderRadius:20, padding:"5px 16px", cursor:"pointer",
    fontFamily:FONT, fontSize:12, fontWeight:active?700:400, outline:"none",
  }}>{children}</button>
);
const TabBtn = ({label,active,onClick}) => (
  <button onClick={onClick} style={{
    background:active?C.accent:"transparent", color:active?"#fff":C.subtext,
    border:`1px solid ${active?C.accent:C.border}`, borderRadius:8,
    padding:"7px 16px", cursor:"pointer", fontFamily:FONT, fontSize:12,
    fontWeight:active?700:400, outline:"none", whiteSpace:"nowrap",
  }}>{label}</button>
);

// ── Dashboard ─────────────────────────────────────────────────────────────────
export default function Dashboard() {
  const [yearFilter,   setYearFilter]   = useState("All");
  const [activeTab,    setActiveTab]    = useState("Revenue");
  const [liveData,     setLiveData]     = useState(null);
  const [status,       setStatus]       = useState("loading");
  const [errMsg,       setErrMsg]       = useState("");
  const [lastUpdate,   setLastUpdate]   = useState(null);
  const [countdown,    setCountdown]    = useState(POLL_MS/1000);

  // ── Source config (persisted in localStorage) ────────────────────────────
  const [srcMode,    setSrcMode]    = useState(() => localStorage.getItem(LS_MODE_KEY)   || "csv");
  const [sheetUrl,   setSheetUrl]   = useState(() => localStorage.getItem(LS_URL_KEY)    || DEFAULT_SHEET_CSV_URL);
  const [apiSheetId, setApiSheetId] = useState(() => localStorage.getItem(LS_API_ID_KEY) || "");
  const [apiKey,     setApiKey]     = useState(() => localStorage.getItem(LS_API_KEY)    || "");
  const [apiRange,   setApiRange]   = useState(() => localStorage.getItem(LS_API_RANGE)  || DEFAULT_API_RANGE);

  // ── Settings panel UI state ──────────────────────────────────────────────
  const [showSettings,  setShowSettings]  = useState(false);
  const [draftMode,     setDraftMode]     = useState(srcMode);
  const [draftUrl,      setDraftUrl]      = useState(sheetUrl);
  const [draftSheetId,  setDraftSheetId]  = useState(apiSheetId);
  const [draftApiKey,   setDraftApiKey]   = useState(apiKey);
  const [draftApiRange, setDraftApiRange] = useState(apiRange);

  const pollRef = useRef(null);
  const cdRef   = useRef(null);

  // ── Core fetch / parse ───────────────────────────────────────────────────
  const load = useCallback(async (overrides = {}) => {
    const mode  = overrides.mode    ?? srcMode;
    const url   = overrides.url     ?? sheetUrl;
    const id    = overrides.id      ?? apiSheetId;
    const key   = overrides.key     ?? apiKey;
    const range = overrides.range   ?? apiRange;
    try {
      setStatus(s => s === "live" ? "live" : "loading");
      let parsed;
      if (mode === "api") {
        const rows = await fetchSheetsAPI(id, key, range || DEFAULT_API_RANGE);
        parsed = parseRows(rows);
      } else {
        const text = await fetchCSV(url);
        parsed = parseCSVText(text);
      }
      if (!parsed) throw new Error("Data fetched but sheet structure not recognised");
      setLiveData(parsed);
      setStatus("live");
      setLastUpdate(new Date());
      setCountdown(POLL_MS/1000);
      setErrMsg("");
    } catch (e) {
      setStatus("error");
      setErrMsg(e.message);
    }
  }, [srcMode, sheetUrl, apiSheetId, apiKey, apiRange]);

  useEffect(() => {
    load();
    pollRef.current = setInterval(() => load(), POLL_MS);
    return () => clearInterval(pollRef.current);
  }, [load]);

  // ── Settings save / reset ────────────────────────────────────────────────
  const restartPolling = async (cfg) => {
    setShowSettings(false);
    setStatus("loading");
    setLiveData(null);
    clearInterval(pollRef.current);
    await load(cfg);
    pollRef.current = setInterval(() => load(cfg), POLL_MS);
  };

  const saveSettings = async () => {
    const next = {
      mode:  draftMode,
      url:   draftUrl.trim(),
      id:    draftSheetId.trim(),
      key:   draftApiKey.trim(),
      range: draftApiRange.trim() || DEFAULT_API_RANGE,
    };
    if (next.mode === "csv" && !next.url) return;
    if (next.mode === "api" && (!next.id || !next.key)) return;

    localStorage.setItem(LS_MODE_KEY,   next.mode);
    localStorage.setItem(LS_URL_KEY,    next.url);
    localStorage.setItem(LS_API_ID_KEY, next.id);
    localStorage.setItem(LS_API_KEY,    next.key);
    localStorage.setItem(LS_API_RANGE,  next.range);

    setSrcMode(next.mode);
    setSheetUrl(next.url);
    setApiSheetId(next.id);
    setApiKey(next.key);
    setApiRange(next.range);
    await restartPolling(next);
  };

  const resetSettings = async () => {
    [LS_MODE_KEY, LS_URL_KEY, LS_API_ID_KEY, LS_API_KEY, LS_API_RANGE].forEach(k => localStorage.removeItem(k));
    const defaults = { mode:"csv", url:DEFAULT_SHEET_CSV_URL, id:"", key:"", range:DEFAULT_API_RANGE };
    setSrcMode(defaults.mode);   setDraftMode(defaults.mode);
    setSheetUrl(defaults.url);   setDraftUrl(defaults.url);
    setApiSheetId(defaults.id);  setDraftSheetId(defaults.id);
    setApiKey(defaults.key);     setDraftApiKey(defaults.key);
    setApiRange(defaults.range); setDraftApiRange(defaults.range);
    await restartPolling(defaults);
  };

  useEffect(() => {
    cdRef.current = setInterval(() => setCountdown(c => c <= 1 ? POLL_MS/1000 : c-1), 1000);
    return () => clearInterval(cdRef.current);
  }, []);

  const allMonthly = useMemo(() => {
    if (!liveData) return HISTORICAL;
    return [...HISTORICAL, {
      month:   `${liveData.currentMonth} '26`,
      revenue: liveData.monthly.revenue,
      roman:   liveData.monthly.roman,
      mdlive:  liveData.monthly.mdlive,
      teladoc: liveData.monthly.teladoc,
      live:    true,
    }];
  }, [liveData]);

  const filtered = useMemo(() =>
    allMonthly.filter(d => yearFilter==="All" || d.month.includes(yearFilter.slice(2))),
    [allMonthly, yearFilter]
  );

  const fd  = filtered.length ? filtered : [{revenue:0,roman:0,mdlive:0,teladoc:0}];
  const avg = k => Math.round(fd.reduce((s,d)=>s+(d[k]||0),0)/fd.length);
  const avgEncounters = avg("roman")+avg("mdlive")+avg("teladoc");
  const avgRevenue    = avg("revenue");
  const avgRoman      = avg("roman");
  const avgMdTel      = avg("mdlive")+avg("teladoc");
  const avgRomanDay   = (avg("roman")  /30).toFixed(1);
  const avgMdDay      = (avg("mdlive") /30).toFixed(1);
  const avgTelDay     = (avg("teladoc")/30).toFixed(1);

  const dotColor   = {loading:"#f97316", live:C.roman, error:"#ef4444"}[status];
  const statusText = {
    loading: "Connecting…",
    live:    `Live · refreshes in ${countdown}s`,
    error:   "Connection error · retrying",
  }[status];

  // ── Charts ──────────────────────────────────────────────────────────────────
  const renderChart = () => {
    if (activeTab === "Revenue") return (
      <ResponsiveContainer width="100%" height={320}>
        <ComposedChart data={filtered} margin={{top:10,right:20,left:10,bottom:0}}>
          <defs>
            <linearGradient id="rg" x1="0" y1="0" x2="0" y2="1">
              <stop offset="5%"  stopColor={C.teladoc} stopOpacity={0.15}/>
              <stop offset="95%" stopColor={C.teladoc} stopOpacity={0}/>
            </linearGradient>
          </defs>
          <GGrid/><GX/><GY fmt={fmt$}/>
          <Tooltip content={p=><TTip {...p} rev/>}/>
          <Area type="monotone" dataKey="revenue" fill="url(#rg)" stroke="none" name="Revenue"/>
          <Line type="monotone" dataKey="revenue" stroke={C.teladoc} strokeWidth={2.5}
            dot={(p)=>{
              const d=filtered[p.index];
              return d?.live
                ? <circle key={p.index} cx={p.cx} cy={p.cy} r={6} fill={C.roman} stroke="#0f1623" strokeWidth={2}/>
                : <circle key={p.index} cx={p.cx} cy={p.cy} r={3.5} fill={C.teladoc} stroke="none"/>;
            }}
            activeDot={{r:7}} name="Revenue"/>
        </ComposedChart>
      </ResponsiveContainer>
    );

    if (activeTab === "Encounter Volume") return (
      <ResponsiveContainer width="100%" height={320}>
        <BarChart data={filtered} margin={{top:10,right:20,left:10,bottom:0}}>
          <GGrid/><GX/><GY fmt={v=>v.toLocaleString()}/><GLeg/>
          <Tooltip content={p=><TTip {...p}/>}/>
          <Bar dataKey="roman"   name="Roman Health" fill={C.roman}   radius={[3,3,0,0]}/>
          <Bar dataKey="mdlive"  name="MDLive"       fill={C.mdlive}  radius={[3,3,0,0]}/>
          <Bar dataKey="teladoc" name="Teladoc"      fill={C.teladoc} radius={[3,3,0,0]}/>
        </BarChart>
      </ResponsiveContainer>
    );

    if (activeTab === "Visit Breakdown") return (
      <ResponsiveContainer width="100%" height={320}>
        <BarChart data={filtered} margin={{top:10,right:20,left:10,bottom:0}} stackOffset="expand">
          <GGrid/><GX/><GY fmt={v=>`${Math.round(v*100)}%`}/><GLeg/>
          <Tooltip content={p=><TTip {...p}/>}/>
          <Bar dataKey="roman"   name="Roman Health" stackId="a" fill={C.roman}/>
          <Bar dataKey="mdlive"  name="MDLive"       stackId="a" fill={C.mdlive}/>
          <Bar dataKey="teladoc" name="Teladoc"      stackId="a" fill={C.teladoc}/>
        </BarChart>
      </ResponsiveContainer>
    );

    if (activeTab === "Roman Health vs Teladoc+MDLive") {
      const combined = filtered.map(d=>({...d, other:(d.mdlive||0)+(d.teladoc||0)}));
      return (
        <ResponsiveContainer width="100%" height={320}>
          <LineChart data={combined} margin={{top:10,right:20,left:10,bottom:0}}>
            <GGrid/><GX/><GY fmt={v=>v.toLocaleString()}/><GLeg/>
            <Tooltip content={p=><TTip {...p}/>}/>
            <Line type="monotone" dataKey="roman" stroke={C.roman} strokeWidth={2.5}
              dot={{r:3.5,fill:C.roman,strokeWidth:0}} name="Roman Health"/>
            <Line type="monotone" dataKey="other" stroke={C.mdlive} strokeWidth={2.5}
              dot={{r:3.5,fill:C.mdlive,strokeWidth:0}} name="MDLive + Teladoc"/>
          </LineChart>
        </ResponsiveContainer>
      );
    }

    // ── Daily Live ───────────────────────────────────────────────────────────
    if (status === "error" || !liveData) return (
      <div style={{height:200,display:"flex",alignItems:"center",justifyContent:"center",
        flexDirection:"column",gap:12,color:C.muted,fontFamily:FONT,fontSize:13}}>
        <div style={{fontSize:28}}>⚠️</div>
        <div style={{color:"#ef4444"}}>Connection error</div>
        <div style={{fontSize:11,color:C.muted,maxWidth:400,textAlign:"center"}}>{errMsg}</div>
        <button onClick={()=>load()} style={{marginTop:4,background:C.accent,color:"#fff",border:"none",
          borderRadius:20,padding:"7px 20px",cursor:"pointer",fontFamily:FONT,fontSize:12}}>
          Try again
        </button>
      </div>
    );

    const {daily, monthly, currentMonth, daysLogged, avgDailyRev} = liveData;
    return (
      <div>
        <div style={{display:"grid",gridTemplateColumns:"repeat(4,1fr)",gap:12,marginBottom:24}}>
          {[
            ["MTD Revenue",    fmtF$(monthly.revenue), C.teladoc, `~${fmtF$(avgDailyRev)}/day avg`],
            ["RO Visits",      monthly.roman.toLocaleString(), C.roman,   `thru day ${daysLogged}`],
            ["Teladoc Visits", monthly.teladoc, C.teladoc, `thru day ${daysLogged}`],
            ["MDLive Visits",  monthly.mdlive,  C.mdlive,  `thru day ${daysLogged}`],
          ].map(([label,val,color,sub])=>(
            <div key={label} style={{background:"#0d1928",borderRadius:10,padding:"14px 16px",border:`1px solid ${C.border}`}}>
              <div style={{fontSize:10,color:C.muted,letterSpacing:1,marginBottom:5}}>{label.toUpperCase()}</div>
              <div style={{fontSize:22,fontWeight:700,color}}>{val}</div>
              <div style={{fontSize:10,color:C.muted,marginTop:3}}>{sub}</div>
            </div>
          ))}
        </div>

        <div style={{fontSize:12,color:C.subtext,marginBottom:8}}>Daily Revenue — {currentMonth} 2026</div>
        <ResponsiveContainer width="100%" height={150}>
          <ComposedChart data={daily} margin={{top:5,right:20,left:10,bottom:0}}>
            <defs>
              <linearGradient id="drg" x1="0" y1="0" x2="0" y2="1">
                <stop offset="5%"  stopColor={C.teladoc} stopOpacity={0.2}/>
                <stop offset="95%" stopColor={C.teladoc} stopOpacity={0}/>
              </linearGradient>
            </defs>
            <GGrid/>
            <XAxis dataKey="day" tick={{fill:C.subtext,fontSize:10,fontFamily:FONT}} axisLine={false} tickLine={false}/>
            <YAxis tickFormatter={fmt$} tick={{fill:C.subtext,fontSize:10,fontFamily:FONT}} axisLine={false} tickLine={false}/>
            <Tooltip content={p=><TTip {...p} rev/>}/>
            <Area type="monotone" dataKey="revenue" fill="url(#drg)" stroke="none" name="Revenue"/>
            <Line type="monotone" dataKey="revenue" stroke={C.teladoc} strokeWidth={2}
              dot={{r:3,fill:C.teladoc,strokeWidth:0}} name="Revenue"/>
          </ComposedChart>
        </ResponsiveContainer>

        <div style={{fontSize:12,color:C.subtext,margin:"20px 0 8px"}}>Daily Visits by Provider — {currentMonth} 2026</div>
        <ResponsiveContainer width="100%" height={150}>
          <BarChart data={daily} margin={{top:5,right:20,left:10,bottom:0}}>
            <GGrid/>
            <XAxis dataKey="day" tick={{fill:C.subtext,fontSize:10,fontFamily:FONT}} axisLine={false} tickLine={false}/>
            <YAxis tick={{fill:C.subtext,fontSize:10,fontFamily:FONT}} axisLine={false} tickLine={false}/>
            <Tooltip content={p=><TTip {...p}/>}/>
            <Legend wrapperStyle={{fontFamily:FONT,fontSize:11,color:C.subtext}}/>
            <Bar dataKey="roman"   name="Roman Health" stackId="a" fill={C.roman}/>
            <Bar dataKey="teladoc" name="Teladoc"      stackId="a" fill={C.teladoc}/>
            <Bar dataKey="mdlive"  name="MDLive"       stackId="a" fill={C.mdlive} radius={[2,2,0,0]}/>
          </BarChart>
        </ResponsiveContainer>
      </div>
    );
  };

  // ── Layout ──────────────────────────────────────────────────────────────────
  return (
    <div style={{background:C.bg,minHeight:"100vh",padding:"28px 24px",fontFamily:FONT,color:C.text,boxSizing:"border-box"}}>

      {/* Header */}
      <div style={{display:"flex",justifyContent:"space-between",alignItems:"flex-start",marginBottom:6}}>
        <div>
          <h1 style={{margin:0,fontSize:26,fontWeight:700,letterSpacing:"-0.5px",color:"#e8f0fc"}}>
            Telemedicine Activity Dashboard
          </h1>
          <div style={{marginTop:6,fontSize:12,color:C.subtext,display:"flex",gap:12,alignItems:"center"}}>
            <span>July 2024 – Present</span>
            {[["Roman Health",C.roman],["MDLive",C.mdlive],["Teladoc",C.teladoc]].map(([n,c])=>(
              <span key={n} style={{display:"flex",alignItems:"center",gap:4}}>
                <span style={{width:8,height:8,borderRadius:2,background:c,display:"inline-block"}}/>{n}
              </span>
            ))}
          </div>
        </div>
        <div style={{display:"flex",alignItems:"center",gap:8}}>
          <button onClick={()=>{
              setDraftMode(srcMode); setDraftUrl(sheetUrl);
              setDraftSheetId(apiSheetId); setDraftApiKey(apiKey); setDraftApiRange(apiRange);
              setShowSettings(s=>!s);
            }}
            style={{background:showSettings?C.accent:"transparent",
            border:`1px solid ${showSettings?C.accent:C.border}`,
            borderRadius:20,padding:"5px 14px",cursor:"pointer",fontFamily:FONT,
            fontSize:11,color:showSettings?"#fff":C.subtext,outline:"none"}}>
            ⚙ Data source
          </button>
          <button onClick={()=>load()} style={{background:"transparent",border:`1px solid ${C.border}`,
            borderRadius:20,padding:"5px 14px",cursor:"pointer",fontFamily:FONT,
            fontSize:11,color:C.subtext,outline:"none"}}>
            ↻ Refresh now
          </button>
          <div style={{background:"#1a2a3a",border:`1px solid ${C.border}`,borderRadius:20,
            padding:"6px 14px",fontSize:12,color:C.subtext,display:"flex",alignItems:"center",gap:6}}>
            <span style={{width:8,height:8,borderRadius:"50%",background:dotColor,
              boxShadow:status==="live"?`0 0 6px ${dotColor}`:"none",display:"inline-block"}}/>
            {statusText}
          </div>
        </div>
      </div>

      {/* Settings panel */}
      {showSettings && (() => {
        const inp = extra => ({
          style:{width:"100%",boxSizing:"border-box",background:"#162032",
            border:`1px solid ${C.border}`,borderRadius:8,padding:"8px 12px",
            color:C.text,fontFamily:FONT,fontSize:11,outline:"none",...extra}
        });
        const canSave = draftMode === "api"
          ? draftSheetId.trim() && draftApiKey.trim()
          : draftUrl.trim();
        return (
          <div style={{background:"#0d1928",border:`1px solid ${C.border}`,borderRadius:12,
            padding:"18px 20px",marginTop:12,marginBottom:4}}>
            {/* Header row */}
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:14}}>
              <div style={{fontSize:13,fontWeight:700,color:"#e8f0fc"}}>📊 Spreadsheet Data Source</div>
              <button onClick={()=>setShowSettings(false)}
                style={{background:"transparent",color:C.muted,border:"none",
                  cursor:"pointer",fontFamily:FONT,fontSize:16,lineHeight:1}}>✕</button>
            </div>

            {/* Mode switcher */}
            <div style={{display:"flex",gap:6,marginBottom:16}}>
              {[["csv","Published CSV URL"],["api","Google Sheets API v4 ⚡"]].map(([m,label])=>(
                <button key={m} onClick={()=>setDraftMode(m)}
                  style={{background:draftMode===m?C.accent:"transparent",
                    color:draftMode===m?"#fff":C.subtext,
                    border:`1px solid ${draftMode===m?C.accent:C.border}`,
                    borderRadius:20,padding:"5px 16px",cursor:"pointer",
                    fontFamily:FONT,fontSize:11,fontWeight:draftMode===m?700:400,outline:"none"}}>
                  {label}
                </button>
              ))}
            </div>

            {draftMode === "csv" ? (
              <div>
                <div style={{fontSize:11,color:C.muted,marginBottom:8}}>
                  Paste the <strong style={{color:C.subtext}}>published-as-CSV</strong> URL
                  (Google Sheet → File → Share → Publish to web → CSV).
                  Note: published CSVs may cache for 5–15 min.
                </div>
                <input value={draftUrl} onChange={e=>setDraftUrl(e.target.value)}
                  placeholder="https://docs.google.com/spreadsheets/d/e/…/pub?output=csv"
                  {...inp()}/>
              </div>
            ) : (
              <div>
                <div style={{fontSize:11,color:C.muted,marginBottom:10}}>
                  Connects directly to your live sheet via the{" "}
                  <strong style={{color:C.subtext}}>Google Sheets API v4</strong> — truly real-time,
                  no caching, no CORS proxy needed. Your sheet must be shared as
                  <em style={{color:C.subtext}}> "Anyone with the link can view"</em>.
                </div>
                <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:10,marginBottom:10}}>
                  <div>
                    <div style={{fontSize:10,color:C.muted,marginBottom:4,letterSpacing:.5}}>SPREADSHEET ID</div>
                    <input value={draftSheetId} onChange={e=>setDraftSheetId(e.target.value)}
                      placeholder="1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms"
                      {...inp()}/>
                    <div style={{fontSize:10,color:C.muted,marginTop:4}}>
                      From your sheet URL: …/spreadsheets/d/<strong style={{color:C.subtext}}>ID</strong>/edit
                    </div>
                  </div>
                  <div>
                    <div style={{fontSize:10,color:C.muted,marginBottom:4,letterSpacing:.5}}>API KEY</div>
                    <input value={draftApiKey} onChange={e=>setDraftApiKey(e.target.value)}
                      type="password" placeholder="AIza…"
                      {...inp()}/>
                    <div style={{fontSize:10,color:C.muted,marginTop:4}}>
                      Google Cloud Console → APIs &amp; Services → Credentials
                    </div>
                  </div>
                </div>
                <div style={{maxWidth:300}}>
                  <div style={{fontSize:10,color:C.muted,marginBottom:4,letterSpacing:.5}}>RANGE (optional)</div>
                  <input value={draftApiRange} onChange={e=>setDraftApiRange(e.target.value)}
                    placeholder={DEFAULT_API_RANGE}
                    {...inp()}/>
                  <div style={{fontSize:10,color:C.muted,marginTop:4}}>
                    Leave blank to use <code style={{color:C.subtext}}>{DEFAULT_API_RANGE}</code>
                  </div>
                </div>
              </div>
            )}

            {/* Action buttons */}
            <div style={{display:"flex",gap:8,marginTop:14,alignItems:"center"}}>
              <button onClick={saveSettings} disabled={!canSave}
                style={{background:canSave?C.accent:"#1e3048",color:canSave?"#fff":C.muted,
                  border:"none",borderRadius:8,padding:"8px 20px",
                  cursor:canSave?"pointer":"default",fontFamily:FONT,fontSize:11,fontWeight:700}}>
                Save &amp; reload
              </button>
              <button onClick={resetSettings}
                style={{background:"transparent",color:C.muted,border:`1px solid ${C.border}`,
                  borderRadius:8,padding:"8px 14px",cursor:"pointer",fontFamily:FONT,fontSize:11}}>
                Reset to default
              </button>
            </div>

            {/* Active source badge */}
            <div style={{marginTop:10,fontSize:10,color:C.muted}}>
              Active: {srcMode === "api"
                ? <span style={{color:C.roman}}>⚡ Google Sheets API · sheet <code style={{color:C.subtext}}>{apiSheetId || "—"}</code></span>
                : sheetUrl !== DEFAULT_SHEET_CSV_URL
                  ? <span style={{color:"#f97316"}}>📄 Custom CSV · <span style={{wordBreak:"break-all"}}>{sheetUrl}</span></span>
                  : <span>📄 Default published CSV</span>
              }
            </div>
          </div>
        );
      })()}

      {lastUpdate && (
        <div style={{fontSize:11,color:C.muted,marginTop:4}}>
          Last updated: {lastUpdate.toLocaleTimeString()}
        </div>
      )}

      {/* Error banner (non-daily tabs) */}
      {status==="error" && activeTab!=="Daily (Live)" && (
        <div style={{background:"#1a0a0a",border:"1px solid #4a1a1a",borderRadius:10,
          padding:"12px 18px",marginTop:14,fontSize:12,color:"#f87171",display:"flex",
          alignItems:"center",justifyContent:"space-between"}}>
          <span>⚠ Could not reach Google Sheet — showing historical data. <span style={{color:C.muted}}>{errMsg}</span></span>
          <button onClick={()=>load()} style={{background:"transparent",border:"1px solid #f87171",
            borderRadius:14,padding:"4px 12px",cursor:"pointer",fontFamily:FONT,
            fontSize:11,color:"#f87171",outline:"none",marginLeft:12}}>Retry</button>
        </div>
      )}

      {/* Filters */}
      <div style={{background:C.card,border:`1px solid ${C.border}`,borderRadius:12,
        padding:"14px 20px",marginTop:16,marginBottom:20,display:"flex",alignItems:"center",gap:10}}>
        <span style={{fontSize:10,fontWeight:700,color:C.muted,letterSpacing:1,minWidth:110}}>FILTER BY YEAR:</span>
        <div style={{display:"flex",gap:4}}>
          {YEARS.map(y=><Pill key={y} active={yearFilter===y} onClick={()=>setYearFilter(y)}>{y}</Pill>)}
        </div>
      </div>

      {/* KPI Cards */}
      <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:16,marginBottom:20}}>
        <div style={{background:C.card,border:`1px solid ${C.border}`,borderRadius:12,padding:"18px 22px"}}>
          <div style={{fontSize:10,fontWeight:700,color:C.muted,letterSpacing:1,marginBottom:12}}>AVG PER MONTH</div>
          <div style={{display:"flex",gap:32}}>
            <div>
              <div style={{fontSize:11,color:C.subtext,marginBottom:4}}>Encounters</div>
              <div style={{fontSize:28,fontWeight:700,color:C.teladoc}}>{avgEncounters.toLocaleString()}</div>
            </div>
            <div>
              <div style={{fontSize:11,color:C.subtext,marginBottom:4}}>Revenue</div>
              <div style={{fontSize:28,fontWeight:700,color:C.mdlive}}>${avgRevenue.toLocaleString()}</div>
            </div>
          </div>
        </div>
        <div style={{background:C.card,border:`1px solid ${C.border}`,borderRadius:12,padding:"18px 22px"}}>
          <div style={{fontSize:10,fontWeight:700,color:C.muted,letterSpacing:1,marginBottom:12}}>AVG VISITS / MONTH</div>
          <div style={{display:"flex",gap:32}}>
            <div>
              <div style={{fontSize:11,color:C.subtext,marginBottom:4}}>Roman Health</div>
              <div style={{fontSize:28,fontWeight:700,color:C.roman}}>{avgRoman.toLocaleString()}</div>
            </div>
            <div>
              <div style={{fontSize:11,color:C.subtext,marginBottom:4}}>MDLive + Teladoc</div>
              <div style={{fontSize:28,fontWeight:700,color:C.teladoc}}>{avgMdTel.toLocaleString()}</div>
            </div>
          </div>
        </div>
        <div style={{background:C.card,border:`1px solid ${C.border}`,borderRadius:12,padding:"18px 22px"}}>
          <div style={{fontSize:10,fontWeight:700,color:C.muted,letterSpacing:1,marginBottom:12}}>AVG VISITS / DAY</div>
          <div style={{display:"flex",gap:20}}>
            {[["Roman Health",avgRomanDay,C.roman],["MDLive",avgMdDay,C.mdlive],["Teladoc",avgTelDay,C.teladoc]].map(([n,v,col])=>(
              <div key={n}>
                <div style={{fontSize:11,color:C.subtext,marginBottom:4}}>{n}</div>
                <div style={{fontSize:28,fontWeight:700,color:col}}>{v}</div>
              </div>
            ))}
          </div>
        </div>
      </div>

      {/* Tabs */}
      <div style={{display:"flex",gap:8,flexWrap:"wrap",marginBottom:20}}>
        {TABS.map(t=><TabBtn key={t} label={t} active={activeTab===t} onClick={()=>setActiveTab(t)}/>)}
      </div>

      {/* Chart panel */}
      <div style={{background:C.card,border:`1px solid ${C.border}`,borderRadius:14,padding:"24px 20px 16px"}}>
        <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:20}}>
          <div style={{fontSize:15,fontWeight:700,color:"#e8f0fc"}}>
            {{"Revenue":"Monthly Revenue","Encounter Volume":"Monthly Encounter Volume",
              "Visit Breakdown":"Visit Type Breakdown",
              "Roman Health vs Teladoc+MDLive":"Roman Health vs Teladoc + MDLive",
              "Daily (Live)":liveData?`Daily Activity — ${liveData.currentMonth} 2026`:"Daily Activity (Live)"}[activeTab]}
            {activeTab==="Daily (Live)"&&liveData&&(
              <span style={{fontSize:12,fontWeight:400,color:C.roman,marginLeft:8}}>● LIVE</span>
            )}
          </div>
          {activeTab==="Daily (Live)"&&liveData&&(
            <div style={{fontSize:11,color:C.subtext}}>
              {liveData.daysLogged} days · MTD: <span style={{color:C.teladoc}}>{fmtF$(liveData.monthly.revenue)}</span>
            </div>
          )}
        </div>
        {renderChart()}
        {activeTab!=="Daily (Live)"&&(
          <div style={{textAlign:"right",marginTop:10,fontSize:11,color:C.muted,
            display:"flex",alignItems:"center",justifyContent:"flex-end",gap:6}}>
            <span style={{width:7,height:7,borderRadius:"50%",background:dotColor,
              boxShadow:status==="live"?`0 0 4px ${dotColor}`:"none",display:"inline-block"}}/>
            {status==="live"?"Current month live · prior months historical":"Historical data"}
          </div>
        )}
      </div>
    </div>
  );
}
