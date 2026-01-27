# ISG_asistani


Bu proje; fabrika ortamlarında pano güvenliği, yasaklı alan denetimi ve personel yetkilendirme süreçlerini yapay zeka ile 7/24 otonom olarak takip eden bir denetim sistemidir. Sistem, ihlalleri anlık olarak tespit eder, yerel diske kaydeder ve Telegram üzerinden yetkililere fotoğraflı bildirim gönderir.

✨ Temel Özellikler

- Pano Durum Analizi: Özel eğitilmiş YOLOv8-Classify modeli ile panonun açık veya kapalı olduğunu gerçek zamanlı takip eder.
- Personel Yetki Ayrımı: Yerel görüntü işleme (HSV Renk Analizi) kullanarak personelin üzerindeki fosforlu yeleği tespit eder. Yetkili (Yeşil) ve Yetkisiz (Kırmızı) ayrımı yapar.
- Akıllı Takip (ID Tracking): Gelişmiş mesafe bazlı takip mantığı ile personellere benzersiz ID'ler atar. Bir kez yetki alan personelin yetkisini video boyunca hatırlar (Persistence).
- Yasaklı Bölge Denetimi (Geofencing): pointPolygonTest algoritması ile belirlenen koordinatlara giren yetkisiz kişileri tespit eder.
- Saniye Çakışması Önleyici (Temporal Smoothing): Anlık saniye çakışmalarını ve görüntü titremelerini engellemek için "Gecikmeli İhlal Filtresi" uygular. Bir durumun ihlal sayılması için ardışık 5 kare boyunca sürmesi gerekir.
- Anlık Bildirim: İhlal anında yakalanan kareyi açıklama metniyle birlikte Telegram Bot API üzerinden gönderir.

🛠️ Kullanılan Teknolojiler

| Bileşen        | Teknoloji                    |
| -------------- | ---------------------------- |
| Ana Dil        | Python 3.13+                 |
| Yapay Zeka     | YOLOv8 (Ultralytics)         |
| Görüntü İşleme | OpenCV (NumPy entegrasyonlu) |
| Renk Uzayı     | HSV (Hue, Saturation, Value) |
| Haberleşme     | Telegram Bot API & Requests  |

🚀 Kurulum

Depoyu Klonlayın:

```bash
git clone https://github.com/kullaniciadi/aisa-project.git
cd aisa-project
```

Gerekli Kütüphaneleri Kurun:

```bash
pip install ultralytics opencv-python numpy requests
```

Model ve Video Yollarını Ayarlayın: Kod içerisindeki `PANO_MODEL_PATH` ve `VIDEO_PATH` değişkenlerini kendi dosya yollarınıza göre güncelleyin.

Telegram Bot Ayarları: `BOT_TOKEN` ve `CHAT_ID` değişkenlerini kendi bilgilerinizle doldurun.

📊 İhlal Senaryoları

- Yetkisiz Pano Müdahalesi: Pano kapağı "AÇIK" iken, belirlenen etki alanı içerisinde bir personel varsa ve bu kişi "YETKİLİ" (Yeşil Yelekli) değilse sistem ihlal tetikler.
- Yasak Yol İhlali: Yetkisiz personelin ayak koordinatları yasaklı yaya yolu (Polygon) içine girdiğinde sistem anında kayıt alır.
- Senkronize Kayıt: İhlaller Masaüstü/ihlal klasörüne `ID_X_TARIH.jpg` formatında kaydedilir ve aynı anda Telegram'a iletilir.
