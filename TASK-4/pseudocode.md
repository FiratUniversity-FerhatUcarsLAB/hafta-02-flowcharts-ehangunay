Harika bir konu! Üniversite ders kayıt sistemleri, birçok kuralı ve kontrolü barındıran önemli sistemlerdir. İşte bu sürecin temel adımlarını ve mantığını yansıtan detaylı bir sözde kod:

Bu kod, bir öğrencinin sisteme giriş yapmasından, ders seçip danışman onayına göndermesine kadar olan ana işlevleri basit ve anlaşılır bir dille açıklar.

BAŞLA

  // Veri Yapıları ve Değişkenler
  TANIMLA Ders:
    ders_kodu
    ders_adı
    kredi
    kontenjan
    kayıtlı_öğrenci_sayısı
    on_kosullar // ["MAT101", "FIZ101"] gibi ders kodlarını içeren liste
    ders_programı // {gun: "Pazartesi", baslangic_saati: "09:00", bitis_saati: "12:00"}
  SON TANIMLA

  TANIMLA Ogrenci:
    ogrenci_no
    ad_soyad
    max_kredi_limiti
    tamamladigi_dersler // ["MAT101", "KIM101"] gibi ders kodlarını içeren liste
    sectigi_dersler_listesi // Ders nesnelerini içeren liste
  SON TANIMLA

  // Örnek Veri Oluşturma
  OLUŞTUR MevcutDerslerListesi [
    {ders_kodu: "CS101", ders_adı: "Bilgisayar Bilimlerine Giriş", kredi: 6, kontenjan: 50, kayıtlı_öğrenci_sayısı: 25, on_kosullar: [], ders_programı: {gun: "Pazartesi", baslangic_saati: "09:00", bitis_saati: "12:00"}},
    {ders_kodu: "MAT102", ders_adı: "İleri Matematik", kredi: 5, kontenjan: 40, kayıtlı_öğrenci_sayısı: 40, on_kosullar: ["MAT101"], ders_programı: {gun: "Salı", baslangic_saati: "13:00", bitis_saati: "16:00"}},
    {ders_kodu: "PHY102", ders_adı: "Modern Fizik", kredi: 5, kontenjan: 30, kayıtlı_öğrenci_sayısı: 15, on_kosullar: ["FIZ101", "MAT101"], ders_programı: {gun: "Pazartesi", baslangic_saati: "10:00", bitis_saati: "13:00"}}
  ]

  OLUŞTUR aktif_ogrenci NESNESİ {ogrenci_no: "20251101", ad_soyad: "Ayşe Vural", max_kredi_limiti: 30, tamamladigi_dersler: ["MAT101", "FIZ101"], sectigi_dersler_listesi: []}
  
  // Ana İşlem Döngüsü
  YAZDIR "Ders Kayıt Sistemine Hoş Geldiniz, " + aktif_ogrenci.ad_soyad
  
  DÖNGÜ SÜRESİNCE (Kullanıcı "Kaydı Tamamla" demedikçe)
  
    YAZDIR "1: Açılan Dersleri Görüntüle"
    YAZDIR "2: Ders Ekle"
    YAZDIR "3: Ders Sil"
    YAZDIR "4: Seçilen Dersleri Görüntüle"
    YAZDIR "5: Danışman Onayına Gönder ve Çık"
    OKU kullanici_secimi

    EĞER kullanici_secimi == 1 İSE
      // Açılan dersleri listele
      HER Ders İÇİN MevcutDerslerListesi'NDE
        YAZDIR Ders.ders_kodu + " - " + Ders.ders_adı + " (Boş Kontenjan: " + (Ders.kontenjan - Ders.kayıtlı_öğrenci_sayısı) + ")"
      SON DÖNGÜ

    DEĞİLSE EĞER kullanici_secimi == 2 İSE
      // Ders Ekleme Süreci
      YAZDIR "Eklemek istediğiniz dersin kodunu giriniz:"
      OKU eklenecek_ders_kodu
      BUL eklenecek_ders MevcutDerslerListesi'NDEN ders_kodu'su eklenecek_ders_kodu OLAN

      EĞER eklenecek_ders YOKSA
        YAZDIR "Hata: Ders bulunamadı."
      DEĞİLSE
        // KONTROL 1: Kredi Limiti
        mevcut_kredi_toplami = 0
        HER SeciliDers İÇİN aktif_ogrenci.sectigi_dersler_listesi'NDE
          mevcut_kredi_toplami = mevcut_kredi_toplami + SeciliDers.kredi
        SON DÖNGÜ
        EĞER (mevcut_kredi_toplami + eklenecek_ders.kredi) > aktif_ogrenci.max_kredi_limiti İSE
          YAZDIR "Hata: Maksimum kredi limitini aşıyorsunuz."
        // KONTROL 2: Ön Koşullar
        DEĞİLSE EĞER TÜM(eklenecek_ders.on_kosullar) aktif_ogrenci.tamamladigi_dersler İÇİNDE DEĞİLSE
          YAZDIR "Hata: Bu dersin ön koşullarını sağlamıyorsunuz."
        // KONTROL 3: Kontenjan
        DEĞİLSE EĞER eklenecek_ders.kayıtlı_öğrenci_sayısı >= eklenecek_ders.kontenjan İSE
          YAZDIR "Hata: Dersin kontenjanı dolu."
        // KONTROL 4: Ders Çakışması
        DEĞİLSE EĞER eklenecek_ders'in ders_programı İLE aktif_ogrenci.sectigi_dersler_listesi'NDEKİ bir dersin programı çakışıyorsa
          YAZDIR "Hata: Ders programınızda çakışma var."
        DEĞİLSE
          // Tüm kontrollerden geçtiyse dersi ekle
          EKLE eklenecek_ders'i aktif_ogrenci.sectigi_dersler_listesi'ne
          eklenecek_ders.kayıtlı_öğrenci_sayısı = eklenecek_ders.kayıtlı_öğrenci_sayısı + 1
          YAZDIR eklenecek_ders.ders_adı + " dersi başarıyla eklendi."
        SON EĞER
      SON EĞER

    DEĞİLSE EĞER kullanici_secimi == 3 İSE
      // Ders Silme
      YAZDIR "Silmek istediğiniz dersin kodunu giriniz:"
      OKU silinecek_ders_kodu
      BUL silinecek_ders aktif_ogrenci.sectigi_dersler_listesi'NDEN ders_kodu'su silinecek_ders_kodu OLAN
      
      EĞER silinecek_ders VARSA
        ÇIKAR silinecek_ders'i aktif_ogrenci.sectigi_dersler_listesi'nden
        // Kontenjanı geri artır
        silinecek_ders.kayıtlı_öğrenci_sayısı = silinecek_ders.kayıtlı_öğrenci_sayısı - 1
        YAZDIR "Ders başarıyla silindi."
      DEĞİLSE
        YAZDIR "Hata: Bu ders seçili değil."
      SON EĞER

    DEĞİLSE EĞER kullanici_secimi == 4 İSE
      // Seçilen dersleri ve toplam krediyi göster
      YAZDIR "--- Seçtiğiniz Dersler ---"
      toplam_kredi = 0
      HER SeciliDers İÇİN aktif_ogrenci.sectigi_dersler_listesi'NDE
        YAZDIR SeciliDers.ders_kodu + " - " + SeciliDers.ders_adı
        toplam_kredi = toplam_kredi + SeciliDers.kredi
      SON DÖNGÜ
      YAZDIR "Toplam Kredi: " + toplam_kredi
      YAZDIR "-------------------------"

    DEĞİLSE EĞER kullanici_secimi == 5 İSE
      YAZDIR "Ders kaydınız danışman onayına gönderilmiştir."
      DÖNGÜDEN ÇIK

    DEĞİLSE
      YAZDIR "Geçersiz bir seçim yaptınız."
    SON EĞER

  SON DÖNGÜ

  YAZDIR "Sistemden çıkış yapıldı."
  
BİTİR
