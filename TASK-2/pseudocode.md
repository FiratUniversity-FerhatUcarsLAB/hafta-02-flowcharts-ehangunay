BAŞLA

  // Veri Yapıları ve Değişkenler
  TANIMLA Ürün:
    ürün_id
    ürün_adı
    fiyat
  SON TANIMLA

  TANIMLA SepetÖğesi:
    ürün BİLGİLERİ
    adet
  SON TANIMLA

  OLUŞTUR ÜrünlerListesi TÜRÜNDE Liste [
    {ürün_id: 1, ürün_adı: "Laptop", fiyat: 15000},
    {ürün_id: 2, ürün_adı: "Mouse", fiyat: 250},
    {ürün_id: 3, ürün_adı: "Klavye", fiyat: 400}
  ]

  OLUŞTUR AlışverişSepeti TÜRÜNDE Liste // Başlangıçta boş

  // Ana İşlem Döngüsü
  DÖNGÜ SÜRESİNCE (Kullanıcı "Çıkış" demedikçe)

    YAZDIR "1: Ürünleri Görüntüle"
    YAZDIR "2: Sepete Ürün Ekle"
    YAZDIR "3: Sepeti Görüntüle"
    YAZDIR "4: Sepetten Ürün Sil"
    YAZDIR "5: Çıkış"
    OKU kullanıcı_seçimi

    EĞER kullanıcı_seçimi == 1 İSE
      // Ürünleri Listeleme
      HER Ürün İÇİN ÜrünlerListesi'NDE
        YAZDIR Ürün.ürün_id + " - " + Ürün.ürün_adı + " - " + Ürün.fiyat + " TL"
      SON DÖNGÜ

    DEĞİLSE EĞER kullanıcı_seçimi == 2 İSE
      // Sepete Ekleme
      YAZDIR "Eklemek istediğiniz ürünün ID'sini girin:"
      OKU seçilen_id
      YAZDIR "Kaç adet eklemek istiyorsunuz?"
      OKU seçilen_adet

      BUL eklenecek_ürün ÜrünlerListesi'NDEN ürün_id'si seçilen_id OLAN
      
      EĞER eklenecek_ürün VARSA
        OLUŞTUR YeniSepetÖğesi {ürün: eklenecek_ürün, adet: seçilen_adet}
        EKLE YeniSepetÖğesi'ni AlışverişSepeti'ne
        YAZDIR eklenecek_ürün.ürün_adı + " sepete eklendi."
      DEĞİLSE
        YAZDIR "Geçersiz ürün ID'si."
      SON EĞER

    DEĞİLSE EĞER kullanıcı_seçimi == 3 İSE
      // Sepeti Görüntüleme
      TANIMLA toplam_tutar = 0
      YAZDIR "--- Alışveriş Sepetiniz ---"
      HER Öğe İÇİN AlışverişSepeti'NDE
        YAZDIR Öğe.adet + " x " + Öğe.ürün.ürün_adı + " - Fiyat: " + (Öğe.ürün.fiyat * Öğe.adet) + " TL"
        toplam_tutar = toplam_tutar + (Öğe.ürün.fiyat * Öğe.adet)
      SON DÖNGÜ
      YAZDIR "-------------------------"
      YAZDIR "Toplam Tutar: " + toplam_tutar + " TL"

    DEĞİLSE EĞER kullanıcı_seçimi == 4 İSE
      // Sepetten Silme
      YAZDIR "Silmek istediğiniz ürünün ID'sini girin:"
      OKU silinecek_id
      
      BUL_VE_SİL AlışverişSepeti'NDEN ürün_id'si silinecek_id OLAN öğeyi
      YAZDIR "Ürün sepetten silindi."

    DEĞİLSE EĞER kullanıcı_seçimi == 5 İSE
      DÖNGÜDEN ÇIK
      
    DEĞİLSE
      YAZDIR "Geçersiz seçim."
    SON EĞER

  SON DÖNGÜ

  YAZDIR "Alışverişten çıktınız."

BİTİR
