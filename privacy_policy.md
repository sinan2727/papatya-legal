# Papatyam — Gizlilik Politikası

🌐 English: [Privacy Policy](privacy_policy_en) · [Terms of Service](terms_of_service_en)

**Gizlilik Politikası · Sürüm 1.1 · 30 Mayıs 2026**
**Yürürlük tarihi:** Uygulamanın Play Store'da yayınlandığı tarih

Bu sürüm önceki sürümlerin yerine geçer. Kayıtlı kullanıcılar değişiklikler hakkında bilgilendirilir ve yeniden açık rıza istenir.

---

## 1. Giriş

Papatyam ("Uygulama", "Biz") kullanıcılarımızın gizliliğine en üst düzeyde önem verir. Bu Gizlilik Politikası, Papatyam mobil uygulamasını kullandığınızda hangi verilerinizi topladığımızı, neden topladığımızı, nasıl koruduğumuzu ve haklarınızı nasıl kullanabileceğinizi açıklar. Politika Türkiye Cumhuriyeti Kişisel Verilerin Korunması Kanunu (KVKK, 6698 sayılı kanun) ve Avrupa Birliği Genel Veri Koruma Tüzüğü (GDPR, 2016/679) çerçevesinde hazırlanmıştır.

Bu uygulamayı kullanmaya başlayarak burada açıklanan veri işleme uygulamalarını kabul etmiş sayılırsınız.

---

## 1.5 Yaş Sınırı

Papatyam yalnızca 18 yaşından büyük kullanıcılara hizmet verir. Kayıt sırasında bir yaş onayı adımı sunulur: kullanıcı bir checkbox ile 18 yaşından büyük olduğunu beyan eder ve 4 haneli doğum yılını girer (gün/ay sorulmaz — KVKK Md.4 veri minimizasyonu ilkesi gereği). Sunucuya yalnızca üç bilgi yazılır: `yasOnayi` (boolean), `dogumYili` (yıl tam sayısı) ve `yasOnayTarihi` (sunucu zaman damgası, açık rıza ispatı için).

Yaş beyanı self-attested (kullanıcı sorumluluğunda) bir beyandır; teknik olarak kötü niyetli bir kullanıcının atlatması mümkündür ancak yasal sorumluluk tamamen kullanıcıdadır. 18 yaşından küçük olduğu sonradan tespit edilen hesaplar derhal askıya alınır ve kullanıcı verisi standart hesap silme akışı ile cascade olarak temizlenir.

Politikamızın yaş sınırı maddesi güncellendiğinde, eski sürümle kayıt olmuş kullanıcılardan uygulama içinden yeniden yaş onayı istenir (KVKK reonay zinciri ile benzer şekilde).

---

## 2. Veri Sorumlusu ve İletişim

Veri sorumlusu: Sinan F.
İletişim: sinanfil@yahoo.com

Veri sorumlusuna bu adresten ulaşarak haklarınızı kullanabilir, soru sorabilir veya şikayette bulunabilirsiniz.

---

## 3. Mimari: Sıfır-Bilgi (Zero-Knowledge) Yaklaşımı

Papatyam'ın en önemli özelliği, sunucunun kullanıcı verilerinin **açık halini görmemesidir**. Bu uygulamanın temel tasarım ilkesidir, ve aşağıdaki teknik tedbirlerle uygulanır:

- **Telefon numarası asla açık olarak saklanmaz.** Numara, cihazınızda HMAC-SHA256 algoritmasıyla bir pepper anahtarı (sunucu-tarafı gizli anahtar; Firebase Remote Config üzerinden dağıtılır ve kısa TTL ile yenilenir) kullanılarak hash'lenir. Sunucu yalnızca bu hash'i bilir; gerçek telefon numarasına erişimi yoktur.
- **PIN sunucuya gönderilmez.** PIN cihazınızda PBKDF2-HMAC-SHA256 algoritmasıyla 100.000 iterasyon ile master key'e dönüştürülür. Master key cihazınızdaki bellekte tutulur, sunucuya iletilmez.
- **E-posta, profil bilgileri ve geçmiş mesajlar şifreli olarak saklanır.** AES-256-CBC algoritması ile master key (veya cihaz UUID'si) kullanılarak şifrelenir. Sunucu bu verilerin yalnızca şifreli halini görür; çözümleyemez.
- **Serbest mesajlar uçtan uca şifrelidir.** Kullanılan kriptografi: X25519 ECDH anahtar değişimi, mesaj başına AES-GCM şifreleme (12-byte nonce + bütünlük etiketi), TOFU (Trust On First Use) ile karşı tarafın açık anahtarının pin'lenmesi. Sunucumuz şifrelenmiş içeriği (ciphertext) saklar ama plaintext'i çözemez — bu nedenle "mesaj içeriklerini normalde okuyamayız" deriz. Bu Signal-absolute bir garanti değildir: şablon mesajları (önceden tanımlanmış 50 metin şablonu + 12 emoji-only şablon) multi-recipient akışı ve anonim moderasyon ihtiyacı için şifresiz işlenir; bu, kullanıcı şikâyetlerinin değerlendirilebilmesi için bilinçli bir ödünleşmedir.

Bu mimari, herhangi bir veri sızıntısı durumunda saldırganın elde edebileceği bilginin sınırlı olmasını sağlar.

---

## 4. Toplanan Veriler ve Amaçları

### 4.1 Doğrudan Topladığımız Veriler

- **Telefon numarası (hash'li):** Yalnızca HMAC-SHA256 + pepper ile hash'lenmiş hali. Kullanıcının kimliğini doğrulamak ve rehber eşleştirmelerini sağlamak için kullanılır. Açık hali asla sunucuda bulunmaz.
- **E-posta adresi (şifreli, opsiyonel):** Hesap kurtarma ve eşleşme bildirimleri için. Master key veya UUID ile şifrelenmiş halde saklanır.
- **PIN (cihazda hash):** PBKDF2-HMAC-SHA256 ile hash'lenir. Hash sunucuda doğrulama için saklanır; ham PIN değeri kimsenin erişiminde değildir.
- **Yedek kod (cihazda hash):** PIN kaybı durumunda kullanılan 6 haneli kod. Hash olarak saklanır.
- **FCM push token:** Bildirim göndermek için Firebase tarafından üretilen cihaz token'ı. Cihazın geçici tanımlayıcısıdır, kullanıcı kimliğine bağlı değildir.
- **Profil bilgileri (opsiyonel, şifreli):** Cinsiyet, yaş aralığı, şehir, ilgi alanları. Tamamen kullanıcı kontrolündedir, doldurulması zorunlu değildir, herhangi bir zaman değiştirilebilir veya silinebilir.
- **Ayarlar:** Rehber erişim tercihi, bildirim kanalı, PIN sorma sıklığı, dil tercihi.
- **Açık rıza kaydı:** Onboarding sırasında verdiğiniz KVKK açık rızasının kanıtı (bkz. 4.5).
- **Yaş onayı kaydı:** Yaş gate'i için verdiğiniz beyan + doğum yılı (yalnız yıl) + onay tarihi. Ayrıntılar Bölüm 1.5.

### 4.2 Rehber Erişimi

Uygulama, rehberinizdeki kişilerin telefon numaralarını da HMAC-SHA256 + pepper ile hash'leyerek eşleşme kontrolü yapar. **Rehberinizin açık hali (isimler, numaralar) hiçbir zaman sunucuya gönderilmez.** Yalnızca hash'lenmiş telefon numaraları, eşleşme kontrolü amacıyla sunucudaki diğer hash'lerle karşılaştırılır.

### 4.3 Otomatik Olarak Toplanan Veriler

- **Uygulama kullanım sayaçları:** Beğeni sayacı, şablon mesaj sayacı, oyun sayacı (aylık limit yönetimi için).
- **Çökme/hata raporları (Firebase Crashlytics):** Crashlytics ile çökme/hata raporlama yaparken pseudonymous (takma adlı) bir kullanıcı bağlantısı kurarız. Hesap UID'nizden HMAC ile türetilmiş 8-hex uzunluğunda bir `user_uid_hash` ve `premium_status` flag'i (premium / ücretsiz) Crashlytics'e yazılır. Ham UID, telefon numaranız veya başka bir kimlik bilgisi Crashlytics'e **asla** gönderilmez. `user_uid_hash` tek yönlüdür (geri çözüm pratik olarak imkânsızdır) ve farklı hesaplarda tekrar etmez. Bu sayede aynı kullanıcının birden fazla çökmesini ilişkilendirip kök neden tespit ederiz; kimliğinizi öğrenmeyiz. Crashlytics'e ayrıca uygulama sürümü, build türü (debug/release), platform (Android/iOS) ve cihaz dili gibi non-PII teknik bilgiler iletilir.
- **Rate limit kayıtları:** Spam koruması için kullanıcı başına saatlik/günlük istek sayısı. Pseudonymous.

### 4.4 Toplamadığımız Veriler

- Açık telefon numaranız (yalnızca hash)
- Rehber kişilerin isimleri (yalnızca telefon hash'leri)
- Serbest mesajlarınızın açık metni (yalnızca uçtan uca şifreli ciphertext)
- Konum bilginiz (uygulama konum izni istemez)
- Reklam tanımlayıcıları (uygulamada reklam yoktur)
- Çerez, kullanım analitiği (Google Analytics, Firebase Analytics gibi izleme yoktur)

### 4.5 Açık Rıza Onayı (KVKK Md.5/1, GDPR Art.6/1/a)

Onboarding sırasında telefon doğrulamasından sonra, Gizlilik Politikası'nı okuduğunuzu ve kişisel verilerinizin bu politika kapsamında işlenmesine açık rıza verdiğinizi onayladığınız bir checkbox sunulur. Bu kutuyu işaretlemeden kayıt tamamlanamaz. Onay verdiğinizde sunucumuza şu üç bilgi yazılır:

- `kvkkOnayTarihi` — sunucu zaman damgası (kanıt amaçlı, KVKK Md.5/1 ispat yükü).
- `kvkkPolitikaSurumu` — onayladığınız politika sürümü (bu belgede "1.1").
- `kvkkPolitikaURL` — politikanın o anki yayın adresi (`https://sinan2727.github.io/papatya-legal/privacy_policy`).

Politika sürümü güncellendiğinde, eski sürümle kayıt olmuş kullanıcılardan uygulama içinden yeniden açık rıza istenir; rıza yenilenmedikçe değiştirilmiş özellikler kısıtlanabilir.

### 4.6 Yapay Zeka (AI) Kullanımı

Papatyam'da yapay zeka yalnızca tek bir amaçla kullanılır: Sır Avı bölümünde jenerik, açık uçlu sohbet soruları üretmek (örneğin "Hayatında seni en çok güldüren an neydi?").

- **Hiçbir kullanıcı veriniz yapay zekâya gönderilmez.** Mesajlarınız, kimliğiniz, rehberiniz, profiliniz veya karşı tarafa ait hiçbir bilgi yapay zekâ sistemine iletilmez.
- Yapay zeka sizi veya karşı tarafı analiz etmez, profillemez; yaş, cinsiyet, niyet gibi hiçbir tahmin yapmaz.
- Üretilen soru önce size gösterilir; siz görüp onaylamadan hiçbir soru gönderilmez. Soruyu düzenleyebilir, yeniden ürettirebilir veya hiç kullanmayabilirsiniz.
- Altyapı olarak Anthropic Claude API kullanılır (bkz. madde 7); bu servise yalnızca soru üretimi için genel bir talimat gönderilir, size ait hiçbir içerik gönderilmez.
- Şeffaflık adına belirtmek isteriz: kullanıcı içeriğini yapay zekâya gönderecek özellikler (yazışma pratiği, konuşma koçu) bir dönem planlanmış, ancak gizlilik ilkemizle bağdaşmadığı için uygulamaya hiç dahil edilmeden kaldırılmıştır.

---

## 5. Veri İşlemenin Hukuki Dayanağı

Verilerinizi aşağıdaki hukuki dayanaklara dayanarak işliyoruz:

1. **Açık rıza (KVKK Md.5/1, GDPR Art.6/1/a):** Onboarding'de checkbox ile bilinçli olarak verdiğiniz açık rıza (ayrıntı 4.5).
2. **Sözleşmenin ifası (KVKK Md.5/2-c, GDPR Art.6/1/b):** Hizmeti sağlamak için zorunlu işlemler (eşleşme kontrolü, mesaj iletimi).
3. **Meşru menfaat (KVKK Md.5/2-f, GDPR Art.6/1/f):** Spam ve kötüye kullanımı önlemek için rate limit, App Check gibi güvenlik tedbirleri.
4. **Hukuki yükümlülük (KVKK Md.5/2-ç, GDPR Art.6/1/c):** Yasal saklama süreleri ve denetim kayıtları.

---

## 6. Veri Saklama Süreleri

Papatyam'ın saklama yaklaşımının temel ilkesi, **kullanıcı kontrolünün esas olmasıdır.** Her veri tipi için ya net bir maksimum süre tanımlanmış ya da kullanıcının uygulama içinden silme kontrolü vardır. KVKK Md.4-d (verilerin işlendikleri amaç için gerekli olan süreyi aşmaması ilkesi) bu kullanıcı kontrolü ile birlikte uygulanır.

- **Aktif kullanıcı verileri (profil, eposta, ayarlar, PIN hash, yedek kod hash, FCM token):** Kullanıcı uygulamayı aktif kullandığı süre boyunca. Hesap silinince cascade olarak temizlenir.
- **Sır Avı oturumları ve içerikleri:** Bir Sır Avı oyunu sona erdiğinden veya iptal edildiğinden 10 gün sonra konuşma içeriği (sorular ve cevaplar), oyun bilgisi ve ilgili tüm tur kayıtları sunuculardan kalıcı ve geri getirilemez biçimde silinir. Bu silme `bulmacaRetentionKontrol` adlı planlı görev ile her gece 03:00 İstanbul saatinde otomatik çalışır. Bir konuşmayı kendi listenden gizlemeniz bu 10 günlük otomatik silmeyi değiştirmez.
- **Sır Avı oyun sayacı (kötüye kullanım önlemi):** Aynı kişiyle açılan oyunların sayısını tutan sayaç, içinde bulunulan ayın sonuna kadar saklanır ve her ay başında sıfırlanır. Yalnızca sayısal bir değerdir; içerik bilgisi içermez.
- **Şablon mesajları:** Kullanıcı uygulama içinden bireysel veya toplu silme arayüzü ile silene kadar saklanır. Hesap silindiğinde cascade ile gönderen ve alıcı tarafında temizlenir. Sabit bir maksimum süre uygulanmaz; silme kararı kullanıcıdadır.
- **Serbest mesajlar (uçtan uca şifreli):** Yalnızca ciphertext (şifreli hâl) sunucuda saklanır; sunucu plaintext'i okuyamaz. Mesaj ciphertext'i kullanıcı tek tek veya toplu silme arayüzüyle silene veya hesap silinene kadar tutulur. **Önemli not:** Mesaj şifresini çözen özel anahtar yalnızca cihazınızda saklanır. Uygulamayı kaldırırsanız veya cihaz verisi temizlenirse, sunucudaki ciphertext'ler artık çözülemez — yedek/kurtarma yolu kasıtlı olarak tasarlanmamıştır.
- **Engelleme kayıtları (`blocks`):** Engellediğiniz kullanıcılarla ilişkili kayıtlar engeli kaldırana kadar saklanır; engel kaldırıldığında veya hesap silindiğinde temizlenir.
- **Eşleşmeler ve beğeniler:** Hesap aktif olduğu sürece saklanır; hesap silmede cascade ile temizlenir.
- **Hesap silme penceresi (soft delete):** Hesabınızı sildiğinizde 30 gün boyunca soft delete halinde tutulur. Bu süre içinde tekrar giriş yapıp geri alabilirsiniz. 30 gün sonra arka plan görevi `_kullaniciVerisiSilHard` çalışır ve aşağıdaki koleksiyonların hepsinden sizinle ilgili kayıtlar kalıcı silinir: kullanıcı profili, beğeniler, eşleşmeler, kotalar, engelleme kayıtları (iki yönlü), şablon mesajlar (gönderen ve alıcı yönü), yazma hakkı oturumları (iki yön), Sır Avı oturumları ve alt-tur kayıtları (iki yön), serbest mesajlar (iki yön). Toplam 13 koleksiyon temizlenir.
- **Audit log (denetim kayıtları):** Hesap silme gibi önemli aksiyonların kanıtı için. KVKK Md.7 gereği 6 yıl saklanır. Bu kayıtlar yalnızca anonimleştirilmiş hash (HMAC ile türetilmiş) içerir; kullanıcı kimliği geri çözülemez.
- **Açık rıza kayıtları (`kvkkOnayTarihi`, `kvkkPolitikaSurumu`, `kvkkPolitikaURL`):** Hesap yaşadığı sürece saklanır; açık rıza ispatı için gereklidir (KVKK Md.5/1). Hesap silindiğinde diğer kullanıcı verisi ile birlikte cascade silinir; yalnızca pseudonymous audit log kaydı kalır.
- **Yaş onayı kayıtları (`yasOnayi`, `dogumYili`, `yasOnayTarihi`):** Hesap yaşadığı sürece saklanır; 18+ beyanı kanıtı için gereklidir (bkz. 1.5). Hesap silindiğinde cascade ile temizlenir; pseudonymous audit log kaydı 6 yıl (KVKK Md.7) saklanır.
- **Hata raporları (Crashlytics):** Firebase Crashlytics varsayılan saklama süresine tabi (yaklaşık 90 gün). Pseudonymous `user_uid_hash` taşır (ayrıntı 4.3).
- **Sistem logları (operasyonel):** Hata kurtarma ve sistem sağlığı izleme amacıyla tutulan teknik kayıtlar en fazla 30 gün saklanır. Bu kayıtlar konuşma içeriği, mesaj metni veya çözülmüş kullanıcı verisi içermez.
- **Rate limit kayıtları (`rate_limits`):** Idempotency için 48 saat TTL ile otomatik silinir. Pseudonymous.
- **Nonce'lar (anti-replay):** 10 dakika sonra otomatik silinir (TTL policy).

Tüm bu kalemlerin üzerinde **kullanıcı kontrolü esastır:** Ayarlar > Tehlikeli Bölge > Hesabımı Sil yolu ile hesap kapatılınca soft delete penceresi başlar; mesajlar, Sır Avı oturumları ve şablon mesajlar için uygulama içinden tek tek veya toplu silme arayüzleri mevcuttur.

---

## 7. Veri Paylaşımı ve Üçüncü Taraflar

Verileriniz aşağıdaki üçüncü taraflar ile sınırlı düzeyde paylaşılır:

- **Google Firebase (Cloud Functions, Firestore, FCM, App Check, Crashlytics, Remote Config):** Uygulamanın altyapı sağlayıcısıdır. Firebase verileri Google'ın güvenlik standartlarına göre korur. Detay: https://firebase.google.com/support/privacy
- **Google Cloud Platform:** Firebase'in altında çalışan bulut altyapısı. Veriler Avrupa veya ABD sunucularında işlenebilir.
- **Anthropic (Claude API):** Sır Avı bölümünde jenerik, açık uçlu sohbet soruları üretmek için kullanılır. Bu servise size ait hiçbir veri gönderilmez — ne mesajlarınız, ne kimliğiniz, ne profiliniz, ne de karşı tarafa ait bilgiler. Yalnızca soru üretimi için genel bir talimat iletilir. Detay: https://www.anthropic.com/legal/privacy

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
- 30 gün sonra tüm verileriniz 13 koleksiyonda cascade ile kalıcı olarak silinir (yalnızca KVKK Md.7 audit log kaydı kalır — anonimleştirilmiş, kullanıcı kimliği geri çözülemez). Ayrıntı için bkz. madde 6.

### 8.4 Veri Taşıma Hakkı (GDPR Art.20)
Verilerinizin makine-okunabilir formatta sağlanmasını talep edebilirsiniz. Bu talep için sinanfil@yahoo.com adresine yazınız.

### 8.5 İtiraz Hakkı
Veri işlemeye itiraz edebilirsiniz. Bu durumda uygulama kullanılamaz hale gelebilir (zira veri işleme hizmetin temel parçasıdır).

### 8.6 Otomatik Karar ve Profilleme
Uygulamada otomatik karar veren veya kullanıcı profilleyen bir mekanizma yoktur. Eşleştirme algoritması yalnızca rehber telefon hash karşılaştırmasıyla çalışır. Uygulamadaki yapay zeka yalnızca jenerik soru üretir (bkz. 4.6); kullanıcıları analiz etmez veya profillemez.

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

- **Şifreleme:** AES-256-CBC (cihazda şifreli profil/eposta), AES-GCM (uçtan uca serbest mesaj), PBKDF2-HMAC-SHA256 (anahtar türetme), HMAC-SHA256 + pepper (telefon ve denetim hash'leri).
- **App Check (Play Integrity):** Sahte istemcilerin sunucuya erişimini engeller (production'da aktif).
- **Replay protection:** Nonce + timestamp ile her istek tek seferlik.
- **Rate limiting:** Spam ve brute force saldırılarına karşı.
- **Cihaz güvenliği kontrolü:** Root/jailbreak edilmiş cihazlarda uyarı.
- **Firestore Security Rules:** Tüm yazma işlemleri Cloud Functions üzerinden, admin SDK ile yapılır. Client doğrudan yazamaz.

Tüm bu önlemlere rağmen internet üzerinden veri iletimi %100 güvenli değildir; mutlak güvenlik garantisi veremeyiz, ancak en iyi uygulamaları takip etmeyi taahhüt ederiz.

---

## 11. Politikadaki Değişiklikler

Bu politikayı periyodik olarak güncelleyebiliriz. Önemli değişikliklerde uygulama içinden bildirim göndereceğiz ve yeniden açık rıza isteyeceğiz (bkz. 4.5). Politikanın güncel sürüm numarası ve tarihi her zaman üst başlıkta yer alır.

---

## 12. İletişim

Soru, talep veya şikayet için:

**E-posta:** sinanfil@yahoo.com

Maksimum 30 iş günü içinde yanıt verilir.

---

*Bu politika Papatyam mobil uygulamasının Play Store öncesi yayın hazırlığı kapsamında hazırlanmıştır. Politikanın yasal bir uzman tarafından incelenmesi tavsiye edilir.*
