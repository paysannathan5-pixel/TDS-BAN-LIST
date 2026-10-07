# TDS-BAN-LIST
Erm , 67

<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>TDS Ban List</title>
<style>
:root{color-scheme:dark;--bg:#0b0d12;--panel:#151923;--card:#1b2030;--border:#30384b;--text:#f2f4f8;--muted:#9ca7ba;--accent:#8b6cff;--danger:#ff647c}
*{box-sizing:border-box}body{margin:0;background:radial-gradient(circle at top,#171c2c 0,#0b0d12 45%);font-family:system-ui,-apple-system,Segoe UI,sans-serif;color:var(--text)}
main{max-width:1050px;margin:auto;padding:28px 16px 50px}.hero{padding:22px;border:1px solid var(--border);background:rgba(21,25,35,.9);border-radius:18px;margin-bottom:18px}
h1{margin:0 0 6px;font-size:30px}.sub{color:var(--muted);margin:0}.top{display:flex;gap:10px;flex-wrap:wrap;align-items:center;justify-content:space-between;margin:16px 0}
.stats{font-weight:700}.actions{display:flex;gap:8px;flex-wrap:wrap}button,input{font:inherit}
input{width:100%;padding:12px 14px;border:1px solid var(--border);border-radius:11px;background:#10141d;color:var(--text);outline:none}
input:focus{border-color:var(--accent)}button{border:1px solid var(--border);background:var(--card);color:var(--text);border-radius:10px;padding:10px 13px;cursor:pointer}button:hover{border-color:var(--accent)}
section{margin-top:22px}.section-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:9px}h2{font-size:18px;margin:0}.muted{font-size:12px;color:var(--muted)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(145px,1fr));gap:9px}
.tower{min-height:67px;text-align:left}.tower strong{display:block}.tower small{display:block;color:var(--muted);margin-top:5px;font-size:11px}
.tower.banned{border-color:var(--danger);background:rgba(255,100,124,.12)}.tower.banned strong{text-decoration:line-through}.tower.banned small{color:#ff9aaa}
.empty{color:var(--muted);padding:18px 0}.foot{margin-top:25px;color:var(--muted);font-size:12px;line-height:1.5}
@media(max-width:520px){main{padding:16px 11px 35px}h1{font-size:25px}.grid{grid-template-columns:repeat(2,minmax(0,1fr))}}
</style>
</head>
<body>
<main id="app">
<div class="hero">
<h1>⚔️ TDS Ban List</h1>
<p class="sub">Choose towers for your challenge. Tap a tower to ban or unban it.</p>
<div class="top">
<div class="stats" id="stats">0 banned</div>
<div class="actions"><button id="reset">Reset bans</button><button id="banAll">Ban all</button></div>
</div>
<input id="search" type="search" placeholder="Search towers…" aria-label="Search towers">
</div>
<div id="sections"></div>
<p class="foot">Standalone local website • Your bans stay on this page while it is open. This version uses the current tower categories listed by the Official Tower Defense Simulator Wiki.</p>
</main>
<script>
const groups={
"Starter":["Scout","Sniper","Paintballer","Demoman","Boomerang","Slime Trooper","Soldier"],
"Intermediate":["Freezer","Assassin","Militant","Shotgunner","Hunter","Pyromancer","Ace Pilot","Medic","Farm","Electroshocker","Rocketeer","Trapper","Pulse Trooper","Military Base","Crook Boss"],
"Advanced":["Commander","Warden","Cowboy","DJ Booth","Tesla","Saboteur","Minigunner","Ranger","Pursuit","Gatling Gun","Turret","Mortar","Mercenary Base"],
"Hardcore":["Brawler","Necromancer","Accelerator","Engineer","Hacker"],
"Evolved":["Operator","Enforcer","Kingpin","Juggernaut"],
"Golden Perks":["Golden Minigunner","Golden Pyromancer","Golden Crook Boss","Golden Scout","Golden Cowboy","Golden Soldier","Golden Demoman","Golden Snowballer"],
"Exclusive":["Gladiator","Commando","Slasher","Frost Blaster","Archer","Swarmer","Toxic Gunner","Sledger","Executioner","Elf Camp","Jester","Cryomancer","Hallow Punk","Harvester","Snowballer","Elementalist","Firework Technician","Biologist","Warlock","Spotlight Tech","War Machine","Mecha Base"]
};
const banned=new Set(), all=Object.values(groups).flat();
const sections=document.querySelector("#sections"),search=document.querySelector("#search"),stats=document.querySelector("#stats");

function render(){
 const q=search.value.trim().toLowerCase(); sections.innerHTML="";
 let visible=0;
 for(const [cat,towers] of Object.entries(groups)){
  const filtered=towers.filter(t=>!q||t.toLowerCase().includes(q));
  if(!filtered.length)continue; visible+=filtered.length;
  const sec=document.createElement("section");
  sec.innerHTML=`<div class="section-head"><h2>${cat}</h2><span class="muted">${filtered.length} tower${filtered.length===1?"":"s"}</span></div><div class="grid"></div>`;
  const grid=sec.querySelector(".grid");
  filtered.forEach(name=>{
   const b=document.createElement("button"),isBanned=banned.has(name);
   b.className="tower"+(isBanned?" banned":"");
   b.setAttribute("aria-pressed",isBanned);
   b.innerHTML=`<strong>${name}</strong><small>${isBanned?"BANNED • tap to unban":"Available • tap to ban"}</small>`;
   b.addEventListener("click",()=>{banned.has(name)?banned.delete(name):banned.add(name);render()});
   grid.appendChild(b);
  });
  sections.appendChild(sec);
 }
 if(!visible)sections.innerHTML='<div class="empty">No tower matches your search.</div>';
 stats.textContent=`${banned.size} banned • ${all.length} total`;
}
search.addEventListener("input",render);
document.querySelector("#reset").addEventListener("click",()=>{banned.clear();render()});
document.querySelector("#banAll").addEventListener("click",()=>{all.forEach(x=>banned.add(x));render()});
render();
</script>
</body>
</html>
