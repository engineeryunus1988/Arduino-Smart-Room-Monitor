/*
 * Proje: Akıllı Oda İzleme ve Termometre İstasyonu (V.Final)
 * Özellikler: 4-Pin DHT11, OLED Ekran, RGB LED, 5 Saniyede Bir Bip Uyarısı
 * Sensör: DHT11 (Sıcaklık ve Nem)
 * Bağlantı Notu: VCC (1. Pin) ve DATA (2. Pin) arasında 10K direnç vardır.
 */

#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <DHT.h>

// Ekran Ayarları
#define SCREEN_WIDTH 128 
#define SCREEN_HEIGHT 64 
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

// Sensör ve Pin Ayarları
#define DHTPIN 7     
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

const int kirmiziLed = 5;
const int yesilLed = 6;
const int buzzerPin = 4;

void setup() {
  pinMode(kirmiziLed, OUTPUT);
  pinMode(yesilLed, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  
  dht.begin();
  
  // OLED Başlatma
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    for(;;); 
  }
  
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(15, 25);
  display.print("SISTEM BASLATILIYOR");
  display.display();
  
  // Başlangıç Sesi
  tone(buzzerPin, 1000, 300); 
  delay(2000);
}

void loop() {
  float t = dht.readTemperature(); 
  float h = dht.readHumidity();

  display.clearDisplay();

  // Sensör Kontrolü
  if (isnan(t) || isnan(h)) {
    display.setCursor(0, 25);
    display.print("HATA: SENSOR OKUNAMADI");
    display.display();
    digitalWrite(kirmiziLed, HIGH);
    digitalWrite(yesilLed, LOW);
  } 
  else {
    // Normal Çalışma: Yeşil Işık
    digitalWrite(kirmiziLed, LOW);
    digitalWrite(yesilLed, HIGH);

    // Ekran Tasarımı
    display.setTextSize(1);
    display.setTextColor(WHITE);
    display.setCursor(0, 0);
    display.print("ODA TAKIP SISTEMI");
    display.drawLine(0, 11, 127, 11, WHITE);

    display.setTextSize(2);
    display.setCursor(0, 22);
    display.print("ISI: "); 
    display.print((int)t); 
    display.print(" C");

    display.setCursor(0, 48);
    display.print("NEM: %"); 
    display.print((int)h);
    
    display.display();

    // 5 Saniyede Bir "Bip" Sesi (Sistemin çalıştığını belirtir)
    tone(buzzerPin, 1200, 80); 
  }

  delay(5000); // 5 saniye bekleme ve yeni ölçüm
}
