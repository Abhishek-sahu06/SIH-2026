<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MediKiosk — AI Clinical Intake</title>
<style>
:root{
  --bg:#07111f; --panel:rgba(13,29,48,.78); --panel2:rgba(17,39,63,.82);
  --text:#eef8ff; --muted:#9db2c8; --cyan:#35d9ff; --blue:#5b7cff;
  --green:#48e0a4; --red:#ff6b7d; --line:rgba(255,255,255,.1);
  --shadow:0 25px 80px rgba(0,0,0,.35);
}
*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{
  font-family:Inter,Segoe UI,Arial,sans-serif;background:var(--bg);color:var(--text);
  overflow-x:hidden;
}
body:before{
  content:"";position:fixed;inset:0;z-index:-3;
  background:
   radial-gradient(circle at 15% 10%,rgba(53,217,255,.14),transparent 30%),
   radial-gradient(circle at 85% 25%,rgba(91,124,255,.16),transparent 30%),
   radial-gradient(circle at 50% 90%,rgba(72,224,164,.08),transparent 35%);
}
.grid{
 position:fixed;inset:0;z-index:-2;opacity:.15;
 background-image:linear-gradient(rgba(255,255,255,.05) 1px,transparent 1px),
 linear-gradient(90deg,rgba(255,255,255,.05) 1px,transparent 1px);
 background-size:45px 45px;mask-image:linear-gradient(to bottom,black,transparent 90%);
}
nav{
 position:fixed;top:0;left:0;right:0;z-index:50;height:74px;padding:0 6%;
 display:flex;align-items:center;justify-content:space-between;
 background:rgba(5,15,28,.7);backdrop-filter:blur(18px);border-bottom:1px solid var(--line)
}
.logo{font-weight:900;font-size:23px;letter-spacing:-1px}.logo span{color:var(--cyan)}
.navlinks{display:flex;gap:28px}.navlinks a{color:#c8d8e8;text-decoration:none;font-size:14px}.navlinks a:hover{color:var(--cyan)}
.btn{
 border:0;border-radius:13px;padding:12px 18px;font-weight:800;cursor:pointer;color:#03121d;
 background:linear-gradient(135deg,var(--cyan),#8af0ff);box-shadow:0 8px 30px rgba(53,217,255,.2);
 transition:.25s
}.btn:hover{transform:translateY(-2px);box-shadow:0 12px 35px rgba(53,217,255,.35)}
.btn.secondary{background:transparent;color:#dcefff;border:1px solid var(--line);box-shadow:none}
.hero{min-height:100vh;padding:150px 7% 90px;display:grid;grid-template-columns:1.1fr .9fr;gap:60px;align-items:center}
.badge{display:inline-flex;gap:8px;align-items:center;padding:8px 13px;border:1px solid rgba(53,217,255,.25);border-radius:30px;background:rgba(53,217,255,.07);color:#a9efff;font-size:12px;font-weight:800}
.dot{width:8px;height:8px;border-radius:50%;background:var(--green);box-shadow:0 0 16px var(--green);animation:pulse 1.5s infinite}
h1{font-size:clamp(45px,6vw,82px);line-height:.96;letter-spacing:-4px;margin:22px 0}
.gradient{background:linear-gradient(110deg,#fff 20%,var(--cyan) 55%,#8b9dff);-webkit-background-clip:text;color:transparent}
.hero p{max-width:650px;color:var(--muted);font-size:18px;line-height:1.7}
.actions{display:flex;gap:12px;margin-top:30px;flex-wrap:wrap}
.stats{display:flex;gap:35px;margin-top:38px}.stat b{font-size:25px}.stat span{display:block;color:var(--muted);font-size:12px;margin-top:4px}
.visual{position:relative;height:550px;display:grid;place-items:center}
.orbit{position:absolute;width:430px;height:430px;border:1px solid rgba(53,217,255,.2);border-radius:50%;animation:spin 18s linear infinite}
.orbit:before,.orbit:after{content:"";position:absolute;width:12px;height:12px;border-radius:50%;background:var(--cyan);box-shadow:0 0 25px var(--cyan)}.orbit:before{top:38px;left:65px}.orbit:after{right:25px;bottom:100px;background:var(--green);box-shadow:0 0 25px var(--green)}
.kiosk{
 width:330px;height:430px;border:1px solid rgba(255,255,255,.18);border-radius:32px;
 background:linear-gradient(145deg,rgba(24,54,83,.9),rgba(7,20,35,.94));box-shadow:var(--shadow);
 padding:22px;position:relative;overflow:hidden;animation:float 4s ease-in-out infinite
}
.kiosk:after{content:"";position:absolute;inset:-50%;background:linear-gradient(115deg,transparent 40%,rgba(255,255,255,.08),transparent 60%);transform:rotate(12deg);animation:shine 4s infinite}
.khead{display:flex;justify-content:space-between;align-items:center}.heart{font-size:27px;color:var(--cyan)}
.avatar{width:82px;height:82px;margin:35px auto 18px;border-radius:50%;display:grid;place-items:center;font-size:36px;background:radial-gradient(circle,#234d6e,#0a1829);border:1px solid rgba(53,217,255,.4)}
.voice{height:70px;display:flex;align-items:center;justify-content:center;gap:4px}.bar{width:4px;border-radius:5px;background:var(--cyan);animation:wave .8s ease-in-out infinite}.bar:nth-child(2){animation-delay:.1s}.bar:nth-child(3){animation-delay:.2s}.bar:nth-child(4){animation-delay:.3s}.bar:nth-child(5){animation-delay:.4s}.bar:nth-child(6){animation-delay:.3s}.bar:nth-child(7){animation-delay:.2s}.bar:nth-child(8){animation-delay:.1s}
.mini{padding:13px;border-radius:14px;background:rgba(255,255,255,.05);margin-top:12px;color:#bed0df;font-size:12px}.mini strong{color:#fff}
section{padding:105px 7%}.section-title{text-align:center;max-width:760px;margin:auto}.eyebrow{color:var(--cyan);font-weight:900;font-size:12px;letter-spacing:2px;text-transform:uppercase}.section-title h2{font-size:clamp(32px,4vw,54px);margin:12px 0}.section-title p{color:var(--muted);line-height:1.7}
.cards{display:grid;grid-template-columns:repeat(4,1fr);gap:17px;margin-top:50px}.card{padding:25px;border:1px solid var(--line);border-radius:22px;background:var(--panel);box-shadow:var(--shadow);transition:.3s}.card:hover{transform:translateY(-8px);border-color:rgba(53,217,255,.35)}.icon{font-size:30px;margin-bottom:20px}.card h3{margin-bottom:9px}.card p{color:var(--muted);font-size:14px;line-height:1.65}
.demo{display:grid;grid-template-columns:.8fr 1.2fr;gap:25px;margin-top:50px}.panel{border:1px solid var(--line);border-radius:24px;background:var(--panel);padding:26px;box-shadow:var(--shadow)}.panel h3{margin-bottom:18px}.steps{display:grid;gap:12px}.step{display:flex;gap:14px;padding:16px;border:1px solid var(--line);border-radius:15px;cursor:pointer;transition:.2s}.step.active{border-color:var(--cyan);background:rgba(53,217,255,.08)}.num{width:28px;height:28px;border-radius:50%;display:grid;place-items:center;background:#17334d;color:#9bdff0;font-weight:900}.step.active .num{background:var(--cyan);color:#05202b}.step small{color:var(--muted)}
.screen{min-height:430px}.screen-top{display:flex;justify-content:space-between;align-items:center;border-bottom:1px solid var(--line);padding-bottom:17px;margin-bottom:20px}.live{color:var(--green);font-size:12px;font-weight:800}.question{font-size:27px;line-height:1.25;max-width:650px;margin:35px 0 20px}.language{display:flex;gap:8px;flex-wrap:wrap}.lang{padding:9px 12px;border:1px solid var(--line);border-radius:10px;color:#b9cddd;font-size:12px;cursor:pointer}.lang.active{border-color:var(--cyan);color:var(--cyan);background:rgba(53,217,255,.08)}
.answer{display:flex;gap:10px;margin-top:28px}.input{flex:1;padding:14px;border-radius:12px;background:#081727;border:1px solid var(--line);color:#fff;outline:none}.timeline{display:grid;gap:12px;margin-top:20px}.event{padding:15px;border-left:3px solid var(--cyan);background:rgba(255,255,255,.04);border-radius:8px}.event span{color:var(--muted);font-size:11px}.event b{display:block;margin-top:4px}
.alert{margin-top:20px;padding:17px;border-radius:15px;border:1px solid rgba(255,107,125,.35);background:rgba(255,107,125,.07);display:none}.alert.show{display:block;animation:pop .35s}.alert b{color:#ff9baa}
.doctor{margin-top:50px;display:grid;grid-template-columns:1fr 1fr;gap:20px}.summary-line{display:flex;justify-content:space-between;padding:13px 0;border-bottom:1px solid var(--line);font-size:13px}.summary-line span{color:var(--muted)}.tag{padding:5px 8px;border-radius:7px;background:rgba(72,224,164,.1);color:var(--green);font-size:11px}
.cta{margin:80px 7% 30px;padding:65px;border-radius:30px;border:1px solid rgba(53,217,255,.2);background:radial-gradient(circle at 80% 20%,rgba(53,217,255,.14),transparent 35%),var(--panel);text-align:center}.cta h2{font-size:42px;margin-bottom:12px}.cta p{color:var(--muted);margin-bottom:25px}
footer{padding:30px 7%;border-top:1px solid var(--line);display:flex;justify-content:space-between;color:#7f94a9;font-size:12px}
.modal{position:fixed;inset:0;background:rgba(0,0,0,.7);backdrop-filter:blur(8px);display:none;place-items:center;z-index:100;padding:20px}.modal.show{display:grid}.modalbox{width:min(600px,100%);background:#0b1d30;border:1px solid var(--line);border-radius:24px;padding:28px;animation:pop .3s}.close{float:right;background:none;border:0;color:#aaa;font-size:25px;cursor:pointer}
.progress{height:5px;background:#10283d;border-radius:10px;margin:18px 0}.progress i{display:block;height:100%;width:20%;background:linear-gradient(90deg,var(--cyan),var(--green));border-radius:10px;transition:.4s}
@keyframes float{50%{transform:translateY(-12px)}}@keyframes spin{to{transform:rotate(360deg)}}@keyframes pulse{50%{opacity:.35;transform:scale(.7)}}@keyframes wave{0%,100%{height:12px}50%{height:50px}}@keyframes shine{to{transform:translateX(60%) rotate(12deg)}}@keyframes pop{from{transform:scale(.95);opacity:0}to{transform:scale(1);opacity:1}}
@media(max-width:900px){.hero,.demo,.doctor{grid-template-columns:1fr}.cards{grid-template-columns:1fr 1fr}.visual{height:480px}.navlinks{display:none}}
@media(max-width:560px){.cards{grid-template-columns:1fr}.hero{padding-top:120px}.stats{gap:18px}.kiosk{width:290px}.orbit{width:350px;height:350px}.cta{margin:50px 5%;padding:40px 20px}section{padding:75px 5%}}
</style>
</head>
<body>
<div class="grid"></div>

<nav>
  <div class="logo">Medi<span>Kiosk</span> <small style="font-size:9px;color:#718ba1">AI CLINICAL INTAKE</small></div>
  <div class="navlinks">
    <a href="#solution">Solution</a><a href="#demo">Live Demo</a><a href="#doctor">Doctor View</a><a href="#abdm">ABDM</a>
  </div>
  <button class="btn" onclick="openIntake()">Start Intake</button>
</nav>

<main>
<section class="hero">
 <div>
   <div class="badge"><i class="dot"></i> AI-POWERED • MULTILINGUAL • CONSENT-FIRST</div>
   <h1>Your story.<br><span class="gradient">Understood before</span><br>the consultation.</h1>
   <p>MediKiosk transforms patient conversations and old medical documents into a structured, physician-ready clinical history — before the patient enters the consultation room.</p>
   <div class="actions">
     <button class="btn" onclick="openIntake()">▶ Experience MediKiosk</button>
     <a class="btn secondary" href="#solution" style="text-decoration:none">Explore solution ↓</a>
   </div>
   <div class="stats"><div class="stat"><b>5 min</b><span>guided intake</span></div><div class="stat"><b>24/7</b><span>AI assistance</span></div><div class="stat"><b>5-step</b><span>patient journey</span></div></div>
 </div>
 <div class="visual">
   <div class="orbit"></div>
   <div class="kiosk">
     <div class="khead"><b>MEDIKIOSK</b><span class="heart">♡</span></div>
     <div class="avatar">🩺</div>
     <center><small style="color:#8fa8bc">AI Health Assistant</small><h3 style="margin-top:7px">Tell me what brings you here.</h3></center>
     <div class="voice"><i class="bar"></i><i class="bar"></i><i class="bar"></i><i class="bar"></i><i class="bar"></i><i class="bar"></i><i class="bar"></i><i class="bar"></i></div>
     <div class="mini">Language <strong>Hindi / English</strong></div>
     <div class="mini">Status <strong style="color:var(--green)">● Listening</strong></div>
   </div>
 </div>
</section>

<section id="solution">
 <div class="section-title"><div class="eyebrow">One platform. Four engines.</div><h2>Built around the real OPD bottleneck.</h2><p>From natural conversation to clinical structure, every module is designed to reduce repetitive history-taking and fragmented records.</p></div>
 <div class="cards">
   <div class="card"><div class="icon">🗣️</div><h3>AI Conversation</h3><p>Adaptive voice + touch interview that branches intelligently based on the patient's complaint.</p></div>
   <div class="card"><div class="icon">📄</div><h3>Smart OCR</h3><p>Digitizes prescriptions, reports and discharge summaries, extracting useful clinical information.</p></div>
   <div class="card"><div class="icon">🚨</div><h3>Red-Flag AI</h3><p>Detects potential emergency symptoms and routes the patient for priority triage.</p></div>
   <div class="card"><div class="icon">🔐</div><h3>ABHA + Consent</h3><p>Consent-first data flow designed for secure integration with hospital systems and ABDM.</p></div>
 </div>
</section>

<section id="demo">
 <div class="section-title"><div class="eyebrow">Interactive prototype</div><h2>See MediKiosk in action.</h2><p>Click through the patient journey as a judge would during an SIH demo.</p></div>
 <div class="demo">
   <div class="panel">
    <h3>Patient Journey</h3>
    <div class="steps">
      <div class="step active" onclick="setStep(1)"><div class="num">1</div><div><b>Identify</b><small> Language + consent</small></div></div>
      <div class="step" onclick="setStep(2)"><div class="num">2</div><div><b>Converse</b><small> Voice + touch history</small></div></div>
      <div class="step" onclick="setStep(3)"><div class="num">3</div><div><b>Scan</b><small> Documents + OCR</small></div></div>
      <div class="step" onclick="setStep(4)"><div class="num">4</div><div><b>Summarize</b><small> AI clinical summary</small></div></div>
      <div class="step" onclick="setStep(5)"><div class="num">5</div><div><b>Consult</b><small> Doctor reviews</small></div></div>
    </div>
   </div>
   <div class="panel screen" id="screen"></div>
 </div>
</section>

<section id="doctor">
 <div class="section-title"><div class="eyebrow">Physician view</div><h2>A complete story in seconds.</h2><p>The AI output is an editable draft — the physician remains in control.</p></div>
 <div class="doctor">
  <div class="panel">
   <div class="screen-top"><b>Patient Summary</b><span class="tag">AI DRAFT</span></div>
   <div class="summary-line"><span>Chief Complaint</span><b>Chest discomfort</b></div>
   <div class="summary-line"><span>HPI</span><b>Intermittent • 2 days</b></div>
   <div class="summary-line"><span>Past History</span><b>Hypertension</b></div>
   <div class="summary-line"><span>Medications</span><b>Amlodipine 5 mg</b></div>
   <div class="summary-line"><span>Allergies</span><b>None reported</b></div>
   <div class="summary-line"><span>Investigations</span><b>2 reports digitized</b></div>
  </div>
  <div class="panel">
   <div class="screen-top"><b>Medical Timeline</b><span class="live">● SYNCED</span></div>
   <div class="timeline">
    <div class="event"><span>12 AUG 2026</span><b>Blood pressure — 148/92 mmHg</b></div>
    <div class="event"><span>04 JUL 2026</span><b>Prescription — Amlodipine 5 mg</b></div>
    <div class="event"><span>18 MAR 2026</span><b>Lab report — HbA1c 6.1%</b></div>
   </div>
  </div>
 </div>
</section>

<section id="abdm">
 <div class="section-title"><div class="eyebrow">Trust layer</div><h2>Designed for India's digital health ecosystem.</h2><p>Consent-first workflow with ABHA-linked records, structured data and hospital-system routing.</p></div>
 <div class="cards">
  <div class="card"><div class="icon">🪪</div><h3>ABHA Ready</h3><p>Patient identification and consent flow designed around ABHA-linked health records.</p></div>
  <div class="card"><div class="icon">🔗</div><h3>FHIR Flow</h3><p>Structured clinical information can be mapped into interoperable healthcare workflows.</p></div>
  <div class="card"><div class="icon">🎙️</div><h3>Indian Languages</h3><p>Voice-first design helps elderly, low-literacy and multilingual users interact naturally.</p></div>
  <div class="card"><div class="icon">🛡️</div><h3>Privacy by Design</h3><p>Granular consent, secure processing and temporary session handling are built into the concept.</p></div>
 </div>
</section>

<div class="cta">
 <h2>Turn OPD waiting time into clinical intelligence.</h2>
 <p>Experience the future of patient intake with MediKiosk.</p>
 <button class="btn" onclick="openIntake()">Launch Live Prototype →</button>
</div>
</main>

<footer><span>© 2026 MediKiosk • SIH Prototype</span><span>AI assists. Doctors decide.</span></footer>

<div class="modal" id="modal">
 <div class="modalbox">
  <button class="close" onclick="closeIntake()">×</button>
  <div class="eyebrow">MediKiosk Live Intake</div>
  <h2 style="margin-top:8px">Let's prepare your consultation.</h2>
  <p style="color:var(--muted);margin:10px 0">Choose your preferred language and begin a guided clinical history.</p>
  <div class="language" style="margin:22px 0"><span class="lang active">English</span><span class="lang">हिन्दी</span><span class="lang">मराठी</span><span class="lang">বাংলা</span><span class="lang">தமிழ்</span></div>
  <div class="progress"><i id="modalProgress"></i></div>
  <div id="modalQuestion" style="font-size:22px;font-weight:800;margin:25px 0">What is the main problem you are experiencing today?</div>
  <input class="input" id="modalInput" placeholder="Type your answer or use voice…">
  <button class="btn" style="width:100%;margin-top:12px" onclick="nextModal()">Continue →</button>
  <div class="alert" id="modalAlert"><b>⚠ Priority triage signal</b><br><small>Potential red-flag symptoms detected. Please approach the triage desk immediately.</small></div>
 </div>
</div>

<script>
const screen=document.getElementById('screen');
function setStep(n){
 document.querySelectorAll('.step').forEach((x,i)=>x.classList.toggle('active',i===n-1));
 const data=[
  ['Identify','Select language, verify identity and provide informed consent.','🪪','Consent recorded • Language: Hindi • Session secure'],
  ['Converse','AI asks adaptive questions based on the patient’s complaint.','🗣️','AI: “When did the chest discomfort begin?”'],
  ['Scan','Upload previous prescriptions and reports for intelligent OCR.','📄','OCR complete • 3 documents • 14 clinical entities extracted'],
  ['Summarize','AI combines the conversation and documents into a physician-ready draft.','🧠','Summary generated • 7 clinical sections • Physician verification required'],
  ['Consult','The physician reviews, edits and confirms the structured history.','👨‍⚕️','Ready for consultation • Priority: Normal']
 ];
 const d=data[n-1];
 screen.innerHTML=`<div class="screen-top"><b>${d[0]}</b><span class="live">● LIVE PROTOTYPE</span></div>
 <div style="font-size:60px;margin-top:25px">${d[2]}</div>
 <div class="question">${d[1]}</div>
 <div class="mini"><strong>System output</strong><br><br>${d[3]}</div>
 <button class="btn" style="margin-top:25px" onclick="setStep(${n===5?1:n+1})">${n===5?'Restart journey':'Continue →'}</button>`;
}
setStep(1);

function openIntake(){document.getElementById('modal').classList.add('show')}
function closeIntake(){document.getElementById('modal').classList.remove('show')}
let modalStep=1;
const qs=[
 'What is the main problem you are experiencing today?',
 'When did this problem start?',
 'Do you have any existing medical conditions or allergies?',
 'Have you brought any previous prescriptions or reports?'
];
function nextModal(){
 if(modalStep===1 && document.getElementById('modalInput').value.toLowerCase().includes('chest')){
  document.getElementById('modalAlert').classList.add('show');
 }
 if(modalStep<4){
  modalStep++;
  document.getElementById('modalQuestion').textContent=qs[modalStep-1];
  document.getElementById('modalProgress').style.width=(modalStep*25)+'%';
  document.getElementById('modalInput').value='';
 }else{
  document.getElementById('modalQuestion').innerHTML='✅ Intake complete<br><small style="color:#9db2c8;font-size:13px;font-weight:400">Your structured clinical history is ready for physician review.</small>';
  document.getElementById('modalProgress').style.width='100%';
  document.querySelector('#modal .btn').textContent='View Physician Summary →';
  document.querySelector('#modal .btn').onclick=()=>{closeIntake();document.getElementById('doctor').scrollIntoView()};
 }
}
document.querySelectorAll('.lang').forEach(x=>x.onclick=()=>{document.querySelectorAll('.lang').forEach(y=>y.classList.remove('active'));x.classList.add('active')});
document.getElementById('modal').addEventListener('click',e=>{if(e.target.id==='modal')closeIntake()});
</script>
</body>
</html>
