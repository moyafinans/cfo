---
name: pdf-to-html
description: Finans Akademi için PDF içeriğini HTML sayfaya dönüştür. Koyu tema, sidebar navigasyon, formüller, tablolar ve callout bloklarıyla eksiksiz sayfa üret. html-page-builder skill'i ile birlikte çalışır.
---

# PDF to HTML — Finans Akademi

## Genel İlkeler

- PDF'deki her ana başlık → bir `section` (konu) olur
- Her section `data-idx` ve `data-title` alır
- İçerik; bloklar, tablolar, formüller, tick listleri ile yapılandırılır
- Takeaway kutusu her zaman son section'ın sonuna eklenir
- Mobil uyumluluk zorunludur

---

## PDF'den HTML'e Dönüşüm Adımları

### 1. İçeriği Analiz Et
```
PDF okunduğunda şunları tespit et:
- Kaç ana başlık/konu var? → section sayısı
- Her konuda ne var? (tablo, formül, liste, metin)
- Özet/takeaway var mı?
- Yazar bilgisi, kaynak var mı?
```

### 2. Sidebar Navigation Oluştur
```html
<!-- Her konu için bir nav-item -->
<button class="nav-item" data-target="k1"><span class="num">01</span>Konu Adı</button>
<button class="nav-item" data-target="k2"><span class="num">02</span>Konu Adı</button>
```

### 3. TOC (İçindekiler) Oluştur
```html
<ul class="toc-list">
  <li class="toc-item" data-target="k1">
    <span class="toc-num">01</span>
    <span class="toc-title">Konu Adı — Alt başlık</span>
  </li>
</ul>
```

### 4. Her Section'ı Oluştur
```html
<div class="section" id="k1" data-idx="1" data-title="Konu Adı">
  <!-- Konu başlık bloğu -->
  <!-- İçerik blokları -->
  <!-- Varsa takeaway -->
</div>
```

---

## İçerik Türü → HTML Bileşeni Eşlemesi

| PDF'de Ne Var | HTML'de Ne Kullan |
|---|---|
| Tanım / açıklama metni | `<div class="block"><div class="block-label">...</div><p>...</p></div>` |
| Formül / hesaplama | `<div class="formula"><span class="flabel">...</span>...</div>` |
| Karşılaştırma tablosu | `<table>` + `td.ok` / `td.warn` / `td.bad` |
| Madde listesi | `<ul class="tick"><li>...</li></ul>` |
| Önemli uyarı | `<div class="callout">...</div>` |
| İki kolonlu içerik | `<div class="grid2"><div class="mini-card">...</div></div>` |
| Bölüm özeti | `<div class="takeaway"><h4>Temel Çıkarımlar</h4>...</div>` |

---

## Tablo Kuralları

### Temel Tablo
```html
<table>
  <caption>Tablo başlığı (opsiyonel)</caption>
  <thead>
    <tr>
      <th>Sütun 1</th>
      <th>Sütun 2</th>
      <th class="right">Sayısal</th>
      <th>Yorum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>İçerik</td>
      <td>İçerik</td>
      <td class="right">1.500</td>
      <td class="ok">Olumlu ✓</td>
    </tr>
    <tr>
      <td>İçerik</td>
      <td>İçerik</td>
      <td class="right warn">800</td>
      <td class="warn">Dikkat ⚠</td>
    </tr>
    <tr>
      <td>İçerik</td>
      <td>İçerik</td>
      <td class="right bad">-200</td>
      <td class="bad">Alarm ✗</td>
    </tr>
  </tbody>
</table>
```

### Renk Kodları
- `class="ok"` → yeşil (`#5fb6a4`) — olumlu sinyal
- `class="warn"` → sarı (`#d9a441`) — dikkat gerektiren
- `class="bad"` → kırmızı (`#d9684f`) — alarm / tehlike
- `class="right"` → sağa hizalı, mono font (sayısal değerler için)
- `class="center"` → ortaya hizalı

### Duyarlılık Tablosu (2 boyutlu)
```html
<div style="overflow-x:auto;">
  <table>
    <thead>
      <tr>
        <th>Değişken A \ B</th>
        <th class="center">%10</th>
        <th class="center">%15 (Baz)</th>
        <th class="center">%20</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>%5</td>
        <td class="center">1.200</td>
        <td class="center">1.400</td>
        <td class="center">1.600</td>
      </tr>
      <tr>
        <td><b>%10 (Baz)</b></td>
        <td class="center">1.350</td>
        <td class="center" style="background:rgba(207,155,63,.15);color:#e8c789;font-weight:700;">1.575</td>
        <td class="center">1.800</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## Formül Kuralları

```html
<!-- Tek satır formül -->
<div class="formula">
  <span class="flabel">Net Kâr Marjı</span>
  Net Kâr / Net Satışlar × 100
</div>

<!-- Çok satır / ayrıntılı formül -->
<div class="formula">
  <span class="flabel">DuPont — 3 Bileşenli</span>
  ROE = Net Kâr Marjı × Varlık Devir Hızı × Finansal Kaldıraç
  <span style="font-family:'Inter',sans-serif;font-size:.75rem;color:#9aa0aa;margin-top:4px;">
    Her bileşen bağımsız iyileştirilebilir.
  </span>
</div>
```

---

## Callout Kuralları

```html
<!-- Genel bilgi / dikkat -->
<div class="callout">
  Önemli bir bilgi veya uyarı metni buraya yazılır.
</div>

<!-- Altın vurgu — temel kural -->
<div class="callout" style="border-left-color:#cf9b3f;">
  Bu içerik özellikle önemli bir ilkeyi vurgular.
</div>

<!-- Yeşil vurgu — olumlu -->
<div class="callout" style="border-left-color:#5fb6a4;">
  Olumlu bir bulgu veya iyi uygulama örneği.
</div>

<!-- Kırmızı vurgu — alarm -->
<div class="callout" style="border-left-color:#d9684f;">
  Kritik bir uyarı veya risk sinyali.
</div>
```

---

## Overview Sayfası Şablonu

```html
<div id="overview-panel">
  <div style="padding:56px 44px 40px;max-width:900px;">
    <!-- Eyebrow -->
    <div style="font-family:'JetBrains Mono',monospace;color:#cf9b3f;font-size:.76rem;letter-spacing:.18em;text-transform:uppercase;margin-bottom:14px;">
      [KATEGORİ ADI]
    </div>
    <!-- Ana başlık -->
    <h1 style="font-family:'Fraunces',serif;font-size:clamp(1.8rem,4vw,3rem);font-weight:700;line-height:1.05;margin-bottom:14px;color:#e9e7e1;letter-spacing:-.5px;">
      [PDF'DEN ALINTI ETKİLEYİCİ BAŞLIK]
    </h1>
    <!-- Açıklama -->
    <p style="color:#9aa0aa;max-width:540px;font-size:.95rem;line-height:1.7;">
      [PDF içeriğinin 2-3 cümlelik özeti]
    </p>
    <!-- İstatistikler -->
    <div style="display:flex;gap:36px;margin-top:28px;flex-wrap:wrap;">
      <div>
        <b style="display:block;font-family:'Fraunces',serif;font-size:1.7rem;color:#e8c789;">[N]</b>
        <span style="font-size:.75rem;color:#9aa0aa;">Konu başlığı</span>
      </div>
      <div>
        <b style="display:block;font-family:'Fraunces',serif;font-size:1.7rem;color:#e8c789;">[N]+</b>
        <span style="font-size:.75rem;color:#9aa0aa;">Formül & tablo</span>
      </div>
    </div>
    <div style="height:1px;background:#2a2f37;margin:36px 0 0;"></div>
  </div>

  <!-- TOC -->
  <div style="padding:32px 44px 48px;max-width:900px;">
    <h3 style="font-family:'Fraunces',serif;font-size:1.2rem;margin-bottom:16px;color:#e9e7e1;">İçindekiler</h3>
    <ul class="toc-list">
      <li class="toc-item" data-target="k1">
        <span class="toc-num">01</span>
        <span class="toc-title">Konu 1 — Alt başlık</span>
      </li>
      <!-- devam... -->
    </ul>
  </div>
</div>
```

---

## Meta Tag Şablonu

```html
<meta name="description" content="[Sayfanın 1-2 cümlelik açıklaması. Anahtar kelimeler doğal olarak geçmeli.]">
<meta name="robots" content="index, follow">
<link rel="canonical" href="https://moyafinans.github.io/cfo/[dosya-adi].html">
<meta property="og:title" content="[Sayfa Başlığı] — Finans Akademi">
<meta property="og:description" content="[Açıklama]">
<meta property="og:url" content="https://moyafinans.github.io/cfo/[dosya-adi].html">
```

---

## Kontrol Listesi — HTML Teslim Öncesi

- [ ] Tüm section'lar `id`, `data-idx`, `data-title` içeriyor mu?
- [ ] `order` array'i sidebar sırasıyla eşleşiyor mu?
- [ ] `titles` objesi tüm id'leri kapsıyor mu?
- [ ] `TOPLAM` değişkeni doğru mu?
- [ ] Tablo hücreleri renk kodlandı mı? (ok/warn/bad)
- [ ] Mobil CSS ekli mi?
- [ ] Footer kaynağı varsa eklendi mi?
- [ ] Meta taglar eksiksiz mi?
- [ ] Sayfa adı `index.html`'deki kartla tutarlı mı?

---

## index.html Kart Şablonu (Yeni Sayfa Eklenince)

```html
<a href="[dosya-adi].html" class="card" style="--card-color:[RENK];">
  <div class="card-top">
    <div class="card-icon">[EMOJİ]</div>
    <span class="card-badge badge-live">Yayında</span>
  </div>
  <div class="card-title">[Sayfa Başlığı]</div>
  <div class="card-desc">[2 cümle açıklama]</div>
  <div class="card-footer">
    <span class="card-meta">[N] konu · [özellik]</span>
    <span class="card-arrow">→</span>
  </div>
</a>
```

### Renk Önerileri
- Kredi / Risk → `#5fb6a4` (teal)
- Bütçe / Planlama → `#cf9b3f` (gold)
- Finansal Analiz → `#8b7cf6` (purple)
- Raporlama → `#d9a441` (amber)
- Vergi / Mevzuat → `#d9684f` (red)
- Hazine → `#5fb6a4` (teal)
- Yatırım → `#8b7cf6` (purple)
