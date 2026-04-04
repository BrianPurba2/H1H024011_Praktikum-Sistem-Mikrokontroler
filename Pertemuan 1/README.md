**Pertanyaan 1.5**
1. Pada kondisi apa program masuk ke blok if?
Jawab:Program masuk ke blok if ketika nilai timeDelay kurang dari atau sama dengan 100, karena kondisi yang diperiksa adalah:
if (timeDelay <= 100)
Artinya saat delay sudah mencapai batas tercepat, program menjalankan reset.

2. Pada kondisi apa program masuk ke blok else?
Jawab:Program masuk ke blok else ketika nilai timeDelay masih lebih besar dari 100,pada kondisi ini delay dikurangi 100 agar LED berkedip semakin cepat.

3. Apa fungsi dari perintah delay(timeDelay)?
Jawab:Fungsi delay(timeDelay) adalah memberi jeda waktu nyala dan mati LED sesuai nilai timeDelay,semakin besar nilainya, LED berkedip semakin lambat. Semakin kecil nilainya, LED berkedip semakin cepat.

4. Jika program yang dibuat memiliki alur mati → lambat → cepat → reset (mati), ubah menjadi LED tidak langsung reset → tetapi berubah dari cepat → sedang → mati dan berikan penjelasan disetiap baris kode nya dalam bentuk README.md!

Jawab:<br>

const int ledPin = 6;
int timeDelay = 100;

void setup() {
  pinMode(ledPin, OUTPUT);
}

void loop() {
  digitalWrite(ledPin, HIGH);
  delay(timeDelay);

  digitalWrite(ledPin, LOW);
  delay(timeDelay);

  if (timeDelay == 100) {
    timeDelay = 500;
  } else if (timeDelay == 500) {
    timeDelay = 1000;
  } else {
    timeDelay = 1000;
  }
}.

Penjelasan singkat:<br>
-ledPin = 6 → LED di pin 6 Arduino Uno,
-timeDelay = 100 → mulai dari cepat,
-LED menyala lalu mati,
-Jika delay 100 → berubah sedang (500),
-Jika delay 500 → berubah lambat/mati.

**Pertanyaan 1.6**
1. Gambarkan rangkaian schematic 5 LED running yang digunakan pada percobaan!
Jawab:Rangkaian terdiri dari 6 LED yang dihubungkan ke pin digital Arduino Uno sebagai berikut:
LED 1 → Pin 2
LED 2 → Pin 3
LED 3 → Pin 4
LED 4 → Pin 5
LED 5 → Pin 6
LED 6 → Pin 7
Setiap LED dihubungkan seri dengan resistor 220Ω lalu ke GND.
Link tinkercad:https://www.tinkercad.com/things/48jcVvuyR5m/editel?returnTo=%2Fdashboard

2. Jelaskan bagaimana program membuat efek LED berjalan dari kiri ke kanan!
Jawab:Efek kiri ke kanan dibuat dengan perulangan for yang dimulai dari pin 2 sampai pin 7. Setiap pin dinyalakan satu per satu, diberi delay, lalu dimatikan sehingga LED terlihat berjalan berurutan.

3. Jelaskan bagaimana program membuat LED kembali dari kanan ke kiri!
Jawab:Efek kanan ke kiri dibuat dengan perulangan for kedua yang dimulai dari pin 7 sampai pin 2. Urutan pin dibalik sehingga LED menyala dari kanan ke kiri.

4. Buatkan program agar LED menyala tiga LED kanan dan tiga LED kiri secara bergantian dan berikan penjelasan disetiap baris kode nya dalam bentuk README.md!
Jawab:<br>
void setup() {
  for (int pin = 2; pin <= 7; pin++) {
    pinMode(pin, OUTPUT);
  }
}

void loop() {
  // tiga kiri nyala
  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(4, HIGH);
  digitalWrite(5, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
  delay(500);

  // tiga kanan nyala
  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  digitalWrite(4, LOW);
  digitalWrite(5, HIGH);
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
  delay(500);
} .
Penjelasan Singkat:<br>
-setup() mengatur pin 2–7 sebagai output,
-Tiga LED kiri dinyalakan lebih dulu,
-Setelah delay, tiga LED kanan dinyalakan,
-Program diulang terus menerus
Untuk link program ada di nomor 1.
