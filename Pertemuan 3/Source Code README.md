**Source Code 3.5**
```
const int ledPin = 8; 

char data;            
bool blinkMode = false;

void setup() {
  pinMode(ledPin, OUTPUT);  
  Serial.begin(9600);       
}

void loop() {

 if (Serial.available() > 0) {

    data = Serial.read(); 

    // Jika input '1'
    if (data == '1') {
      digitalWrite(ledPin, HIGH); 
      blinkMode = false;         
      Serial.println("LED ON");
    }

    // Jika input '0'
    else if (data == '0') {
      digitalWrite(ledPin, LOW); 
      blinkMode = false;         
      Serial.println("LED OFF");
    }

    // Jika input '2'
    else if (data == '2') {
      blinkMode = true;           
      Serial.println("LED BLINK MODE");
    }

    else {
      Serial.println("ERROR: Perintah tidak dikenal");
    }
  }

  // Jika mode blink aktif
  if (blinkMode) {
    digitalWrite(ledPin, HIGH); 
    delay(500);                
    digitalWrite(ledPin, LOW);  
    delay(500);                
  }
}
```


**Source Code 3.6**
```
#include <Wire.h>                  // Library komunikasi I2C
#include <LiquidCrystal_I2C.h>    // Library LCD I2C
#include <Arduino.h>              // Library dasar Arduino

LiquidCrystal_I2C lcd(0x27, 16, 2); // Inisialisasi LCD (alamat 0x27, 16 kolom, 2 baris)

const int pinPot = A0;            // Pin potensiometer terhubung ke A0

void setup() {
  Serial.begin(9600);             // Memulai komunikasi UART (Serial Monitor)
  lcd.init();                     // Inisialisasi LCD
  lcd.backlight();                // Menghidupkan lampu LCD
}

void loop() {

  int nilai = analogRead(pinPot); // Membaca nilai ADC (0–1023)

  float volt = nilai * (5.0 / 1023.0); // Mengubah ADC menjadi tegangan (Volt)

  int persen = map(nilai, 0, 1023, 0, 100); // Mengubah ADC menjadi persen (0–100%)

  int bar = map(nilai, 0, 1023, 0, 16); // Mengubah ADC menjadi panjang bar (0–16)

  // ================= UART (Serial Monitor) =================
  Serial.print("ADC: ");          // Menampilkan teks "ADC:"
  Serial.print(nilai);            // Menampilkan nilai ADC
  Serial.print(" | Volt: ");      // Pemisah
  Serial.print(volt, 2);          // Menampilkan volt (2 angka desimal)
  Serial.print(" V | Persen: ");  // Pemisah
  Serial.print(persen);           // Menampilkan persen
  Serial.println("%");            // Pindah baris

  // ================= I2C (LCD) =================
  lcd.clear();                    // Membersihkan layar LCD

  // Baris 1 → ADC dan Persen
  lcd.setCursor(0, 0);            // Posisi kolom 0 baris 0
  lcd.print("ADC:");              // Tampilkan teks
  lcd.print(nilai);               // Tampilkan nilai ADC
  lcd.print(" ");                 // Spasi
  lcd.print(persen);              // Tampilkan persen
  lcd.print("%");                 // Tampilkan simbol %

  // Baris 2 → Bar level
  lcd.setCursor(0, 1);            // Pindah ke baris kedua

  for (int i = 0; i < 16; i++) {  // Loop 16 kolom LCD
    if (i < bar) {
      lcd.print((char)255);       // Karakter penuh (bar aktif)
    } else {
      lcd.print(" ");             // Kosong (bar tidak aktif)
    }
  }

  delay(200);                     // Delay agar tampilan stabil
}
