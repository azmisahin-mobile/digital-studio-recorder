# Digital Studio Recorder - Mobil Uygulama Yol Haritası

Bu belge, "Digital Studio Recorder" mobil uygulamasının geliştirme hedeflerini ve fazlarını özetlemektedir. Uygulamanın temel amacı, [Digital Studio X](https://github.com/azmisahin-ai/digital-studio) projesi için sanatçılardan yüksek kaliteli ve iyi etiketlenmiş ses, resim ve video kayıtları toplamaktır.

## Proje Vizyonu

Sanatçıların kolayca ve sezgisel bir şekilde `Digital Studio X` projesinin gereksinim duyduğu formatta ve meta verilerle medya (öncelikle ses, daha sonra resim ve video) kaydetmelerini sağlayan, kullanıcı dostu bir Flutter tabanlı mobil uygulama geliştirmek. Toplanan veriler, `Digital Studio X` projesindeki TTS motoru eğitimi, referans ses varlıkları ve görsel varlıklar için kullanılacaktır.

---

## Gelecek Planları ve Yol Haritası

### Faz 0: Temel Ses Kaydı ve Meta Veri Toplama (MVP - Kısa Vade: ~2-4 Hafta)

Bu faz, uygulamanın temel işlevselliğini oluşturmaya ve `Digital Studio X` projesiyle veri uyumluluğunu sağlamaya odaklanacaktır.

-   **[ ] Proje Kurulumu ve Temel Arayüz:**
    -   Flutter proje iskeleti.
    -   Basit bir ana ekran, kayıt başlatma/durdurma arayüzü.
-   **[ ] Ses Kayıt Fonksiyonu:**
    -   Cihaz mikrofonundan ses kaydı (WAV formatında öncelikli, yapılandırılabilir kalite).
    -   Kayıt süresini gösterme.
-   **[ ] Meta Veri Giriş Ekranı (Kayıt Sonrası):**
    -   **`artist_id`**: Kullanıcı girişi (uygulama ilk açıldığında veya ayarlarda kaydedilebilir).
    -   **`text_snippet` (Seslendirilen Metin)**:
        -   Öncelikli olarak, `Digital Studio X` projesinin `data/transcripts/` klasöründeki (örn: `tr_standard_sentences.yaml`, `en_standard_sentences.yaml`) YAML dosyalarından (veya bunların JSON versiyonlarından) çekilen/gömülen önceden tanımlanmış metin listesinden seçtirme.
        -   (Daha Sonra) Kullanıcının kendi metnini girmesine izin verme.
    -   **`language`**: Dropdown (örn: "Türkçe", "İngilizce"). `Digital Studio X` `config/definitions/language.yaml`'daki anahtarları (`tr`, `en`) kaydetmeli.
    -   **`accent`**: `language` seçimine bağlı olarak filtrelenmiş dropdown. `Digital Studio X` `config/definitions/language.yaml`'daki ilgili dilin `accents` anahtarlarını (`Standard_TR`, `American_General` vb.) kaydetmeli.
    -   **`emotion`**: Dropdown. `Digital Studio X` `config/definitions/emotion.yaml`'daki Türkçe açıklamalar gösterilir, İngilizce anahtar (`happy`, `sad` vb.) kaydedilir.
    -   **`style`**: Dropdown. `Digital Studio X` `config/definitions/styles.yaml`'daki Türkçe açıklamalar gösterilir, İngilizce anahtar (`narrative`, `dramatic` vb.) kaydedilir.
    -   **`age_style`**: Dropdown. `Digital Studio X` `config/definitions/age_style.yaml`'daki değerler (`young_adult`, `senior` vb.) kaydedilir.
-   **[ ] Veri Paketleme ve Dışa Aktarma:**
    -   Yapılan bir veya birden fazla kaydı (ses dosyası + meta veriler), `Digital Studio X` projesinin `docs/DATA_FORMATS.md` belgesinde tanımlanacak ZIP formatında (`index.json` ile birlikte) paketleme.
    -   ZIP dosyasını cihaza kaydetme ve/veya paylaşma seçeneği (örn: "Dışa Aktar" butonu ile e-posta, bulut depolama vb. platformlara aktarım).
-   **[ ] Ayarlar Ekranı (Basit):**
    -   Varsayılan `artist_id` girme/saklama.
    -   Varsayılan dil seçimi.
-   **[ ] Dokümantasyon (`README.md`):**
    -   Uygulamanın amacı, kurulumu (gerekirse), kullanımı.
    -   `Digital Studio X` projesiyle veri uyumluluğu ve `docs/DATA_FORMATS.md`'ye (ana projedeki) referans.

### Faz 0.5: Kullanıcı Deneyimi İyileştirmeleri ve Ön Tanımlı Metin Yönetimi (Kısa-Orta Vade)

-   **[ ] Gelişmiş Metin Sunumu:**
    -   Sanatçıya okunacak metni net bir şekilde gösterme.
    -   Uzun metinler için kaydırma.
-   **[ ] Kayıt Listesi ve Yönetimi:**
    -   Uygulama içinde yapılan kayıtları listeleme, ön dinleme, silme.
    -   Henüz dışa aktarılmamış kayıtları işaretleme/gösterme.
-   **[ ] `config/definitions/` YAML'larını Dinamik Yükleme veya Gömme Stratejisi:**
    -   `Digital Studio X` projesindeki `emotion.yaml`, `styles.yaml` vb. tanımlama dosyalarının içeriklerini mobil uygulamadaki dropdown'ları doldurmak için kullanma.
        -   *Strateji 1 (Başlangıç):* Bu dosyaların içeriğini manuel olarak Flutter koduna (örn: sabit listeler/map'ler olarak) gömmek. Ana projede değişiklik olduğunda manuel güncelleme gerektirir.
        -   *Strateji 2 (İdeal):* Bu YAML/JSON dosyalarını uygulamanın varlıkları (assets) arasına dahil etmek ve oradan okumak.
        -   *Strateji 3 (Uzun Vade/API Sonrası):* API üzerinden dinamik olarak çekmek.
-   **[ ] Hata Yönetimi ve Geri Bildirim:** Kullanıcıya daha açıklayıcı hata mesajları ve başarılı işlem bildirimleri.

### Faz 1: Resim ve Video Kaydı Entegrasyonu (Orta Vade)

-   **[ ] Resim Çekme/Cihazdan Seçme Fonksiyonu:**
    -   Meta veri girişi: `expression` (dropdown, `Digital Studio X` `expression.yaml`'dan), `viewpoint` (dropdown, `viewpoint.yaml`'dan), `lighting_type` (dropdown, `lighting_type.yaml`'dan).
-   **[ ] Video Kayıt Fonksiyonu (Kısa Klipler):**
    -   Benzer meta veri girişi (uygun olanlar).
-   **[ ] Veri Paketlemeye Resim/Video Desteği:**
    -   ZIP paketi formatını bu yeni medya türlerini içerecek şekilde genişletme (`image/`, `video/` klasörleri).
    -   `index.json`'da resim/video kayıtları için gerekli alanların eklenmesi.

### Faz 2: API Entegrasyonu ve Gelişmiş Özellikler (Uzun Vade)

-   `Digital Studio X` projesinde API'ler (Faz 2) hazır olduğunda:
    -   **[ ] API Üzerinden Veri Gönderimi:** Kayıtları (ses, resim, video) ve meta verileri doğrudan API üzerinden `Digital Studio X` sistemine gönderme.
    -   **[ ] API Üzerinden Tanım Çekme:** `emotion`, `style`, `language` gibi tanımları API üzerinden dinamik olarak çekme.
-   **[ ] SFX Kayıt Modülü (Değerlendirilecek):**
    -   Eğer sanatçıların SFX kaydetmesi isteniyorsa, bunun için özel bir arayüz.
    -   Bu, `Digital Studio X` projesindeki genel SFX stratejisine bağlı olacaktır.

---