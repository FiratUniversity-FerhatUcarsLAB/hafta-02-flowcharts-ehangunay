BAŞLA

  // Sistem Değişkenleri ve Durumları
  TANIMLA SİSTEM_DURUMU = "PASİF" // Diğer durumlar: "KURULU", "ALARM"
  TANIMLA DOGRU_PIN = "1234"
  TANIMLA SENSÖRLER [
    {sensor_id: "KAPI01", tip: "Kapı Sensörü", durum: "KAPALI"},
    {sensor_id: "PENCERE01", tip: "Pencere Sensörü", durum: "KAPALI"},
    {sensor_id: "HAREKET01", tip: "Hareket Sensörü", durum: "HAREKET YOK"}
  ]
  TANIMLA ALARM_CALIYOR = HAYIR

  // Ana Kontrol Döngüsü (Sistem sürekli çalışır)
  SONSUZ DÖNGÜ BAŞLAT

    // Kullanıcıdan Girdi Alma (Simülasyon)
    // Gerçekte bu, bir tuş takımı veya mobil uygulama olabilir.
    YAZDIR "1: Sistemi Kur (PIN Gerekli)"
    YAZDIR "2: Sistemi Pasif Hale Getir (PIN Gerekli)"
    YAZDIR "3: Sensör Durumlarını Değiştir (Simülasyon)"
    OKU kullanici_secimi

    // Seçim 1: Sistemi Kurma
    EĞER kullanici_secimi == 1 İSE
      YAZDIR "Sistemi kurmak için PIN girin:"
      OKU girilen_pin
      EĞER girilen_pin == DOGRU_PIN İSE
        SİSTEM_DURUMU = "KURULU"
        YAZDIR "Sistem 15 saniye içinde kurulacak. Lütfen evden çıkın."
        BEKLE 15 saniye
        YAZDIR "Sistem Kuruldu. Sensörler izleniyor."
      DEĞİLSE
        YAZDIR "Hatalı PIN."
      SON EĞER
    SON EĞER

    // Seçim 2: Sistemi Pasif Hale Getirme
    EĞER kullanici_secimi == 2 İSE
      YAZDIR "Sistemi pasif hale getirmek için PIN girin:"
      OKU girilen_pin
      EĞER girilen_pin == DOGRU_PIN İSE
        SİSTEM_DURUMU = "PASİF"
        ALARM_CALIYOR = HAYIR
        YAZDIR "Sistem Pasif Hale Getirildi."
      DEĞİLSE
        YAZDIR "Hatalı PIN."
      SON EĞER
    SON EĞER
    
    // Seçim 3: Simülasyon için sensör durumu değiştirme
    EĞER kullanici_secimi == 3 İSE
        // Örneğin, kapı sensörünün durumunu "AÇIK" yap
        SENSÖRLER[0].durum = "AÇIK" 
        YAZDIR SENSÖRLER[0].sensor_id + " durumu 'AÇIK' olarak değiştirildi."
    SON EĞER

    // Çekirdek Güvenlik Mantığı
    EĞER SİSTEM_DURUMU == "KURULU" İSE
      // Sensörleri kontrol et
      HER Sensor İÇİN SENSÖRLER'de
        EĞER (Sensor.tip == "Kapı Sensörü" VE Sensor.durum == "AÇIK") VEYA
             (Sensor.tip == "Pencere Sensörü" VE Sensor.durum == "AÇIK") VEYA
             (Sensor.tip == "Hareket Sensörü" VE Sensor.durum == "HAREKET VAR") İSE
          
          YAZDIR "İHLAL TESPİT EDİLDİ! Sensor: " + Sensor.sensor_id
          SİSTEM_DURUMU = "ALARM"
          ALARM_CALIYOR = EVET
          DÖNGÜDEN ÇIK // Diğer sensörleri kontrol etmeyi bırak
        SON EĞER
      SON DÖNGÜ
    SON EĞER

    // Alarm Durumu Yönetimi
    EĞER SİSTEM_DURUMU == "ALARM" VE ALARM_CALIYOR == EVET İSE
      YAZDIR "ALARM ÇALIYOR! Yetkililere haber veriliyor."
      // Alarm çalma, bildirim gönderme gibi işlemler burada yapılır.
    SON EĞER
    
    BEKLE 1 saniye // Döngüyü yavaşlatmak için
  SONSUZ DÖNGÜ SONU

BİTİR
