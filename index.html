import { useState, useRef, useCallback, useEffect } from “react”;

const FCL_ID = 1871;
const FCL_COLOR = “#0066B3”;
const FCL_DARK = “#003E6B”;
const OPP_COLOR = “#CC2200”;
const OPP_DARK = “#880000”;

// ── Demo data ──────────────────────────────────────────────────────────────
const DEMO = {
match: { homeTeam: { id: FCL_ID, name: “FC Luzern”, crest: “https://crests.football-data.org/1871.png” }, awayTeam: { id: 999, name: “Servette FC”, crest: “https://crests.football-data.org/487.png” }, utcDate: new Date(Date.now() + 5 * 86400000).toISOString(), status: “TIMED”, competition: { name: “Super League” } },
home: {
formation: “4-3-3”,
startXI: [
{ player: { name: “P. Loretz”, shirtNumber: 1 }, position: “GK” },
{ player: { name: “A. Cigániks”, shirtNumber: 2 }, position: “RB” },
{ player: { name: “S. Knezevic”, shirtNumber: 5 }, position: “CB” },
{ player: { name: “R. Fernandes”, shirtNumber: 6 }, position: “CB” },
{ player: { name: “S. Ottiger”, shirtNumber: 3 }, position: “LB” },
{ player: { name: “P. Dorn”, shirtNumber: 8 }, position: “CM” },
{ player: { name: “T. Owusu”, shirtNumber: 14 }, position: “CM” },
{ player: { name: “M. Di Giusto”, shirtNumber: 10 }, position: “AM” },
{ player: { name: “L. Villiger”, shirtNumber: 11 }, position: “RW” },
{ player: { name: “A. Grbic”, shirtNumber: 9 }, position: “ST” },
{ player: { name: “K. Spadanuda”, shirtNumber: 7 }, position: “LW” },
],
substitutes: [{ player: { name: “D. Heller”, shirtNumber: 12 }, position: “GK” }, { player: { name: “A. Bajrami”, shirtNumber: 13 }, position: “DEF” }, { player: { name: “T. Abe”, shirtNumber: 20 }, position: “MID” }],
},
away: {
formation: “4-2-3-1”,
startXI: [
{ player: { name: “J. Frick”, shirtNumber: 1 }, position: “GK” },
{ player: { name: “R. Cognat”, shirtNumber: 2 }, position: “RB” },
{ player: { name: “S. Rouiller”, shirtNumber: 5 }, position: “CB” },
{ player: { name: “T. Severin”, shirtNumber: 6 }, position: “CB” },
{ player: { name: “M. Vouilloz”, shirtNumber: 3 }, position: “LB” },
{ player: { name: “T. Ondoua”, shirtNumber: 8 }, position: “CDM” },
{ player: { name: “B. Fofana”, shirtNumber: 4 }, position: “CDM” },
{ player: { name: “C. Stevanovic”, shirtNumber: 11 }, position: “RW” },
{ player: { name: “T. Camara”, shirtNumber: 10 }, position: “AM” },
{ player: { name: “J. Kutesa”, shirtNumber: 7 }, position: “LW” },
{ player: { name: “B. Crivelli”, shirtNumber: 9 }, position: “ST” },
],
substitutes: [{ player: { name: “A. Diallo”, shirtNumber: 16 }, position: “GK” }, { player: { name: “H. Mayembo”, shirtNumber: 21 }, position: “DEF” }],
},
};

// ── Helpers ────────────────────────────────────────────────────────────────
function lastName(n) { if (!n) return “?”; return n.trim().split(” “).pop(); }

function buildPositions(lineup, side) {
// side: “left” = 0..0.5, “right” = 0.5..1
const formation = lineup.formation || “4-4-2”;
const parts = formation.split(”-”).map(Number);
const players = […(lineup.startXI || [])];
const gk = players.filter(p => p.position === “GK”);
const rest = players.filter(p => p.position !== “GK”);
const lines = [gk];
let idx = 0;
parts.forEach(n => { lines.push(rest.slice(idx, idx + n)); idx += n; });

const totalCols = lines.length;
const positions = {};

lines.forEach((line, ci) => {
const totalRows = line.length;
line.forEach((p, ri) => {
// x: columns left-to-right within the half
let xFrac = (ci + 0.5) / totalCols; // 0..1 within half
let yFrac = (ri + 0.5) / totalRows; // 0..1 top-to-bottom

```
  let x, y;
  if (side === "left") {
    x = 0.03 + xFrac * 0.44; // 3%..47%
  } else {
    x = 0.97 - xFrac * 0.44; // 97%..53% (mirrored)
  }
  y = 0.07 + yFrac * 0.86;
  const key = `${p.player?.name}-${p.player?.shirtNumber}`;
  positions[key] = { x, y, ...p, id: key };
});
```

});
return positions;
}

// ── Claude proxy fetch ─────────────────────────────────────────────────────
async function proxyFetch(apiKey, path) {
const prompt = `Make a GET request to: https://api.football-data.org/v4${path} Header: X-Auth-Token: ${apiKey} Reply with ONLY the raw JSON body. No explanation, no markdown.`;
const res = await fetch(“https://api.anthropic.com/v1/messages”, {
method: “POST”,
headers: { “Content-Type”: “application/json” },
body: JSON.stringify({ model: “claude-sonnet-4-20250514”, max_tokens: 4000, messages: [{ role: “user”, content: prompt }] }),
});
const d = await res.json();
const text = d.content?.find(b => b.type === “text”)?.text || “{}”;
return JSON.parse(text.replace(/`json|`/g, “”).trim());
}

// ── Draggable Player Dot ───────────────────────────────────────────────────
function PlayerDot({ id, x, y, number, name, color, dark, fieldRef, onMove, flipped }) {
const dragging = useRef(false);
const startPos = useRef({});

function getFieldRect() {
return fieldRef.current?.getBoundingClientRect() || { left: 0, top: 0, width: 1, height: 1 };
}

function onPointerDown(e) {
e.preventDefault();
dragging.current = true;
const r = getFieldRect();
startPos.current = { cx: e.clientX, cy: e.clientY, ox: x, oy: y };
window.addEventListener(“pointermove”, onPointerMove);
window.addEventListener(“pointerup”, onPointerUp);
}

function onPointerMove(e) {
if (!dragging.current) return;
const r = getFieldRect();
const dx = (e.clientX - startPos.current.cx) / r.width;
const dy = (e.clientY - startPos.current.cy) / r.height;
const nx = Math.max(0.02, Math.min(0.98, startPos.current.ox + dx));
const ny = Math.max(0.02, Math.min(0.98, startPos.current.oy + dy));
onMove(id, nx, ny);
}

function onPointerUp() {
dragging.current = false;
window.removeEventListener(“pointermove”, onPointerMove);
window.removeEventListener(“pointerup”, onPointerUp);
}

const px = x * 100 + “%”;
const py = y * 100 + “%”;

return (
<div onPointerDown={onPointerDown} style={{ position: “absolute”, left: px, top: py, transform: “translate(-50%, -50%)”, cursor: “grab”, userSelect: “none”, touchAction: “none”, zIndex: 10, display: “flex”, flexDirection: “column”, alignItems: “center”, gap: 2 }}>
<div style={{ width: 36, height: 36, borderRadius: “50%”, background: `linear-gradient(135deg, ${dark}, ${color})`, border: “2.5px solid rgba(255,255,255,0.92)”, display: “flex”, alignItems: “center”, justifyContent: “center”, fontSize: 12, fontWeight: 900, color: “#fff”, boxShadow: “0 3px 12px rgba(0,0,0,0.55)”, transition: “transform 0.1s” }}>
{number}
</div>
<div style={{ fontSize: 9, fontWeight: 700, color: “#fff”, textShadow: “0 1px 5px rgba(0,0,0,1)”, textAlign: “center”, maxWidth: 52, lineHeight: 1.2, pointerEvents: “none” }}>
{lastName(name)}
</div>
</div>
);
}

// ── Main App ───────────────────────────────────────────────────────────────
export default function App() {
const [screen, setScreen] = useState(“setup”); // setup | matches | field
const [inputKey, setInputKey] = useState(””);
const [apiKey, setApiKey] = useState(””);
const [tab, setTab] = useState(“upcoming”);
const [matches, setMatches] = useState([]);
const [loadingMatches, setLoadingMatches] = useState(false);
const [loadingLineup, setLoadingLineup] = useState(false);
const [error, setError] = useState(””);
const [matchInfo, setMatchInfo] = useState(null);
const [fclPositions, setFclPositions] = useState({});
const [oppPositions, setOppPositions] = useState({});
const [fclLineup, setFclLineup] = useState(null);
const [oppLineup, setOppLineup] = useState(null);
const [flipped, setFlipped] = useState(false); // false = FCL left, true = FCL right
const [demo, setDemo] = useState(false);
const fieldRef = useRef(null);

function initPositions(homeLineup, awayLineup, fclIsHome, flp = false) {
const fclSide = flp ? “right” : “left”;
const oppSide = flp ? “left” : “right”;
const fclPos = buildPositions(homeLineup && fclIsHome ? homeLineup : awayLineup, fclSide);
const oppPos = buildPositions(homeLineup && fclIsHome ? awayLineup : homeLineup, oppSide);
setFclPositions(fclPos);
setOppPositions(oppPos);
}

function flip() {
const newFlipped = !flipped;
setFlipped(newFlipped);
// Re-mirror all positions
setFclPositions(prev => {
const next = {};
Object.entries(prev).forEach(([k, v]) => { next[k] = { …v, x: 1 - v.x }; });
return next;
});
setOppPositions(prev => {
const next = {};
Object.entries(prev).forEach(([k, v]) => { next[k] = { …v, x: 1 - v.x }; });
return next;
});
}

function moveFcl(id, x, y) { setFclPositions(p => ({ …p, [id]: { …p[id], x, y } })); }
function moveOpp(id, x, y) { setOppPositions(p => ({ …p, [id]: { …p[id], x, y } })); }

async function handleKey() {
if (!inputKey.trim()) return;
setLoadingMatches(true); setError(””);
try {
const data = await proxyFetch(inputKey.trim(), `/teams/${FCL_ID}/matches?status=SCHEDULED,TIMED,LIVE,FINISHED&limit=8`);
if (!data.matches) throw new Error(“Ungültiger Key”);
setApiKey(inputKey.trim());
const sorted = […(data.matches || [])].sort((a, b) => new Date(b.utcDate) - new Date(a.utcDate));
setMatches(sorted);
setScreen(“matches”);
} catch (e) { setError(“Key ungültig oder API nicht erreichbar.”); }
finally { setLoadingMatches(false); }
}

async function selectMatch(m) {
setLoadingLineup(true); setError(””);
try {
const data = await proxyFetch(apiKey, `/matches/${m.id}`);
const fclIsHome = m.homeTeam?.id === FCL_ID;
const homeL = data.lineups?.home;
const awayL = data.lineups?.away;
if (!homeL?.startXI?.length && !awayL?.startXI?.length) throw new Error(“Aufstellung noch nicht verfügbar (ca. 1h vor Anpfiff).”);
const fclL = fclIsHome ? homeL : awayL;
const oppL = fclIsHome ? awayL : homeL;
setFclLineup(fclL); setOppLineup(oppL);
setMatchInfo(m);
setFlipped(false);
initPositions(homeL, awayL, fclIsHome, false);
setScreen(“field”);
} catch (e) { setError(e.message); }
finally { setLoadingLineup(false); }
}

function startDemo() {
const m = DEMO.match;
const fclIsHome = m.homeTeam.id === FCL_ID;
setFclLineup(DEMO.home); setOppLineup(DEMO.away);
setMatchInfo(m); setFlipped(false); setDemo(true);
initPositions(DEMO.home, DEMO.away, fclIsHome, false);
setScreen(“field”);
}

function switchTab(t) {
setTab(t);
const filtered = matches.filter(m => t === “upcoming” ? [“TIMED”, “SCHEDULED”, “LIVE”].includes(m.status) : m.status === “FINISHED”);
return filtered;
}

const fmtDate = (iso) => new Date(iso).toLocaleDateString(“de-CH”, { weekday: “short”, day: “numeric”, month: “short”, hour: “2-digit”, minute: “2-digit” });

const visibleMatches = matches.filter(m => tab === “upcoming” ? [“TIMED”, “SCHEDULED”, “LIVE”].includes(m.status) : m.status === “FINISHED”);

// ── SETUP SCREEN ──
if (screen === “setup”) return (
<div style={{ fontFamily: “‘Arial Narrow’, ‘Barlow Condensed’, sans-serif”, background: “linear-gradient(160deg,#0a0f1e 0%,#0c1a0e 60%,#0a0f1e 100%)”, minHeight: “100vh”, color: “#fff”, display: “flex”, flexDirection: “column”, alignItems: “center”, justifyContent: “center”, padding: 20 }}>
<style>{`*{box-sizing:border-box;margin:0;padding:0}@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}input:focus{outline:none}button:active{transform:scale(.97)}`}</style>
<img src=“https://crests.football-data.org/1871.png” alt=”” style={{ width: 64, height: 64, objectFit: “contain”, marginBottom: 16, filter: “drop-shadow(0 4px 12px rgba(0,100,200,0.5))” }} onError={e => e.currentTarget.style.display = “none”} />
<div style={{ fontSize: 28, fontWeight: 900, letterSpacing: 2, marginBottom: 4 }}>FC LUZERN</div>
<div style={{ fontSize: 12, fontWeight: 700, letterSpacing: 4, opacity: 0.5, marginBottom: 32 }}>TAKTIK-BOARD</div>
<div style={{ width: “100%”, maxWidth: 340, background: “rgba(255,255,255,0.05)”, border: “1px solid rgba(255,255,255,0.1)”, borderRadius: 14, padding: 22 }}>
<div style={{ fontSize: 13, opacity: 0.6, marginBottom: 14 }}>API-Key von <span style={{ color: “#4db8ff” }}>football-data.org</span></div>
<div style={{ display: “flex”, gap: 8, marginBottom: 10 }}>
<input value={inputKey} onChange={e => setInputKey(e.target.value)} onKeyDown={e => e.key === “Enter” && handleKey()}
placeholder=“API-Key eingeben…” style={{ flex: 1, background: “rgba(255,255,255,0.07)”, border: “1px solid rgba(255,255,255,0.15)”, borderRadius: 8, padding: “11px 13px”, color: “#fff”, fontSize: 13, fontFamily: “monospace” }} />
<button onClick={handleKey} disabled={loadingMatches} style={{ background: “#0066B3”, border: “none”, borderRadius: 8, padding: “11px 16px”, color: “#fff”, fontSize: 14, fontWeight: 700, fontFamily: “inherit”, cursor: “pointer” }}>
{loadingMatches ? <span style={{ animation: “pulse 1s infinite”, display: “inline-block” }}>…</span> : “OK”}
</button>
</div>
{error && <div style={{ color: “#ff8080”, fontSize: 12, marginBottom: 10 }}>{error}</div>}
<button onClick={startDemo} style={{ width: “100%”, background: “transparent”, border: “1px solid rgba(255,255,255,0.13)”, borderRadius: 8, padding: 10, color: “rgba(255,255,255,0.45)”, fontSize: 13, fontFamily: “inherit”, cursor: “pointer” }}>
Demo-Modus (Beispieldaten)
</button>
</div>
</div>
);

// ── MATCHES SCREEN ──
if (screen === “matches”) return (
<div style={{ fontFamily: “‘Arial Narrow’, ‘Barlow Condensed’, sans-serif”, background: “linear-gradient(160deg,#0a0f1e 0%,#0c1a0e 60%,#0a0f1e 100%)”, minHeight: “100vh”, color: “#fff” }}>
<style>{`*{box-sizing:border-box;margin:0;padding:0}@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}@keyframes fadeUp{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}.fade{animation:fadeUp .3s ease}button:active{transform:scale(.97)}`}</style>
<div style={{ background: “linear-gradient(90deg,#003E6B,#0066B3)”, padding: “13px 16px”, display: “flex”, alignItems: “center”, gap: 11, boxShadow: “0 4px 20px rgba(0,62,107,.7)” }}>
<img src=“https://crests.football-data.org/1871.png” alt=”” style={{ width: 36, height: 36, objectFit: “contain” }} onError={e => e.currentTarget.style.display = “none”} />
<div>
<div style={{ fontSize: 19, fontWeight: 900, letterSpacing: 1.5, lineHeight: 1 }}>FC LUZERN</div>
<div style={{ fontSize: 10, fontWeight: 700, letterSpacing: 3, opacity: .75 }}>TAKTIK-BOARD</div>
</div>
<button onClick={() => setScreen(“setup”)} style={{ marginLeft: “auto”, background: “transparent”, border: “none”, color: “rgba(255,255,255,.5)”, fontSize: 12, fontFamily: “inherit”, cursor: “pointer” }}>← Key ändern</button>
</div>
<div style={{ maxWidth: 640, margin: “0 auto”, padding: “16px 13px 40px” }}>
<div style={{ display: “flex”, gap: 8, marginBottom: 16 }}>
{[“upcoming”, “past”].map(t => (
<button key={t} onClick={() => setTab(t)} style={{ background: tab === t ? “#0066B3” : “rgba(255,255,255,.06)”, border: `1px solid ${tab === t ? "#0066B3" : "rgba(255,255,255,.11)"}`, borderRadius: 8, padding: “8px 16px”, color: tab === t ? “#fff” : “rgba(255,255,255,.45)”, fontSize: 13, fontWeight: 700, fontFamily: “inherit”, cursor: “pointer” }}>
{t === “upcoming” ? “Nächste Spiele” : “Letzte Spiele”}
</button>
))}
</div>
{loadingLineup && <div style={{ textAlign: “center”, padding: 30, opacity: .5, animation: “pulse 1.4s infinite” }}>Lade Aufstellung…</div>}
{error && <div style={{ background: “rgba(200,0,0,.13)”, border: “1px solid rgba(200,0,0,.28)”, borderRadius: 10, padding: “12px 14px”, color: “#ff8080”, fontSize: 13, marginBottom: 14 }}>{error}</div>}
{visibleMatches.length === 0 && !loadingLineup && <div style={{ opacity: .4, textAlign: “center”, padding: 30 }}>Keine Spiele gefunden.</div>}
{visibleMatches.map((m, i) => {
const isHome = m.homeTeam?.id === FCL_ID;
const opp = isHome ? m.awayTeam : m.homeTeam;
const isFinished = m.status === “FINISHED”;
const isLive = m.status === “LIVE”;
const hs = m.score?.fullTime?.home, as_ = m.score?.fullTime?.away;
const score = isFinished && hs != null ? `${isHome ? hs : as_} : ${isHome ? as_ : hs}` : null;
return (
<div key={m.id} className=“fade” onClick={() => selectMatch(m)}
style={{ background: “rgba(255,255,255,.04)”, border: “1px solid rgba(255,255,255,.08)”, borderRadius: 11, padding: “13px 14px”, marginBottom: 8, cursor: “pointer”, display: “flex”, alignItems: “center”, justifyContent: “space-between”, gap: 10, animationDelay: `${i * .04}s` }}>
<div style={{ display: “flex”, alignItems: “center”, gap: 11 }}>
<img src={opp?.crest || “”} alt=”” style={{ width: 34, height: 34, objectFit: “contain” }} onError={e => e.currentTarget.style.opacity = “.15”} />
<div>
<div style={{ fontSize: 15, fontWeight: 800 }}>{isHome ? “FCL” : opp?.name} vs {isHome ? opp?.name : “FCL”}</div>
<div style={{ fontSize: 11, opacity: .45, marginTop: 1 }}>{m.competition?.name} · {fmtDate(m.utcDate)}</div>
</div>
</div>
{score
? <div style={{ fontSize: 20, fontWeight: 900, letterSpacing: 1 }}>{score}</div>
: <div style={{ fontSize: 10, fontWeight: 700, letterSpacing: 1, padding: “3px 9px”, borderRadius: 20, background: isLive ? “rgba(255,50,50,.18)” : “rgba(255,255,255,.07)”, color: isLive ? “#ff6060” : “rgba(255,255,255,.45)”, animation: isLive ? “pulse 1.5s infinite” : “none” }}>{isLive ? “LIVE” : “Geplant”}</div>
}
<span style={{ fontSize: 18, opacity: .3 }}>→</span>
</div>
);
})}
</div>
</div>
);

// ── FIELD SCREEN ──
const fclIsHome = matchInfo?.homeTeam?.id === FCL_ID;
const opp = fclIsHome ? matchInfo?.awayTeam : matchInfo?.homeTeam;
const fclOnLeft = !flipped;

return (
<div style={{ fontFamily: “‘Arial Narrow’, ‘Barlow Condensed’, sans-serif”, background: “#0a0f14”, minHeight: “100vh”, color: “#fff”, display: “flex”, flexDirection: “column” }}>
<style>{`*{box-sizing:border-box;margin:0;padding:0}@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}button:active{transform:scale(.97)}`}</style>

```
  {/* TOP BAR */}
  <div style={{ background: "linear-gradient(90deg,#003E6B,#0066B3)", padding: "10px 14px", display: "flex", alignItems: "center", gap: 10, boxShadow: "0 3px 16px rgba(0,62,107,.7)", flexShrink: 0 }}>
    <button onClick={() => setScreen("matches")} style={{ background: "rgba(255,255,255,.15)", border: "none", borderRadius: 7, padding: "6px 11px", color: "#fff", fontSize: 13, fontFamily: "inherit", cursor: "pointer" }}>←</button>
    <img src={matchInfo?.homeTeam?.crest || ""} alt="" style={{ width: 28, height: 28, objectFit: "contain" }} onError={e => e.currentTarget.style.display = "none"} />
    <div style={{ fontSize: 14, fontWeight: 800, flex: 1, lineHeight: 1.2 }}>
      {matchInfo?.homeTeam?.name} vs {matchInfo?.awayTeam?.name}
      <div style={{ fontSize: 10, fontWeight: 600, opacity: .6, letterSpacing: 1 }}>{matchInfo?.competition?.name} · {fmtDate(matchInfo?.utcDate)}</div>
    </div>
    <img src={opp?.crest || ""} alt="" style={{ width: 28, height: 28, objectFit: "contain" }} onError={e => e.currentTarget.style.display = "none"} />
  </div>

  {/* TEAM LABELS */}
  <div style={{ display: "flex", justifyContent: "space-between", padding: "8px 14px 4px", flexShrink: 0 }}>
    <div style={{ display: "flex", alignItems: "center", gap: 6 }}>
      <div style={{ width: 12, height: 12, borderRadius: "50%", background: `linear-gradient(135deg, ${FCL_DARK}, ${FCL_COLOR})`, border: "1.5px solid rgba(255,255,255,.7)" }} />
      <span style={{ fontSize: 12, fontWeight: 800, letterSpacing: 1 }}>{fclOnLeft ? "FCL" : opp?.name}</span>
      <span style={{ fontSize: 10, opacity: .4 }}>({fclOnLeft ? fclLineup?.formation : oppLineup?.formation})</span>
    </div>
    <button onClick={flip} style={{ background: "rgba(255,255,255,.08)", border: "1px solid rgba(255,255,255,.15)", borderRadius: 8, padding: "5px 12px", color: "rgba(255,255,255,.8)", fontSize: 12, fontWeight: 700, fontFamily: "inherit", cursor: "pointer", letterSpacing: .5 }}>
      ⇄ Seite wechseln
    </button>
    <div style={{ display: "flex", alignItems: "center", gap: 6 }}>
      <span style={{ fontSize: 10, opacity: .4 }}>({fclOnLeft ? oppLineup?.formation : fclLineup?.formation})</span>
      <span style={{ fontSize: 12, fontWeight: 800, letterSpacing: 1 }}>{fclOnLeft ? opp?.name : "FCL"}</span>
      <div style={{ width: 12, height: 12, borderRadius: "50%", background: `linear-gradient(135deg, ${OPP_DARK}, ${OPP_COLOR})`, border: "1.5px solid rgba(255,255,255,.7)" }} />
    </div>
  </div>

  {/* FIELD */}
  <div style={{ flex: 1, padding: "0 6px 6px", minHeight: 0 }}>
    <div ref={fieldRef} style={{ position: "relative", width: "100%", height: "100%", minHeight: 320, borderRadius: 10, overflow: "hidden", boxShadow: "0 6px 30px rgba(0,0,0,.7)" }}>
      {/* Grass */}
      <div style={{ position: "absolute", inset: 0, background: "linear-gradient(90deg,#1a5c2a,#1e7a33,#1a5c2a)" }} />
      {[...Array(12)].map((_, i) => (
        <div key={i} style={{ position: "absolute", top: 0, bottom: 0, left: `${i * (100 / 12)}%`, width: `${100 / 12}%`, background: i % 2 === 0 ? "rgba(255,255,255,.03)" : "transparent" }} />
      ))}

      {/* SVG Field Lines */}
      <svg style={{ position: "absolute", inset: 0, width: "100%", height: "100%" }} viewBox="0 0 800 500" preserveAspectRatio="none">
        <rect x="20" y="20" width="760" height="460" fill="none" stroke="rgba(255,255,255,.4)" strokeWidth="2" />
        <line x1="400" y1="20" x2="400" y2="480" stroke="rgba(255,255,255,.38)" strokeWidth="2" />
        <circle cx="400" cy="250" r="70" fill="none" stroke="rgba(255,255,255,.38)" strokeWidth="2" />
        <circle cx="400" cy="250" r="4" fill="rgba(255,255,255,.4)" />
        {/* Left penalty */}
        <rect x="20" y="140" width="130" height="220" fill="none" stroke="rgba(255,255,255,.35)" strokeWidth="2" />
        <rect x="20" y="180" width="55" height="140" fill="none" stroke="rgba(255,255,255,.28)" strokeWidth="1.5" />
        <circle cx="120" cy="250" r="4" fill="rgba(255,255,255,.3)" />
        {/* Right penalty */}
        <rect x="650" y="140" width="130" height="220" fill="none" stroke="rgba(255,255,255,.35)" strokeWidth="2" />
        <rect x="725" y="180" width="55" height="140" fill="none" stroke="rgba(255,255,255,.28)" strokeWidth="1.5" />
        <circle cx="680" cy="250" r="4" fill="rgba(255,255,255,.3)" />
      </svg>

      {/* FCL Players */}
      {Object.values(fclPositions).map(p => (
        <PlayerDot key={p.id} id={p.id} x={p.x} y={p.y}
          number={p.player?.shirtNumber || "?"} name={p.player?.name}
          color={FCL_COLOR} dark={FCL_DARK}
          fieldRef={fieldRef} onMove={moveFcl} flipped={flipped} />
      ))}

      {/* Opponent Players */}
      {Object.values(oppPositions).map(p => (
        <PlayerDot key={p.id} id={p.id} x={p.x} y={p.y}
          number={p.player?.shirtNumber || "?"} name={p.player?.name}
          color={OPP_COLOR} dark={OPP_DARK}
          fieldRef={fieldRef} onMove={moveOpp} flipped={flipped} />
      ))}

      {/* No lineup message */}
      {Object.keys(fclPositions).length === 0 && (
        <div style={{ position: "absolute", inset: 0, display: "flex", alignItems: "center", justifyContent: "center", color: "rgba(255,255,255,.4)", fontSize: 14 }}>
          Aufstellung nicht verfügbar
        </div>
      )}
    </div>
  </div>

  {/* BENCH */}
  {(fclLineup?.substitutes?.length > 0 || oppLineup?.substitutes?.length > 0) && (
    <div style={{ padding: "8px 12px 10px", flexShrink: 0, background: "rgba(0,0,0,.3)" }}>
      <div style={{ fontSize: 9, fontWeight: 700, letterSpacing: 2, opacity: .35, marginBottom: 6, textTransform: "uppercase" }}>Ersatzbank</div>
      <div style={{ display: "flex", gap: 6, flexWrap: "wrap" }}>
        {(fclLineup?.substitutes || []).map((s, i) => (
          <div key={i} style={{ background: `rgba(0,100,180,.25)`, border: "1px solid rgba(0,150,255,.2)", borderRadius: 6, padding: "4px 9px", display: "flex", alignItems: "center", gap: 5, fontSize: 11 }}>
            <span style={{ fontWeight: 800, opacity: .6, fontSize: 10 }}>{s.player?.shirtNumber}</span>
            <span>{lastName(s.player?.name)}</span>
          </div>
        ))}
        {(oppLineup?.substitutes || []).map((s, i) => (
          <div key={i} style={{ background: `rgba(180,30,0,.22)`, border: "1px solid rgba(220,50,0,.2)", borderRadius: 6, padding: "4px 9px", display: "flex", alignItems: "center", gap: 5, fontSize: 11 }}>
            <span style={{ fontWeight: 800, opacity: .6, fontSize: 10 }}>{s.player?.shirtNumber}</span>
            <span>{lastName(s.player?.name)}</span>
          </div>
        ))}
      </div>
    </div>
  )}
</div>
```

);
}
