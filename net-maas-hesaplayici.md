# İtalya Net Maaş Hesaplayıcı

> 2026’da İtalya’da brütten nete: çalışan sosyal güvenlik payı, istihdam indirimleriyle gelir vergisi (IRPEF), bölge ve belediye ek vergileri; Milano, Roma, Torino, Bologna ve Napoli için.


<section> <span class="eyebrow">Net maaş hesaplayıcı</span> <h1 id="calisaninizin-italyada-eline-gecen">Çalışanınızın İtalya’da eline geçen</h1> <p class="lede">Yıllık brüt maaşı ve şehri girin. Tahmin 2026 kurallarını uygular: çalışan sosyal güvenlik payı, istihdam indirimleriyle gelir vergisi ve yerel ek vergiler. Türkiye’den taşınan bir çalışana net teklif sunmadan önce kullanın.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Yıllık brüt maaş, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Şehir<select id="city"><option value="Milan">Milano</option><option value="Rome">Roma</option><option value="Turin">Torino</option><option value="Bologna">Bologna</option><option value="Naples">Napoli</option></select></label> <label for="inst">Ödeme<select id="inst"><option value="13">13 taksit</option><option value="14">14 taksit</option><option value="12">12 (13./14. maaş orantılı)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Kalem</th><th class="num">Yıllık</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="hesaplama-nasil-yapilir">Hesaplama nasıl yapılır?</h2> <ol> <li>Çalışan brüt ücret üzerinden %9,19 sosyal güvenlik payı öder; yıllık €56.224’ü aşan kısım için ayrıca %1.</li> <li>Kalan tutara gelir vergisi (IRPEF) uygulanır: €28.000’e kadar %23, €28.000–50.000 arası %33, üzeri %43.</li> <li>İstihdam vergi indirimleri vergiyi azaltır. Düşük maaşlarda en yüksektir ve €50.000’de sıfıra iner. €20.000 ile €40.000 arasında €1.000’a kadar ek indirim uygulanır.</li> <li>Gelir şehrin muafiyet eşiğinin altında değilse bölge ve belediye ek vergileri eklenir.</li> </ol> <p>Bunların hiçbiri işveren olarak ödediğinizi değiştirmez: <a href="maliyet-hesaplayici.html">işveren maliyet hesaplayıcısına</a> bakın.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardiya","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lazio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piyemonte","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"Emilia-Romagna","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Campania","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−€' : '€') + Math.round(Math.abs(n)).toLocaleString('en-US');
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Brüt maaş', g], ['Çalışan sosyal güvenlik payı', -inps], ['İndirimler sonrası gelir vergisi (IRPEF)', -irpef], [`Bölge ek vergisi (${c.region})`, -reg], ['Belediye ek vergisi', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Net maaş</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `${n} aylık bordronun her birinde yaklaşık ${eur(net / n)} net (brütün %${(net / g * 100).toFixed(0)}’i).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://italyadaistihdam.com/net-maas-hesaplayici.html
