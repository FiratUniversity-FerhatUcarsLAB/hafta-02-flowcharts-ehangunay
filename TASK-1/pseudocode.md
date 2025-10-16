BAŞLA

  // Değişken Tanımlamaları
  KART_TAKILDI_MI = HAYIR
  PIN_DOGRU_MU = HAYIR
  HESAP_BAKIYESI = 1500  // Örnek bakiye
  ISTENEN_MIKTAR = 0
  ATM_NAKIT_LIMITI = 10000 // ATM'deki mevcut para

  // 1. Adım: Kartın Takılması
  YAZDIR "Lütfen kartınızı takınız."
  OKU KART_TAKILDI_MI

  EĞER KART_TAKILDI_MI == EVET İSE
    // 2. Adım: PIN Girişi ve Doğrulama
    3 deneme hakkı için DÖNGÜ BAŞLAT
      YAZDIR "Lütfen 4 haneli PIN kodunuzu giriniz:"
      OKU KULLANICI_PINI

      EĞER KULLANICI_PINI == GERCEK_PIN İSE
        PIN_DOGRU_MU = EVET
        DÖNGÜDEN ÇIK
      DEĞİLSE
        YAZDIR "Hatalı PIN. Kalan deneme hakkı: " + (2 - deneme_sayısı)
      SON EĞER
    DÖNGÜ SONU

    // 3. Adım: PIN Doğruysa İşlemlere Devam Et
    EĞER PIN_DOGRU_MU == EVET İSE
      YAZDIR "Giriş başarılı."
      YAZDIR "1: Para Çekme"
      YAZDIR "2: Bakiye Sorgulama"
      YAZDIR "3: Çıkış"
      OKU KULLANICI_SECIMI

      EĞER KULLANICI_SECIMI == 1 İSE
        // 4. Adım: Çekilecek Miktarın Girilmesi
        YAZDIR "Lütfen çekmek istediğiniz miktarı giriniz:"
        OKU ISTENEN_MIKTAR

        // 5. Adım: Kontroller
        EĞER ISTENEN_MIKTAR > HESAP_BAKIYESI İSE
          YAZDIR "Yetersiz bakiye."
        DEĞİLSE EĞER ISTENEN_MIKTAR > ATM_NAKIT_LIMITI İSE
          YAZDIR "ATM'de yeterli nakit bulunmamaktadır."
        DEĞİLSE EĞER ISTENEN_MIKTAR <= 0 İSE
            YAZDIR "Geçersiz miktar girdiniz."
        DEĞİLSE
          // 6. Adım: Paranın Verilmesi ve Bakiye Güncelleme
          YAZDIR ISTENEN_MIKTAR + " TL hazırlanıyor..."
          YAZDIR "Lütfen paranızı alınız."
          HESAP_BAKIYESI = HESAP_BAKIYESI - ISTENEN_MIKTAR
          ATM_NAKIT_LIMITI = ATM_NAKIT_LIMITI - ISTENEN_MIKTAR
          YAZDIR "Yeni bakiyeniz: " + HESAP_BAKIYESI + " TL"
        SON EĞER
      DEĞİLSE EĞER KULLANICI_SECIMI == 2 İSE
        YAZDIR "Mevcut bakiyeniz: " + HESAP_BAKIYESI + " TL"
      SON EĞER

    DEĞİLSE
      YAZDIR "PIN 3 kez hatalı girildi. Kartınız bloke edilmiştir."
    SON EĞER

  DEĞİLSE
    YAZDIR "Kart algılanmadı."
  SON EĞER

  // Son Adım: Kart İadesi ve Çıkış
  YAZDIR "İşleminiz tamamlanmıştır. Lütfen kartınızı almayı unutmayınız."

BITIR
