const parks = {
  "青铜乐园": { icon:"🥉", special:["刀皮","角色","六格","四格"], money:["300万","266万","200万","150万","100万"] },
  "白银乐园": { icon:"🥈", special:["刀皮","角色","晶钻","六格","四格"], money:["400万","350万","300万","266万","200万"] },
  "黄金乐园": { icon:"🥇", special:["刀皮","角色","晶钻","人物皮肤","六格","四格"], money:["600万","550万","500万","450万","400万"] },
  "钻石乐园": { icon:"💎", special:["刀皮","角色","晶钻","人物皮肤","610大富翁","六格","四格"], money:["700万","660万","600万","550万","500万"] },
  "王牌乐园": { icon:"👑", special:["刀皮","角色","晶钻","人物皮肤","610大富翁","1288大富翁","六格","四格"], money:["900万","850万","800万","750万","700万"] }
};

export default {
  async fetch(request, env) {
    const url = new URL(request.url);
    try {
      if (url.pathname === "/api/state") return state(env, url);
      if (url.pathname === "/api/open" && request.method === "POST") return openBox(request, env);
      if (url.pathname === "/api/reset" && request.method === "POST") return resetPark(request, env);

      return new Response(html, {
        headers: { "content-type": "text/html;charset=UTF-8" }
      });
    } catch (err) {
      return json({ ok:false, msg:String(err.message || err) }, 500);
    }
  }
};

function json(data, status = 200) {
  return new Response(JSON.stringify(data), {
    status,
    headers: { "content-type": "application/json;charset=UTF-8" }
  });
}

function shuffle(arr) {
  return arr.sort(() => Math.random() - 0.5);
}

async function ensurePark(env, park) {
  if (!parks[park]) return;

  const count = await env.DB.prepare(
    "SELECT COUNT(*) AS c FROM boxes WHERE park=?"
  ).bind(park).first();

  if (count && count.c >= 1000) return;

  await env.DB.prepare("DELETE FROM boxes WHERE park=?").bind(park).run();

  const cfg = parks[park];
  let rewards = [...cfg.special];

  while (rewards.length < 1000) {
    rewards.push(cfg.money[Math.floor(Math.random() * cfg.money.length)]);
  }

  rewards = shuffle(rewards);

  const batchSize = 100;
  for (let start = 0; start < rewards.length; start += batchSize) {
    const part = rewards.slice(start, start + batchSize);
    await env.DB.batch(
      part.map((reward, j) =>
        env.DB.prepare(
          "INSERT INTO boxes (park, box_index, reward, opened) VALUES (?, ?, ?, 0)"
        ).bind(park, start + j, reward)
      )
    );
  }
}

async function state(env, url) {
  const park = url.searchParams.get("park") || "青铜乐园";

  await ensurePark(env, park);

  const boxes = await env.DB.prepare(
    "SELECT box_index, opened FROM boxes WHERE park=? ORDER BY box_index ASC"
  ).bind(park).all();

  const records = await env.DB.prepare(
    "SELECT player, park, reward, created_at FROM records ORDER BY id DESC LIMIT 30"
  ).all();

  return json({
    ok:true,
    parks,
    boxes: boxes.results || [],
    records: records.results || []
  });
}

async function openBox(request, env) {
  const { park, player, index } = await request.json();

  if (!park || !parks[park]) return json({ ok:false, msg:"乐园不存在" });
  if (!player || !player.trim()) return json({ ok:false, msg:"请先输入老板名称" });

  await ensurePark(env, park);

  let boxIndex = index;

  if (boxIndex === undefined || boxIndex === null) {
    const one = await env.DB.prepare(
      "SELECT box_index FROM boxes WHERE park=? AND opened=0 ORDER BY RANDOM() LIMIT 1"
    ).bind(park).first();

    if (!one) return json({ ok:false, msg:"当前乐园已经开完" });
    boxIndex = one.box_index;
  }

  const box = await env.DB.prepare(
    "SELECT * FROM boxes WHERE park=? AND box_index=?"
  ).bind(park, boxIndex).first();

  if (!box) return json({ ok:false, msg:"宝箱不存在" });
  if (box.opened) return json({ ok:false, msg:"这个宝箱已经被别人开过了" });

  const now = new Date().toISOString();

  const update = await env.DB.prepare(
    "UPDATE boxes SET opened=1, player=?, opened_at=? WHERE park=? AND box_index=? AND opened=0"
  ).bind(player.trim(), now, park, boxIndex).run();

  if (!update.meta || update.meta.changes !== 1) {
    return json({ ok:false, msg:"慢了一步，这个宝箱已被别人开走" });
  }

  await env.DB.prepare(
    "INSERT INTO records (player, park, reward, created_at) VALUES (?, ?, ?, ?)"
  ).bind(player.trim(), park, box.reward, now).run();

  return json({
    ok:true,
    reward:box.reward,
    special:parks[park].special.includes(box.reward)
  });
}

async function resetPark(request, env) {
  const { park, password } = await request.json();

  if (password !== "TY6666") return json({ ok:false, msg:"管理员密码错误" });
  if (!parks[park]) return json({ ok:false, msg:"乐园不存在" });

  await env.DB.prepare("DELETE FROM boxes WHERE park=?").bind(park).run();
  await ensurePark(env, park);

  return json({ ok:true });
}

const html = `<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>儿童节天亿专属活动</title>
<script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"><\/script>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Microsoft YaHei,Arial;color:#fff;background:linear-gradient(rgba(0,0,0,.55),rgba(0,0,0,.86)),radial-gradient(circle at top,#233f34,#060b12 55%,#000);min-height:100vh}
body:before{content:"";position:fixed;inset:0;background:repeating-linear-gradient(90deg,rgba(0,255,255,.06) 0 1px,transparent 1px 80px),repeating-linear-gradient(0deg,rgba(255,255,255,.035) 0 1px,transparent 1px 80px);pointer-events:none}
.wrap{position:relative;z-index:1;width:96%;max-width:1500px;margin:auto}
.header{text-align:center;padding:22px 8px 12px}
.god{width:135px;height:135px;margin:auto;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:78px;background:radial-gradient(circle,#ffd54a,#7c2d12 60%,#020617 72%);border:2px solid #facc15;box-shadow:0 0 35px #facc15,0 0 70px #00eaff;animation:f 2.2s infinite ease-in-out;background-size:cover;background-position:center}
@keyframes f{50%{transform:translateY(-12px)}}
.title{font-size:34px;color:#ffeb3b;font-weight:900;text-shadow:0 0 15px #ff007a}
h1{font-size:56px;margin:8px 0;background:linear-gradient(#fff,#ffd166,#ff2bd6);-webkit-background-clip:text;color:transparent;filter:drop-shadow(0 0 15px #00eaff)}
.sub{color:#ffe58a;font-size:18px}
.tabs{display:grid;grid-template-columns:repeat(5,1fr);gap:14px;margin:18px 0}
.tab{padding:15px 8px;text-align:center;border-radius:16px;background:rgba(5,20,45,.88);border:1px solid #00eaff;cursor:pointer;box-shadow:0 0 18px rgba(0,234,255,.35)}
.tab.active{border-color:#ff2bd6;box-shadow:0 0 28px #ff2bd6}
.tab-icon{font-size:36px}.tab-name{font-size:21px;font-weight:bold}
.main{display:grid;grid-template-columns:1fr 430px 370px;gap:18px}
.panel{background:rgba(3,10,30,.86);border:1px solid #00eaff;border-radius:20px;padding:16px;box-shadow:0 0 25px rgba(0,234,255,.25)}
.line{display:flex;gap:10px;margin-bottom:12px}
input{flex:1;padding:14px;border-radius:10px;border:1px solid #334155;background:#020617;color:#fff;font-size:16px}
button{border:0;border-radius:10px;padding:12px 18px;color:#fff;font-weight:bold;cursor:pointer;background:linear-gradient(135deg,#00eaff,#a855f7)}
.status{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;padding:10px;border:1px solid rgba(255,0,200,.45);border-radius:12px;margin-bottom:12px;text-align:center;font-size:14px}.status b{color:#facc15}
.boxes{display:grid;grid-template-columns:repeat(auto-fill,minmax(76px,1fr));gap:12px;max-height:630px;overflow:auto;padding:4px}
.box{height:76px;position:relative;cursor:pointer;border-radius:12px;background-size:contain!important;background-position:center!important;background-repeat:no-repeat!important;transition:.18s}
.box.default{background:linear-gradient(145deg,#ffd33d 0%,#bd8500 55%,#332000 100%);box-shadow:0 0 14px rgba(255,210,0,.7),inset 0 0 18px rgba(255,255,255,.25);transform:skewX(-5deg)}
.box.default:before{content:"";position:absolute;left:8px;right:8px;top:10px;height:16px;background:#111827;border-radius:4px;box-shadow:inset 0 0 8px #00eaff}
.box.default:after{content:"";position:absolute;left:50%;top:28px;transform:translateX(-50%);width:34px;height:18px;background:#020617;border:3px solid #facc15;border-radius:4px}
.box.custom{background-color:transparent!important;box-shadow:0 0 10px rgba(250,204,21,.55)}
.box:hover{transform:scale(1.12)}
.box.opened{filter:grayscale(1) brightness(.42);opacity:.55;pointer-events:none}
.open-title{text-align:center;font-size:34px;color:#70f7ff;text-shadow:0 0 18px #00eaff}
.big{height:360px;display:flex;align-items:center;justify-content:center}
.case{width:310px;height:190px;border-radius:24px;background-size:contain!important;background-position:center!important;background-repeat:no-repeat!important;animation:f 2s infinite ease-in-out}
.case.default{position:relative;background:linear-gradient(145deg,#ffd633,#b57b00 60%,#1f2937);box-shadow:0 0 55px #00eaff,inset 0 0 25px rgba(255,255,255,.25)}
.case.default:before{content:"";position:absolute;left:25px;right:25px;top:28px;height:42px;background:#111827;border-radius:8px}
.case.default:after{content:"OPEN";position:absolute;left:50%;top:82px;transform:translateX(-50%);width:116px;height:40px;line-height:40px;text-align:center;background:#0f172a;color:#ffd633;border-radius:8px;font-weight:bold;letter-spacing:3px}
table{width:100%;border-collapse:collapse;font-size:14px}th,td{border:1px solid rgba(0,234,255,.3);padding:8px;text-align:center}th{color:#ffeb3b}
.export{width:100%;margin-top:15px;background:linear-gradient(135deg,#16a34a,#22c55e)}
.rules{margin:20px 0;display:grid;grid-template-columns:repeat(5,1fr);gap:14px}.rule{padding:14px;background:rgba(3,10,30,.86);border:1px solid #00eaff;border-radius:16px}.rule h3{text-align:center;color:#ffeb3b}.rule p{font-size:14px;line-height:1.7}
.admin{width:240px;margin:35px auto 20px;text-align:center;opacity:.23;transition:.3s}.admin:hover{opacity:1}.admin label{display:block;cursor:pointer;padding:10px;margin:8px 0;border-radius:8px;background:#0f172a;border:1px solid #334155;color:#94a3b8;font-size:12px}.admin input{display:none}
.popup{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.78);z-index:9}
.card{width:370px;height:480px;border-radius:26px;background:linear-gradient(160deg,#020617,#312e81,#7e22ce);border:2px solid #00eaff;box-shadow:0 0 45px #00eaff,0 0 80px #ff2bd6;text-align:center;padding:28px;animation:d .7s ease}
.card.special{box-shadow:0 0 50px #facc15,0 0 100px #ff2bd6;border-color:#facc15}
@keyframes d{from{transform:scale(.2) rotateY(180deg);opacity:0}to{transform:scale(1) rotateY(0);opacity:1}}
.ri{font-size:92px;margin:35px 0}.rt{font-size:34px;color:#ffeb3b;font-weight:bold}
.broadcast{position:fixed;top:18px;left:50%;transform:translateX(-50%);background:linear-gradient(135deg,#facc15,#ef4444);color:#111827;padding:12px 28px;border-radius:999px;font-weight:bold;display:none;z-index:10}
.loading{text-align:center;color:#facc15;font-size:22px;padding:30px}
@media(max-width:1100px){.main{grid-template-columns:1fr}.tabs,.rules{grid-template-columns:repeat(2,1fr)}h1{font-size:38px}.title{font-size:24px}}
</style>
</head>
<body>
<div class="broadcast" id="broadcast"></div>
<div class="wrap">
  <div class="header">
    <div class="god" id="god">🤖💰</div>
    <div class="title">儿童节天亿专属活动！下单即可参与抽奖！</div>
    <h1>天亿电竞红包乐园</h1>
    <div class="sub">联网同步版｜暗区突围无限主题｜特殊奖励唯一</div>
  </div>

  <div class="tabs" id="tabs"></div>

  <div class="main">
    <div class="panel">
      <div class="line">
        <input id="player" placeholder="请输入老板名称">
        <button onclick="setPlayer()">确认进入</button>
      </div>
      <div class="status">
        <div>当前<br><b id="parkName">青铜乐园</b></div>
        <div>总数<br><b>1000</b></div>
        <div>已开<br><b id="opened">0</b></div>
        <div>剩余<br><b id="left">1000</b></div>
      </div>
      <div class="boxes" id="boxes"><div class="loading">正在加载宝箱...</div></div>
    </div>

    <div class="panel">
      <div class="open-title">开箱时刻</div>
      <div class="big"><div class="case default" id="bigCase"></div></div>
      <button style="display:block;margin:20px auto 0" onclick="randomOpen()">随机开箱</button>
      <button style="display:block;margin:12px auto 0;background:linear-gradient(135deg,#ef4444,#f97316)" onclick="resetPark()">管理员重置当前乐园</button>
    </div>

    <div class="panel">
      <h2 style="text-align:center;color:#70f7ff">历史抽奖记录</h2>
      <table>
        <thead><tr><th>时间</th><th>名称</th><th>乐园</th><th>奖励</th></tr></thead>
        <tbody id="records"></tbody>
      </table>
      <button class="export" onclick="exportExcel()">导出Excel表格</button>
    </div>
  </div>

  <div class="rules" id="rules"></div>

  <div class="admin">
    <div style="font-size:12px;color:#64748b">管理员工具</div>
    <label for="boxImg">更换宝箱图片</label>
    <input id="boxImg" type="file" accept="image/*" onchange="uploadImg(event,'box')">
    <label for="bgImg">更换背景图片</label>
    <input id="bgImg" type="file" accept="image/*" onchange="uploadImg(event,'bg')">
    <label for="godImg">更换上方赛博财神</label>
    <input id="godImg" type="file" accept="image/*" onchange="uploadImg(event,'god')">
    <label onclick="clearImgs()">清除自定义图片</label>
  </div>
</div>

<div class="popup" id="popup">
  <div class="card" id="card">
    <h2>恭喜获得</h2>
    <div class="ri" id="rewardIcon">💰</div>
    <div class="rt" id="rewardText"></div>
    <button style="margin-top:35px" onclick="closePop()">确定</button>
  </div>
</div>

<script>
let parks={};
let currentPark="青铜乐园";
let player=localStorage.getItem("player")||"";
let records=[];
let boxImg=localStorage.getItem("boxImg")||"";

if(player) document.addEventListener("DOMContentLoaded",()=>document.getElementById("player").value=player);

function icon(r){
  if(r.includes("万"))return"💰";
  if(r.includes("晶钻"))return"💎";
  if(r.includes("刀皮"))return"🔪";
  if(r.includes("角色"))return"🧑‍🚀";
  if(r.includes("皮肤"))return"🎭";
  if(r.includes("大富翁"))return"🎲";
  if(r.includes("六格"))return"🎁";
  if(r.includes("四格"))return"📦";
  return"🎁";
}

async function api(path,opt={}){
  const r=await fetch(path,opt);
  const data=await r.json();
  if(!data.ok) throw new Error(data.msg || "请求失败");
  return data;
}

async function boot(){
  try{
    await load();
    setInterval(load,3000);
  }catch(e){
    document.getElementById("boxes").innerHTML='<div class="loading">加载失败：'+e.message+'</div>';
  }
}

async function load(){
  const data=await api("/api/state?park="+encodeURIComponent(currentPark));
  parks=data.parks;
  records=data.records;
  renderTabs();
  renderBoxes(data.boxes || []);
  renderRecords();
  renderRules();
  applyImgs();
}

function setPlayer(){
  const v=document.getElementById("player").value.trim();
  if(!v)return alert("请先输入老板名称");
  player=v;
  localStorage.setItem("player",player);
  alert("欢迎进入："+player);
}

function renderTabs(){
  const el=document.getElementById("tabs");
  el.innerHTML="";
  Object.keys(parks).forEach(name=>{
    const d=document.createElement("div");
    d.className="tab"+(name===currentPark?" active":"");
    d.innerHTML='<div class="tab-icon">'+parks[name].icon+'</div><div class="tab-name">'+name+'</div>';
    d.onclick=()=>{currentPark=name;load()};
    el.appendChild(d);
  });
}

function renderBoxes(list){
  const el=document.getElementById("boxes");
  el.innerHTML="";
  const opened=list.filter(x=>x.opened).length;
  document.getElementById("parkName").innerText=currentPark;
  document.getElementById("opened").innerText=opened;
  document.getElementById("left").innerText=1000-opened;

  list.forEach(b=>{
    const d=document.createElement("div");
    d.className="box "+(boxImg?"custom":"default")+(b.opened?" opened":"");
    if(boxImg)d.style.backgroundImage="url('"+boxImg+"')";
    d.onclick=()=>openBox(b.box_index);
    el.appendChild(d);
  });
}

async function openBox(index){
  if(!player)return alert("请先输入老板名称");
  try{
    const res=await api("/api/open",{
      method:"POST",
      headers:{"content-type":"application/json"},
      body:JSON.stringify({park:currentPark,player,index})
    });

    document.getElementById("rewardIcon").innerText=icon(res.reward);
    document.getElementById("rewardText").innerText=res.reward;
    document.getElementById("card").classList.toggle("special",res.special);
    document.getElementById("popup").style.display="flex";

    if(res.special)showBroadcast("🎉 "+player+" 抽中特殊奖励："+res.reward+"！");
    await load();
  }catch(e){
    alert(e.message);
  }
}

async function randomOpen(){
  if(!player)return alert("请先输入老板名称");
  try{
    const res=await api("/api/open",{
      method:"POST",
      headers:{"content-type":"application/json"},
      body:JSON.stringify({park:currentPark,player})
    });

    document.getElementById("rewardIcon").innerText=icon(res.reward);
    document.getElementById("rewardText").innerText=res.reward;
    document.getElementById("card").classList.toggle("special",res.special);
    document.getElementById("popup").style.display="flex";

    if(res.special)showBroadcast("🎉 "+player+" 抽中特殊奖励："+res.reward+"！");
    await load();
  }catch(e){
    alert(e.message);
  }
}

async function resetPark(){
  const password=prompt("请输入管理员密码");
  if(!password)return;
  try{
    const res=await api("/api/reset",{
      method:"POST",
      headers:{"content-type":"application/json"},
      body:JSON.stringify({park:currentPark,password})
    });
    alert("已重置");
    load();
  }catch(e){
    alert("失败："+e.message);
  }
}

function renderRecords(){
  const el=document.getElementById("records");
  el.innerHTML="";
  records.slice(0,30).forEach(r=>{
    el.innerHTML+="<tr><td>"+new Date(r.created_at).toLocaleString()+"</td><td>"+r.player+"</td><td>"+r.park+"</td><td>"+r.reward+"</td></tr>";
  });
}

function renderRules(){
  const el=document.getElementById("rules");
  el.innerHTML="";
  Object.keys(parks).forEach(name=>{
    const p=parks[name];
    el.innerHTML+='<div class="rule"><h3>'+p.icon+' '+name+'</h3><p><b>特殊奖励：</b><br>'+p.special.join(" / ")+'</p><p><b>金币奖励：</b><br>'+p.money.join(" / ")+'</p></div>';
  });
}

function exportExcel(){
  if(!records.length)return alert("暂无抽奖记录");
  const ws=XLSX.utils.json_to_sheet(records.map(r=>({
    时间:new Date(r.created_at).toLocaleString(),
    名称:r.player,
    乐园:r.park,
    奖励:r.reward
  })));
  const wb=XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb,ws,"抽奖记录");
  XLSX.writeFile(wb,"天亿电竞红包乐园抽奖记录.xlsx");
}

function uploadImg(e,type){
  const file=e.target.files[0];
  if(!file)return;
  const reader=new FileReader();
  reader.onload=function(ev){
    localStorage.setItem(type+"Img",ev.target.result);
    if(type==="box")boxImg=ev.target.result;
    applyImgs();
    load();
  };
  reader.readAsDataURL(file);
}

function applyImgs(){
  const bg=localStorage.getItem("bgImg");
  const god=localStorage.getItem("godImg");
  boxImg=localStorage.getItem("boxImg")||"";

  if(bg){
    document.body.style.background="linear-gradient(rgba(0,0,0,.52),rgba(0,0,0,.84)),url('"+bg+"') center center / cover no-repeat fixed";
  }

  if(god){
    const g=document.getElementById("god");
    g.innerHTML="";
    g.style.background="url('"+god+"') center center / cover no-repeat";
  }

  const c=document.getElementById("bigCase");
  if(boxImg){
    c.style.backgroundImage="url('"+boxImg+"')";
    c.classList.remove("default");
  }else{
    c.style.backgroundImage="";
    c.classList.add("default");
  }
}

function clearImgs(){
  localStorage.removeItem("boxImg");
  localStorage.removeItem("bgImg");
  localStorage.removeItem("godImg");
  alert("已清除，刷新后恢复默认");
}

function showBroadcast(text){
  const b=document.getElementById("broadcast");
  b.innerText=text;
  b.style.display="block";
  setTimeout(()=>b.style.display="none",3500);
}

function closePop(){
  document.getElementById("popup").style.display="none";
}

boot();
<\/script>
</body>
</html>`;