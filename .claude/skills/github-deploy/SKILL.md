---
name: github-deploy
description: Dosyaları GitHub API üzerinden moyafinans/cfo reposuna yükle, güncelle veya sil. GitHub Pages'e deploy et. Token, SHA yönetimi ve conflict önleme dahil.
---

# GitHub Deploy — Finans Akademi

## Proje Bilgileri

| Bilgi | Değer |
|---|---|
| Repo | moyafinans/cfo |
| Branch | main |
| GitHub Pages URL | https://moyafinans.github.io/cfo/ |
| Token | [GITHUB_TOKEN_BURAYA] |

> Token çalışmıyorsa yenilenmiş olabilir — kullanıcıdan yeni token iste.

---

## Temel Akış — Her Yüklemede Uygulanır

```bash
TOKEN="[GITHUB_TOKEN_BURAYA]"
REPO="moyafinans/cfo"

# 1. Dosyayı base64'e çevir
CONTENT=$(base64 -w 0 /path/to/file.html)

# 2. Mevcut SHA'yı al (dosya varsa güncelleme için zorunlu)
SHA=$(curl -s -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/$REPO/contents/dosya.html" | \
  python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('sha',''))" 2>/dev/null)

# 3. Payload oluştur (SHA varsa ekle)
if [ -n "$SHA" ]; then
  PAYLOAD="{\"message\":\"commit mesajı\",\"content\":\"$CONTENT\",\"sha\":\"$SHA\"}"
else
  PAYLOAD="{\"message\":\"commit mesajı\",\"content\":\"$CONTENT\"}"
fi

# 4. PUT ile yükle
curl -s -X PUT "https://api.github.com/repos/$REPO/contents/dosya.html" \
  -H "Authorization: token $TOKEN" \
  -H "Content-Type: application/json" \
  -d "$PAYLOAD"
```

---

## Çoklu Dosya Yükleme Fonksiyonu

```bash
TOKEN="[GITHUB_TOKEN_BURAYA]"
REPO="moyafinans/cfo"

upload() {
  local file=$1 path=$2 msg=$3
  CONTENT=$(base64 -w 0 "$file")
  SHA=$(curl -s -H "Authorization: token $TOKEN" \
    "https://api.github.com/repos/$REPO/contents/$path" | \
    python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('sha',''))" 2>/dev/null)
  if [ -n "$SHA" ]; then
    PAYLOAD="{\"message\":\"$msg\",\"content\":\"$CONTENT\",\"sha\":\"$SHA\"}"
  else
    PAYLOAD="{\"message\":\"$msg\",\"content\":\"$CONTENT\"}"
  fi
  RESULT=$(curl -s -X PUT "https://api.github.com/repos/$REPO/contents/$path" \
    -H "Authorization: token $TOKEN" -H "Content-Type: application/json" -d "$PAYLOAD")
  echo "$path → $(echo $RESULT | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('content',{}).get('name','') or d.get('message',''))" 2>/dev/null)"
}

# Kullanım:
upload /tmp/index.html "index.html" "Ana sayfa güncellendi"
upload /tmp/yeni-sayfa.html "yeni-sayfa.html" "Yeni sayfa eklendi"
```

---

## Repo Dosyalarını Listeleme

```bash
TOKEN="[GITHUB_TOKEN_BURAYA]"
curl -s -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/moyafinans/cfo/contents/" | \
  python3 -c "
import sys,json
for f in json.load(sys.stdin):
    print(f['name'], '—', f['size'], 'byte')
"
```

---

## Mevcut Dosyayı Okuma (SHA almak için)

```bash
TOKEN="[GITHUB_TOKEN_BURAYA]"
curl -s -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/moyafinans/cfo/contents/index.html" | \
  python3 -c "
import sys,json,base64
d=json.load(sys.stdin)
print('SHA:', d['sha'])
open('/tmp/mevcut.html','w').write(base64.b64decode(d['content']).decode())
print('Dosya /tmp/mevcut.html olarak kaydedildi')
"
```

---

## Kritik Kurallar

- ⚠️ **SHA olmadan PUT → 409 Conflict hatası.** Her güncellemede SHA alınmalı.
- ⚠️ **index.html çok sık güncellenir** — SHA'sını her seferinde taze çek, hardcode etme.
- ✅ Yeni dosya yüklerken SHA gerekmez, boş bırak.
- ✅ Yükleme sonrası GitHub Pages 1-2 dakika içinde güncellenir.
- ✅ Başarılı yüklemede response'da `content.name` dolu gelir.
- ❌ Hata durumunda `message` alanı dolu gelir — logla ve tekrar dene.

---

## Yeni Sayfa Eklenince Yapılacaklar

1. `yeni-sayfa.html` → GitHub'a yükle
2. `index.html` → ilgili kartı `disabled` kaldır, `<div>` → `<a href="yeni-sayfa.html">` yap, badge'i "Yayında" yap
3. `sitemap.xml` → yeni URL'yi ekle
4. `llms.txt` → yeni sayfayı listele
5. Hepsini tek `upload()` fonksiyonuyla yükle

---

## Mevcut Repo Dosyaları

| Dosya | Açıklama |
|---|---|
| index.html | Ana portal sayfası |
| kredi-analizi.html | Kredi Analizi (78 senaryo) |
| butce.html | Bütçe Defteri (15 konu) |
| finansal-analiz.html | Finansal Analiz Teknikleri (6 konu) |
| iletisim.html | İletişim formu |
| admin.html | Admin paneli (şifre: finans2026) |
| fa-system.css | Design system CSS |
| robots.txt | SEO bot izinleri |
| sitemap.xml | Google sitemap |
| llms.txt | AI indeksleme |
