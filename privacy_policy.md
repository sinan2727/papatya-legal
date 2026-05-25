# Papatyam — Gizlilik Politikası

**Son güncelleme:** 25 Mayıs 2026
**Yürürlük tarihi:** Uygulamanın Play Store'da yayınlandığı tarih

---

## 1. Giriş

Papatyam ("Uygulama", "Biz") kullanıcılarımızın gizliliğine en üst düzeyde önem verir. Bu Gizlilik Politikası, Papatyam mobil uygulamasını kullandığınızda hangi verilerinizi topladığımızı, neden topladığımızı, nasıl koruduğumuzu ve haklarınızı nasıl kullanabileceğinizi açıklar. Politika Türkiye Cumhuriyeti Kişisel Verilerin Korunması Kanunu (KVKK, 6698 sayılı kanun) ve Avrupa Birliği Genel Veri Koruma Tüzüğü (GDPR, 2016/679) çerçevesinde hazırlanmıştır.

Bu uygulamayı kullanmaya başlayarak burada açıklanan veri işleme uygulamalarını kabul etmiş sayılırsınız.

---

## 2. Veri Sorumlusu ve İletişim

Veri sorumlusu: Sinan F.
İletişim: sinanfil@yahoo.com

Veri sorumlusuna bu adresten ulaşarak haklarınızı kullanabilir, soru sorabilir veya şikayette bulunabilirsiniz.

---

## 3. Mimari: Sıfır-Bilgi (Zero-Knowledge) Yaklaşımı

Papatyam'ın en önemli özelliği, sunucunun kullanıcı verilerinin **açık halini görmemesidir**. Bu uygulamanın temel tasarım ilkesidir, ve aşağıdaki teknik tedbirlerle uygulanır:

- **Telefon numarası asla açık olarak saklanmaz.** Numara, cihazınızda SHA-256 algoritmasıyla pepper (sunucu-tarafı gizli anahtar) kullanılarak hash'lenir. Sunucu yalnızca bu hash'i bilir; gerçek telefon numarasına erişimi yoktur.
- **PIN sunucuya gönderilmez.** PIN cihazınızda PBKDF2-HMAC-SHA256 algoritmasıyla 100.000 iterasyon ile master key'e dönüştürülür. Master key cihazınızdaki bellekte tutulur, sunucuya iletilmez.
- **E-posta, profil bilgileri, mesajlar şifreli olarak saklanır.** AES-256-CBC algoritması ile master key (veya cihaz UUID'si) kullanılarak şifrelenir. Sunucu bu verilerin yalnızca şifreli halini görür; çözümleyemez.
- **Mesaj içerikleri taraflar arası şifrelidir.** Mesaj alıcısına gönderilirken alıcının doğrulanmış anahtarı ile şifrelenir.

Bu mimari, herhangi bir veri sızıntısı durumunda saldırganın elde edebileceği bilginin sınırlı olmasını sağlar.

---

## 4. Toplanan Veriler ve Amaçları

### 4.1 Doğrudan Topladığımız Veriler

- **Telefon numarası (hash'li):** Yalnızca SHA-256 + pepper ile hash'lenmiş hali. Kullanıcının kimliğini doğrulamak ve rehber eşleştirmelerini sağlamak için kullanılır. Açık hali asla sunucuda bulunmaz.
- **E-posta adresi (şifreli, opsiyonel):** Hesap kurtarma ve eşleşme bildirimleri için. Master key veya UUID ile şifrelenmiş halde saklanır.
- **PIN (cihazda hash):** PBKDF2-HMAC-SHA256 ile hash'lenir. Hash sunucuda doğrulama için saklanır; ham PIN değeri kimsenin erişiminde değildir.
- **Yedek kod (cihazda hash):** PIN kaybı durumunda kullanılan 6 haneli kod. Hash olarak saklanır.
- **FCM push token:** Bildirim göndermek için Firebase tarafından üretilen cihaz token'ı. Cihazın geçici tanımlayıcısıdır, kullanıcı kimliğine bağlı değildir.
- **Profil bilgileri (opsiyonel, şifreli):** Cinsiyet, yaş aralığı, şehir, ilgi alanları. Tamamen kullanıcı kontrolündedir, doldurulması zorunlu değildir, herhangi bir zaman değiştirilebilir veya silinebilir.
- **Ayarlar:** Rehber erişim tercihi, bildirim kanalı, PIN sorma sıklığı.

### 4.2 Rehber Erişimi

Uygulama, rehberinizdeki kişilerin telefon numaralarını da SHA-256 + pepper ile hash'leyerek eşleşme kontrolü yapar. **Rehberinizin açık hali (isimler, numaralar) hiçbir zaman sunucuya gönderilmez.** Yalnızca hash'lenmiş telefon numaraları, eşleşme kontrolü amacıyla sunucudaki diğer hash'lerle karşılaştırılır.

### 4.3 Otomatik Olarak Toplanan Veriler

- **Uygulama kullanım sayaçları:** Beğeni sayacı, şablon mesaj sayacı, oyun sayacı (aylık limit yönetimi için).
- **Hata raporları (Crashlytics):** Uygulama çökmelerinde Firebase Crashlytics aracılığıyla anonim hata raporu toplanır. Bu raporlar kullanıcı kimliğine bağlı değildir.
- **Rate limit kayıtları:** Spam koruması için kullanıcı başına saatlik/günlük istek sayısı.

### 4.4 Toplamadığımız Veriler

- Açık telefon numaranız (yalnızca hash)
- Rehber kişilerin isimleri (yalnızca telefon hash'leri)
- Mesajlarınızın açık metni (yalnızca şifreli hali)
- Konum bilginiz (uygulama konum izni istemez)
- Reklam tanımlayıcıları (uygulamada reklam yoktur)
- Çerez, kullanım analitiği (Google Analytics, Firebase Analytics gibi izleme yoktur)

---

## 5. Veri İşlemenin Hukuki Dayanağı

Verilerinizi aşağıdaki hukuki dayanaklara dayanarak işliyoruz:

1. **Açık rıza (KVKK Md.5/1, GDPR Art.6/1/a):** Uygulamayı kurarken Hizmet Şartları ve Gizlilik Politikası'nı kabul ediyorsunuz.
2. **Sözleşmenin ifası (KVKK Md.5/2-c, GDPR Art.6/1/b):** Hizmeti sağlamak için zorunlu işlemler (eşleşme kontrolü, mesaj iletimi).
3. **Meşru menfaat (KVKK Md.5/2-f, GDPR Art.6/1/f):** Spam ve kötüye kullanımı önlemek için rate limit, App Check gibi güvenlik tedbirleri.
4. **Hukuki yükümlülük (KVKK Md.5/2-ç, GDPR Art.6/1/c):** Yasal saklama süreleri ve denetim kayıtları.

---

## 6. Veri Saklama Süreleri

- **Aktif kullanıcı verileri:** Kullanıcı uygulamayı aktif kullandığı süre boyunca.
- **Silinmiş hesap verileri:** Kullanıcı silme talebi sonrası 30 gün boyunca soft delete (geri alınabilir), sonra kalıcı silme.
- **Sır Avı konuşma içeriği:** Bir Sır Avı konuşması (oyunu) sona erdikten 10 gün sonra konuşma içeriği (sorular ve cevaplar), oyun bilgisi ve ilgili tüm tur kayıtları sunuculardan kalıcı ve geri getirilemez biçimde silinir. Bir konuşmayı kendi listenden gizlemeniz bu 10 günlük otomatik silmeyi değiştirmez; süre dolduğunda silme, gizleme tercihinden bağımsız uygulanır.
- **Sır Avı oyun sayacı (kötüye kullanım önlemi):** Aynı kişiyle açılan oyunların sayısını tutan sayaç, içinde bulunulan ayın sonuna kadar saklanır ve her ay başında sıfırlanır. Bu sayaç yalnızca sayısal değerden ibarettir; konuşma içeriği, mesaj veya başka bir kişisel veri içermez.
- **Audit log (denetim kayıtları):** KVKK Md.7 gereği 6 yıl saklanır. Bu kayıtlar anonimleştirilmiş hash içerir, kullanıcı kimliği geri çözülemez.
- **Hata raporları (Crashlytics):** Firebase varsayılan saklama süresine tabi (genelde 90 gün).
- **Sistem logları (operasyonel):** Hata kurtarma ve sistem sağlığı izleme amacıyla tutulan teknik kayıtlar en fazla 30 gün saklanır. Bu kayıtlar konuşma içeriği, mesaj metni veya çözülmüş kullanıcı verisi içermez.
- **Rate limit kayıtları:** 48 saat sonra otomatik silinir (TTL policy).
- **Nonce'lar (anti-replay):** 10 dakika sonra otomatik silinir (TTL policy).

---

## 7. Veri Paylaşımı ve Üçüncü Taraflar

Verileriniz aşağıdaki üçüncü taraflar ile sınırlı düzeyde paylaşılır:

- **Google Firebase (Cloud Functions, Firestore, FCM, App Check, Crashlytics, Remote Config):** Uygulamanın altyapı sağlayıcısıdır. Firebase verileri Google'ın güvenlik standartlarına göre korur. Detay: https://firebase.google.com/support/privacy
- **Google Cloud Platform:** Firebase'in altında çalışan bulut altyapısı. Veriler Avrupa veya ABD sunucularında işlenebilir.

Verileriniz **reklam ortakları, veri tüccarları veya pazarlama firmalarıyla paylaşılmaz.** Verilerinizi satmıyor, kiralamıyor veya başka bir ticari amaçla paylaşmıyoruz.

---

## 8. KVKK ve GDPR Haklarınız

KVKK Md.11 ve GDPR Art.15-22 kapsamında aşağıdaki haklara sahipsiniz:

### 8.1 Bilgi Alma Hakkı (Erişim)
İşlenen verileriniz hakkında bilgi talep edebilirsiniz. Çoğu veriniz uygulama içinde zaten görüntülenebilir (Ayarlar > Profil Bilgisi).

### 8.2 Düzeltme Hakkı
Yanlış veya eksik verilerinizi uygulama üzerinden düzeltebilirsiniz (Ayarlar > E-posta, Profil Bilgisi).

### 8.3 Silme Hakkı ("Unutulma Hakkı")
**Hesabınızı uygulama içinden silebilirsiniz:** Ayarlar > Tehlikeli Bölge > Hesabımı Sil. İşlem sonrası:
- Hesabınız 30 gün boyunca soft delete halinde tutulur.
- 30 gün içinde tekrar giriş yaparak geri alabilirsiniz.
- 30 gün sonra tüm verileriniz kalıcı olarak silinir (yalnızca KVKK Md.7 audit log kaydı kalır — anonimleştirilmiş, kullanıcı kimliği geri çözülemez).

### 8.4 Veri Taşıma Hakkı (GDPR Art.20)
Verilerinizin makine-okunabilir formatta sağlanmasını talep edebilirsiniz. Bu talep için sinanfil@yahoo.com adresine yazınız.

### 8.5 İtiraz Hakkı
Veri işlemeye itiraz edebilirsiniz. Bu durumda uygulama kullanılamaz hale gelebilir (zira veri işleme hizmetin temel parçasıdır).

### 8.6 Otomatik Karar ve Profilleme
Uygulamada otomatik karar veren veya kullanıcı profilleyen bir mekanizma yoktur. Eşleştirme algoritması yalnızca rehber telefon hash karşılaştırmasıyla çalışır.

### 8.7 Hak Kullanım Süreci
Yukarıdaki haklarınızı kullanmak için:
1. Uygulama içinden doğrudan (silme, düzeltme): anında etki.
2. Erişim/taşıma/itiraz için: sinanfil@yahoo.com adresine talebinizi yazınız. Maksimum 30 gün içinde yanıt verilir.

### 8.8 Şikayet Hakkı
Talebimiz tatmin edici değilse:
- Türkiye: Kişisel Verileri Koruma Kurulu (KVKK) — https://www.kvkk.gov.tr
- AB: Yerel veri koruma otoritesi (örn. Almanya: BfDI, İrlanda: DPC).

---

## 9. Çocukların Gizliliği

Papatyam 18 yaş ve üzeri kullanıcılar içindir. 18 yaş altı çocuklardan bilerek veri toplamayız. Ebeveynlerin çocuklarının uygulamayı kullanmadığından emin olması gerekir. Bir çocuğun kayıt olduğunu öğrenirsek hesap silinir.

---

## 10. Güvenlik Önlemleri

- **Şifreleme:** AES-256-CBC (veri), PBKDF2-HMAC-SHA256 (anahtar türetme), SHA-256 + pepper (hash).
- **App Check (Play Integrity):** Sahte istemcilerin sunucuya erişimini engeller (production'da aktif).
- **Replay protection:** Nonce + timestamp ile her istek tek seferlik.
- **Rate limiting:** Spam ve brute force saldırılarına karşı.
- **Cihaz güvenliği kontrolü:** Root/jailbreak edilmiş cihazlarda uyarı.
- **Firestore Security Rules:** Tüm yazma işlemleri Cloud Functions üzerinden, admin SDK ile yapılır. Client doğrudan yazamaz.

Tüm bu önlemlere rağmen internet üzerinden veri iletimi %100 güvenli değildir; mutlak güvenlik garantisi veremeyiz, ancak en iyi uygulamaları takip etmeyi taahhüt ederiz.

---

## 11. Politikadaki Değişiklikler

Bu politikayı periyodik olarak güncelleyebiliriz. Önemli değişikliklerde uygulama içinden bildirim göndereceğiz. "Son güncelleme" tarihini her zaman kontrol edebilirsiniz.

---

## 12. İletişim

Soru, talep veya şikayet için:

**E-posta:** sinanfil@yahoo.com

Maksimum 30 iş günü içinde yanıt verilir.

---

*Bu politika Papatyam mobil uygulamasının Play Store öncesi yayın hazırlığı kapsamında hazırlanmıştır. Politikanın yasal bir uzman tarafından incelenmesi tavsiye edilir.*
