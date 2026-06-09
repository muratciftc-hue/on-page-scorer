# On-Page SEO Skorlama — Araştırma Notları

Tarih: 2026-06-09

## 1. Endüstri Standartları — Kategori Ağırlıkları

### BrightSEO Tools Metodolojisi
| Kategori | Ağırlık |
|----------|---------|
| Technical SEO | %25-30 |
| On-Page SEO | %20-25 |
| Content Quality | %15-20 |
| User Experience | %15-20 |
| Backlink Profile | %10-15 |
| Site Architecture | %10-15 |
| Social Signals | %5-10 |

Kaynak: https://brightseotools.com/post/How-SEO-Score-is-Calculated-The-Complete-Breakdown

### theStacc Content Scoring Framework
SEO odaklı strateji için:
| Kriter | Ağırlık |
|--------|---------|
| Keyword optimization | %25 |
| Topical depth | %20 |
| Search intent alignment | %20 |
| Readability | %15 |
| Internal links | %10 |
| E-E-A-T signals | %10 |

Değerlendirme ölçeği: 0-5 puan, ağırlıklarla çarpılıp 0-100'e normalize.

Aksiyon eşikleri:
- 80-100: Maintain (üç ayda bir gözden geçir)
- 60-79: Optimize/update (30 gün)
- 40-59: Rewrite/merge (14 gün)
- 0-39: Delete/redirect (acil)

Kaynak: https://thestacc.com/blog/content-scoring-guide/

### Surfer SEO Content Score
- 0-33: Düşük kalite
- 34-66: Kısmen optimize
- 67-100: Optimum kalite
- Hedef: rakiplerden 10-20 puan yukarı
- Ağırlık: NLP terimleri ve doğal kullanım > yapısal metrikler
- "Content Score of 100 is NOT the goal" — aşırı optimizasyon zararlı

Kaynak: https://docs.surferseo.com/en/articles/5700317-what-is-content-score

## 2. Spesifik Metrik Eşikleri (2026 Güncel)

### Meta Title
- Karakter: 50-60 (ideal), 580px pixel genişliği
- Mobilde 70-80 karakter gösterilebilir
- Keyword ilk 30-35 karakterde olmalı (truncation koruması)
- %90 oranında tam görünüm için 55 karakter hedefle
- Kaynak: https://zyppy.com/title-tags/meta-title-tag-length/, https://www.scalenut.com/blogs/meta-title-length-best-practices-2026

### Meta Description
- Desktop: 158 karakter / 920px
- Mobil: 120 karakter / 680px
- Optimal: 120-158 karakter arası
- Önemli bilgi ilk 120 karakterde olmalı (mobile-first)
- Semrush 2026 checklist: ~105 karakter (konservatif)
- Kaynak: https://lettercounter.org/blog/meta-description-length-seo-guide/, https://www.semrush.com/blog/on-page-seo-checklist/

### Keyword Density
- İdeal aralık: %0.5-2 (sanity check olarak)
- Google keyword density'yi ranking faktörü olarak KULLANMIYOR
- Modern yaklaşım: frequency-based değil context-based
- BERT, MUM, Gemini ile semantic coverage değerlendiriliyor
- Aşırı yüksek density (%7+) zararlı (keyword stuffing)
- Kaynak: https://www.rankability.com/ranking-factors/google/keyword-density/

### Internal Linking
- 2-5 contextual link / 1000 kelime
- 5-10 link / 2000 kelime (uzun içerik)
- ~1 link / 200-300 kelime
- Toplam sayfa linkleri < 150
- Anchor text: descriptive, varied, doğal
- Generic anchor ("tıkla", "click here") kullanma
- Önemli sayfalar 3 tıklama içinde erişilebilir olmalı
- Kaynak: https://www.shopify.com/blog/internal-links-seo, https://upwardengine.com/internal-linking-best-practices-seo/

### Readability
- Flesch Reading Ease: 60-70 arası hedef
- Flesch-Kincaid Grade Level: 7-8 (genel kitle için)
- Cümle uzunluğu: 15 kelime altı (theStacc önerisi)
- Readability doğrudan ranking faktörü DEĞİL
- Ama UX etkisi var → bounce rate → dolaylı ranking etkisi
- Kaynak: https://portent.com/blog/content/study-how-content-readability-affects-seo-and-rankings.htm

### CTA
- Above the fold CTA: below the fold'a göre %304 daha iyi performans
- Tek CTA: conversion'ı %266 artırır
- Büyük buton: tıklamaları %90 artırır
- Urgency CTA: conversion'ı %332 artırır
- Mobil CTA optimizasyonu: %32.5 iyileşme
- Kaynak: https://wisernotify.com/blog/call-to-action-stats/

### Content Depth / Word Count
- SERP ortalamasıyla eşleşmeli (mutlak sayı değil, rakibe göre)
- theStacc: 8 birincil faktör arasında "Content Length: Matching SERP average word count"
- Kaynak: https://thestacc.com/blog/content-scoring-guide/

### AI Content Score
- Google AI içeriği ranking'den düşürmez (Mayıs 2025 güncellemesi)
- Koşul: doğru, faydalı, insan tarafından gözden geçirilmiş
- AI detection: burstiness (cümle yapısı varyasyonu) + perplexity (tahmin edilebilirlik)
- Kontrol: özgün insight, gerçek örnekler, actionable tavsiye, kaynak atıfı
- Kaynak: https://wellows.com/blog/how-to-use-ai-content-scoring/

## 3. E-E-A-T (2026)
- Experience, Expertise, Authoritativeness, Trustworthiness
- Doğrudan ranking faktörü DEĞİL (Google'ın resmi pozisyonu)
- Ama ~%8 ranking ağırlığıyla korelasyon gösteriyor
- YMYL konularında kritik önem
- Eylül 2025 QRG güncellemesi: AI Overviews değerlendirme bölümü eklendi
- Kaynak: https://linkbuilder.com/blog/google-eeat-guide

## 4. Heading Yapısı (2026)
- Tek H1, primary keyword içermeli
- H1→H2→H3 hiyerarşisi, seviye atlamadan
- AI arama (Bing Copilot) H1'i sayfa amacını yorumlamak için kullanıyor
- Her bölüm bir soruyu/alt konuyu ele almalı
- Kaynak: https://www.semrush.com/blog/on-page-seo-checklist/

## 5. Schema Markup (2026)
- FAQ schema: sadece gov/health siteleri için (kısıtlandı)
- HowTo schema: deprecated
- Article, Product, Event, LocalBusiness hala geçerli
- Universal Commerce Protocol (UCP) ve Agentic Commerce Protocol (ACP) takip edilmeli
- Kaynak: https://www.semrush.com/blog/on-page-seo-checklist/

## 6. Open Source Referanslar
- seord (Node.js): keyword density, heading analysis, SEO score — https://github.com/Bishwas-py/seord
- python-seo-analyzer: site crawl, word count, technical SEO — https://github.com/sethblack/python-seo-analyzer
- seo-analyzer (maddevsio): HTML defect analysis — https://github.com/maddevsio/seo-analyzer
- seonaut: Go-based open source SEO audit — https://github.com/StJudeWasHere/seonaut

## 7. Mevcut Skill'deki Düzeltilmesi Gereken Noktalar

Araştırmaya göre mevcut skill'deki güncellemeler:

1. **Keyword Density**: %0.5-2.5 yerine %0.5-2 ve "sadece sanity check" notu
2. **Meta Title**: 50-60 char iyi ama pixel (580px) bazlı da düşünülmeli. Keyword ilk 30-35 char'da olmalı.
3. **Meta Description**: 150-160 yerine 120-158 arası (mobile-first). İlk 120 char kritik.
4. **Content Depth**: Mutlak word count yerine SERP ortalamasıyla karşılaştırma eklenmeli
5. **Internal Linking**: 10+ link statik eşik yerine "2-5 link / 1000 kelime" dinamik ölçüm
6. **Readability**: Flesch Reading Ease 60-70, Grade Level 7-8 hedefi eklenmeli
7. **CTA**: Above-the-fold kontrolü eklenmeli
8. **AI Content Score**: burstiness + perplexity kavramları, Google'ın "AI OK if helpful" pozisyonu
9. **Ağırlıklar**: Keyword optimization %25, content depth %20 olmalı (industry standard)
10. **Schema**: FAQ deprecated notu, HowTo deprecated notu
11. **E-E-A-T sinyalleri**: yazar bilgisi, kaynak atıfı, deneyim kanıtı kontrol edilmeli
12. **Skor aralıkları**: 80-100 mükemmel, 60-79 optimize et, 40-59 yeniden yaz, 0-39 acil müdahale
