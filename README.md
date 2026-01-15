# WPLSoft_To_Arduino
WPLSoft Ladder To Arduino UNO programmer
Karmaşık Kodlara Son! WPLSoft ve Ladder Diyagramı ile Arduino Programlama
    Yıllardır kumanda panoları kuran, Delta PLC dünyasına aşina olan bir elektrik teknisyeniyseniz, Arduino'nun dünyası size karmaşık görünebilir. Ancak artık void setup() veya void loop() satırları arasında kaybolmanıza gerek yok. Türkiye'de en yaygın kullanılan ve kaynağı en bol olan WPLSoft 2.0 arayüzünü kullanarak, tıpkı bir PLC programlar gibi Arduino'yu programlayacağız. Amacım, elinizdeki o güçlü mikrodenetleyiciyi, bildiğiniz 'elektrik dili' ile kontrol etmenizi sağlamak.


WPLSoft Ladder Örnek

WPLSoft IL dönüşümü


Arduino UNO/MEGA Kod dönüşüm Arayüzü


IL to Arduino penceresi

🛠️ Yazılım Arayüzü: Karmaşıklıktan Sadeliğe
Geliştirdiğim arayüz, WPLSoft'un endüstriyel gücü ile Arduino'nun esnekliğini tek bir ekranda birleştiriyor. Arayüzü, iş akışını en kolay hale getirecek şekilde iki ana bölüme ayırdım:

1. Sol Panel: Veri Girişi ve Dönüştürme
Bu bölüm, Delta WPLSoft'tan aldığımız verilerin sisteme dahil edildiği yerdir:

IL Kod Yükle: WPLSoft üzerinde yazdığınız programın .csv formatındaki Instruction List (Komut Listesi) dosyasını bu butonla yazılıma aktarırsınız.

Arduino Çevir: Bu buton, projenin kalbidir. Yüklenen PLC komutlarını saniyeler içinde Arduino'nun anlayacağı C++ kodlarına dönüştürür ve sağdaki panelde görmenizi sağlar.

2. Sağ Panel: Kod Yönetimi ve Yükleme
Dönüştürülen kodun son kontrollerinin yapıldığı ve donanıma aktarıldığı merkezdir. Burası üç fonksiyonel satırdan oluşur:

Üst Satır (Kontrol Paneli):

Board Seç & Com Port: Kullanacağınız Arduino modelini (Uno, Mega vb.) ve bağlantı portunu seçersiniz.

Kod Kontrol & Arduino Yükle: Kodun doğruluğunu test eder ve doğrudan Arduino'ya gönderirsiniz.

.ino Kaydet: Dönüştürülen kodu daha sonra Arduino IDE ile açmak isterseniz bilgisayarınıza kaydeder.

Seri Terminal: Arduino ile bilgisayar arasındaki iletişimi canlı olarak izlemenizi sağlar.

Orta Bölüm (Kod Editörü): Çevrilen Arduino kodlarının görüntülendiği alandır. Burada isterseniz manuel düzeltmeler de yapabilirsiniz.

Alt Bölüm (Terminal): Derleme sürecini, hataları veya yükleme durumunu anlık olarak buradan takip edebilirsiniz.

Bu arayüzün tasarımı, bir elektrikçinin aşina olduğu "Giriş -> İşlem -> Çıkış" mantığına göre kurgulanmıştır. Sol taraf "Giriş" (IL Kodları), sağ taraf ise "Çıkış" (Arduino Programı) olarak düşünülebilir. 

📑 Adım Adım: WPLSoft Projesini Arduino'ya Hazırlama
WPLSoft'ta yazdığınız Ladder (Merdiven) diyagramını, hazırladığımız arayüze aktarmak için "Instruction List" (Komut Listesi) olarak dışa aktarmamız gerekiyor. İşte yapmanız gerekenler:

1. Programınızı Derleyin (Compile)
WPLSoft'ta çiziminizi bitirdikten sonra mutlaka "Ctrl + F7" tuşlarına basarak veya araç çubuğundaki "Ladder to Instruction" ikonuna tıklayarak programı derleyin. Hata almadığınızdan emin olun.

2. Komut Listesi (IL) Görünümüne Geçin
Programınız Ladder modundayken, üst menüden "View" (Görünüm) sekmesine gelin ve "Instruction List" seçeneğini seçin. Artık çiziminizin kod satırlarına dönüştüğünü göreceksiniz.




3. CSV Olarak Kaydetme
Hazırladığım arayüzün bu kodları okuyabilmesi için dosyayı Excel'in de tanıyabildiği .csv formatında kaydetmelisiniz:

Fare Sağ Click menüsüne gidin.

Tümünü Seç



Dosya türü olarak CSV (Comma Delimited) seçtiğinizden emin olun.



Kaydet penceresi açılır. dosya adını yazıp uzantı "Kayıt Türü "olarak "CSV" seçili oladuğundan emin olunuz. 

Kaydet dügmesine tıklayarak IL dosyamızı disk üzerine kayıt işlemini tamamlayalaım. sonra açılan bilgi kutucuguna Tamam diyerek işlemi tamamlayalım.
🚀 Şimdi Yazılımımıza Geçebiliriz!
CSV dosyanız hazırsa, hazırladığım "IL to Arduino" arayüzünü açın:

"IL Kod Yükle" butonuna basın ve kaydettiğiniz CSV dosyasını seçin.



Sol panelde kodların sıralandığını göreceksiniz.



"Arduino Çevir" butonuna bastığınız anda, yılların Delta tecrübesi artık bir Arduino koduna dönüşmüş olacak! Varsayılan olarak Arduino UNO modeli için kod çevrilir. Çeviri işlemi tamamlandıktan sonra Board Seç başlığı altından "NANO, UNO, MEGA" bordlarından farklı bord seçimi yapılıp yeniden SOL pencereden "Arduino Çevir" butonu tıklanarak 

tekrar kod dönüşümü yapılabilir.
Derleme işlemi doğrudan "Kod Kontrol" düğmesi kullanılarak DERLEME işlemi yapılır. Derleme den sonra hatalı satır var ise Kırmızı renk ile hatalı satır işaretlenir ve Terminal ekranında hata açıklaması Türkçe ve İngilizce olarak ayrı ayrı gösterilir. Hata düzeltmesi doğrudan kod penceresi üzerinde düzeltip yeniden derleme yapılabilir.


Derleme İşlem Sonu Pencere Görüntüsü
Hata düzeltme durumu örnek olması için bazı hatalar yapacağım noktalı virgül unutması veya komut yazım hataları gibi ekran görüntüleri aşagıda dır;
result = M[0] satırının sonunda ki ';' noktalı virgülü sildim ve derledim.


if(result) { D[10] = 100; } satırında if deyiminden i harfinin yazılamadığını varsaydım:

Hata kontrol işlemi yukarıdan aşağıya doğru satır satır yapılmaktadır. bir den fazla hata olsa dahi ilk bulduğu hatayı işaretli olarak gösterir. Her hata düzeltildikten sonraki "Kod Kontrol" butonu ile yeniden DERLEME yapılmalıdır. En son hata düzeltilene kadar bu işleme devam edilmelidir.
