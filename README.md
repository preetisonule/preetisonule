<div align="center">

<h1>Hi, I'm Preeti 👋</h1>

<h3>Computer Science Engineer • Full-Stack Developer</h3>

<br>

<img
  src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=500&size=18&duration=3000&pause=1000&color=7C3AED&center=true&vCenter=true&width=720&lines=I+build+software+from+idea+to+implementation.;Exploring+backend+systems%2C+cloud%2C+and+infrastructure.;Turning+curiosity+into+things+that+actually+work."
  alt="Typing animation"
/>

<br><br>

</div>

---





<style>
  :root{ --bg:#0a1620; }
  *{box-sizing:border-box;}
  body{
    margin:0;
    min-height:100vh;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    gap:18px;
    background:radial-gradient(ellipse 900px 600px at 50% 20%, #123042, var(--bg));
    font-family:'Segoe UI', system-ui, sans-serif;
    padding:40px 20px;
  }
  .frame{
    position:relative;
    border-radius:10px;
    padding:14px;
    background:linear-gradient(160deg, #123244, #081824);
    box-shadow:
      0 0 0 1px rgba(255,255,255,0.06),
      0 30px 80px -20px rgba(0,0,0,0.75),
      0 0 60px -10px rgba(111,232,255,0.18);
  }
  canvas{
    display:block;
    image-rendering:pixelated;
    image-rendering:-moz-crisp-edges;
    image-rendering:crisp-edges;
    border-radius:4px;
    width:560px;
    height:420px;
    max-width:88vw;
    height:auto;
    background:#0a1620;
  }
  .caption{
    display:flex;
    align-items:center;
    gap:10px;
    font-size:12px;
    letter-spacing:.08em;
    text-transform:uppercase;
    color:#8fc7d6;
    font-family:'Courier New', monospace;
  }
  .dot{
    width:7px;height:7px;border-radius:50%;
    background:#6fe8ff;
    box-shadow:0 0 8px 2px rgba(111,232,255,0.8);
    animation:blink 2.6s ease-in-out infinite;
  }
  @keyframes blink{0%,100%{opacity:.4;}50%{opacity:1;}}
  @media (prefers-reduced-motion: reduce){ .dot{animation:none; opacity:.8;} }
</style>
</head>
<body>

<div class="frame">
  <canvas id="scene" width="104" height="78"></canvas>
</div>
<div class="caption"><span class="dot"></span>rainy evening · loop ~2.6s</div>

<script>
(function(){
  "use strict";
  var canvas = document.getElementById('scene');
  var ctx = canvas.getContext('2d');
  ctx.imageSmoothingEnabled = false;
  var reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  // Lower virtual resolution than v1 on purpose -> chunkier, more visibly
  // "blocky" pixels once scaled up, closer to a texture-pack look.
  var W = 104, H = 78;

  var COL = {
    bgTop:"#123042", bgBot:"#081824",
    wall:"#153847", wallShade:"#0f2c39",
    floor:"#0a1e28", floorLine:"#123340",

    outline:"#04141c",

    windowFrame:"#1f4a55", windowGlass:"#08202c",
    rain:"#a9f2ff", 
    neonSignA:"#6fe8ff", neonSignB:"#ff7fc0",

    deskTop:"#3a6b5f", deskTopHi:"#4f8574", deskLeg:"#22403a",

    tankGlass:"#0c3a44", tankWater:"#155a68", tankGlow:"#4fe0e8",
    fishA:"#ff9f6b", fishB:"#ff7fc0",

    mugBody:"#f3e6da", mugShade:"#dcc7b6", coffee:"#4a2c1a", steam:"#eaf6ff",

    laptopBody:"#25384a", laptopBase:"#1c2c3a",
    screenGlowA:"#6fe8ff", screenGlowB:"#a78bfa", screenLine:"#d8f6ff",

    lampWarm:"#ffd28a", lampShade:"#3a2e20",

    plantPot:"#2f5248", plantLeaf:"#6fd6a8", plantLeaf2:"#4fb488",
    flowerPink:"#ff8fc0", flowerYellow:"#ffe08a",

    hair:"#1c1418", hairHi:"#2c2024",
    skin:"#e9bd93", skinShade:"#d3a072",
    glasses:"#ffc2e0", glassesLine:"#ff9fc9",
    hoodie:"#9fd8cf", hoodieShade:"#7cb8ae", hoodieTrim:"#6fe8ff",
    pants:"#3a4a5c", pantsShade:"#2c3a4a",
    chair:"#1f2e38",

    shelf:"#1f4a55", book1:"#ff8fc0", book2:"#6fe8ff", book3:"#ffe08a",

    sign:"#0c2530", signGlowA:"#6fe8ff", signGlowB:"#ff7fc0"
  };

  function px(x,y,w,h,color,alpha){
    ctx.globalAlpha = alpha===undefined?1:alpha;
    ctx.fillStyle = color;
    ctx.fillRect(Math.round(x),Math.round(y),Math.round(w),Math.round(h));
    ctx.globalAlpha = 1;
  }
  function outline(x,y,w,h,alpha){
    ctx.save();
    ctx.globalAlpha = alpha===undefined?0.9:alpha;
    ctx.strokeStyle = COL.outline;
    ctx.lineWidth = 1;
    ctx.strokeRect(Math.round(x)+0.5, Math.round(y)+0.5, Math.round(w)-1, Math.round(h)-1);
    ctx.restore();
  }
  function glow(x,y,w,h,color,blur,alpha){
    ctx.save();
    ctx.shadowColor = color;
    ctx.shadowBlur = blur;
    px(x,y,w,h,color,alpha);
    ctx.restore();
  }

  function drawBackground(){
    var g = ctx.createLinearGradient(0,0,0,H);
    g.addColorStop(0, COL.bgTop);
    g.addColorStop(1, COL.bgBot);
    ctx.fillStyle = g;
    ctx.fillRect(0,0,W,H);
    px(0,0,W,54,COL.wall);
    px(0,44,W,3,COL.wallShade,0.6);
    px(0,54,W,H-54,COL.floor);
    for(var i=0;i<7;i++){ px(i*16-3,58,1,H-62,COL.floorLine,0.5); }
  }

  // small wall sign with abstract glowing pixel "lettering" (not literal
  // text, avoids reproducing any real signage) — nods to the reference's
  // illuminated shop sign without copying it.
  function drawSign(t){
    px(66,10,26,10,COL.sign);
    outline(66,10,26,10,0.8);
    var pulse = 0.6 + 0.3*Math.sin(t*2.2);
    var bars = [[69,13,4,1.6],[75,13,8,1.6],[85,13,4,1.6],[69,16,14,1.6]];
    bars.forEach(function(b,i){
      glow(b[0],b[1],b[2],b[3], i%2===0?COL.signGlowA:COL.signGlowB, 4, pulse);
    });
  }

  function drawShelf(){
    px(66,24,26,20,COL.shelf);
    outline(66,24,26,20,0.7);
    px(66,33,26,1,COL.outline,0.5);
    px(69,27,3,6,COL.book1);
    px(73,27,3,6,COL.book2);
    px(77,26,4,7,COL.book3);
    px(82,27,3,6,COL.book1);
    px(70,36,4,5,COL.book2);
    px(75,35,3,6,COL.book3);
    px(85,34,4,7,COL.plantPot);
    px(86,29,3,6,COL.plantLeaf);
  }

  function drawTank(t){
    // small glowing fish tank, echoes the reference scene's aquariums
    px(6,34,26,20,COL.tankGlass);
    outline(6,34,26,20,0.8);
    px(8,36,22,16,COL.tankWater);
    glow(8,36,22,16,COL.tankGlow,6,0.18+0.05*Math.sin(t*1.8));
    // fish, gently drifting
    var f1x = 10 + ((t*10) % 18);
    var f2x = 26 - ((t*7) % 16);
    px(f1x,40,3,2,COL.fishA);
    px(f2x,46,3,2,COL.fishB);
    // bubbles
    for(var i=0;i<3;i++){
      var cyc = ((t*0.6 + i*0.3) % 1);
      var by = 50 - cyc*14;
      glow(12+i*6, by, 1,1, COL.tankGlow, 3, (1-cyc)*0.6);
    }
    px(4,52,30,3,COL.deskLeg);
  }

  function drawWindow(t){
    px(66,44,0,0,"",0); // no-op spacer (kept for readability)
  }

  function drawWindowUpper(t){
    px(38,8,26,26,COL.windowFrame);
    outline(38,8,26,26,0.8);
    px(40,10,22,22,COL.windowGlass);
    px(50,10,1.6,22,COL.windowFrame);
    px(40,20,22,1.6,COL.windowFrame);
    // cool exterior light blobs
    var lights=[[43,13],[48,16],[56,12],[58,24]];
    lights.forEach(function(p,i){
      var flick = 0.3+0.3*Math.abs(Math.sin(t*1.3+i*1.6));
      glow(p[0],p[1],1.6,1.6,COL.neonSignA,3,flick);
    });
    ctx.save();
    ctx.beginPath(); ctx.rect(40,10,22,22); ctx.clip();
    for(var i=0;i<10;i++){
      var seedX = 40 + (i*7)%22;
      var speed = 30+(i%4)*6;
      var offset = (i*11)%30;
      var y = ((t*speed+offset)%34)-6;
      glow(seedX,y,1,4,COL.rain,3,0.5);
    }
    ctx.restore();
  }

  function drawDesk(){
    px(30,50,32,4,COL.deskTop);
    outline(30,50,32,4,0.6);
    px(30,50,32,1.2,COL.deskTopHi);
    px(33,54,2.4,16,COL.deskLeg);
    px(57,54,2.4,16,COL.deskLeg);
  }

  function drawPlantOnDesk(){
    px(58,42,6,8,COL.plantPot);
    outline(58,42,6,8,0.5);
    px(59,37,1.6,6,COL.plantLeaf2);
    px(61,36,1.6,6.5,COL.plantLeaf);
    px(63,38,1.6,5,COL.plantLeaf2);
    px(56,44,3,3,COL.flowerPink);
    px(64,45,2.4,2.4,COL.flowerYellow);
  }

  function drawLamp(t){
    px(30,32,1.6,15,"#1c2c34");
    px(25,28,10,4.4,COL.lampWarm);
    outline(25,28,10,4.4,0.6);
    var flick = 0.22+0.06*Math.sin(t*3.1);
    glow(26,32,30,20,COL.lampWarm,14,flick);
  }

  function drawMug(t){
    px(48,44,7,6.4,COL.mugBody);
    outline(48,44,7,6.4,0.5);
    px(48,48.5,7,2,COL.mugShade);
    px(49.4,45.4,4.2,2,COL.coffee);
    ctx.strokeStyle = COL.mugBody; ctx.lineWidth=1.2;
    ctx.beginPath(); ctx.arc(56,46.5,2.2,-Math.PI*0.5,Math.PI*0.5); ctx.stroke();
    for(var i=0;i<3;i++){
      var cycle=((t*0.55+i*0.33)%1);
      var yy=43-cycle*12; var xx=50+i*1.5+Math.sin(t*2+i*2)*1;
      glow(xx,yy,1,2.2,COL.steam,3,(1-cycle)*0.5);
    }
  }

  function drawLaptop(t){
    px(30,50,18,2.2,COL.laptopBase);
    px(31,38,16,12.5,COL.laptopBody);
    outline(31,38,16,12.5,0.7);
    var pulse = 0.55+0.25*Math.sin(t*2.6);
    px(32.4,39.4,13.2,9.6,"#0e1522");
    glow(32.4,39.4,13.2,9.6,COL.screenGlowA,6,pulse*0.5);
    px(33.4,41,7,1,COL.screenLine,0.8);
    px(33.4,43.2,10,1,COL.screenLine,0.5);
    px(33.4,45.4,5,1,COL.screenGlowB,0.8);
    px(33.4,47.6,8.5,1,COL.screenLine,0.35);
  }

  function drawCharacter(t){
    var bob = Math.sin(t*1.6)*0.5;
    var typeL = Math.sin(t*7)*0.9;
    var typeR = Math.sin(t*7+Math.PI)*0.9;

    px(41,30+bob,1.6,22,COL.chair,0.9);
    px(42.4,26+bob,1.6,10,COL.chair,0.9);

    // torso
    px(37,36+bob,16,15,COL.hoodie);
    outline(37,36+bob,16,15,0.55);
    px(37,47+bob,16,4.5,COL.hoodieShade);
    px(43,33+bob,4.4,1.6,COL.hoodieTrim);
    px(43.6,33+bob,1,4.4,COL.hoodieTrim,0.9);
    px(46.8,33+bob,1,4.4,COL.hoodieTrim,0.9);

    // arms
    px(35.6,39+bob,3.6,7.4,COL.hoodie);
    px(50,39+bob,3.6,7.4,COL.hoodie);

    // hands typing
    px(35.6,45.4+bob+typeL,3.6,2.2,COL.skin);
    px(50,45.4+bob+typeR,3.6,2.2,COL.skin);

    // sweatpants sliver
    px(38.6,58+bob,11,4.4,COL.pants,0.9);
    px(38.6,60.8+bob,11,2.2,COL.pantsShade,0.9);

    // neck
    px(42.4,28.4+bob,4.4,3.6,COL.skin);

    // head
    px(38.6,17.6+bob,13.2,11.8,COL.skin);
    outline(38.6,17.6+bob,13.2,11.8,0.35);
    px(38.6,26.4+bob,13.2,2.2,COL.skinShade,0.6);

    // hair — side part, medium length swept to viewer-left
    px(37,14.6+bob,16,6,COL.hair);
    px(37,19+bob,3.6,10.2,COL.hair);
    px(49,19+bob,3.6,8.8,COL.hair);
    px(41.4,16+bob,3,2.2,COL.hairHi,0.5);
    px(35.6,22+bob,2.2,7.4,COL.hair,0.95);

    // glasses — baby pink w/ neon glint
    px(40.6,22+bob,5,3.6,COL.glasses,0.85);
    px(46.4,22+bob,5,3.6,COL.glasses,0.85);
    px(45,23.4+bob,1.4,1,COL.glassesLine);
    ctx.strokeStyle = COL.glassesLine; ctx.lineWidth=0.6; ctx.globalAlpha=0.9;
    ctx.strokeRect(40.6,22+bob,5,3.6);
    ctx.strokeRect(46.4,22+bob,5,3.6);
    ctx.globalAlpha=1;
    glow(40.6,22+bob,11,3.6,COL.glasses,2.4,0.16);

    px(41.6,24.4+bob,7.4,2.2,COL.screenGlowA,0.06);
  }

  function drawVignette(){
    var g = ctx.createRadialGradient(W/2,H/2,H*0.25,W/2,H/2,H*0.9);
    g.addColorStop(0,"rgba(0,0,0,0)");
    g.addColorStop(1,"rgba(4,10,16,0.6)");
    ctx.fillStyle=g;
    ctx.fillRect(0,0,W,H);
  }

  function drawFrame(t){
    ctx.clearRect(0,0,W,H);
    drawBackground();
    drawShelf();
    drawSign(t);
    drawTank(t);
    drawWindowUpper(t);
    drawLamp(t);
    drawDesk();
    drawPlantOnDesk();
    drawCharacter(t);
    drawLaptop(t);
    drawMug(t);
    drawVignette();
  }

  var LOOP = 2.6;
  var start = null;
  function tick(ts){
    if(start===null) start = ts;
    var t = ((ts-start)/1000) % LOOP;
    drawFrame(t);
    if(!reducedMotion) requestAnimationFrame(tick);
  }
  if(reducedMotion){ drawFrame(0.6); } else { requestAnimationFrame(tick); }
})();
</script>
</body>
</html>




## 👋 About Me

I'm **Preeti Sonule**, a Computer Science undergraduate who enjoys turning ideas into things that actually work.

I build across the stack — from **interactive frontends and backend APIs to databases, containers, and cloud infrastructure**. I like understanding not just *how* to build something, but also *why* it works the way it does.

Currently, I'm going deeper into **software engineering, system design, distributed systems, and cloud technologies** while continuing to build projects that solve practical problems.

> **Build → Break → Understand → Improve.**

## 🚀 Currently Building & Learning

<table>
<tr>
<td width="50%" valign="top">

---

## 🚀 Currently

<table>
<tr>
<td width="50%" valign="top">

### 🔨 Building

- Full-stack applications with **React + TypeScript**
- Backend services with **Node.js & Express**
- Containerized applications with **Docker**
- Practical projects around **automation & developer tools**

</td>

<td width="50%" valign="top">

### 📚 Exploring

- **Data Structures & Algorithms**
- **System Design**
- **SQL & NoSQL**
- **Kubernetes & AWS**
- **Distributed Systems**

</td>
</tr>
</table>

## 🛠️ Technologies I Work With

### 💻 Languages

<p>
  <img src="https://skillicons.dev/icons?i=cpp,c,java,js,ts,python" />
</p>

### 🎨 Frontend

<p>
  <img src="https://skillicons.dev/icons?i=html,css,react,vite,tailwind,mui" />
</p>

### ⚙️ Backend & Databases

<p>
  <img src="https://skillicons.dev/icons?i=nodejs,express,mongodb,mysql" />
</p>

### ☁️ DevOps & Tools

<p>
  <img src="https://skillicons.dev/icons?i=docker,kubernetes,aws,git,github,linux" />
</p>

---

---

## ⭐ Featured Projects

<table>
<tr>

<td width="50%" valign="top">

### 🗣️ SarthiAI

**Voice-First Grocery Ordering Assistant**

A voice-driven grocery platform that lets users discover and order products through spoken interaction.

**Built with**

`React` `Node.js` `Express` `MongoDB`

**What I built**

- 🎙️ Speech-to-Text interaction
- 🧠 Intent & entity extraction
- 🛒 Product discovery and ordering flow
- 🔐 Backend APIs & authentication

<br>

<a href="https://github.com/preetisonule/YOUR_REPO">
<img src="https://img.shields.io/badge/View%20Project-171717?style=flat-square&logo=github&logoColor=white">
</a>

</td>

<td width="50%" valign="top">

### ☁️ CloudPulse

**Containerized Application & Kubernetes**

A hands-on infrastructure project focused on containerizing an application and deploying it to a local Kubernetes cluster.

**Built with**

`Docker` `Kubernetes` `kind` `kubectl`

**What I built**

- 🐳 Docker image workflow
- ☸️ Kubernetes deployment
- 📦 Container image management
- ⚙️ Local cluster experimentation

<br>

<a href="https://github.com/preetisonule/YOUR_REPO">
<img src="https://img.shields.io/badge/View%20Project-171717?style=flat-square&logo=github&logoColor=white">
</a>

</td>

</tr>

<tr>

<td width="50%" valign="top">

### 🔍 GitHub Portfolio Analyzer

**Developer Profile Analysis Platform**

A TypeScript-based full-stack application that analyzes GitHub profiles and repositories to generate developer insights.

**Built with**

`React` `TypeScript` `Node.js` `Express` `MongoDB`

**What I built**

- 🔎 GitHub API integration
- 📊 Repository & language analysis
- ⭐ Stars and forks analysis
- 🧮 Developer scoring system

<br>

<a href="https://github.com/preetisonule/YOUR_REPO">
<img src="https://img.shields.io/badge/View%20Project-171717?style=flat-square&logo=github&logoColor=white">
</a>

</td>

<td width="50%" valign="top">

### 🧠 Orchestrix

**Developer Automation Platform**

A backend-focused project exploring automated development workflows and service orchestration.

**Built with**

`Python` `FastAPI` `Docker`

**What I built**

- ⚙️ API-driven backend
- 🔄 Automated workflows
- 🧩 Service-oriented architecture
- 🐳 Containerized services

<br>

<a href="https://github.com/preetisonule/YOUR_REPO">
<img src="https://img.shields.io/badge/View%20Project-171717?style=flat-square&logo=github&logoColor=white">
</a>

</td>

</tr>
</table>

### 🔍 CodeDNA

**Developer Profile Analysis Platform**

A full-stack application that analyzes GitHub profiles and repositories to generate developer insights.

**Built with**

`React` `TypeScript` `Node.js` `Express` `MongoDB`

**Highlights**

- 🔎 GitHub API integration
- 📊 Repository & language analysis
- 🧮 Developer scoring system

<br>

<a href="https://github.com/preetisonule/YOUR_REPO">
<img src="https://img.shields.io/badge/GitHub-View%20Project-171717?style=flat-square&logo=github">
</a>

</td>

<td width="50%" valign="top">

### 🧠 Orchestrix

**Developer Automation Platform**

A backend-focused project exploring automated development workflows and service orchestration.

**Built with**

`Python` `FastAPI` `Docker`

**Highlights**

- ⚙️ API-driven architecture
- 🔄 Automated workflows
- 🧩 Service-oriented backend design

<br>

<a href="https://github.com/preetisonule/YOUR_REPO">
<img src="https://img.shields.io/badge/GitHub-View%20Project-171717?style=flat-square&logo=github">
</a>

</td>

</tr>
</table>
---

## 🏆 Achievements

| Achievement | Result |
|:---|:---|
| 🥉 **Hacking Bad Hackathon** | 3rd Runner-Up • Team Mirage |
| 🚀 **GDSC Hackathon** | Top 12 Finalist |
| 👩‍💻 **Official Coding Club** | President • DYPCET |
| 🔥 **LeetCode** | Consistent DSA & problem-solving practice |

---

## 📊 GitHub Activity

<div align="center">

<img height="165" src="https://github-readme-stats.vercel.app/api?username=preetisonule&show_icons=true&hide_border=true&count_private=true&include_all_commits=true&rank_icon=github&theme=transparent" />

<img height="165" src="https://github-readme-stats.vercel.app/api/top-langs/?username=preetisonule&layout=compact&hide_border=true&langs_count=6&theme=transparent" />

</div>

<br>

<div align="center">

<img src="https://streak-stats.demolab.com?user=preetisonule&hide_border=true&theme=transparent" />

</div>

---

## 🌐 Find Me Online

<div align="center">

<a href="https://www.linkedin.com/in/preeti-sonule/">
  <img src="https://img.shields.io/badge/LinkedIn-Connect-171717?style=for-the-badge&logo=linkedin&logoColor=white">
</a>

<a href="https://YOUR_PORTFOLIO_URL.com">
  <img src="https://img.shields.io/badge/Portfolio-Visit-171717?style=for-the-badge&logo=vercel&logoColor=white">
</a>

<a href="mailto:preetisonule3186@gmail.com">
  <img src="https://img.shields.io/badge/Email-Contact-171717?style=for-the-badge&logo=gmail&logoColor=white">
</a>

</div>

<br>

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&height=100&section=footer&color=gradient&customColorList=12,14,16,18"/>

</div>
