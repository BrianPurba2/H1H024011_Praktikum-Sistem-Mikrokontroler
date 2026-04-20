<h4>**Pertanyaan Praktikum 3.5**<br></h4>
1) Jelaskan proses dari input keyboard hingga LED menyala/mati! <br>
Jawab:<br>
Prosesnya sebagai berikut:<br>
-User mengetik perintah di Serial Monitor (misalnya ‘1’ atau ‘0’)<br>
-Data dikirim dari komputer ke Arduino melalui komunikasi serial (UART)<br>
-Arduino membaca data menggunakan Serial.read()<br>
-Program mengecek nilai input:<br>
  -Jika sesuai → LED dikontrol (nyala/mati)<br>
  -Jika tidak sesuai → tampilkan pesan error<br>
-Arduino mengirim feedback kembali ke Serial Monitor<br>

2) Mengapa digunakan Serial.available() sebelum membaca data? Apa yang terjadi jika 
baris tersebut dihilangkan? <br>
Jawab:<br>
Serial.available() digunakan untuk:<br>
=>mengecek apakah ada data yang masuk dari Serial<br>
<br>
Jika dihilangkan:<br>
<br>
-Arduino tetap mencoba membaca data<br>
-Bisa membaca data kosong / sampah<br>
-Program menjadi tidak stabil / error<br>

3) Modifikasi program agar LED berkedip (blink) ketika menerima input '2' dengan kondisi 
jika ‘2’ aktif maka LED akan terus berkedip sampai perintah selanjutnya diberikan dan 
berikan penjelasan disetiap baris kode nya dalam bentuk README.md! <br>
jawab:<br>
Program ini digunakan untuk mengontrol LED melalui Serial Monitor:<br>

* '1' → LED ON<br>
* '0' → LED OFF<br>
* '2' → LED berkedip (blink terus)<br>

```
const int ledPin = 8; // Menentukan pin LED<br>

char data;            // Variabel untuk menyimpan input dari serial<br>
bool blinkMode = false; // Status mode blink<br>
<br>
void setup() {<br>
  pinMode(ledPin, OUTPUT);     // Set pin LED sebagai output<br>
  Serial.begin(9600);          // Memulai komunikasi serial<br>
}<br>

void loop() {<br>

  if (Serial.available() > 0) {<br>

  data = Serial.read(); // Membaca data dari serialv

  // Jika input '1'<br>
    if (data == '1') {<br>
      digitalWrite(ledPin, HIGH); // LED menyala<br>
      blinkMode = false;          // Matikan mode blink<br>
      Serial.println("LED ON");<br>
    }<br>

   // Jika input '0'<br>
    else if (data == '0') {<br>
      digitalWrite(ledPin, LOW);  // LED mati<br>
      blinkMode = false;          // Matikan mode blink<br>
      Serial.println("LED OFF");<br>
    }<br>

   // Jika input '2'<br>
    else if (data == '2') {<br>
      blinkMode = true;           // Aktifkan mode blink<br>
      Serial.println("LED BLINK MODE");<br>
    }<br>

  // Jika input tidak dikenali<br>
    else {<br>
      Serial.println("ERROR: Perintah tidak dikenal");<br>
    }<br>
  }

  // Jika mode blink aktif<br>
  if (blinkMode) {<br>
    digitalWrite(ledPin, HIGH); // LED nyala<br>
    delay(500);                 // Tunggu 0.5 detik<br>
    digitalWrite(ledPin, LOW);  // LED mati<br>
    delay(500);                 // Tunggu 0.5 detik<br>
  }<br>
}<br>
```
 Penjelasan Singkat:<br>
* Serial digunakan untuk komunikasi antara komputer dan Arduino<br>
* Input karakter digunakan untuk mengontrol LED<br>
* Mode blink akan terus berjalan sampai ada input baru<br>

Program ini menunjukkan bahwa komunikasi serial dapat digunakan untuk mengontrol perangkat secara real-time menggunakan input dari keyboard.<br>

4) Tentukan apakah menggunakan delay() atau milis()! Jelaskan pengaruhnya terhadap 
sistem<br>
Jawab:<br>
Menggunakan delay() pengaruhnya adalah:<br>
-Program berhenti sementara<br>
-LED berkedip sederhana<br>
-Tapi tidak bisa membaca input selama delay<br>



<h4>**Pertanyaan Praktikum 3.6**</h4>
1) Jelaskan bagaimana cara kerja komunikasi I2C antara Arduino dan LCD pada rangkaian 
tersebut! <br>
Jawab:<br>
Alur kerja:<br>
1.Arduino bertindak sebagai master<br>
2.LCD I2C sebagai slave (punya alamat, misal 0x27)<br>
3.Arduino mengirim data:<br>
    -alamat LCD<br>
    -data yang ingin ditampilkan<br>
4.LCD menerima dan menampilkan ke layar<br>

2) Apakah pin potensiometer harus seperti itu? Jelaskan yang terjadi apabila pin kiri dan pin kanan tertukar! <br>
Jawab:<br>
Tidak wajib urutannya, tapi ada efeknya<br>
Penjelasan:<br>
=>Potensiometer punya 3 pin:<br>
-Kiri → VCC (5V)<br>
-Tengah → Output (ke A0)<br>
-Kanan → GND<br>
=>Jika kiri dan kanan ditukar:<br>
-Nilai tetap terbaca<br>
-Tapi arah putaran terbalik<br>
=>Contoh:<br>
-Harusnya kiri = 0 → jadi kanan = 0<br>
<br>
3) Modifikasi program dengan menggabungkan antara UART dan I2C (keduanya sebagai 
output) sehingga:<br>
-Data tidak hanya ditampilkan di LCD tetapi juga di Serial Monitor <br>
-Adapun data yang ditampilkan pada Serial Monitor sesuai dengan table berikut: <br>
|ADC: 0 |Volt: 0.00 V |Persen: 0% |<br>
-Tampilan jika potensiometer dalam kondisi diputar paling kiri <br>
-ADC: 0  0% | setCursor(0, 0) dan Bar (level) | setCursor(0, 1) <br>
-Berikan penjelasan disetiap baris kode nya dalam bentuk README.md! <br>
Jawab:<br>
## Program + Penjelasan per Baris<br>
```

#include <Wire.h>                  // Library komunikasi I2C<br>
#include <LiquidCrystal_I2C.h>    // Library LCD I2C<br>
#include <Arduino.h>              // Library dasar Arduino<br>

LiquidCrystal_I2C lcd(0x27, 16, 2); // Inisialisasi LCD (alamat 0x27, 16 kolom, 2 baris)<br>

const int pinPot = A0;            // Pin potensiometer terhubung ke A0<br>

void setup() {<br>
  Serial.begin(9600);             // Memulai komunikasi UART (Serial Monitor)<br>
  lcd.init();                     // Inisialisasi LCD<br>
  lcd.backlight();                // Menghidupkan lampu LCD<br>
}<br>

void loop() {<br>

  int nilai = analogRead(pinPot); // Membaca nilai ADC (0–1023)<br>

  float volt = nilai * (5.0 / 1023.0); // Mengubah ADC menjadi tegangan (Volt)<br>

  int persen = map(nilai, 0, 1023, 0, 100); // Mengubah ADC menjadi persen (0–100%)<br>
<br>
  int bar = map(nilai, 0, 1023, 0, 16); // Mengubah ADC menjadi panjang bar (0–16)<br>

  // ================= UART (Serial Monitor) =================<br>
  Serial.print("ADC: ");          // Menampilkan teks "ADC:"<br>
  Serial.print(nilai);            // Menampilkan nilai ADC<br>
  Serial.print(" | Volt: ");      // Pemisah<br>
  Serial.print(volt, 2);          // Menampilkan volt (2 angka desimal)<br>
  Serial.print(" V | Persen: ");  // Pemisah<br>
  Serial.print(persen);           // Menampilkan persen<br>
  Serial.println("%");            // Pindah baris<br>

  // ================= I2C (LCD) =================<br>
  lcd.clear();                    // Membersihkan layar LCD<br>

  // Baris 1 → ADC dan Persen<br>
  lcd.setCursor(0, 0);            // Posisi kolom 0 baris 0<br>
  lcd.print("ADC:");              // Tampilkan teks<br>
  lcd.print(nilai);               // Tampilkan nilai ADC<br>
  lcd.print(" ");                 // Spasi<br>
  lcd.print(persen);              // Tampilkan persen<br>
  lcd.print("%");                 // Tampilkan simbol %<br>

  // Baris 2 → Bar level<br>
  lcd.setCursor(0, 1);            // Pindah ke baris kedua<br>

  for (int i = 0; i < 16; i++) {  // Loop 16 kolom LCD<br>
    if (i < bar) {<br>
      lcd.print((char)255);       // Karakter penuh (bar aktif)<br>
    } else {<br>
      lcd.print(" ");             // Kosong (bar tidak aktif)<br>
    }<br>
  }<br>

  delay(200);                     // Delay agar tampilan stabil<br>
}<br>
```
```
## Kesimpulan<br>

* UART digunakan untuk monitoring data di komputer<br>
* I2C digunakan untuk menampilkan data di LCD<br>
* Nilai ADC dapat dikonversi menjadi Volt dan Persen<br>
* Bar level mempermudah visualisasi nilai<br>
```
4) Lengkapi table berikut berdasarkan pengamatan pada Serial Monitor
Jawab:
```
| ADC | Volt(V) | Persen(%) |
|-----|---------|-----------|
| 1   | 0.00 V  |   0%      |
| 21  | 0.10 V  |   2%      |
| 49  | 0.24 V  |   5%      |
| 74  | 0.36 V  |   7%      |
| 96  | 0.47 V  |   9%      |


