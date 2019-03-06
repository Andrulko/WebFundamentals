project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Ilk olusturmada mümkün olan en hizli süreyi saglamak için üç degiskeni optimize etmemiz gerekir: Kritik kaynaklarin sayisini en aza indirme, kritik baytlarin sayisini en aza indirme ve kritik yol uzunlugunu en aza indirme.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Kritik Olusturma Yolunu Optimize Etme {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Ilk olusturmada mümkün olan en hizli süreyi saglamak için üç degiskeni optimize etmemiz gerekir:

- The number of critical resources.
- The critical path length.
- The number of critical bytes.

Kritik kaynak, sayfanin ilk olusturmasini engelleyebilecek herhangi bir kaynaktir. Bu kaynaklardan sayfada ne kadar az olursa, tarayicinin içerigi ekrana getirmek için yapmasi gereken is ve CPU ile diger kaynaklar için yapilan yarisma da o kadar azalir.

Benzer bir sekilde, tarayicinin indirmesi gereken kritik bayt sayisi azaldikça, içerigi daha hizli islemeye geçebilir ve içerigi ekranda gösterebilir. Bayt sayisini azaltmak için kaynak sayisini azaltabiliriz (kaldirabilir veya kritik olmaktan çikarabiliriz) ve her bir kaynagi sikistirarak ve optimize ederek aktarim boyutunu en aza indirdigimizden emin olabiliriz.

Son olarak, kritik yol uzunlugu sayfanin gerektirdigi tüm kritik kaynaklar ve bunlarin bayt boyutu arasindaki bir bagimlilik grafigi islevidir: Bazi kaynak indirmeleri yalnizca önceki bir kaynak islendiyse baslatilabilir ve kaynak büyüdükçe, kaynagi indirmek için gerçeklestirilmesi gereken gidis gelis sayisi artar.

**The general sequence of steps to optimize the critical rendering path is:**

1. Kritik yolunuzu analiz etme ve tanimlama: kaynak sayisi, bayt sayisi, uzunluk.
2. Kritik kaynaklarin sayisini en aza indirme: çikarma, indirilmelerini erteleme, zaman uyumsuz olarak isaretleme vb.
3. Kalan kritik kaynaklarin yüklenme siralamasini optimize etme: kritik yol uzunlugunu kisaltmak için tüm kritik varliklari mümkün oldugunca erken indirmek istersiniz.
4. Indirme süresini azaltmak için kritik bayt sayisini optimize edin (gidis gelis sayisi).

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}