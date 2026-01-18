<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>FF Sensitivity Generator</title>
<style>
body {
  background:#020617;
  color:white;
  font-family:Arial;
  text-align:center;
  padding:15px;
}
input, select, button {
  padding:10px;
  margin:6px;
  font-size:16px;
}
.box {
  background:#0f172a;
  padding:15px;
  border-radius:10px;
  margin-top:12px;
}
h1 { color:#22c55e; }
.small { font-size:12px; color:#94a3b8; }
</style>
</head>

<body>

<h1>🔥 FF All-Red Sensitivity Generator 🔥</h1>

<div class="box">
<h3>📱 Phone Info</h3>

<select id="os">
  <option value="ios">iPhone (iOS)</option>
  <option value="android">Android</option>
</select>

<select id="brand">
  <option>Apple</option>
  <option>Samsung</option>
  <option>Xiaomi / Redmi</option>
  <option>OnePlus</option>
  <option>Realme</option>
  <option>Oppo</option>
  <option>Vivo</option>
  <option>Other</option>
</select>

<input id="model" placeholder="Type your phone model (e.g. iPhone 11, S21 FE)">

<select id="sensType">
  <option value="low">Low Sensitivity</option>
  <option value="medium">Medium Sensitivity</option>
  <option value="high">High Sensitivity</option>
</select>

<select id="style">
  <option value="rusher">Rusher</option>
  <option value="balanced">Balanced</option>
  <option value="onetap">One-Tap / All-Red</option>
</select>

<br>
<button onclick="generate()">Generate</button>
</div>

<div class="box" id="result"></div>

<script>
function detectTier(model) {
  model = model.toLowerCase();
  if (model.match(/6|7|8|old|2016|2017|2018/)) return "old";
  if (model.match(/x|xr|xs|10|11|note 9|note 10|s10|s20/)) return "mid";
  return "new";
}

function generate() {
  let os = document.getElementById("os").value;
  let model = document.getElementById("model").value;
  let sensType = document.getElementById("sensType").value;
  let style = document.getElementById("style").value;

  let tier = detectTier(model);

  let base = sensType==="low"?125:sensType==="medium"?150:175;

  let g=base, r=base-10, x2=base-20, x4=base-40, awm=base-65;
  if (style==="rusher") { g+=10; r+=5; }
  if (style==="onetap") { g-=10; r-=10; }

  g=Math.min(200,Math.max(0,g));
  r=Math.min(200,Math.max(0,r));
  x2=Math.min(200,Math.max(0,x2));
  x4=Math.min(200,Math.max(0,x4));
  awm=Math.min(200,Math.max(0,awm));

  let dpi, fire;

  if (os==="ios") {
    dpi = sensType==="low"?95:sensType==="medium"?105:115;
    if (dpi>120) dpi=120;
    fire = tier==="old"?60:tier==="mid"?55:50;
  } else {
    dpi = tier==="old"?420:tier==="mid"?480:540;
    fire = tier==="old"?58:tier==="mid"?54:50;
  }

  document.getElementById("result").innerHTML = `
  <h3>🎯 Recommended Settings</h3>
  <p>General: <b>${g}</b></p>
  <p>Red Dot: <b>${r}</b></p>
  <p>2× Scope: <b>${x2}</b></p>
  <p>4× Scope: <b>${x4}</b></p>
  <p>AWM Scope: <b>${awm}</b></p>
  <hr>
  <p>🔘 Fire Button: <b>${fire}%</b></p>
  <p>📐 DPI: <b>${dpi}</b> ${os==="ios"?"(iOS max 120)":""}</p>
  <p class="small">Works for ANY phone model</p>
  `;
}
</script>

</body>
</html>
