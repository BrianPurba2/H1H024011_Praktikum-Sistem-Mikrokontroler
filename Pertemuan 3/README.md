**Pertanyaan Praktikum 3.5**
1) Jelaskan proses dari input keyboard hingga LED menyala/mati! 
Jawab:
Prosesnya sebagai berikut:
-User mengetik perintah di Serial Monitor (misalnya ‘1’ atau ‘0’)
-Data dikirim dari komputer ke Arduino melalui komunikasi serial (UART)
-Arduino membaca data menggunakan Serial.read()
-Program mengecek nilai input:
  -Jika sesuai → LED dikontrol (nyala/mati)
  -Jika tidak sesuai → tampilkan pesan error
-Arduino mengirim feedback kembali ke Serial Monitor

2) Mengapa digunakan Serial.available() sebelum membaca data? Apa yang terjadi jika 
baris tersebut dihilangkan? 
Jawab:
Serial.available() digunakan untuk:
=>mengecek apakah ada data yang masuk dari Serial

Jika dihilangkan:

-Arduino tetap mencoba membaca data
-Bisa membaca data kosong / sampah
-Program menjadi tidak stabil / error

3) Modifikasi program agar LED berkedip (blink) ketika menerima input '2' dengan kondisi 
jika ‘2’ aktif maka LED akan terus berkedip sampai perintah selanjutnya diberikan dan 
berikan penjelasan disetiap baris kode nya dalam bentuk README.md! 
jawab:
Program ini digunakan untuk mengontrol LED melalui Serial Monitor:

* '1' → LED ON
* '0' → LED OFF
* '2' → LED berkedip (blink terus)


const int ledPin = 8; // Menentukan pin LED

char data;            // Variabel untuk menyimpan input dari serial
bool blinkMode = false; // Status mode blink

void setup() {
  pinMode(ledPin, OUTPUT);     // Set pin LED sebagai output
  Serial.begin(9600);          // Memulai komunikasi serial
}

void loop() {

  if (Serial.available() > 0) {

  data = Serial.read(); // Membaca data dari serial

  // Jika input '1'
    if (data == '1') {
      digitalWrite(ledPin, HIGH); // LED menyala
      blinkMode = false;          // Matikan mode blink
      Serial.println("LED ON");
    }

   // Jika input '0'
    else if (data == '0') {
      digitalWrite(ledPin, LOW);  // LED mati
      blinkMode = false;          // Matikan mode blink
      Serial.println("LED OFF");
    }

   // Jika input '2'
    else if (data == '2') {
      blinkMode = true;           // Aktifkan mode blink
      Serial.println("LED BLINK MODE");
    }

  // Jika input tidak dikenali
    else {
      Serial.println("ERROR: Perintah tidak dikenal");
    }
  }

  // Jika mode blink aktif
  if (blinkMode) {
    digitalWrite(ledPin, HIGH); // LED nyala
    delay(500);                 // Tunggu 0.5 detik
    digitalWrite(ledPin, LOW);  // LED mati
    delay(500);                 // Tunggu 0.5 detik
  }
}
 Penjelasan Singkat:
* Serial digunakan untuk komunikasi antara komputer dan Arduino
* Input karakter digunakan untuk mengontrol LED
* Mode blink akan terus berjalan sampai ada input baru

Program ini menunjukkan bahwa komunikasi serial dapat digunakan untuk mengontrol perangkat secara real-time menggunakan input dari keyboard.

4) Tentukan apakah menggunakan delay() atau milis()! Jelaskan pengaruhnya terhadap 
sistem
Jawab:
Menggunakan delay() pengaruhnya adalah:
-Program berhenti sementara
-LED berkedip sederhana
-Tapi tidak bisa membaca input selama delay



**Pertanyaan Praktikum 3.6**
1) Jelaskan bagaimana cara kerja komunikasi I2C antara Arduino dan LCD pada rangkaian 
tersebut! 
Jawab:
Alur kerja:
1.Arduino bertindak sebagai master
2.LCD I2C sebagai slave (punya alamat, misal 0x27)
3.Arduino mengirim data:
    -alamat LCD
    -data yang ingin ditampilkan
4.LCD menerima dan menampilkan ke layar

2) Apakah pin potensiometer harus seperti itu? Jelaskan yang terjadi apabila pin kiri dan 
pin kanan tertukar! 
Jawab:
Tidak wajib urutannya, tapi ada efeknya
Penjelasan:
=>Potensiometer punya 3 pin:
-Kiri → VCC (5V)
-Tengah → Output (ke A0)
-Kanan → GND
=>Jika kiri dan kanan ditukar:
-Nilai tetap terbaca
-Tapi arah putaran terbalik
=>Contoh:
-Harusnya kiri = 0 → jadi kanan = 0

3) Modifikasi program dengan menggabungkan antara UART dan I2C (keduanya sebagai 
output) sehingga:
-Data tidak hanya ditampilkan di LCD tetapi juga di Serial Monitor 
-Adapun data yang ditampilkan pada Serial Monitor sesuai dengan table berikut: 
|ADC: 0 |Volt: 0.00 V |Persen: 0% |
-Tampilan jika potensiometer dalam kondisi diputar paling kiri 
-ADC: 0  0% | setCursor(0, 0) dan Bar (level) | setCursor(0, 1) 
-Berikan penjelasan disetiap baris kode nya dalam bentuk README.md! 
Jawab:
## Program + Penjelasan per Baris

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

## Kesimpulan

* UART digunakan untuk monitoring data di komputer
* I2C digunakan untuk menampilkan data di LCD
* Nilai ADC dapat dikonversi menjadi Volt dan Persen
* Bar level mempermudah visualisasi nilai

4) Lengkapi table berikut berdasarkan pengamatan pada Serial Monitor
Jawab:
| ADC | Volt(V) | Persen(%) |
|-----|---------|-----------|
| 1   | 0.00 V  |   0%      |
| 21  | 0.10 V  |   2%      |
| 49  | 0.24 V  |   5%      |
| 74  | 0.36 V  |   7%      |
| 96  | 0.47 V  |   9%      |


