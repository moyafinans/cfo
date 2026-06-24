---
name: html-page-builder
description: Finans Akademi için yeni HTML sayfası oluştur. Koyu tema, sidebar, topbar, progress bar, mobil hamburger menü yapısını otomatik uygular. Tasarım tutarlılığını garantiler.
---

# HTML Page Builder — Finans Akademi

## Tasarım Sistemi

### Renk Tokenları
```css
--bg:       #0d0f12   /* Ana arka plan */
--panel:    #15181d   /* Sidebar / kart */
--panel-2:  #1b1f25   /* İkincil panel */
--line:     #2a2f37   /* Border / çizgi */
--ink:      #e9e7e1   /* Ana metin */
--ink-dim:  #9aa0aa   /* İkincil metin */
--ink-faint:#5a6070   /* Soluk metin */
--gold:     #cf9b3f   /* Birincil vurgu */
--gold-soft:#e8c789   /* Yumuşak altın */
--teal:     #5fb6a4   /* Yeşil vurgu */
--red:      #d9684f   /* Hata / alarm */
--amber:    #d9a441   /* Uyarı */
--purple:   #8b7cf6   /* Analiz / özel */
```

### Fontlar
```html
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600;9..144,700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```
- **Başlıklar:** `font-family:'Fraunces',serif`
- **Metin:** `font-family:'Inter',sans-serif`
- **Kod/mono/etiket:** `font-family:'JetBrains Mono',monospace`

---

## Sayfa İskeleti (Her sayfada kullanılır)

```html
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[SAYFA ADI] — Finans Akademi</title>
<meta name="description" content="[AÇIKLAMA]">
<meta name="robots" content="index, follow">
<link rel="canonical" href="https://moyafinans.github.io/cfo/[DOSYA].html">
<meta property="og:title" content="[SAYFA ADI] — Finans Akademi">
<meta property="og:description" content="[AÇIKLAMA]">
<meta property="og:url" content="https://moyafinans.github.io/cfo/[DOSYA].html">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600;9..144,700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
/* BURAYA CSS */
</style>
</head>
<body>

<!-- ANA NAV -->
<nav>...</nav>

<!-- SHELL: sidebar + main -->
<div class="shell">
  <aside class="sidebar" id="sidebar">...</aside>
  <main class="main">
    <div class="topbar">...</div>
    <div class="content">...</div>
  </main>
</div>

<footer>...</footer>
<script>/* JS */</script>
</body>
</html>
```

---

## Ana Nav (Tüm sayfalarda aynı)

```html
<nav style="position:sticky;top:0;z-index:100;display:flex;align-items:center;justify-content:space-between;padding:0 40px;height:60px;background:rgba(13,15,18,.95);backdrop-filter:blur(10px);border-bottom:1px solid #2a2f37;">
  <a href="index.html" style="display:flex;align-items:center;gap:10px;text-decoration:none;">
    <div style="width:32px;height:32px;background:#cf9b3f;border-radius:6px;display:flex;align-items:center;justify-content:center;font-family:'Fraunces',serif;font-size:15px;font-weight:700;color:#16140d;">FA</div>
    <span style="font-size:15px;font-weight:600;color:#e9e7e1;">Finans Akademi</span>
  </a>
  <a href="index.html" style="font-size:13px;font-weight:500;color:#9aa0aa;text-decoration:none;">← Tüm Konular</a>
</nav>
```

---

## Shell Layout

```css
.shell { display: grid; grid-template-columns: 268px 1fr; min-height: 100vh; }
@media(max-width:860px) { .shell { grid-template-columns: 1fr; } }
```

---

## Sidebar

```html
<aside class="sidebar" id="sidebar">
  <div class="sb-brand" onclick="toggleSidebar()">
    <div>
      <div style="font-family:'JetBrains Mono',monospace;font-size:10px;color:#cf9b3f;letter-spacing:.16em;text-transform:uppercase;">[KATEGORİ]</div>
      <div style="font-family:'Fraunces',serif;font-weight:700;font-size:1.15rem;color:#e9e7e1;">[SAYFA ADI]</div>
      <div style="font-size:.75rem;color:#9aa0aa;">[KONU SAYISI] konu başlığı</div>
    </div>
    <button id="menu-icon" onclick="event.stopPropagation();toggleSidebar()" style="background:none;border:none;color:#cf9b3f;font-size:18px;cursor:pointer;">☰</button>
  </div>

  <!-- Nav grup örneği -->
  <div>
    <div style="font-family:'JetBrains Mono',monospace;font-size:10px;color:#9aa0aa;padding:12px 20px 6px;letter-spacing:.14em;text-transform:uppercase;">Konular</div>
    <button class="nav-item active" data-target="k1"><span class="num">01</span>Konu Adı</button>
    <button class="nav-item" data-target="k2"><span class="num">02</span>Konu Adı</button>
  </div>
</aside>
```

### Sidebar CSS
```css
.sidebar {
  background: #15181d;
  border-right: 1px solid #2a2f37;
  position: sticky; top: 60px;
  height: calc(100vh - 60px);
  overflow-y: auto; padding: 24px 0 40px;
}
.nav-item {
  display: flex; gap: 10px; align-items: center;
  width: 100%; text-align: left; background: none;
  border: none; border-left: 2px solid transparent;
  color: #9aa0aa; font-family: 'Inter', sans-serif;
  font-size: .85rem; padding: 8px 20px; cursor: pointer;
  transition: all .15s;
}
.nav-item .num { font-family: 'JetBrains Mono',monospace; font-size:.72rem; color:#cf9b3f; opacity:.7; }
.nav-item:hover { background: #1b1f25; color: #e9e7e1; }
.nav-item.active { background: #1b1f25; color: #e9e7e1; border-left-color: var(--accent-color, #cf9b3f); font-weight: 600; }
```

---

## Topbar (Breadcrumb + Progress)

```html
<div class="topbar">
  <div style="font-family:'JetBrains Mono',monospace;font-size:.72rem;color:#9aa0aa;">
    Konu <b id="bc-num" style="color:#e8c789;">—</b> / [TOPLAM] · <span id="bc-title">Genel Bakış</span>
  </div>
  <div style="width:140px;height:3px;background:#2a2f37;border-radius:2px;overflow:hidden;">
    <div id="progressFill" style="height:100%;background:#cf9b3f;width:0%;transition:width .25s;"></div>
  </div>
</div>
```

```css
.topbar {
  position: sticky; top: 60px; z-index: 90;
  display: flex; align-items: center; justify-content: space-between;
  padding: 12px 44px;
  background: rgba(13,15,18,.96); backdrop-filter: blur(6px);
  border-bottom: 1px solid #2a2f37;
}
```

---

## İçerik Bileşenleri

### Callout Kutusu
```html
<div class="callout">Normal bilgi notu metni.</div>
<div class="callout gold">Altın vurgulu önemli bilgi.</div>
<div class="callout teal">Yeşil vurgulu bilgi.</div>
```
```css
.callout { background:#15181d; border:1px solid #2a2f37; border-left:3px solid #cf9b3f; padding:14px 18px; margin:16px 0; font-size:.88rem; color:#cfd0d3; line-height:1.65; }
.callout.teal { border-left-color:#5fb6a4; }
.callout.gold { border-left-color:#cf9b3f; }
```

### Formül Kutusu
```html
<div class="formula">
  <span class="flabel">Formül Adı</span>
  SONUÇ = Değişken A / Değişken B × 100
</div>
```
```css
.formula { background:#15181d; border:1px solid #2a2f37; border-left:3px solid #5fb6a4; padding:14px 18px; font-family:'JetBrains Mono',monospace; font-size:.82rem; color:#e8c789; display:flex; flex-direction:column; gap:4px; margin:14px 0; }
.flabel { font-family:'Inter',sans-serif; font-size:.68rem; text-transform:uppercase; letter-spacing:.08em; color:#9aa0aa; }
```

### Tablo
```html
<table>
  <thead><tr><th>Başlık</th><th class="right">Sayı</th><th>Yorum</th></tr></thead>
  <tbody>
    <tr><td>Kalem</td><td class="right ok">1.500</td><td class="ok">İyi ✓</td></tr>
    <tr><td>Kalem</td><td class="right warn">800</td><td class="warn">Dikkat ⚠</td></tr>
    <tr><td>Kalem</td><td class="right bad">-200</td><td class="bad">Alarm ✗</td></tr>
  </tbody>
</table>
```
```css
table { width:100%; border-collapse:collapse; margin:16px 0; font-size:.82rem; background:#15181d; border:1px solid #2a2f37; }
th { background:#1b1f25; color:#e8c789; font-weight:600; font-size:.7rem; text-transform:uppercase; letter-spacing:.04em; padding:9px 12px; border-bottom:1px solid #2a2f37; text-align:left; }
td { padding:9px 12px; border-bottom:1px solid #2a2f37; color:#cfd0d3; vertical-align:top; }
tr:last-child td { border-bottom:none; }
td.ok { color:#5fb6a4; font-weight:600; }
td.warn { color:#d9a441; font-weight:600; }
td.bad { color:#d9684f; font-weight:600; }
td.right, th.right { text-align:right; font-family:'JetBrains Mono',monospace; }
```

### Tick List
```html
<ul class="tick">
  <li><b>Önemli nokta:</b> Açıklama metni buraya yazılır.</li>
  <li>Normal liste maddesi.</li>
</ul>
```
```css
ul.tick { list-style:none; margin:14px 0; padding:0; max-width:700px; }
ul.tick li { position:relative; padding:8px 0 8px 24px; color:#cfd0d3; font-size:.88rem; border-bottom:1px solid #2a2f37; line-height:1.6; }
ul.tick li:last-child { border-bottom:none; }
ul.tick li::before { content:'—'; position:absolute; left:0; color:#cf9b3f; font-family:'JetBrains Mono',monospace; }
ul.tick li b { color:#e9e7e1; }
```

### Grid 2 Kolon
```html
<div class="grid2">
  <div class="mini-card"><h4>Başlık</h4><p>Açıklama metni.</p></div>
  <div class="mini-card"><h4>Başlık</h4><p>Açıklama metni.</p></div>
</div>
```
```css
.grid2 { display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-top:16px; }
.mini-card { background:#15181d; border:1px solid #2a2f37; padding:16px; }
.mini-card h4 { font-family:'Fraunces',serif; font-size:1rem; color:#e8c789; margin-bottom:6px; }
.mini-card p { font-size:.82rem; color:#9aa0aa; }
@media(max-width:640px) { .grid2 { grid-template-columns:1fr; } }
```

### Takeaway Kutusu (Özet)
```html
<div class="takeaway">
  <h4>Temel Çıkarımlar</h4>
  <ul class="tick">
    <li>Madde 1</li>
    <li>Madde 2</li>
  </ul>
</div>
```
```css
.takeaway { margin-top:32px; background:#1b1f25; border:1px solid #2a2f37; border-top:2px solid #cf9b3f; padding:22px 24px; }
.takeaway h4 { font-family:'JetBrains Mono',monospace; font-size:.7rem; text-transform:uppercase; letter-spacing:.1em; color:#cf9b3f; margin-bottom:12px; }
```

---

## JavaScript Navigasyon Şablonu

```javascript
var order = ['overview','k1','k2','k3']; // tüm section id'leri
var titles = { overview:'Genel Bakış', k1:'Konu 1', k2:'Konu 2', k3:'Konu 3' };
var TOPLAM = 3; // içerik section sayısı (overview hariç)

function show(target) {
  document.getElementById('overview-panel').style.display = target === 'overview' ? 'block' : 'none';
  document.querySelectorAll('.section').forEach(function(s){
    s.classList.toggle('active', s.id === target);
  });
  document.querySelectorAll('.nav-item').forEach(function(b){
    b.classList.toggle('active', b.dataset.target === target);
  });
  var el = document.getElementById(target);
  document.getElementById('bc-num').textContent = el ? el.dataset.idx : '—';
  document.getElementById('bc-title').textContent = titles[target] || 'Genel Bakış';
  document.getElementById('progressFill').style.width = el ? (parseInt(el.dataset.idx)/TOPLAM*100)+'%' : '0%';
  window.scrollTo({top:0, behavior:'smooth'});
  renderArrows(target);
  if (window.innerWidth <= 860) {
    document.getElementById('sidebar').classList.remove('open');
    document.getElementById('menu-icon').textContent = '☰';
  }
}

function renderArrows(target) {
  document.querySelectorAll('.nav-arrows').forEach(function(e){ e.remove(); });
  if (target === 'overview') return;
  var idx = order.indexOf(target);
  var prev = order[idx-1]; var next = order[idx+1];
  var wrap = document.createElement('div'); wrap.className = 'nav-arrows';
  wrap.innerHTML =
    (prev ? '<button data-go="'+prev+'"><span>← Önceki</span>'+titles[prev]+'</button>'
           : '<button disabled><span>← Önceki</span>—</button>') +
    (next ? '<button class="next" data-go="'+next+'"><span>Sonraki →</span>'+titles[next]+'</button>'
           : '<button class="next" disabled><span>Sonraki →</span>—</button>');
  document.getElementById(target).appendChild(wrap);
  wrap.querySelectorAll('button[data-go]').forEach(function(b){
    b.addEventListener('click', function(){ show(b.dataset.go); });
  });
}

function toggleSidebar() {
  var sb = document.getElementById('sidebar');
  sb.classList.toggle('open');
  document.getElementById('menu-icon').textContent = sb.classList.contains('open') ? '✕' : '☰';
}

document.querySelectorAll('.nav-item, .toc-item').forEach(function(b){
  b.addEventListener('click', function(){ show(b.dataset.target); });
});

show('overview');
```

---

## Mobil CSS Kuralları

```css
@media(max-width:860px){
  .shell { grid-template-columns: 1fr; }
  .sidebar { position:relative; height:auto; border-right:none; border-bottom:1px solid #2a2f37; max-height:52px; overflow:hidden; transition:max-height .3s; }
  .sidebar.open { max-height:100vh; overflow-y:auto; }
  .sb-brand { display:flex; align-items:center; justify-content:space-between; padding:14px 18px; cursor:pointer; }
  .content, .overview, .toc { padding-left:18px; padding-right:18px; }
  .topbar { padding:10px 18px; }
  table { display:block; overflow-x:auto; -webkit-overflow-scrolling:touch; }
  .grid2 { grid-template-columns:1fr; }
  .nav-arrows { flex-direction:column; }
  .nav-arrows button { min-width:100%; }
}
```

---

## Footer (Tüm sayfalarda aynı)

```html
<footer style="padding:32px 40px;border-top:1px solid #2a2f37;text-align:center;">
  <div style="font-family:'Fraunces',serif;font-size:17px;font-weight:700;color:#e9e7e1;margin-bottom:6px;">
    Finans <span style="color:#cf9b3f">Akademi</span>
  </div>
  <div style="font-family:'JetBrains Mono',monospace;font-size:11px;color:#9aa0aa;">
    © 2026 · moyafinans.github.io/cfo
  </div>
</footer>
```

---

## Section HTML Şablonu

```html
<div class="section" id="k1" data-idx="1" data-title="Konu Adı">
  <!-- Konu başlığı -->
  <div style="display:flex;align-items:flex-start;gap:16px;margin-bottom:10px;">
    <div style="font-family:'Fraunces',serif;font-size:2.4rem;font-weight:700;color:#cf9b3f;opacity:.3;line-height:1;">01</div>
    <div>
      <h2 style="font-family:'Fraunces',serif;font-size:1.65rem;font-weight:700;color:#e9e7e1;margin-bottom:4px;">Konu Başlığı</h2>
      <p style="color:#9aa0aa;font-size:.88rem;">Alt başlık / özet cümle</p>
    </div>
  </div>

  <!-- İçerik blokları buraya -->
  <div class="block">
    <div class="block-label">Bölüm Adı</div>
    <p>İçerik metni.</p>
  </div>
</div>
```

```css
.section { padding:52px 0; border-bottom:1px solid #2a2f37; display:none; }
.section.active { display:block; animation:fadein .3s ease; }
@keyframes fadein { from{opacity:0;transform:translateY(6px)} to{opacity:1;transform:none} }
.block { margin-top:28px; }
.block-label { font-family:'JetBrains Mono',monospace; font-size:.72rem; letter-spacing:.12em; text-transform:uppercase; color:#5fb6a4; margin:0 0 12px; display:flex; align-items:center; gap:8px; }
.block-label::before { content:''; width:14px; height:1px; background:#5fb6a4; display:block; }
.block p { color:#cfd0d3; max-width:720px; font-size:.9rem; }
```
