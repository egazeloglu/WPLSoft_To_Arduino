# IL_To_Arduino (Instruction List ile Arduino)
Instruction List ile Ladder To Arduino UNO programmer
Karmaşık Kodlara Son! Instruction List ve Ladder Diyagramı ile Arduino Programlama
    Yıllardır kumanda panoları kuran, PLC dünyasına aşina olan bir elektrik teknisyeniyseniz, Arduino'nun dünyası size karmaşık görünebilir. Ancak artık void setup() veya void loop() satırları arasında kaybolmanıza gerek yok. Türkiye'de en yaygın kullanılan ve kaynağı en bol olan Ladder dan Instruction List kodlarına dönüşüm yapabilen programların arayüzünü kullanarak, tıpkı bir PLC programlar gibi Arduino'yu programlayacağız. Amacım, elinizdeki o güçlü mikrodenetleyiciyi, bildiğiniz 'elektrik dili' ile kontrol etmenizi sağlamak.


WPLSoft Ladder Örnek, WPLSoft IL dönüşümü
000000,LDP,X0
000003,SET,M1
000004,LDF,X1
000007,RST,M1
000010,LDP,M1
000013,MOV,K100,D0
000018,ADD,D1,K1,D1
000025,OUT,M0
000026,LDP,M0
000029,INC,D2
000032,LD=,K100,D0
000037,OUT,M0
000038,LD>=,D1,K5
000043,RST,M0
000046,MOV,K0,D1
000051,MOV,K0,D0
000056,LD,M0
000057,OUT,Y3
000058,END



Arduino UNO/MEGA Kod dönüşüm Arayüzü
void loop() {
  //  GIRIS SCAN ---
  X0 = !digitalRead(Pin_X0);
  X1 = !digitalRead(Pin_X1);

  updateClocks(); // M1011, M1012, M1013 saat üretimleri ---
// Sistem Özel Röle Güncellemeleri ---
  if (firstScan) { M1002 = true; M1003 = false; }
  else { M1002 = false; M1003 = true; }
// !!! /////////////////// ANA PROGRAM ///////////////////  ---
// LDP Komutu ---
  curr = X0; result = (curr && !X0_LastState); X0_LastState = curr; 
// SET Komutu ---
 if( result) { M[1] = HIGH; }
// LDF Komutu ---
 { curr = X1; result = (!curr && X1_LastState); X1_LastState = curr; }
// RST Komutu ---
 if(result) {
  M[1] = false; 
  M1_LastState = false; // Her PLS değişkeni için ---
 }
// LDP Komutu ---
  curr = M[1]; result = (curr && !M1_LastState); M1_LastState = curr; 
// MOV Komutu ---
 if(result) { D[0] = 100; }
// ADD Komutu ---
 if(result) { D[1] = D[1] + 1; }
// OUT Komutu ---
  M[0] = result;
// LDP Komutu ---
  curr = M[0]; result = (curr && !M0_LastState); M0_LastState = curr; 
// INC Komutu ---
 if(result) { D[2] = D[2] + 1; }
// LD= Komutu ---
 result = (100 == D[0]);
// OUT Komutu ---
  M[0] = result;
// LD>= Komutu ---
 result = (D[1] >= 5);
// RST Komutu ---
 if(result) {
  M[0] = false; 
  M0_LastState = false; // Her PLS değişkeni için ---
 }
// MOV Komutu ---
 if(result) { D[1] = 0; }
// MOV Komutu ---
 if(result) { D[0] = 0; }
// LD Komutu ---
 result = M[0];
// OUT Komutu ---
  Y3 = result;
// END Komutu ---
 //  ÇIKIŞ REFRESH ---
  // RAM'deki Y0...Yn durumlarını fiziksel pinlere aktar ---
  digitalWrite(Pin_Y0, Y0);
 firstScan = false;
 stackPtr = 0; // Stack temizle ---
 return; // Döngüden çık ve Output Refresh'e git ---
// !!! ////////////////// PROGRAM SONU /////////////////////////
}

🛠️ Yazılım Arayüzü: Karmaşıklıktan Sadeliğe
Geliştirdiğim arayüz, Instruction List Kod Editörü 'un endüstriyel gücü ile Arduino'nun esnekliğini tek bir ekranda birleştiriyor. Arayüzü, iş akışını en kolay hale getirecek şekilde iki ana bölüme ayırdım:

1. Sol Panel: Veri Girişi ve Dönüştürme
Bu bölüm, Delta Instruction List Kod Editörü'den aldığımız verilerin sisteme dahil edildiği yerdir:

IL Kod Yükle: Instruction List Kod Editörü (PLC programlama Yazılımı) üzerinde yazdığınız programın .csv, .txt formatındaki Instruction List (Komut Listesi) dosyasını bu butonla yazılıma aktarırsınız.

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

📑 Adım Adım: Instruction List (IL) Kod Editörü Projesini Arduino'ya Hazırlama
Instruction List (IL) Kod Editörü 'de yazdığınız Ladder (Merdiven) diyagramını, hazırladığımız arayüze aktarmak için "Instruction List" (Komut Listesi) olarak dışa aktarmamız gerekiyor. İşte yapmanız gerekenler:

1. Programınızı Derleyin (Compile)
Instruction List (IL) Kod Editörü 'de çiziminizi bitirdikten sonra mutlaka "Ctrl + F7" tuşlarına basarak veya araç çubuğundaki "Ladder to Instruction" ikonuna tıklayarak programı derleyin. Hata almadığınızdan emin olun.

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

IL_To_Arduino indir (Download)
https://drive.google.com/file/d/1fNcPnl6GMbmaJXXALc1zNqiBhfRGCJ0s/view?usp=sharing
