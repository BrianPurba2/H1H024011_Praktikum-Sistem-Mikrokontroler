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
Jawab:
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
} 
Penjelasan Singkat:
-setup() mengatur pin 2–7 sebagai output
-Tiga LED kiri dinyalakan lebih dulu
-Setelah delay, tiga LED kanan dinyalakan
-Program diulang terus menerus
Untuk link program ada di nomor 1
Dokumentasi Percobaan:![IMG-20260331-WA0008](https://github.com/user-attachments/assets/e6fd9d4b-9fc1-40bd-8dd1-c8f726882c9c)
                      ![IMG-20260331-WA0010](https://github.com/user-attachments/assets/99ff36d6-7431-48dd-a20e-bd428e9e45de)

