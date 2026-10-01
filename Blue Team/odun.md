# Log Analizi Temelleri

Windows, Linux, e-posta ve ağ kaynaklarından gelen güvenlik kayıtlarını toplamak, okumak ve ilişkilendirmek için uygulamalı bir giriş.

![Log Analizine Giriş](https://github.com/user-attachments/assets/9b453562-c6f0-4205-a0df-de11f2dc453d)

Loglar; ne olduğunu, ne zaman olduğunu ve hangi sistemlerin veya hesapların olayla ilişkili olduğunu anlamamıza yardımcı olur. Bu ders, yaygın log kaynaklarını tanıtır ve sınıf örnekleri üzerinden e-posta analizi, log aramaları, paket incelemesi ve veri dönüştürme işlemlerini bir araya getirir.

## Öğrenme Hedefleri

![Öğrenme Hedefleri](https://github.com/user-attachments/assets/3e6834c3-5c80-4c37-9e02-576b6ffdf1ab)

Bu dersin sonunda:

- Loglama ve log analizini açıklayabileceksiniz.
- Yaygın log kaynaklarını tanıyabileceksiniz.
- Temel Windows ve Linux olaylarını yorumlayabileceksiniz.
- Toplama, ayrıştırma, normalleştirme ve ilişkilendirme kavramlarını açıklayabileceksiniz.
- E-posta başlıklarını, logları ve paket yakalamalarını araçlarla inceleyebileceksiniz.
- Gözlemlenen gerçekleri, doğrulama gerektiren bulgulardan ayırabileceksiniz.

Her olay kimliğini ezberlemeniz gerekmez. Nereye bakacağınızı ve hangi soruları soracağınızı bilmeye odaklanın.

## 1. Log Analizi Nedir?

![Log Analizi Nedir](https://github.com/user-attachments/assets/58e1e400-5f38-4957-a705-302d65251515)

Log; bir kullanıcının oturum açması, bir uygulamanın hata vermesi veya bir güvenlik duvarının bağlantıyı engellemesi gibi bir olayın kaydıdır.

Log analizi bir inceleme sorusuyla başlar. Analistler ilgili kayıtları ve olayın bağlamını inceleyerek kanıtların hangi sonuçları desteklediğini değerlendirir.

Bir alarm, dikkat gerektirebilecek etkinliği gösterir. Alarmı destekleyen olayların yine de incelenmesi gerekir.

### Yaygın Log Alanları

![Yaygın Log Alanları](https://github.com/user-attachments/assets/73122da5-6338-4339-8f1e-8989ac5ff809)

Farklı kaynaklar farklı alanlar kaydeder. Yaygın örnekler şunlardır:

| Alan | İncelemeye Katkısı |
|---|---|
| Zaman damgası | Olayın ne zaman kaydedildiği |
| Sistem | Olayı kaydeden veya olaydan etkilenen sistem |
| Hesap | Etkinlikle ilişkili hesap |
| Servis veya sağlayıcı | Kaydı oluşturan bileşen |
| Kaynak ve hedef | Bağlantının nereden geldiği ve nereye gittiği |
| Eylem veya sonuç | Ne olduğu ve işlemin başarılı olup olmadığı |
| Önem seviyesi | Kaynak tarafından atanan önem derecesi, varsa |

Kullanıcı adı ve IP adresi farklı bağlam bilgileri sağlar. Tek başlarına, eylemi hangi kişinin gerçekleştirdiğini kanıtlamazlar.

### Syslog Önem Seviyeleri

![Syslog Önem Seviyeleri](https://github.com/user-attachments/assets/8f18afc7-9109-4c97-932f-f48b166da89d)

| Seviye | Adı | Anlamı |
|---|---|---|
| 0 | Emergency | Sistem kullanılamaz durumda |
| 1 | Alert | Hemen müdahale gerekli |
| 2 | Critical | Kritik bir durum |
| 3 | Error | Bir hata durumu |
| 4 | Warning | Dikkat gerektiren bir durum |
| 5 | Notice | Normal fakat önemli bir olay |
| 6 | Informational | Sistem etkinliği hakkında bilgi |
| 7 | Debug | Ayrıntılı sorun giderme bilgisi |

**Sayı küçüldükçe önem seviyesi artar.** Bunlar syslog seviyeleridir; diğer platformlar farklı sınıflandırmalar kullanabilir.

## 2. Ham Veriden İnceleme Bulgularına

![Karmaşadan Anlamlı Bulgulara](https://github.com/user-attachments/assets/ade818d6-4a89-40ca-b46c-1009def23f83)

Tipik bir analiz süreci şu adımları içerir:

1. İlgili kayıtları toplayın.
2. İnceleme sorusuyla ilişkili olayları filtreleyin.
3. Kullanılacak alanları ayrıştırın.
4. Gerektiğinde alanları normalleştirin.
5. Farklı kaynaklardaki etkinlikleri ilişkilendirin.
6. Bir zaman çizelgesi oluşturun.
7. Kanıtların desteklediği bulguları ve yanıtlanmamış soruları açıklayın.

## 3. Loglama Nedir?

![Loglama Nedir](https://github.com/user-attachments/assets/1a7e0012-3af6-4acf-9b63-a2665cc23404)

Loglama, sistemlerde ve cihazlarda gerçekleşen etkinliklerin kaydedilmesidir. Sunucular, iş istasyonları, ağ cihazları, uygulamalar ve bulut hizmetleri log oluşturabilir.

Kaydedilen ayrıntılar yapılandırmaya bağlıdır. Bir bağlantı logu, paket içeriğini kaydetmeden adresleri ve portları gösterebilir.

### Sınıf Tartışması

![Loglama Örnekleri](https://github.com/user-attachments/assets/0cb7dc4e-f9e3-42e5-a2a4-199912b6616f)

Aşağıdaki kaynakların her birinden bir log örneği verin:

- Bir sunucu.
- Bir iş istasyonu.
- Bir ağ cihazı.

Dosya erişim olaylarının kaydedilmesi için denetimin etkinleştirilmesi gerekebilir. Bir sistem, hiç kaydetmediği bir olayı raporlayamaz.

Güvenlik duvarı bağlantı logu, bağlantı etkinliğini özetler. Paket yakalaması ise paket düzeyinde kanıt sağlar.

## 4. İnceleme Soruları

![İnceleme Soruları](https://github.com/user-attachments/assets/99575f89-c172-4dce-a848-5ee6a947fef5)

İncelemenizi yönlendirmek için şu soruları kullanın:

- Ne oldu?
- Ne zaman oldu?
- Hangi sistem olayla ilişkiliydi?
- Etkinlikle hangi hesap veya süreç ilişkiliydi?
- Bağlantı nereden geldi?
- İşlem başarılı oldu mu?
- Bu yorumu destekleyen başka hangi kanıtlar var?

Bir hesap adı tek başına kişisel sorumluluğu kanıtlamaz. Böyle bir sonuca varmadan önce ek kanıtları ilişkilendirin.

## 5. Log Analizi Neden Önemlidir?

![Log Analizinin Önemi](https://github.com/user-attachments/assets/a2572dab-feb5-475c-a3b8-f98a834521c5)

Loglar şu alanlarda yardımcı olur:

- Sorun giderme.
- Güvenlik incelemeleri.
- Olay müdahalesi.
- Operasyonel izleme.
- Denetim ve kayıt saklama gereksinimleri.

Bir uçağın uçuş kayıt cihazı gibi, loglar da sorun öncesindeki etkinlikleri yeniden anlamlandırmaya yardımcı olur.

Yalnızca log toplamak bir saldırıyı durdurmaz. Analistlerin kanıtları incelemesi ve uygun adımları atması gerekir.

## 6. Temel Terimler

![Temel Loglama Terimleri](https://github.com/user-attachments/assets/4181c7c2-dc14-4e00-bd58-9666c8398090)

Bir **log dosyası**, çok sayıda **log kaydı** içerebilir. Her kayıt, kaydedilmiş tek bir olayı açıklar.

Zaman damgasını doğru yorumlamak için yıl, saat dilimi ve diğer olaylarla ilişkisi hakkında yeterli bağlam gerekir.

![Toplama ve Analiz Terimleri](https://github.com/user-attachments/assets/33165d1d-8ea3-45a6-8bec-75806ea0cd3b)

| Terim | Anlamı |
|---|---|
| Toplama | Kaynaklardan kayıtların alınması |
| Birleştirme | Kayıtların bir araya getirilmesi |
| Ayrıştırma | Kayıtlardan alanların çıkarılması |
| Normalleştirme | Verilerin ortak bir yapıya eşlenmesi |
| İlişkilendirme | Bağlantılı olabilecek olayların birbirine bağlanması |

**Tartışma:** Windows ve Linux kimlik doğrulama kayıtlarını karşılaştırırken hangi adımlar yardımcı olur?

## 7. Yaygın Log Türleri ve Kaynakları

![Yaygın Log Türleri](https://github.com/user-attachments/assets/30e5e0d1-05f3-4666-9651-4d2b9967ffa6)

Uygulamalar, denetim sistemleri, güvenlik kontrolleri ve işletim sistemleri farklı kayıtlar oluşturur. Bu kategoriler birbiriyle örtüşebilir.

Örneğin bir hesap değişikliği, hem denetim olayı hem de güvenlik olayı olabilir.

![Ek Log Kaynakları](https://github.com/user-attachments/assets/dba98371-1c36-412b-a993-aaecf159fc3b)

Diğer kaynaklar şunlardır:

- Web erişim logları.
- Veritabanı denetim logları.
- Bulut etkinlik logları.
- Kimlik ve kimlik doğrulama logları.
- E-posta ağ geçidi logları.
- Uç nokta telemetrisi.

Her kaynak ek bağlam sağlar. Veritabanı sorguları ve diğer ayrıntılı etkinlikler için özel loglama ayarları gerekebilir.

## 8. Log Toplama

![Log Toplama Süreci](https://github.com/user-attachments/assets/156c77a6-51ad-44ce-b9b2-106eefe6c621)

İyi bir analiz, güvenilir veri toplamayla başlar.

1. Kaynakları ve inceleme gereksinimlerini belirleyin.
2. Desteklenen bir toplama yöntemi seçin.
3. İlgili olayları ve güvenli veri aktarımını yapılandırın.
4. Sistem saatlerini eşitleyin ve saat dilimlerini kaydedin.
5. Bir test olayı oluşturun.
6. Kaydın ulaştığını, eksiksiz olduğunu ve saklandığını doğrulayın.

Toplama yöntemleri arasında rsyslog, syslog-ng, Splunk forwarder, Windows Event Forwarding ve bulut API’leri bulunabilir.

![Log Toplama Adımları](https://github.com/user-attachments/assets/39df76ce-4358-454d-aaf8-c0be57a20acb)

Yapılandırılmış bir veri aktarım süreci yine de doğrulanmalıdır. Beklenen olayın kullanılabilir alanlar ve zaman damgalarıyla toplayıcıya ulaştığını kontrol edin.

## 9. Log Normalleştirme

![Log Normalleştirme](https://github.com/user-attachments/assets/3bf01a1d-52f3-4b48-b01b-bed844633dbb)

Farklı sistemler farklı alan adları ve biçimler kullanır. Normalleştirme, ilgili etkinlikleri karşılaştırabilmek için bu alanları ortak bir yapıya eşler.

Splunk eklentileri ve Common Information Model (CIM) eşlemeleri, arama sırasında normalleştirmeyi destekleyebilir. Uygun yapılandırma gerekir.

## 10. Manuel ve Otomatik Analiz

### Manuel Log Analizi

![Manuel Log Analizi](https://github.com/user-attachments/assets/28cfb36b-e876-4600-b14f-9cf2a57b66e8)

Manuel analiz, kayıtların aşağıdaki gibi araçlarla doğrudan incelenmesidir:

```bash
grep
less
cat
head
tail
```

Belirli bir konuya odaklanan incelemeler ve tek tek kayıtların ayrıntılı değerlendirilmesi için yararlıdır.

### Otomatik Log Analizi

![Otomatik Log Analizi](https://github.com/user-attachments/assets/763c1094-c2b2-4d41-98ad-4f31f4e64cd5)

Otomatik araçlar, büyük miktarda olayın toplanmasına, aranmasına, filtrelenmesine, görselleştirilmesine ve alarm oluşturulmasına yardımcı olur.

Bir alarm incelemeyi başlatır. Etkinliği sınıflandırmadan önce destekleyen kayıtları ve bağlamı değerlendirin.

## 11. Windows Güvenlik Logları

![Windows Güvenlik Logları](https://github.com/user-attachments/assets/128e47cb-98eb-468d-a9c5-bfec0cf84a52)

Windows Güvenlik logları; oturum açma, hesap kilitlenmesi, hesap değişiklikleri ve diğer denetlenen etkinlikleri kaydedebilir.

Şu yolu açın:

```text
Event Viewer > Windows Logs > Security
```

Kaydedilen olaylar denetim ayarlarına bağlıdır. Olay kimliğini; hesap, zaman damgası, kaynak ve sonuçla birlikte değerlendirin.

### Event Viewer Arayüzü

![Event Viewer Arayüzü](https://github.com/user-attachments/assets/3acd1417-6430-4509-81bf-c9809a031ff7)

| Bölüm | Amaç |
|---|---|
| Gezinti | Log kategorilerini ve özel görünümleri seçmek |
| Ana bölüm | Olayları, özetleri ve seçilen olayın ayrıntılarını incelemek |
| Eylemler | Filtreleme, dışa aktarma ve görünüm yönetimi |

### Başarısız Oturum Açma Örneği

![Windows Başarısız Oturum Açma Örneği](https://github.com/user-attachments/assets/fceae1e4-57fb-4aaa-a5be-c43751cb9095)

**4625** numaralı olay, başarısız bir oturum açmayı kaydeder. Oturum açma türü **10**, RemoteInteractive etkinliğini ifade eder.

Kimlik doğrulamanın neden başarısız olduğunu değerlendirmeden önce **Failure Reason**, **Status** ve **SubStatus** alanlarını inceleyin.

Tek bir başarısız oturum açma, saldırı olduğunu kanıtlamaz.

### İnceleme Başlangıç Noktası Olarak Olay Kimlikleri

![Windows Kimlik Doğrulama Olay Kimlikleri](https://github.com/user-attachments/assets/e3ebddf5-d22d-4cd3-89fa-9b502d4e0cda)

Başarılı ve ayrıcalıklı oturum açmalar rutin olabilir. Hesabı, sistemi, zamanı ve çevresindeki olayları karşılaştırın.

![Windows Hesap ve Süreç Olayları](https://github.com/user-attachments/assets/ffa250cb-5eb3-4ea2-b53b-16530410c4d9)

Süreç oluşturma ve hesap değişiklikleri, oturum açma sonrasındaki etkinlikleri açıklamaya yardımcı olabilir.

- Windows **4688**, süreç oluşturma denetiminin etkin olmasını gerektirir.
- Komut satırı ayrıntıları için ek yapılandırma gerekir.
- Windows **1102**, Güvenlik denetim logunun temizlenmesini kaydeder.
- Sysmon **1**, süreç oluşturmayı kaydeder.
- Sysmon **3**, etkinleştirildiğinde ağ bağlantılarını kaydeder.

Windows ve Sysmon farklı olay sağlayıcıları kullanır. Olay kimlikleri birbirinin yerine kullanılamaz.

![Windows Olay Kimliği Referansı](https://github.com/user-attachments/assets/6344b56a-01ab-4755-86c5-d0a4b41de032)

Kaynak: [Microsoft — Event Viewer](https://learn.microsoft.com/en-us/shows/inside/event-viewer)

## 12. Linux Logları

![Yaygın Linux Log Konumları](https://github.com/user-attachments/assets/6d71b985-00aa-4976-b330-f053b8eef845)

Linux log konumları, dağıtıma ve loglama yapılandırmasına bağlıdır.

| Konum veya Komut | Yaygın Kullanım |
|---|---|
| `/var/log/auth.log` | Uygun yapılandırılmış Debian/Ubuntu sistemlerinde kimlik doğrulama |
| `/var/log/secure` | Uygun yapılandırılmış RHEL benzeri sistemlerde kimlik doğrulama |
| `/var/log/syslog` veya `/var/log/messages` | Genel sistem ve servis mesajları |
| `/var/log/audit/audit.log` | Etkinleştirildiğinde auditd olayları |
| `journalctl` | systemd journal kayıtlarını sorgulamak |
| `journalctl -k` | Çekirdek journal mesajlarını sorgulamak |
| `dmesg` | Çekirdek halka tamponunu görüntülemek |

### Linux auth.log Örneği

![Linux Kimlik Doğrulama Log Örneği](https://github.com/user-attachments/assets/7cfd8313-be00-42d0-ab39-6e0e8fb710c4)

```text
Apr 7 10:42:15 ubuntu sshd[12345]: Failed password for invalid user C3team from 192.168.1.100 port 54321 ssh2
```

| Alan | Değer | Anlamı |
|---|---|---|
| Zaman damgası | `Apr 7 10:42:15` | Kaydedilen tarih ve saat; yıl ve saat dilimi yok |
| Sistem adı | `ubuntu` | Kaydı oluşturan sistem |
| Servis ve PID | `sshd[12345]` | SSH sunucu süreci ve süreç kimliği |
| Kimlik doğrulama sonucu | `Failed password` | Parola ile kimlik doğrulama başarısız |
| Denenen kullanıcı adı | `C3team` | Bu denemede geçersiz kabul edilen kullanıcı adı |
| Kaynak IP | `192.168.1.100` | Sunucunun gördüğü bağlantı kaynağı |
| Kaynak port | `54321` | İstemci tarafındaki bağlantı portu |
| Protokol | `ssh2` | SSH sürüm 2 |

Tekrarlanan başarısızlıkları, denenen diğer kullanıcı adlarını ve yakın zamandaki başarılı oturum açmaları kontrol edin. Kayıtları ilişkilendirmeden önce yılı, saat dilimini ve sistem saatinin doğruluğunu teyit edin.

### Windows ve Linux Karşılaştırması

![Windows ve Linux Log Karşılaştırması](https://github.com/user-attachments/assets/3c2bdb0c-2d68-4e6b-9db0-8635be2227d3)

Her iki platform da inceleme için yararlı kanıtlar sağlar. Windows genellikle yapılandırılmış olaylar ve olay kimlikleri kullanır. Linux ise metin dosyaları veya systemd journal kullanabilir.

Her iki platformda da aynı inceleme sorularını uygulayın. Tutarlı alanlar ve doğru zaman damgaları ilişkilendirmeyi kolaylaştırır.

Kaynak: [systemd — journalctl](https://www.freedesktop.org/software/systemd/man/latest/journalctl.html)

## 13. Uygulama 1 — Kimlik Avı E-postası İncelemesi

![Kimlik Avı E-postası İncelemesi](https://github.com/user-attachments/assets/eaa99dd0-d69c-405d-be1c-22389bbad322)

İncelemeye e-postanın kendisinden başlayın:

- Görünen adı, From ve Reply-To alanlarını karşılaştırın.
- Bağlantının gerçek hedefini ve ek ayrıntılarını inceleyin.
- Aciliyet ifadelerine, olağan dışı taleplere ve uyuşmayan alan adlarına bakın.
- Alıcıyı, mesaj kimliğini ve teslim zamanını kaydedin.
- Orijinal mesajı ve başlıklarını koruyun.

Bu uygulamada sınıf için hazırlanmış sentetik mesajı kullanın.

### E-posta Başlığı Analizi

![E-posta Başlığı Analizi](https://github.com/user-attachments/assets/779dcb0e-2140-4746-b317-601dbd24824c)

E-posta başlıkları, iletim yolu ve kimlik doğrulama hakkında ipuçları sağlar.

**Received** kayıtlarını, listelenen en eski aktarım noktasından başlayarak aşağıdan yukarıya okuyun. Güvenilmeyen alanlar sahte olabileceği için güvenilir e-posta sistemlerinin eklediği kayıtlara odaklanın.

**SPF, DKIM ve DMARC**, belirli kimlik doğrulama kontrolleri gerçekleştirir. Başarılı sonuçlar, e-postanın güvenli olduğunu kanıtlamaz.

### MxToolbox Gösterimi

![MxToolbox E-posta Başlığı Gösterimi](https://github.com/user-attachments/assets/f72965b6-7081-4373-8688-5d70020dd469)

1. [Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx) aracını açın.
2. Verilen sentetik başlığı yapıştırın.
3. Aktarım noktalarını ve zaman farklarını inceleyin.
4. Göndericiyle ilgili alanları karşılaştırın.
5. Kimlik doğrulama sonuçlarını inceleyin.
6. Bir uyuşmazlık ve doğrulama gerektiren bir bulgu kaydedin.

**Soru:** SPF kontrolünün başarılı olması tek başına e-postayı güvenli yapar mı?

**Beklenen cevap:** Hayır.

## 14. Splunk — Ham Olaylar ve Çıkarılan Alanlar

![Splunk Ham Olaylar ve Çıkarılan Alanlar](https://github.com/user-attachments/assets/e32139cb-8348-41b9-abed-13adb825a0c4)

Splunk, ham kayıtlara erişimi korurken büyük miktarda olayın aranmasını sağlar.

Çıkarılan alanlar, sonuçların filtrelenmesini ve ilgili etkinliklerin ilişkilendirilmesini kolaylaştırır. Kullanılabilir alanlar, veri alımı ve alan çıkarma ayarlarına bağlıdır.

### Laboratuvar Görevleri

- Sınıfın Splunk laboratuvarını açın.
- Ham olayı inceleyin.
- `host`, `source` ve `sourcetype` alanlarını belirleyin.
- Çıkarılan alanları orijinal kayıtla karşılaştırın.
- Göndericiyi, alıcıyı, bağlantıyı, eki ve teslim durumunu inceleyin.

**Temel nokta:** `delivery_status=delivered`, teslim durumunu açıklar; içeriğin güvenilir olduğunu göstermez.

Kaynak: [Splunk — Arama Sırasında CIM ile Verileri Normalleştirme](https://help.splunk.com/en/data-management/common-information-model/6.0/using-the-common-information-model/use-the-cim-to-normalize-data-at-search-time)

## 15. Wireshark — Ağ Trafiğinin Ön İncelemesi

![Wireshark Ağ Trafiği Ön İncelemesi](https://github.com/user-attachments/assets/190f864e-049e-4ab9-b156-a6537491dc82)

Wireshark, paket yakalamalarını analiz eder. PCAP; uç nokta, güvenlik duvarı ve uygulama loglarıyla ilişkilendirilebilen paket düzeyinde kanıt sağlar.

| İnceleme Odağı | Görüntüleme Filtresi |
|---|---|
| Belirli bir sistem | `ip.addr == 192.168.1.10` |
| HTTP trafiği | `http` |
| TCP port 443 | `tcp.port == 443` |
| Paket baytlarında anahtar kelime | `frame contains "login"` |

Bir filtre eşleşmesi, bağlamla değerlendirilmesi gereken bir ipucudur.

### Oturum İncelemesi

![Wireshark Oturum İncelemesi](https://github.com/user-attachments/assets/420054e4-1305-4117-ba91-1b8d921e00c1)

Şunları inceleyin:

- TCP el sıkışması.
- HTTP isteğinin URI değeri.
- Yanıt içeriği ve aktarım boyutu.
- Oturumun kapatılmaya başlaması.
- İlgili uç nokta ve uygulama etkinlikleri.

### Gösterim Görevleri

1. **View > Time Display Format** bölümünden tutarlı bir zaman gösterimi seçin.
2. **Statistics > Conversations** bölümünü açın.
3. İncelenen iş istasyonunun iletişimlerini belirleyin.
4. İlgili akışı ve çevresindeki paketleri inceleyin.

İçeriğin görünürlüğü; şifrelemeye, yakalama noktasına ve yakalamanın eksiksizliğine bağlıdır.

## 16. CyberChef — Veri Dönüştürme ve Ön İnceleme

![CyberChef Log Analizi](https://github.com/user-attachments/assets/8f85a312-2cd8-4796-a2eb-bb10dd7d22c7)

CyberChef; inceleme sırasında kod çözme, veri çıkarma, ayrıştırma ve dönüştürme işlemlerini destekler.

| İşlem | İncelemede Kullanımı |
|---|---|
| From Base64 | Kodlanmış içeriği incelemek için çözmek |
| Extract IP Addresses | Doğrulanacak aday IP adreslerini belirlemek |
| Parse DateTime | Zaman damgalarını tutarlı ve okunabilir biçime dönüştürmek |
| Find / Highlight | İlgili terimleri ve örüntüleri bulmak |

Dönüştürülen çıktıyı orijinal girdiyle karşılaştırarak doğrulayın.

### Sentetik Log Veri Seti

Aşağıdaki kayıtlar, ürünlerden alınmış yerel log çıktıları yerine sadeleştirilmiş sınıf örnekleridir.

```text
Jul 07 09:30:15 mail-gateway postfix/smtpd[12345]: from=<payments@finance-invoice-2026.com>, to=<john.nash@cybertechllc.com>, message-id=<20260703093015.abc123@finance-invoice-2026.com>, status=sent, relay=10.10.15.23[10.10.15.23]:25, size=5832

Jul 07 09:30:16 mail-gateway spamd[23412]: Email from payments@finance-invoice-2026.com scored 5.2 (Phishing heuristics + Suspicious link), severity=medium

Jul 07 09:30:18 webproxy01 squid[41251]: CONNECT finance-invoice-2026.com:80 [185.72.94.11] user=john.nash@cybertechllc.com uri=/invoice-access

Jul 07 09:30:19 webproxy01 squid[41251]: GET http://finance-invoice-2026.com/invoice-access attachment=invoice_98231.pdf status=200 size=87233 content-type=application/pdf

Jul 07 09:30:21 endpoint-win10 Sysmon EventID=1: Process Create - Image=powershell.exe

Jul 07 09:30:25 endpoint-win10 Sysmon EventID=3: Network Connection - Image=powershell.exe DestinationIp=185.72.94.11 DestinationPort=80 DestinationHostname=finance-invoice-2026.com
```

**İnceleme odağı:** Zaman damgalarını ve ilgili alanları kullanarak e-posta, proxy isteği ve uç nokta etkinliğini ilişkilendirin. Olayların zaman olarak yakın olması, PDF dosyasının PowerShell’in çalışmasına neden olduğunu tek başına kanıtlamaz.

### Alan Adı Çıkarma

![CyberChef Alan Adı Analizi](https://github.com/user-attachments/assets/05cba7bc-9169-4979-8501-aefe3c6f36d2)

Aday alan adı ifadelerini belirlemek için CyberChef’in yerleşik **Domain** düzenli ifadesini kullanın.

**Email address**, **URL** ve **Windows file path** gibi ek örüntüler, ilişkilendirme için kullanılacak değerleri bulmaya yardımcı olabilir.

Her eşleşmeyi orijinal kayıtla doğrulayın. Bir düzenli ifade eşleşmesi, değerin kötü amaçlı bir gösterge olduğunu tek başına kanıtlamaz.

## 17. İncelemeden Çıkarılacak Temel Noktalar

- Açık bir soruyla başlayın.
- Kaynak yapılandırmasını ve kanıtların eksiksizliğini kontrol edin.
- Orijinal kayıtları koruyun.
- İlişkilendirmeden önce zaman damgalarını uyumlu hale getirin.
- Farklı kaynaklardaki kanıtları karşılaştırın.
- Gerçekleri varsayımlardan ayırın.
- Yanıtlanmamış soruları ve sonraki adımları belgeleyin.

## Teşekkürler

![Teşekkürler](https://github.com/user-attachments/assets/58a39ef3-7bb5-4bf6-81f7-5f035287687b)
