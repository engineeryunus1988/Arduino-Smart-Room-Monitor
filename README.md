# 🌡️ Akıllı Oda İzleme İstasyonu (V.Final)

Bu proje, bir odanın sıcaklık ve nem değerlerini anlık olarak ölçen, OLED ekran üzerinden grafiksel arayüz sunan ve her 5 saniyede bir sesli bildirim veren bir Arduino sistemidir.

## ✨ Özellikler
- **Hassas Ölçüm:** 4-Pin DHT11 sensörü ve Pull-up direnç mimarisi.
- **Görsel Arayüz:** 128x64 SSD1306 OLED Ekran.
- **Durum Bildirimi:** RGB LED ile sistem aktiflik takibi.
- **Sesli Uyarı:** Her 5 saniyede bir sistemin çalıştığını belirten kısa bip sesi.

## 🛠️ Bağlantı Şeması
- **DHT11 (2. Pin):** Arduino D7
- **Buzzer:** Arduino D4
- **OLED:** SDA -> A4, SCL -> A5
- **RGB LED:** Kırmızı -> D5, Yeşil -> D6
- **Not:** Sensörün 1. (VCC) ve 2. (Data) bacakları arasına **10K Ohm direnç** bağlanmıştır.
