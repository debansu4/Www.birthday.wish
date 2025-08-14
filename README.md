
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Happy 18th Birthday — Candle Blow Experience</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;800&display=swap" rel="stylesheet">

<style>
  :root{
    --bg:#0b1020;
    --card:#ffd3e6;              /* soft pink card */
    --ink:#503646;               /* text */
    --cake:#ffffff;              /* cake body */
    --frost:#ffe1ef;             /* top frosting */
    --plate:#ead4d4;
    --flame:#ffb400;
    --flame2:#ff6a00;
    --accent:#7b5cff;            /* 18 badge */
    --shadow:0 20px 60px rgba(0,0,0,.35);
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0; background:radial-gradient(1200px 800px at 50% 10%, #15204a 0%, var(--bg) 60%);
    color:var(--ink); font-family:Poppins,system-ui,-apple-system,Segoe UI,Roboto,Arial,Helvetica,sans-serif;
    overflow:hidden; /* cinematic */
  }

  /* Floating hearts (background) */
  .hearts{position:fixed; inset:0; overflow:hidden; z-index:0; pointer-events:none}
  .heart{
    position:absolute; width:16px; height:16px;
    transform: rotate(45deg); opacity:.12;
    animation: floatUp linear infinite;
  }
  .heart:before,.heart:after{
    content:""; position:absolute; width:16px; height:16px; background:#fff; border-radius:50%;
  }
  .heart:before{left:-8px}
  .heart:after{top:-8px}
  .heart{background:#fff}
  @keyframes floatUp{
    from{transform:translateY(120vh) rotate(45deg) scale(.7)}
    to{transform:translateY(-20vh) rotate(45deg) scale(1)}
  }

  /* Canvas confetti on top of everything except modal */
  canvas#fx{position:fixed; inset:0; z-index:3; pointer-events:none}

  /* Card */
  .wrap{position:relative; z-index:1; height:100%; display:grid; place-items:center; padding:20px}
  .card{
    width:min(92vw,560px); background:var(--card);
    border-radius:20px; padding:28px 22px 34px; box-shadow:var(--shadow);
    text-align:center; position:relative; overflow:hidden;
    outline:6px solid #fff;
    animation:pop .6s ease-out both;
  }
  @keyframes pop{from{transform:translateY(10px);opacity:.0}to{transform:none;opacity:1}}

  .header{
    display:flex; align-items:center; justify-content:center; gap:10px; margin-bottom:8px; flex-wrap:wrap;
  }
  .title{margin:0; font-weight:800; letter-spacing:.4px; font-size:clamp(20px,4.6vw,28px); color:#1b3a33}
  .badge18{
    background:linear-gradient(135deg,var(--accent),#c977ff);
    color:#fff; font-weight:700; padding:6px 12px; border-radius:999px; font-size:14px; box-shadow:0 6px 14px rgba(123,92,255,.35)
  }
  .subtitle{margin:0 0 14px; opacity:.8; font-size:14px}

  /* Plate & cake */
  .plate{width:300px;height:16px;margin:22px auto 0;border-radius:999px;background:var(--plate);box-shadow:0 10px 16px rgba(0,0,0,.18)}
  .cake{
    position:relative; width:260px; height:102px; margin:-70px auto 0;
    background:var(--cake);
    border-radius:14px 14px 18px 18px;
    box-shadow:0 12px 26px rgba(0,0,0,.18), inset 0 -10px 0 #f6e5e5;
  }
  /* frosting top */
  .cake:before{
    content:""; position:absolute; inset:-26px -10px auto -10px; height:38px;
    background:linear-gradient(#fff1f7,var(--frost));
    border-radius:18px 18px 0 0; box-shadow:inset 0 -10px 0 #ffc7e3;
  }
  .drip{position:absolute; bottom:72px; width:24px; height:34px; background:var(--frost); border-radius:12px}
  .d1{left:26px}
  .d2{left:82px; height:42px}
  .d3{left:144px; width:18px}
  .d4{left:198px; height:38px}

  /* Candle */
  .candle{
    --w:18px; --h:68px;
    width:var(--w); height:var(--h);
    background:repeating-linear-gradient(90deg,#e6ccff 0 9px,#c59aff 9px 18px);
    border-radius:9px; position:absolute; left:50%; top:-56px; transform:translateX(-50%);
    box-shadow:0 4px 10px rgba(0,0,0,.2); cursor:pointer;
  }
  .wick{position:absolute; left:50%; top:-10px; transform:translateX(-50%); width:3px; height:16px; background:#372525; border-radius:2px}
  .flame{
    position:absolute; left:50%; top:-34px; transform:translateX(-50%);
    width:18px;height:28px;border-radius:12px 12px 10px 10px/20px 20px 10px 10px;
    background:radial-gradient(circle at 50% 60%, #fff 0 28%, var(--flame) 28% 58%, var(--flame2) 58% 100%);
    filter:drop-shadow(0 0 8px #ffd37a) drop-shadow(0 0 16px #ff7a00);
    animation:flick .9s ease-in-out infinite;
    transform-origin:50% 90%;
  }
  @keyframes flick{0%,100%{transform:translateX(-50%) rotate(-4deg) scale(1)}50%{transform:translateX(-50%) rotate(5deg) scale(1.1)}}

  /* Blow affordance */
  .holdRing{
    --size:110px;
    position:absolute; left:50%; top:-86px; transform:translateX(-50%);
    width:var(--size); height:var(--size); border-radius:50%;
    display:grid; place-items:center; pointer-events:none;
  }
  .ring{position:absolute; inset:0; border-radius:50%; border:3px dashed rgba(27,58,51,.25); opacity:.7}
  .progress{
    position:absolute; inset:0; border-radius:50%; border:3px solid #1b3a33;
    clip-path: polygon(50% 50%, 50% 0, 100% 0, 100% 100%, 0 100%,0 0,50% 0); /* fallback visual */
    transform:rotate(-90deg) scale(0); transition:transform .15s linear;
    opacity:.35;
  }
  .hint{font-size:13px;margin:14px 0 0;opacity:.85}
  .relight{
    margin-top:10px; display:none;
  }
  .btn{
    display:inline-block; padding:10px 16px; border-radius:12px; border:none; cursor:pointer; font-weight:600;
    background:#1b3a33; color:#fff; box-shadow:0 6px 12px rgba(27,58,51,.28)
  }
  .btn.secondary{background:#7b5cff}

  /* After blown */
  .blown .flame{display:none}
  .blown .wick{height:7px; top:-3px; background:#6b6b6b}
  .blown .hint{opacity:0}
  .blown .relight{display:block}

  /* Smoke */
  .smoke{position:absolute; left:50%; top:-30px; transform:translateX(-50%); pointer-events:none}
  .puff{
    position:absolute; left:-6px; top:0; width:14px; height:14px; border-radius:50%;
    background:rgba(185,185,185,.9); filter:blur(.3px); opacity:0;
  }
  @keyframes rise{
    0%{transform:translate(0,0) scale(.6); opacity:.95}
    100%{transform:translate(-12px,-70px) scale(1.3); opacity:0}
  }

  /* Footer wish */
  .wish{margin-top:18px; font-size:14px; opacity:.9}
  .wish b{font-weight:800}

  /* Modal (wish input) */
  .modalWrap{position:fixed; inset:0; display:none; place-items:center; z-index:4}
  .modalWrap.show{display:grid}
  .backdrop{position:absolute; inset:0; background:rgba(0,0,0,.5); backdrop-filter:blur(2px)}
  .modal{
    position:relative; z-index:1; width:min(92vw,520px); background:#fff; border-radius:16px; padding:18px 16px 16px;
    box-shadow:var(--shadow); text-align:center;
  }
  .modal h3{margin:0 0 6px}
  .field{display:flex; gap:8px; margin:10px 0 6px}
  .field input{flex:1; padding:10px 12px; border-radius:10px; border:1px solid #ddd; font-family:inherit}
  .modal .btn{margin-top:8px}

  /* Accessibility focus */
  .btn:focus-visible, .candle:focus-visible {outline:3px solid #000; outline-offset:2px; border-radius:12px}
</style>
</head>
<body>

<!-- Floating hearts background -->
<div class="hearts" id="hearts"></div>

<!-- Confetti canvas -->
<canvas id="fx"></canvas>

<div class="wrap">
  <article class="card" id="card" aria-live="polite">
    <div class="header">
      <h1 class="title">happy 18th birthday!</h1>
      <span class="badge18">18 ✨</span>
    </div>
    <p class="subtitle">For my favorite person</p>

    <div class="plate"></div>

    <div class="cake" aria-hidden="true">
      <span class="drip d1"></span><span class="drip d2"></span><span class="drip d3"></span><span class="drip d4"></span>

      <button class="candle" id="candle" aria-label="Hold to blow the candle">
        <div class="wick"></div>
        <div class="flame" id="flame"></div>
        <div class="smoke" id="smoke"></div>

        <!-- Hold ring -->
        <div class="holdRing" aria-hidden="true">
          <div class="ring"></div>
          <div class="progress" id="progress"></div>
        </div>
      </button>
    </div>

    <p class="hint" id="hint">Press & hold the candle to blow it out 🎂</p>
    <div class="relight">
      <button class="btn secondary" id="relightBtn">Relight candle</button>
      <button class="btn" id="wishBtn">Make a wish</button>
    </div>

    <p class="wish" id="wishText">May all your dreams glow brighter than this candle, <b id="who">Lovie</b> 💖</p>
  </article>
</div>

<!-- Wish modal -->
<div class="modalWrap" id="modal">
  <div class="backdrop"></div>
  <div class="modal">
    <h3>Add name & a tiny wish ✍️</h3>
    <div class="field">
      <input id="nameInput" placeholder="Her name" />
    </div>
    <div class="field">
      <input id="wishInput" placeholder="Your short wish (e.g., Always be happy!)" />
    </div>
    <button class="btn" id="saveWish">Save</button>
  </div>
</div>

<script>
  /* ---------- Floating hearts ---------- */
  (function makeHearts(){
    const box = document.getElementById('hearts');
    const n = 26;
    for(let i=0;i<n;i++){
      const h = document.createElement('div');
      h.className = 'heart';
      const left = Math.random()*100;
      const delay = Math.random()*6;
      const dur = 10 + Math.random()*14;
      const s = .6 + Math.random()*1.1;
      h.style.left = left+'%';
      h.style.animationDuration = dur+'s';
      h.style.animationDelay = (-delay)+'s';
      h.style.transform = `rotate(45deg) scale(${s})`;
      h.style.opacity = 0.08 + Math.random()*.12;
      box.appendChild(h);
    }
  })();

  /* ---------- Confetti (tiny, light) ---------- */
  const canvas = document.getElementById('fx');
  const ctx = canvas.getContext('2d');
  let W,H; const confetti=[];
  function resize(){ W = canvas.width = innerWidth; H = canvas.height = innerHeight; }
  addEventListener('resize', resize); resize();
  function burst(x,y){
    for(let i=0;i<90;i++){
      confetti.push({
        x, y,
        vx:(Math.random()*2-1)*5,
        vy:(Math.random()*-1)*5 - Math.random()*6,
        r:2+Math.random()*2.8,
        life:60+Math.random()*40
      });
    }
  }
  function loop(){
    ctx.clearRect(0,0,W,H);
    for(let i=confetti.length-1;i>=0;i--){
      const p = confetti[i];
      p.life--; if(p.life<0){confetti.splice(i,1); continue;}
      p.vy += 0.12;
      p.x += p.vx; p.y += p.vy;
      ctx.globalAlpha = Math.max(0,p.life/100);
      ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,Math.PI*2); ctx.fillStyle = '#fff'; ctx.fill();
    }
    requestAnimationFrame(loop);
  } loop();

  /* ---------- Candle logic (press & hold) ---------- */
  const card = document.getElementById('card');
  const candle = document.getElementById('candle');
  const progress = document.getElementById('progress');
  const smokeBox = document.getElementById('smoke');
  const hint = document.getElementById('hint');
  const relightBtn = document.getElementById('relightBtn');
  const wishBtn = document.getElementById('wishBtn');
  const whoEl = document.getElementById('who');

  let holdTimer=null, holdStart=0, blown=false;
  const HOLD_MS = 1400;

  function startHold(){
    if(blown) return;
    holdStart = performance.now();
    progress.style.transform = 'rotate(-90deg) scale(1)';
    holdTimer = requestAnimationFrame(updateHold);
  }
  function endHold(cancelled=false){
    cancelAnimationFrame(holdTimer);
    progress.style.transform = 'rotate(-90deg) scale(0)';
    if(cancelled || blown) return;
    const ms = performance.now() - holdStart;
    if(ms >= HOLD_MS) blow();
  }
  function updateHold(ts){
    const pct = Math.min(1,(ts-holdStart)/HOLD_MS);
    progress.style.clipPath = `polygon(50% 50%, 50% 0, ${50+50*Math.cos(pct*2*Math.PI)}% ${50-50*Math.sin(pct*2*Math.PI)}%, 50% 50%)`;
    if(pct>=1){ endHold(); return; }
    holdTimer = requestAnimationFrame(updateHold);
  }

  function makePuff(delay){
    const p=document.createElement('span');
    p.className='puff';
    p.style.animation=`rise 900ms ${delay}ms ease-out forwards`;
    smokeBox.appendChild(p);
    setTimeout(()=> p.remove(), 1300);
  }

  function blow(){
    blown = true;
    card.classList.add('blown');
    hint.textContent = 'Make a wish… ✨';
    [0,120,220,320,420].forEach(t=>makePuff(t));
    // tiny confetti burst from candle position
    const rect = candle.getBoundingClientRect();
    burst(rect.left + rect.width/2, rect.top + rect.height/2);
    // Optional: sound
    // new Audio('blow.mp3').play().catch(()=>{});
  }

  function relight(){
    blown = false;
    card.classList.remove('blown');
    hint.textContent = 'Press & hold the candle to blow it out 🎂';
  }

  /* pointer events */
  candle.addEventListener('pointerdown', e => { candle.setPointerCapture(e.pointerId); startHold(); });
  candle.addEventListener('pointerup', ()=> endHold(false));
  candle.addEventListener('pointercancel', ()=> endHold(true));
  candle.addEventListener('keydown', e => { if(e.key==='Enter' || e.key===' ') { e.preventDefault(); startHold(); setTimeout(()=>endHold(false),HOLD_MS+10);} });

  relightBtn.addEventListener('click', relight);

  /* ---------- Wish Modal ---------- */
  const modal = document.getElementById('modal');
  const saveWish = document.getElementById('saveWish');
  const nameInput = document.getElementById('nameInput');
  const wishInput = document.getElementById('wishInput');
  const wishText = document.getElementById('wishText');

  wishBtn.addEventListener('click', ()=>{
    modal.classList.add('show');
    nameInput.focus();
  });
  modal.querySelector('.backdrop').addEventListener('click', ()=> modal.classList.remove('show'));
  saveWish.addEventListener('click', ()=>{
    const name = nameInput.value.trim() || 'Lovie';
    const wish = wishInput.value.trim() || 'Always be happy!';
    whoEl.textContent = name;
    wishText.innerHTML = `May all your dreams glow brighter than this candle, <b>${name}</b> — ${wish} 💖`;
    modal.classList.remove('show');
    // celebration
    burst(innerWidth/2, innerHeight/3);
  });

  /* ---------- Nice default values ---------- */
  // Auto open wish modal once after blow (optional):
  // setTimeout(()=> { if(blown) modal.classList.add('show'); }, 1200);
</script>
</body>
</html>
