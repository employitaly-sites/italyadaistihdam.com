# Teklif Alın

> İtalya’da çalışan istihdam etmek için teklif isteyin. Üç kısa adım, bir iş günü içinde yanıt.


<section> <span class="eyebrow">Teklif alın</span> <h1 id="kimi-ise-almak-istediginizi-anlatin">Kimi işe almak istediğinizi anlatın</h1> <p class="lede">Üç kısa adım. Bir iş günü içinde toplam maliyet ve sözleşme taslağıyla yanıt veririz.</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">Adım 1 / 3</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">İş e-postası<input id="email" type="email" autocomplete="email" required></label> <label for="company">Şirket<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">Neye ihtiyacınız var?<select id="service"><option value="Employer of Record">Employer of Record (EOR)</option><option value="Payroll (we have an Italian entity)">Bordro (İtalya’da şirketimiz var)</option><option value="Work visa / Blue Card">Çalışma vizesi / Mavi Kart</option><option value="Recruitment">İşe alım</option></select></label> <label for="count">İtalya’da kaç kişi?<input id="count" type="number" min="1" value="1"></label> <label for="city">Şehir veya bölge<input id="city" placeholder="Milano, Roma, uzaktan…"></label> <label for="salary">Planlanan yıllık brüt maaş (EUR)<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">Adınız<input id="name" autocomplete="name"></label> <label for="start">Tahmini başlangıç tarihi<input id="start" type="date"></label> <label for="notes">Eklemek istedikleriniz<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">Bu talebe yanıt verilmesi için İtalya’da İstihdam’ın verilerimi işlemesini kabul ediyorum (gizlilik politikası).</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>Geri</button><button type="submit" class="btn" id="next">Devam</button></div> </form> </section> <script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'italyadaistihdam'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `Adım ${step} / 3`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? 'Talebi gönder' : 'Devam'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = 'Devam etmek için geçerli bir iş e-postası girin.'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = 'Yanıt verebilmemiz için onay kutusunu işaretleyin.'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = 'Teşekkürler. Bir iş günü içinde yanıt vereceğiz.'; } });
</script>

<script>(function(){var sj=new URLSearchParams(location.search).get('subject');var n=document.getElementById('notes');if(sj&&n&&!n.value)n.value='['+sj.slice(0,40)+'] ';})();</script>


---
Source: https://italyadaistihdam.com/iletisim.html
