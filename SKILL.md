---
name: on-page-scorer
description: >
  AI-powered content and on-page scoring system based on industry-standard
  methodologies (Surfer SEO, Clearscope, Semrush, theStacc framework).
  Analyzes pages against a target keyword across 9 weighted categories and
  returns a score-based prioritized action list. Supports single URL or
  batch URL analysis. Use when user says "on-page skor", "sayfa skorla",
  "on-page score", "content score", "skorlama", "sayfa analiz et ve skorla",
  or provides URL(s) with a keyword for scoring.
user-invokable: true
argument-hint: "<url> <keyword>"
license: MIT
metadata:
  author: inbound-seo
  version: "2.0.0"
  category: seo
  sources:
    - "theStacc Content Scoring Framework (2026)"
    - "Surfer SEO Content Score methodology"
    - "Semrush On-Page SEO Checklist (2026)"
    - "Google Helpful Content / E-E-A-T guidelines"
    - "Zyppy meta title research"
    - "Shopify/Upward Engine internal linking studies"
---

# On-Page SEO Skorlama Sistemi v2

Bu skill, verilen URL ve hedef keyword icin sayfanin icerik ve on-page
elementlerinin uyumunu otomatik kontrol eder ve skor bazli aksiyon listesi verir.

Skorlama metodolojisi su kaynaklardan turetilmistir:
- theStacc Content Scoring Framework (agirliklar ve esik degerleri)
- Surfer SEO / Clearscope (NLP ve content score benchmarklari)
- Semrush 2026 On-Page SEO Checklist (kontrol maddeleri)
- Google Helpful Content & E-E-A-T guidelines (kalite sinyalleri)
- Zyppy / Portent / Shopify arastirmalari (metrik esik degerleri)

## Giris Formati

Kullanici su sekillerde giris yapabilir:

**Tekli:** `/on-page-scorer https://example.com/page hedef-keyword`

**Toplu (birden fazla URL):** Kullanici birden fazla URL verirse her birini ayni keyword ile analiz et.

## Analiz Adimlari

### Adim 1 — Sayfayi Fetch Et (Cascade Fetch Stratejisi)

Sayfa icerigini almak icin asagidaki sirada dene. Bir yontem basarisiz
olursa (bot korumasi, JS-only rendering, timeout, bos icerik) bir
sonrakine gec. Kullaniciya hangi yontemle basarili oldugunu bildir.

**Yontem 1 — WebFetch (varsayilan, en hizli)**
WebFetch ile hedef URL'i cek. Cogu statik ve SSR sayfa icin calisir.
Basarisizlik isaretleri: "Pardon Our Interruption", "Enable JavaScript",
"Access Denied", "403 Forbidden", veya icerik <200 kelime geliyorsa.

**Yontem 2 — Google Cache**
WebFetch ile Google'in onbellege alinmis surumunu dene:
`https://webcache.googleusercontent.com/search?q=cache:[URL]`
Not: Google cache her zaman mevcut olmayabilir.

**Yontem 3 — Google AMP / Lite**
Bazi sayfalar AMP surumleri sunar. Dene:
- `[URL]/amp` veya `[URL]?amp=1`
- `[domain]/amp/[path]`

**Yontem 4 — Ahrefs Crawled Content**
Eger Ahrefs MCP bagli ise `mcp__ahrefs__site-audit-page-content` ile
Ahrefs'in crawl ettigi HTML icerigini al. Bu, JS-rendered sayfalari
bile kapsar cunku Ahrefs headless browser kullanir.

**Yontem 5 — Chrome ile Fetch (son care)**
Eger Chrome MCP (`mcp__Claude_in_Chrome__*`) bagli ise:
- `tabs_create_mcp` ile yeni tab ac
- `navigate` ile URL'ye git
- `get_page_text` ile sayfa metnini al
- `read_page` ile DOM yapisini al
Bu yontem JS-rendered sayfalari ve bot korumali siteleri handler.

**Basarisizlik durumu:**
Hicbir yontem calismiyorsa kullaniciya bildir:
"⚠️ Sayfa icerigi alinamadi. Site JS-only rendering ve agresif bot
korumasi kullaniyor. Manuel kontrol oneriyorum: [URL]"

Basarili fetch sonrasi su bilgileri cikar:
- Title tag, meta description, canonical, OG tags
- Tum heading'ler (H1-H6) icerikleriyle birlikte
- Body text (ilk 5000 karakter), word count
- Tum linkler (internal/external) ve anchor text'leri
- Tum gorseller ve alt text'leri
- CTA elementleri (buton, link)
- JSON-LD schema bloklari
- Paragraf ve cumle yapisi

### Adim 2 — 9 Kategori Skorla (her biri 0-100)

Her kategori icin asagidaki kurallara gore skor hesapla.
Toplam skor = agirlikli ortalama.

---

### Kategori 1: Keyword Coverage (Agirlik: %20)

Kaynak: theStacc (%25 keyword optimization), Semrush checklist, Surfer NLP

Kontrol edilecekler:
- **Title tag'de keyword var mi?** (20 puan)
  - Keyword ilk 30-35 karakterde mi? → tam 20p
  - Keyword title'da var ama basta degil? → 12p
  - Keyword title'da yok? → 0p
- **H1'de keyword var mi?** (20 puan)
  - H1'in basinda keyword → 20p
  - H1'de keyword var ama basta degil → 12p
  - H1'de keyword yok → 0p
- **Meta description'da keyword var mi?** (15 puan)
  - Keyword ilk 120 karakterde → 15p
  - Keyword var ama 120. karakterden sonra → 10p
  - Keyword yok → 0p
- **Ilk 100 kelimede keyword var mi?** (15 puan)
  - Keyword ilk cumlede → 15p
  - Keyword ilk 100 kelimede → 10p
  - Keyword ilk 100 kelimede yok → 0p
- **URL'de keyword var mi?** (10 puan)
- **Gorsel alt text'lerde keyword var mi?** (10 puan)
  - En az 1 gorsel alt text'inde → 10p
  - Hicbir gorselde yok → 0p
- **Keyword density sanity check** (10 puan)
  - %0.5-2 arasi → 10p (saglikli)
  - %0.1-0.5 veya %2-3 arasi → 5p (dusuk/biraz yuksek)
  - %0 veya %3+ → 0p (yok veya stuffing riski)

Not: Keyword density 2026'da dogrudan ranking faktoru degildir (Google
BERT/MUM/Gemini semantic coverage kullanir). Ama hala pratik bir sanity
check olarak degerlidir. Asiri yuksek density (%3+) uyarisi ver.

---

### Kategori 2: Content Depth & Topical Coverage (Agirlik: %20)

Kaynak: theStacc (%20 topical depth), Surfer (SERP average word count)

Kontrol edilecekler:
- **Word count** (30 puan)
  - 2500+ kelime = 30p (kapsamli)
  - 1500-2499 = 25p (iyi)
  - 1000-1499 = 18p (orta)
  - 500-999 = 10p (kisa)
  - <500 = 0p (thin content — Google Helpful Content riski)
- **H2 alt baslik sayisi** (20 puan)
  - 5+ = 20p, 3-4 = 15p, 1-2 = 8p, 0 = 0p
- **Paragraf yapisi** (15 puan)
  - 8+ anlamli paragraf (>20 kelime) = 15p
  - 5-7 = 10p
  - <5 = 5p
- **Gorsel ve medya** (15 puan)
  - 4+ gorsel = 15p, 2-3 = 10p, 1 = 5p, 0 = 0p
- **Schema (JSON-LD) var mi?** (10 puan)
  - Article/Product/LocalBusiness/Organization vb. = 10p
  - FAQ schema → uyari ver (2024'te sadece gov/health icin kisitlandi)
  - HowTo schema → uyari ver (deprecated)
  - Schema yok = 0p
- **E-E-A-T sinyalleri** (10 puan)
  - Yazar bilgisi (byline/bio) var mi? → 4p
  - Yayin/guncelleme tarihi var mi? → 3p
  - Kaynak/referans linkleri var mi? → 3p

---

### Kategori 3: Heading Structure (Agirlik: %10)

Kaynak: Semrush checklist, Google QRG, Bing AI search H1 usage

Kontrol edilecekler:
- **H1 sayisi tam 1 mi?** (25 puan)
  - 1 H1 = 25p
  - 0 H1 = 0p (kritik eksik)
  - 2+ H1 = 8p (birden fazla — duzeltilmeli)
- **H1'de keyword var mi?** (20 puan)
  - Keyword H1'in basinda → 20p
  - Keyword H1'de var → 15p
  - Keyword H1'de yok → 0p
- **H2'lerde keyword veya varyasyonu var mi?** (15 puan)
  - En az 1 H2'de keyword/varyasyon → 15p
  - H2 var ama keyword yok → 5p
  - H2 yok → 0p
- **Heading hiyerarsisi dogru mu?** (20 puan)
  - H1→H2→H3 sirayla, atlamasiz = 20p
  - Kucuk atlama (H2→H4 gibi) = 8p
  - Buyuk atlama veya ters siralama = 0p
- **Heading yogunlugu yeterli mi?** (20 puan)
  - Her 200-300 kelimede 1 heading = 20p (Semrush onerisi)
  - Her 300-500 kelimede 1 = 12p
  - Daha az = 5p

---

### Kategori 4: Meta Title (Agirlik: %10)

Kaynak: Zyppy title tag research, Semrush 2026 checklist

Kontrol edilecekler:
- **Uzunluk — pixel ve karakter** (25 puan)
  - 50-55 karakter (~580px) = 25p (optimal — %90 gorunum)
  - 45-60 karakter = 20p (guvenli aralik)
  - 30-45 veya 60-70 = 12p (kisaltilabilir)
  - <30 veya >70 = 5p (sorunlu)
  - 0 (title yok) = 0p (kritik)
- **Keyword pozisyonu** (30 puan)
  - Keyword ilk 30-35 karakterde = 30p (truncation'dan korunur)
  - Keyword title'da var ama 35+ char'da = 18p
  - Keyword title'da yok = 0p
- **Power word / CTR artirici** (15 puan)
  - Power word var (en iyi, nasil, rehber, guide, best, top, ultimate,
    complete, 2025, 2026, ucretsiz, free, yeni, new) = 15p
  - Sayi icerir (5, 10, 7 adim vb.) = 15p
  - Yok = 0p
- **Tekil mi?** (15 puan)
  - Title baska sayfalarda tekrar etmiyor (varsayilan evet) = 15p
- **OG Title tutarliligi** (15 puan)
  - OG Title = Title veya OG Title yok = 15p
  - OG Title farkli = 5p (tutarsizlik)

---

### Kategori 5: Meta Description (Agirlik: %10)

Kaynak: Semrush checklist, lettercounter.org 2026 guide

Kontrol edilecekler:
- **Uzunluk — mobile-first** (25 puan)
  - 120-155 karakter = 25p (hem mobile hem desktop guvenli)
  - 100-120 veya 155-170 = 18p (kabul edilebilir)
  - 50-100 = 10p (kisa)
  - >170 = 8p (kesilecek)
  - 0 (description yok) = 0p (kritik)
- **Keyword ilk 120 karakterde mi?** (25 puan)
  - Evet = 25p (mobile gorunum icinde)
  - Keyword var ama 120+ char'da = 12p
  - Keyword yok = 0p
- **CTA / aksiyon ifadesi var mi?** (20 puan)
  - CTA var (hemen, kesfet, incele, ogren, basvur, dene, discover,
    explore, learn, try, get, start) = 20p
  - Yok = 0p
- **Unique value proposition iceriyor mu?** (15 puan)
  - Fark yaratan bir ifade var (rakamlar, ucretsiz, garantili vb.) = 15p
  - Generic description = 5p
- **OG Description tutarliligi** (15 puan)
  - Tutarli veya OG Description yok = 15p
  - Farkli = 5p

---

### Kategori 6: Internal Linking (Agirlik: %8)

Kaynak: Shopify 2026 guide, Upward Engine, trafficthinktank

Kontrol edilecekler:
- **Link yogunlugu (word count'a oranli)** (30 puan)
  - 2-5 link / 1000 kelime = 30p (Shopify 2026 onerisi)
  - 1-2 link / 1000 kelime = 18p
  - <1 link / 1000 kelime = 8p
  - 0 internal link = 0p
- **Anchor text kalitesi** (25 puan)
  - Tum anchor'lar descriptive ve varied = 25p
  - Cogu descriptive, birkac generic = 15p
  - Cogu generic ("tikla", "buraya tikla", "click here", "devami",
    "read more", "here") = 5p
- **Link hedef cesitliligi** (20 puan)
  - %80+ unique hedef path = 20p
  - %60-80 unique = 12p
  - %60 alti = 5p
- **Toplam sayfa linki < 150 mi?** (10 puan)
  - < 150 = 10p
  - 150+ = 3p (cok fazla — link equity dagiliyor)
- **Contextual mi yoksa sadece nav/footer mi?** (15 puan)
  - Icerik icinde contextual linkler var = 15p
  - Sadece navigation/footer linkleri = 5p

---

### Kategori 7: CTA & Conversion Elements (Agirlik: %7)

Kaynak: WiserNotify CTA stats 2026, Unbounce studies, VWO

CTA pattern'leri (TR): satin al, hemen al, simdi al, siparis ver, ucretsiz dene,
basla, baslayalim, kaydol, kayit ol, teklif al, fiyat al, iletisime gec,
iletisim, kesfet, incele, daha fazla, hemen basvur, ucretsiz, demo al, dene

CTA pattern'leri (EN): buy now, get started, try free, sign up, subscribe,
download, order now, add to cart, contact us, learn more, apply now, get a quote,
start free trial, book a demo, request a demo

Kontrol edilecekler:
- **CTA elementi var mi?** (30 puan)
  - 2+ farkli CTA = 30p
  - 1 CTA = 18p
  - 0 CTA = 0p
- **CTA formati** (25 puan)
  - Buton/form olarak stilize edilmis CTA = 25p
  - Link olarak CTA var = 12p
  - CTA yok = 0p
- **CTA konum tahmini** (20 puan)
  - CTA sayfanin ust kisimlarinda (ilk 2 paragraf veya header'da) = 20p
  - CTA sadece alt kisimda = 10p
  - CTA yok = 0p
  (Not: Above-the-fold kesin olarak belirlenemez HTML'den, ama konum
  tahmin edilebilir)
- **CTA mesaj cesitliligi** (15 puan)
  - 2+ farkli CTA mesaji = 15p
  - Tek tip CTA = 8p
- **Urgency/scarcity ifadesi** (10 puan)
  - Sinirli sure, son X adet, bugun, simdi gibi ifadeler = 10p
  - Yok = 0p (zorunlu degil, bonus)

---

### Kategori 8: Readability & UX (Agirlik: %8)

Kaynak: Portent readability study, Flesch-Kincaid research, theStacc (%15)

Kontrol edilecekler:
- **Ortalama cumle uzunlugu** (25 puan)
  - <=15 kelime = 25p (theStacc onerisi)
  - 15-20 kelime = 20p (iyi)
  - 20-25 kelime = 12p (uzun)
  - >25 kelime = 5p (cok uzun)
- **Paragraf uzunlugu** (20 puan)
  - Ort. <=80 kelime = 20p
  - 80-120 = 12p
  - >120 = 5p
- **Scanability elementleri** (20 puan)
  - Bullet/numbered list var = 8p
  - Bold/italic vurgular var = 6p
  - Tablolar var = 6p
  (Toplam max 20p)
- **Heading yogunlugu** (15 puan)
  - Her 200-300 kelimede 1 heading = 15p
  - Her 300-500 kelimede 1 = 8p
  - Daha seyrek = 3p
- **Gorsel dagilimi** (10 puan)
  - Her 300-500 kelimede 1+ gorsel = 10p
  - Daha seyrek = 5p
  - Gorsel yok = 0p
- **Mobile uyumluluk sinyalleri** (10 puan)
  - Viewport meta tag var = 5p
  - Responsive ipuclari (srcset, picture element vb.) = 5p

---

### Kategori 9: AI Content Quality (Agirlik: %7)

Kaynak: Google Helpful Content guidelines, AI detection methodology
(burstiness + perplexity), Wellows 2026 AI content scoring

Bu kategoriyi SEN (Claude) degerlendir. Sayfanin icerigini oku ve su
kriterlere gore 0-100 arasi skor ver:

- **Dogallik / Burstiness** (25 puan)
  Cumle uzunluklari ve yapilari cesitli mi? Monoton bir yapi mi var?
  Yuksek burstiness = insan yazimi sinyali = iyi.
  - Yuksek cesitlilik = 25p
  - Orta = 15p
  - Monoton/kalip = 5p

- **Ozgunluk ve katma deger** (25 puan)
  Icerik ozgun insight, veri, ornek veya deneyim sunuyor mu?
  Yoksa her yerde bulunabilecek generic bilgi mi?
  - Ozgun insight/veri/ornek var = 25p
  - Kismen ozgun = 15p
  - Tamamen generic = 5p

- **Keyword dogalligi** (20 puan)
  Keyword zoraki yerlestirilmis mi yoksa dogal akis icinde mi?
  Google 2025: "AI content can rank if helpful and reviewed by human."
  - Tamamen dogal = 20p
  - Hafif zoraki = 12p
  - Acik stuffing = 0p

- **Search intent karsilama** (15 puan)
  Icerik, bu keyword'u arayan kisinin ihtiyacini GERCEKTEN karsilar mi?
  - Tam karsilar = 15p
  - Kismen = 8p
  - Karsilamiyor = 0p

- **Actionable / uygulanabilir mi?** (15 puan)
  Okuyucu bu icerikten somut bir adim/bilgi/deger cikarabilir mi?
  - Somut adimlar/tavsiyeler var = 15p
  - Kismen = 8p
  - Tamamen teorik/yuzeysel = 0p

---

## Adim 3 — Toplam Skor Hesapla

```
Toplam = (Keyword Coverage     * 0.20)
       + (Content Depth        * 0.20)
       + (Heading Structure    * 0.10)
       + (Meta Title           * 0.10)
       + (Meta Description     * 0.10)
       + (Internal Linking     * 0.08)
       + (CTA                  * 0.07)
       + (Readability          * 0.08)
       + (AI Content Quality   * 0.07)
```

Agirlik mantigi (theStacc + industry benchmark):
- Keyword + Content = %40 (en buyuk etki — Surfer ve Clearscope da bunu agirlikli tutar)
- Headings + Title + Desc = %30 (on-page HTML elementleri)
- Linking + CTA + Readability + AI = %30 (UX ve kalite sinyalleri)

## Skor Araliklari ve Aksiyon Esikleri

Kaynak: theStacc action thresholds

| Skor | Durum | Renk | Aksiyon |
|------|-------|------|---------|
| 80-100 | Mukemmel | Yesil | Uc ayda bir gozden gecir |
| 60-79 | Iyi / Optimize edilebilir | Sari | 30 gun icinde optimize et |
| 40-59 | Zayif / Yeniden yazilmali | Turuncu | 14 gun icinde yeniden yaz veya birlestir |
| 0-39 | Kritik | Kirmizi | Acil mudahale — redirect veya tamamen yeniden olustur |

## Cikti Formati

### ZORUNLU CIKTI FORMATI — Bu formattan sapma!

```
═══════════════════════════════════════════════════
  ON-PAGE SEO SKOR RAPORU v2
═══════════════════════════════════════════════════

URL:      [analiz edilen URL]
Keyword:  [hedef keyword]
Tarih:    [bugunun tarihi]

───────────────────────────────────────────────────
  TOPLAM SKOR: XX/100  [DURUM]
───────────────────────────────────────────────────

  Keyword Coverage (%20):    XX/100  ████████░░
  Content Depth (%20):       XX/100  ██████████
  Heading Structure (%10):   XX/100  ███████░░░
  Meta Title (%10):          XX/100  █████░░░░░
  Meta Description (%10):    XX/100  ████████░░
  Internal Linking (%8):     XX/100  ██████░░░░
  CTA (%7):                  XX/100  ████░░░░░░
  Readability (%8):          XX/100  █████████░
  AI Content Quality (%7):   XX/100  ███████░░░

───────────────────────────────────────────────────
  DETAYLI ANALIZ
───────────────────────────────────────────────────

### 1. Keyword Coverage — XX/100 (Agirlik: %20)
[Her kontrol maddesini listele: ✓ gecti / ✗ gecmedi / ~ kismen]
- Title'da keyword: [metin] ✓/✗
- H1'de keyword: [metin] ✓/✗
- Meta desc'de keyword (ilk 120 char): ✓/✗
- Ilk 100 kelimede keyword: ✓/✗
- URL'de keyword: ✓/✗
- Gorsel alt text'de keyword: ✓/✗
- Keyword density: %X.X ✓/✗
💡 Aksiyon: [somut, uygulanabilir Turkce oneri]

### 2. Content Depth — XX/100 (Agirlik: %20)
- Word count: X kelime ✓/✗
- H2 sayisi: X ✓/✗
- Paragraf sayisi: X ✓/✗
- Gorsel sayisi: X ✓/✗
- Schema: [tur veya yok] ✓/✗
- E-E-A-T: yazar bilgisi ✓/✗, tarih ✓/✗, kaynaklar ✓/✗
💡 Aksiyon: [somut oneri]

[...tum 9 kategori ayni formatta...]

───────────────────────────────────────────────────
  ONCELIKLI AKSIYON LISTESI
───────────────────────────────────────────────────

Dusuk skordan yuksege, agirlik etkisiyle sirali:

1. 🔴 [Kategori] (XX/100, agirlik %XX) → [aksiyon]
2. 🟠 [Kategori] (XX/100, agirlik %XX) → [aksiyon]
3. 🟡 [Kategori] (XX/100, agirlik %XX) → [aksiyon]

Renk kodlari: 🔴 0-39 | 🟠 40-59 | 🟡 60-79 | 🟢 80-100

Sadece 80 altindaki kategoriler listelenir.
80+ alan kategoriler icin: "✓ [Kategori] iyi durumda" yaz.

───────────────────────────────────────────────────
  METODOLOJI NOTU
───────────────────────────────────────────────────
Skorlama: theStacc Content Scoring Framework, Surfer SEO,
Semrush 2026 Checklist, Google E-E-A-T guidelines.
Detaylar: RESEARCH.md
```

## Toplu Analiz

Birden fazla URL verildiginde:
1. Her URL'i ayni keyword ile analiz et
2. Sonunda karsilastirma tablosu goster:

```
───────────────────────────────────────────────────
  KARSILASTIRMA TABLOSU
───────────────────────────────────────────────────

| URL | Toplam | KW(%20) | Depth(%20) | Hx(%10) | Title(%10) | Desc(%10) | Link(%8) | CTA(%7) | Read(%8) | AI(%7) |
|-----|--------|---------|------------|---------|------------|-----------|----------|---------|----------|--------|
| /p1 |   72   |   85    |    60      |   70    |    80      |    65     |    55    |   40    |    75    |   80   |
| /p2 |   58   |   45    |    70      |   50    |    60      |    40     |    65    |   30    |    60    |   65   |

Ortalama Skorlar:
[Tum URL'lerin kategori ortalamasi]

En Kritik Aksiyonlar (tum sayfalar icin):
1. 🔴 ...
2. 🟠 ...
3. 🟡 ...
```

## Onemli Kurallar

- Skor hesaplamasini YUKARIDAKI kurallara gore yap, tahmin etme
- Her kontrol maddesi icin ✓ veya ✗ veya ~ isle, gercek degeri goster
- Aksiyon onerileri TURKCE ve SOMUT olsun — "sunu su sekilde degistir" formatinda
- WebFetch ile sayfayi fetch edemezsen kullaniciya hatayi bildir
- Keyword karsilastirmasi buyuk-kucuk harf duyarsiz yapilmali
- FAQ schema onerme (gov/health haric kisitli), HowTo schema onerme (deprecated)
- Keyword density'yi ranking faktoru olarak sunma, sanity check olarak kullan
- Toplu analizde en fazla 10 URL kabul et (API limit korumasi)
- Progress bar'lari tam skorla eslestir: 75/100 = ████████░░ (8 dolu, 2 bos)
