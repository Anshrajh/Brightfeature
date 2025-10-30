<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Bright Future — Student Portal</title>
<meta name="description" content="Bright Future teaching app — courses, live classes, downloads, contact. Demo front-end." />
<!--
  Save as index.html and open in Chrome.
  Optional: run local server for best tel/WA behaviour:
    python -m http.server 8000
    then open http://localhost:8000/index.html
-->
<style>
  :root{
    --bg:#071023;
    --accent:#0ea5ff;
    --accent-2:#06b6d4;
    --card: rgba(255,255,255,0.04);
    --muted: rgba(255,255,255,0.75);
    --glass: rgba(255,255,255,0.03);
    --success:#10b981;
    --danger:#ef4444;
    --radius:12px;
    --small-icon:18px;
  }
  *{box-sizing:border-box}
  html,body{height:100%;margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,Arial; -webkit-font-smoothing:antialiased; background:linear-gradient(180deg, rgba(2,6,23,0.6), rgba(2,6,23,0.6)), url('https://images.unsplash.com/photo-1503676260728-1c00da094a0b?auto=format&fit=crop&w=1600&q=80') center/cover no-repeat; color: #e6eef8;}
  .app {min-height:100vh; display:flex; flex-direction:column; align-items:stretch;}
  header{backdrop-filter: blur(6px); background:linear-gradient(90deg, rgba(2,6,23,0.6), rgba(2,6,23,0.45)); padding:12px 18px; display:flex;align-items:center;justify-content:space-between; gap:12px;}
  .brand{display:flex;gap:12px;align-items:center}
  .logo{width:48px;height:48px;border-radius:10px;background:linear-gradient(135deg,var(--accent),var(--accent-2));display:flex;align-items:center;justify-content:center;font-weight:700;color:#012;box-shadow:0 6px 20px rgba(2,6,23,0.6)}
  .brand h1{font-size:18px;margin:0}
  nav{display:flex;gap:8px;align-items:center}
  .nav-btn{background:transparent;border:0;color:var(--muted);padding:8px 10px;border-radius:8px;cursor:pointer;font-weight:600;display:flex;gap:8px;align-items:center}
  .nav-btn:hover{color:#fff;background:var(--glass)}
  .nav-btn.active{background:linear-gradient(90deg, rgba(255,255,255,0.06), rgba(255,255,255,0.03)); color:#fff}
  main{flex:1; max-width:1100px;margin:20px auto;padding:18px;width:calc(100% - 36px)}
  .card{background:var(--card); border-radius:var(--radius); padding:16px; box-shadow:0 10px 30px rgba(2,6,23,0.45)}
  .hero{display:flex; gap:18px; align-items:center; justify-content:space-between; flex-wrap:wrap}
  .hero .left{max-width:680px}
  h2{margin:0 0 8px 0;font-size:24px}
  p.lead{margin:0;color:rgba(230,238,248,0.85)}
  .cta{margin-top:14px;display:flex;gap:10px;flex-wrap:wrap}
  .btn{background:linear-gradient(90deg,var(--accent),var(--accent-2)); color:#001f2d; padding:10px 14px;border-radius:10px;border:0;font-weight:700;cursor:pointer;box-shadow:0 6px 18px rgba(10,112,150,0.12)}
  .btn.secondary{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--muted)}
  .grid{display:grid;gap:14px}
  .courses{grid-template-columns:repeat(auto-fit,minmax(220px,1fr));}
  .course{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:12px;border-radius:10px;display:flex;flex-direction:column;gap:8px}
  .course h3{margin:0;font-size:16px}
  .course p{margin:0;color:var(--muted);font-size:13px}
  .small{font-size:13px;color:var(--muted)}
  .video-wrap{margin-top:12px}
  .video{width:100%;height:420px;border-radius:10px;border:0;background:#000}
  .download-list{display:flex;gap:12px;flex-wrap:wrap;margin-top:12px}
  .tiny{font-size:12px;color:var(--muted)}
  footer{padding:12px;text-align:center;color:var(--muted);font-size:13px}

  /* login modal */
  .modal{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.5);visibility:hidden;opacity:0;transition:opacity .12s}
  .modal.show{visibility:visible;opacity:1}
  .modal-panel{width:380px;max-width:94%;background:linear-gradient(180deg,#071823,#0b1826);padding:18px;border-radius:12px;box-shadow:0 20px 60px rgba(2,6,23,0.7)}
  .input{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit;margin-top:10px}
  .tiny-muted{font-size:12px;color:rgba(230,238,248,0.7)}

  /* small floating action buttons */
  .fab-wrap{position:fixed;right:14px;bottom:14px;display:flex;flex-direction:column;gap:10px;z-index:60}
  .fab{width:44px;height:44px;border-radius:10px;display:inline-flex;align-items:center;justify-content:center;background:#25D366;color:#012;box-shadow:0 10px 30px rgba(2,6,23,0.6);text-decoration:none}
  .fab.phone{background:#111;color:#fff}
  .fab svg{width:20px;height:20px}
  /* responsive */
  @media(max-width:800px){
    .video{height:260px}
    .hero{flex-direction:column;align-items:flex-start}
    nav{gap:6px}
  }
</style>
</head>
<body>
  <div class="app">
    <header>
      <div class="brand">
        <div class="logo">BF</div>
        <div>
          <h1>Bright Future</h1>
          <div class="tiny">Student Portal</div>
        </div>
      </div>

      <nav id="navbar" aria-label="Main navigation">
        <button class="nav-btn active" data-page="home" title="Home">🏠 <span class="tiny">Home</span></button>
        <button class="nav-btn" data-page="courses" title="Courses">📚 <span class="tiny">Courses</span></button>
        <button class="nav-btn" data-page="live" title="Live">🎥 <span class="tiny">Live</span></button>
        <button class="nav-btn" data-page="downloads" title="Downloads">⬇️ <span class="tiny">Downloads</span></button>
        <button class="nav-btn" data-page="contact" title="Contact">☎️ <span class="tiny">Contact</span></button>
        <button id="navAccount" class="nav-btn" style="margin-left:8px">🔐 <span class="tiny">Login</span></button>
      </nav>
    </header>

    <main>
      <!-- Home -->
      <section id="page-home" class="card" style="display:block">
        <div class="hero">
          <div class="left">
            <h2>Welcome to Bright Future</h2>
            <p class="lead">Interactive courses • Live classes • Downloads • WhatsApp support</p>
            <div class="cta">
              <button class="btn" onclick="goTo('courses')">Start Learning</button>
              <button class="btn secondary" onclick="openModal()">Sign in</button>
            </div>
            <div style="margin-top:14px" class="tiny-muted">Email: brightfutureni910@gmail.com • Phone: +91 9102349266</div>
          </div>
          <div style="min-width:300px">
            <div class="card">
              <div class="small">Next live</div>
              <div style="font-weight:700;margin-top:6px">Math Practice • Today 6:00 PM</div>
              <div class="tiny" style="margin-top:8px">Join via Live tab</div>
              <div style="margin-top:12px">
                <button class="nav-btn" onclick="openWhatsApp()">💬 WhatsApp</button>
                <button class="nav-btn" onclick="callNumber()">📞 Call</button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Courses -->
      <section id="page-courses" class="card" style="display:none">
        <h2>Courses</h2>
        <div class="grid courses" id="coursesGrid" style="margin-top:12px"></div>
      </section>

      <!-- Live -->
      <section id="page-live" class="card" style="display:none">
        <h2>Live Classes</h2>
        <div class="video-wrap">
          <iframe class="video" src="https://www.youtube.com/embed/dQw4w9WgXcQ" title="Live" allowfullscreen></iframe>
        </div>
      </section>

      <!-- Downloads -->
      <section id="page-downloads" class="card" style="display:none">
        <h2>Downloads</h2>
        <div class="download-list" id="downloadList"></div>
      </section>

      <!-- Contact -->
      <section id="page-contact" class="card" style="display:none">
        <h2>Contact & Support</h2>
        <div style="margin-top:10px" class="card">
          <div style="font-weight:700">Email</div>
          <div class="small">brightfutureni910@gmail.com</div>
          <div style="margin-top:10px;font-weight:700">Phone</div>
          <div class="small">+91 9102349266</div>
          <div style="margin-top:12px">
            <button class="nav-btn" onclick="openWhatsApp()">💬 WhatsApp</button>
            <button class="nav-btn" onclick="callNumber()">📞 Call</button>
          </div>
        </div>
      </section>
    </main>

    <footer>© <span id="year"></span> Bright Future — Demo portal</footer>
  </div>

  <!-- small FABs -->
  <div class="fab-wrap">
    <a id="fabWA" class="fab" href="#" title="WhatsApp">💬</a>
    <a id="fabPhone" class="fab phone" href="#" title="Call">📞</a>
  </div>

  <!-- Login modal -->
  <div id="modal" class="modal" aria-hidden="true">
    <div class="modal-panel">
      <h3 style="margin:0 0 8px 0">Sign in</h3>
      <div class="tiny-muted">Demo login: <strong>student@gmail.com</strong> / <strong>12345</strong></div>
      <input id="loginEmail" class="input" placeholder="Email (or mobile)" />
      <input id="loginPass" class="input" placeholder="Password" type="password" />
      <div style="display:flex;gap:8px;margin-top:12px">
        <button class="btn" id="doLogin">Sign in</button>
        <button class="btn secondary" id="closeModal">Close</button>
      </div>
    </div>
  </div>

<script>
/* ====== Config ====== */
const CONTACT_PHONE = '+91 9102349266';
const WA_NUMBER = '91' + '9102349266'; // country code + number for wa.me
const EMAIL = 'brightfutureni910@gmail.com';

/* ====== Demo Data ====== */
const COURSES = [
  { id:'c1', title:'Intro to Mathematics', desc:'Algebra • Geometry • Problem solving', price:199, resource:'math-notes.pdf', video:'https://www.youtube.com/embed/dQw4w9WgXcQ' },
  { id:'c2', title:'Guitar for Beginners', desc:'Chords • Strumming patterns', price:149, resource:'guitar-notes.pdf', video:'https://www.youtube.com/embed/ScMzIvxBSi4' },
  { id:'c3', title:'English Communication', desc:'Speaking • Grammar • Confidence', price:129, resource:'english-notes.pdf', video:'https://www.youtube.com/embed/5qap5aO4i9A' },
];

/* ====== Utilities ====== */
const el = id => document.getElementById(id);
const escapeHtml = s => (s+'').replace(/[&<>"']/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m]));

/* ====== UI: render courses & downloads ====== */
function renderCourses(){
  const container = el('coursesGrid');
  container.innerHTML = '';
  COURSES.forEach(c=>{
    const card = document.createElement('div');
    card.className = 'course';
    card.innerHTML = `<h3>${escapeHtml(c.title)}</h3><p>${escapeHtml(c.desc)}</p>
      <div style="margin-top:auto;display:flex;gap:8px">
        <button class="nav-btn" onclick="previewCourse('${c.id}')">Preview</button>
        <button class="nav-btn" onclick="enrollCourse('${c.id}')">Enroll • ₹${c.price}</button>
      </div>`;
    container.appendChild(card);
  });
}
function renderDownloads(){
  const dl = el('downloadList'); dl.innerHTML = '';
  COURSES.forEach(c=>{
    const d = document.createElement('div'); d.className='card'; d.style.minWidth='200px';
    d.innerHTML = `<div style="font-weight:700">${escapeHtml(c.title)}</div><div class="small">${escapeHtml(c.desc)}</div>
      <div style="margin-top:8px"><button class="nav-btn" onclick="downloadResource('${c.resource}')">Download</button></div>`;
    dl.appendChild(d);
  });
}

/* ====== Actions ====== */
function previewCourse(id){
  const c = COURSES.find(x=>x.id===id);
  if(!c) return alert('Course not found');
  // open a small preview window (demo)
  const win = window.open('', '_blank', 'width=900,height=600');
  win.document.write(`<title>${c.title} — Preview</title><style>body{margin:0;background:#000;color:#fff;display:flex;align-items:center;justify-content:center;height:100vh}</style><iframe src="${c.video}" frameborder="0" style="width:100%;height:100vh" allowfullscreen></iframe>`);
}
function enrollCourse(id){
  if(!isLogged()){ openModal(); return; }
  // simulate purchase/enroll
  const c = COURSES.find(x=>x.id===id);
  alert('Enrolled in "'+c.title+'" (demo). Check Dashboard later.');
  // store enrollment in localStorage
  const user = getUser(); const arr = JSON.parse(localStorage.getItem('bf_enroll')||'[]'); arr.push({user: user.email||user.id, courseId:id, date:new Date().toISOString()}); localStorage.setItem('bf_enroll', JSON.stringify(arr));
}
function downloadResource(name){
  const txt = `Bright Future — ${name}\n\nDemo resource. Replace with real PDF in production.`;
  const blob = new Blob([txt], {type:'application/pdf'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href=url; a.download=name; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
}

/* ====== Contact actions ====== */
function openWhatsApp(){
  const wa = `https://wa.me/${WA_NUMBER}?text=${encodeURIComponent('Hello Bright Future!')}`;
  const api = `https://api.whatsapp.com/send?phone=${WA_NUMBER}&text=${encodeURIComponent('Hello Bright Future!')}`;
  // open wa.me; fallback to api
  try { window.open(wa,'_blank'); } catch(e){ window.open(api,'_blank'); }
}
function callNumber(){
  const tel = 'tel:' + CONTACT_PHONE.replace(/\s+/g,'');
  if(/Mobi|Android|iPhone|iPad|iPod/i.test(navigator.userAgent)) {
    window.location.href = tel;
  } else {
    // desktop: copy number to clipboard
    navigator.clipboard?.writeText(CONTACT_PHONE).then(()=> alert('Phone copied: '+CONTACT_PHONE)).catch(()=> alert('Call: '+CONTACT_PHONE));
  }
}

/* ====== Login modal & auth (demo) ====== */
const modal = el('modal');
function openModal(){ modal.classList.add('show'); modal.setAttribute('aria-hidden','false'); }
function closeModal(){ modal.classList.remove('show'); modal.setAttribute('aria-hidden','true'); }

el('closeModal').addEventListener('click', closeModal);
el('doLogin').addEventListener('click', ()=>{
  const email = el('loginEmail').value.trim();
  const pass = el('loginPass').value;
  if((email==='student@gmail.com' && pass==='12345') || (email.includes('@') && pass.length>=3)) {
    // store user
    const user = { email };
    localStorage.setItem('bf_user', JSON.stringify(user));
    closeModal();
    refreshAuth();
    alert('Signed in (demo)');
  } else {
    alert('Use demo credentials: student@gmail.com / 12345 or any valid email + password (demo).');
  }
});

/* ====== Auth helpers ====== */
function getUser(){ try{ return JSON.parse(localStorage.getItem('bf_user')); }catch(e){return null;} }
function isLogged(){ return !!getUser(); }
function signOut(){ localStorage.removeItem('bf_user'); refreshAuth(); alert('Signed out'); }

el('navAccount').addEventListener('click', ()=>{
  if(isLogged()) { // open small menu or sign out prompt
    if(confirm('Sign out?')) signOut();
  } else openModal();
});
el('btnLogout')?.addEventListener('click', signOut);

/* ====== Routing (simple) ====== */
const pages = {
  home: 'page-home',
  courses: 'page-courses',
  live: 'page-live',
  downloads: 'page-downloads',
  contact: 'page-contact'
};
document.querySelectorAll('button[data-page]').forEach(b=>{
  b.addEventListener('click', ()=>{ showPage(b.dataset.page); });
});
function showPage(key){
  Object.values(pages).forEach(id => document.getElementById(id).style.display = 'none');
  document.getElementById(pages[key]).style.display = 'block';
  document.querySelectorAll('.nav-btn').forEach(nb => nb.classList.toggle('active', nb.dataset.page === key));
}

/* ====== refresh UI ====== */
function refreshAuth(){
  const u = getUser();
  if(u){
    el('navAccount').textContent = u.email.split('@')[0] || 'Account';
    el('btnLogout').style.display = 'inline-flex';
  } else {
    el('navAccount').textContent = '🔐 Login';
    el('btnLogout').style.display = 'none';
  }
  renderCourses(); renderDownloads();
  // default show home
  showPage('home');
}

/* ====== initial setup ====== */
function init(){
  // attach small nav buttons (already present)
  document.querySelectorAll('#navbutton, .nav-btn').forEach(btn=>{
    // already handlers via data-page
  });
  renderCourses(); renderDownloads();
  refreshAuth();
  el('year').textContent = new Date().getFullYear();
}
init();

/* keyboard escape to close modal */
window.addEventListener('keydown', e => { if(e.key==='Escape') closeModal(); });

</script>
</body>
</html>
