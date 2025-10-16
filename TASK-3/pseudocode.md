BAŞLA

  // Veri Yapıları ve Değişkenler
  TANIMLA Doktor:
    doktor_id
    ad_soyad
    uzmanlik_alani
    calisma_saatleri // ["09:00", "10:00", ...] gibi bir liste
  SON TANIMLA

  TANIMLA Hasta:
    hasta_id
    ad_soyad
    iletisim_bilgileri
  SON TANIMLA
  
  TANIMLA Randevu:
    randevu_id
    doktor_bilgisi
    hasta_bilgisi
    tarih
    saat
    durum // "Onaylandı", "İptal Edildi"
  SON TANIMLA

  OLUŞTUR DoktorListesi [
    {doktor_id: 101, ad_soyad: "Dr. Ahmet Yılmaz", uzmanlik_alani: "Kardiyoloji", calisma_saatleri: ["09:00", "10:00", "11:00"]},
    {doktor_id: 102, ad_soyad: "Dr. Zeynep Kaya", uzmanlik_alani: "Nöroloji", calisma_saatleri: ["13:00", "14:00", "15:00"]}
  ]
  
  OLUŞTUR HastaListesi // Hastaların kayıt olduğu liste
  OLUŞTUR AlınanRandevular // Alınan tüm randevuların listesi (başlangıçta boş)

  // Ana İşlem Döngüsü
  YAZDIR "Hastane Randevu Sistemine Hoş Geldiniz."
  
  // Hasta Girişi veya Kaydı (Basitleştirilmiş)
  YAZDIR "Hasta ID'nizi girin:"
  OKU aktif_hasta_id
  BUL aktif_hasta HastaListesi'NDEN hasta_id'si aktif_hasta_id OLAN

  EĞER aktif_hasta YOKSA
    YAZDIR "Kayıt bulunamadı. Yeni kayıt oluşturuluyor."
    // Yeni hasta kayıt adımları...
  SON EĞER

  DÖNGÜ SÜRESİNCE (Kullanıcı "Çıkış" demedikçe)
  
    YAZDIR "1: Randevu Al"
    YAZDIR "2: Randevularımı Görüntüle"
    YAZDIR "3: Randevu İptal Et"
    YAZDIR "4: Çıkış"
    OKU kullanici_secimi

    EĞER kullanici_secimi == 1 İSE
      // Randevu Alma Süreci
      YAZDIR "Mevcut Bölümler:"
      // DoktorListesi'nden tekrar etmeyecek şekilde uzmanlık alanlarını yazdır
      YAZDIR "Lütfen bir bölüm seçiniz (Örn: Kardiyoloji):"
      OKU secilen_bolum

      YAZDIR secilen_bolum + " bölümündeki doktorlar:"
      HER Doktor İÇİN DoktorListesi'NDE
        EĞER Doktor.uzmanlik_alani == secilen_bolum İSE
          YAZDIR Doktor.doktor_id + " - " + Doktor.ad_soyad
        SON EĞER
      SON DÖNGÜ

      YAZDIR "Lütfen bir doktor ID'si seçiniz:"
      OKU secilen_doktor_id
      BUL secilen_doktor DoktorListesi'NDEN doktor_id'si secilen_doktor_id OLAN

      YAZDIR "Müsait saatler:"
      YAZDIR secilen_doktor.calisma_saatleri

      YAZDIR "Lütfen bir saat seçiniz:"
      OKU secilen_saat

      // Seçilen saatin dolu olup olmadığını kontrol et
      RANDEVU_VAR_MI = HAYIR
      HER Randevu İÇİN AlınanRandevular'DA
        EĞER Randevu.doktor_bilgisi.doktor_id == secilen_doktor_id VE Randevu.saat == secilen_saat İSE
          RANDEVU_VAR_MI = EVET
          DÖNGÜDEN ÇIK
        SON EĞER
      SON DÖNGÜ

      EĞER RANDEVU_VAR_MI == EVET İSE
        YAZDIR "Seçtiğiniz saat dolu. Lütfen başka bir saat deneyin."
      DEĞİLSE
        // Yeni randevu oluştur ve listeye ekle
        OLUŞTUR YeniRandevu {randevu_id: YENİ_ID, doktor_bilgisi: secilen_doktor, hasta_bilgisi: aktif_hasta, saat: secilen_saat, durum: "Onaylandı"}
        EKLE YeniRandevu'yu AlınanRandevular'a
        YAZDIR "Randevunuz başarıyla oluşturuldu."
      SON EĞER

    DEĞİLSE EĞER kullanici_secimi == 2 İSE
      // Randevuları Görüntüleme
      YAZDIR "--- Aktif Randevularınız ---"
      HER Randevu İÇİN AlınanRandevular'DA
        EĞER Randevu.hasta_bilgisi.hasta_id == aktif_hasta_id İSE
          YAZDIR "ID: " + Randevu.randevu_id + " - Dr: " + Randevu.doktor_bilgisi.ad_soyad + " - Saat: " + Randevu.saat
        SON EĞER
      SON DÖNGÜ
      YAZDIR "--------------------------"

    DEĞİLSE EĞER kullanici_secimi == 3 İSE
      // Randevu İptal Etme
      YAZDIR "İptal etmek istediğiniz randevunun ID'sini girin:"
      OKU iptal_edilecek_id
      
      BUL_VE_GÜNCELLE AlınanRandevular'DA randevu_id'si iptal_edilecek_id OLAN randevunun durumunu "İptal Edildi" olarak
      YAZDIR "Randevunuz iptal edildi."

    DEĞİLSE EĞER kullanici_secimi == 4 İSE
      DÖNGÜDEN ÇIK

    DEĞİLSE
      YAZDIR "Geçersiz seçim yaptınız."
    SON EĞER

  SON DÖNGÜ

  YAZDIR "Sistemden çıkış yapıldı."
  
BİTİR
