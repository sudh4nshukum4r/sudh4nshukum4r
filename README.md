<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Sudhanshu — Progress Tracker</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700;800&display=swap');

  :root{
    --bg: #0a0d0c;
    --panel: #0f1412;
    --panel-2: #121815;
    --border: #1d2a24;
    --green: #00ff9d;
    --green-dim: #0a5c3f;
    --cyan: #4fd1ff;
    --pink: #ff5fa2;
    --text: #d3ddd8;
    --muted: #6f8079;
    --danger: #ff5f5f;
  }
  *{box-sizing:border-box;}
  html,body{
    margin:0;
    background:transparent;
    font-family:'JetBrains Mono', monospace;
    color:var(--text);
  }
  .wrap{
    max-width:760px;
    margin:0 auto;
    padding:18px 14px 40px;
  }

  .term{
    background:var(--panel);
    border:1px solid var(--border);
    border-radius:10px;
    overflow:hidden;
    box-shadow:0 20px 60px rgba(0,0,0,.45);
  }
  .term-bar{
    display:flex;
    align-items:center;
    gap:8px;
    padding:10px 14px;
    background:var(--panel-2);
    border-bottom:1px solid var(--border);
  }
  .dot{width:10px;height:10px;border-radius:50%;}
  .dot.r{background:#ff5f56;}
  .dot.y{background:#ffbd2e;}
  .dot.g{background:#27c93f;}
  .term-title{
    margin-left:8px;
    font-size:12px;
    color:var(--muted);
    letter-spacing:.3px;
  }
  .term-body{padding:22px 20px 8px;}

  .prompt{
    color:var(--green);
    font-size:13px;
    margin:0 0 4px;
  }
  .prompt span{color:var(--cyan);}
  h1{
    font-size:22px;
    margin:2px 0 4px;
    color:#fff;
    letter-spacing:.2px;
  }
  .sub{
    color:var(--muted);
    font-size:13px;
    margin:0 0 24px;
  }

  section{margin-bottom:26px;}
  .sec-head{
    display:flex;
    align-items:baseline;
    gap:8px;
    margin-bottom:12px;
    padding-bottom:8px;
    border-bottom:1px dashed var(--border);
  }
  .sec-head .tag{color:var(--green);font-size:13px;}
  .sec-head h2{font-size:14px;margin:0;color:var(--text);font-weight:600;}

  .streak-row{
    display:flex;
    gap:12px;
    flex-wrap:wrap;
  }
  .streak-card{
    flex:1;
    min-width:120px;
    background:var(--panel-2);
    border:1px solid var(--border);
    border-radius:8px;
    padding:14px;
    text-align:center;
  }
  .streak-num{
    font-size:28px;
    font-weight:800;
    color:var(--green);
  }
  .streak-label{
    font-size:11px;
    color:var(--muted);
    margin-top:4px;
  }
  .checkin-btn{
    width:100%;
    margin-top:14px;
    padding:12px;
    background:var(--green-dim);
    border:1px solid var(--green);
    color:var(--green);
    border-radius:8px;
    font-family:inherit;
    font-size:13px;
    font-weight:700;
    cursor:pointer;
    letter-spacing:.3px;
  }
  .checkin-btn:disabled{
    opacity:.45;
    cursor:default;
    background:transparent;
    color:var(--muted);
    border-color:var(--border);
  }
  .checkin-btn:not(:disabled):active{transform:scale(.99);}

  .task{
    display:flex;
    align-items:flex-start;
    gap:10px;
    padding:9px 4px;
    border-bottom:1px solid var(--border);
    font-size:13px;
    cursor:pointer;
  }
  .task:last-child{border-bottom:none;}
  .box{
    flex:none;
    width:16px;height:16px;
    border:1px solid var(--muted);
    border-radius:4px;
    margin-top:1px;
    display:flex;align-items:center;justify-content:center;
  }
  .task.done .box{background:var(--green);border-color:var(--green);}
  .task.done .box:after{content:'✓';font-size:11px;color:#04140c;font-weight:800;}
  .task.done .txt{color:var(--muted);text-decoration:line-through;}

  .skill{margin-bottom:14px;}
  .skill-top{
    display:flex;
    justify-content:space-between;
    font-size:12px;
    margin-bottom:6px;
    color:var(--text);
  }
  .skill-top .pct{color:var(--cyan);}
  .bar-track{
    height:8px;
    background:var(--panel-2);
    border:1px solid var(--border);
    border-radius:5px;
    overflow:hidden;
  }
  .bar-fill{
    height:100%;
    background:linear-gradient(90deg,var(--green),var(--cyan));
    border-radius:5px;
    transition:width .15s ease;
  }
  input[type=range]{
    width:100%;
    margin-top:6px;
    accent-color:var(--green);
    background:transparent;
  }

  .links{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:10px;
  }
  .link{
    display:flex;
    align-items:center;
    gap:8px;
    padding:11px 12px;
    background:var(--panel-2);
    border:1px solid var(--border);
    border-radius:8px;
    color:var(--text);
    text-decoration:none;
    font-size:12px;
    font-weight:600;
  }
  .link .dotc{width:8px;height:8px;border-radius:50%;flex:none;}
  .link .ico{width:18px;height:18px;flex:none;object-fit:contain;}
  .link:hover{border-color:var(--green);}

  .footer-line{
    text-align:center;
    color:var(--muted);
    font-size:11px;
    padding:16px 0 4px;
  }
  .footer-line span{color:var(--green);}

  @media(max-width:420px){
    .links{grid-template-columns:1fr;}
  }
</style>
</head>
<body>
<div class="wrap">
  <div class="term">
    <div class="term-bar">
      <div class="dot r"></div><div class="dot y"></div><div class="dot g"></div>
      <div class="term-title">sudhanshu@progress-tracker: ~</div>
    </div>
    <div class="term-body">
      <p class="prompt">root@kali:~# <span>./track_progress.sh</span></p>
      <h1>Progress Tracker</h1>
      <p class="sub">AI-first OS journey · Zenith School of AI · updated locally, no login needed</p>

      <section>
        <div class="sec-head"><span class="tag">#</span><h2>daily_streak</h2></div>
        <div class="streak-row">
          <div class="streak-card">
            <div class="streak-num" id="curStreak">0</div>
            <div class="streak-label">CURRENT STREAK</div>
          </div>
          <div class="streak-card">
            <div class="streak-num" id="bestStreak">0</div>
            <div class="streak-label">LONGEST STREAK</div>
          </div>
          <div class="streak-card">
            <div class="streak-num" id="totalDays">0</div>
            <div class="streak-label">TOTAL DAYS LOGGED</div>
          </div>
        </div>
        <button class="checkin-btn" id="checkinBtn">mark today as a study/build day</button>
      </section>

      <section>
        <div class="sec-head"><span class="tag">#</span><h2>roadmap</h2></div>
        <div id="roadmapList"></div>
      </section>

      <section>
        <div class="sec-head"><span class="tag">#</span><h2>skills</h2></div>
        <div id="skillsList"></div>
      </section>

      <section>
        <div class="sec-head"><span class="tag">#</span><h2>quick_links</h2></div>
        <div class="links">
          <a class="link" href="https://www.linkedin.com/in/sudhanshu-kumar-960319409" target="_blank"><img class="ico" src="https://cdn.simpleicons.org/linkedin/0A66C2" alt="">LinkedIn</a>
          <a class="link" href="https://leetcode.com/u/sudh4nshukum4r" target="_blank"><img class="ico" src="https://cdn.simpleicons.org/leetcode/FFA116" alt="">LeetCode</a>
          <a class="link" href="https://www.youtube.com/@sudh4shuYadav" target="_blank"><img class="ico" src="https://cdn.simpleicons.org/youtube/FF0000" alt="">YouTube</a>
          <a class="link" href="https://www.instagram.com/sudhanshuy4dav" target="_blank"><img class="ico" src="https://cdn.simpleicons.org/instagram/E4405F" alt="">Instagram</a>
        </div>
      </section>

      <p class="footer-line">connection stable · <span>0 vulnerabilities</span> · keep shipping</p>
    </div>
  </div>
</div>

<script>
const KEY = 'sudhanshu-tracker-v1';

const DEFAULT_ROADMAP = [
  { id:'r1', txt:'Started B.Tech AI + Mini-MBA at Zenith School of AI', done:true },
  { id:'r2', txt:'Built foundations in Python, DSA, SQL, Git/GitHub', done:true },
  { id:'r3', txt:'Complete ML & Deep Learning specialization', done:false },
  { id:'r4', txt:'Ship first Generative AI project', done:false },
  { id:'r5', txt:'Complete first industry externship', done:false },
  { id:'r6', txt:'Land first AI/ML internship', done:false },
  { id:'r7', txt:'Begin AI-first Operating System architecture work', done:false },
];

const DEFAULT_SKILLS = [
  { id:'s1', name:'Python', pct:40 },
  { id:'s2', name:'DSA', pct:30 },
  { id:'s3', name:'SQL', pct:35 },
  { id:'s4', name:'Machine Learning', pct:20 },
  { id:'s5', name:'Deep Learning', pct:10 },
  { id:'s6', name:'Generative AI', pct:15 },
  { id:'s7', name:'Prompt Engineering', pct:25 },
  { id:'s8', name:'Computer Vision', pct:10 },
];

let state = {
  streak: { current:0, best:0, total:0, lastDate:null },
  roadmap: DEFAULT_ROADMAP,
  skills: DEFAULT_SKILLS,
};

function todayStr(){
  return new Date().toISOString().slice(0,10);
}
function daysBetween(a,b){
  return Math.round((new Date(b) - new Date(a)) / 86400000);
}

async function loadState(){
  try{
    const raw = localStorage.getItem(KEY);
    if(raw){
      const parsed = JSON.parse(raw);
      state = { ...state, ...parsed };
    }
  }catch(e){
    // no existing data yet, use defaults
  }
  render();
}

async function saveState(){
  try{
    localStorage.setItem(KEY, JSON.stringify(state));
  }catch(e){
    console.error('save failed', e);
  }
}

function render(){
  document.getElementById('curStreak').textContent = state.streak.current;
  document.getElementById('bestStreak').textContent = state.streak.best;
  document.getElementById('totalDays').textContent = state.streak.total;

  const btn = document.getElementById('checkinBtn');
  const already = state.streak.lastDate === todayStr();
  btn.disabled = already;
  btn.textContent = already ? "today's check-in logged ✓" : 'mark today as a study/build day';

  const rl = document.getElementById('roadmapList');
  rl.innerHTML = '';
  state.roadmap.forEach(item=>{
    const div = document.createElement('div');
    div.className = 'task' + (item.done ? ' done':'');
    div.innerHTML = `<div class="box"></div><div class="txt">${item.txt}</div>`;
    div.onclick = ()=>{
      item.done = !item.done;
      saveState();
      render();
    };
    rl.appendChild(div);
  });

  const sl = document.getElementById('skillsList');
  sl.innerHTML = '';
  state.skills.forEach(sk=>{
    const div = document.createElement('div');
    div.className = 'skill';
    div.innerHTML = `
      <div class="skill-top"><span>${sk.name}</span><span class="pct">${sk.pct}%</span></div>
      <div class="bar-track"><div class="bar-fill" style="width:${sk.pct}%"></div></div>
      <input type="range" min="0" max="100" value="${sk.pct}" data-id="${sk.id}">
    `;
    sl.appendChild(div);
  });
  sl.querySelectorAll('input[type=range]').forEach(inp=>{
    inp.oninput = (e)=>{
      const sk = state.skills.find(s=>s.id===e.target.dataset.id);
      sk.pct = parseInt(e.target.value,10);
      render();
    };
    inp.onchange = ()=> saveState();
  });
}

document.getElementById('checkinBtn').onclick = ()=>{
  const today = todayStr();
  if(state.streak.lastDate === today) return;

  if(state.streak.lastDate && daysBetween(state.streak.lastDate, today) === 1){
    state.streak.current += 1;
  } else {
    state.streak.current = 1;
  }
  state.streak.lastDate = today;
  state.streak.total += 1;
  if(state.streak.current > state.streak.best) state.streak.best = state.streak.current;

  saveState();
  render();
};

loadState();
</script>
</body>
</html>
