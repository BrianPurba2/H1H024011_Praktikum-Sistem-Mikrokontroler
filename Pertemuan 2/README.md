**Pertanyaan 2.5**
1. Gambarkan rangkaian schematic yang digunakan pada percobaan!
   <img width="584" height="453" alt="image" src="https://github.com/user-attachments/assets/626af443-4736-4e78-ae3f-c91d5dc9930f" />

2. Apa yang terjadi jika nilai num lebih dari 15?
    Jawab:Nilai tidak valid karena hanya 0–15 (hex). Tampilan akan tidak sesuai atau error.

3. Apakah program ini menggunakan common cathode atau common anode? Jelaskan
alasanya!
    Jawab:Biasanya menggunakan common cathode, karena LED menyala saat diberi HIGH.

4. Modifikasi program agar tampilan berjalan dari F ke 0 dan berikan penjelasan disetiap
baris kode nya dalam bentuk README.md!
Jawab:Kode Program dan Penjelasan
// Pin 7-Segment (a b c d e f g dp)<br>
// Menentukan pin Arduino yang terhubung ke masing-masing segmen<br>
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};<br>

// Counter dimulai dari 15 (F dalam heksadesimal)<br>
int counter = 15;<br>

// Pola digit 0-F untuk seven segment<br>
// 1 = LED menyala, 0 = LED mati<br>
byte digitPattern[16][8] = {<br>
  {1,1,1,1,1,1,0,0}, //0<br>
  {0,1,1,0,0,0,0,0}, //1<br>
  {1,1,0,1,1,0,1,0}, //2<br>
  {1,1,1,1,0,0,1,0}, //3<br>
  {0,1,1,0,0,1,1,0}, //4<br>
  {1,0,1,1,0,1,1,0}, //5 <br>
  {1,0,1,1,1,1,1,0}, //6<br>
  {1,1,1,0,0,0,0,0}, //7<br>
  {1,1,1,1,1,1,1,0}, //8
  {1,1,1,1,0,1,1,0}, //9<br>
  {1,1,1,0,1,1,1,0}, //A
  {0,0,1,1,1,1,1,0}, //b
  {1,0,0,1,1,1,0,0}, //C<br>
  {0,1,1,1,1,0,1,0}, //d
  {1,0,0,1,1,1,1,0}, //E
  {1,0,0,0,1,1,1,0}  //F
};

// Fungsi untuk menampilkan angka ke seven segment<br>
void displayDigit(int num)<br>
{
  // Mengatur setiap segmen sesuai pola<br>
  for(int i=0; i<8; i++)<br>
  {<br>
    digitalWrite(segmentPins[i], digitPattern[num][i]);<br>
  }<br>
}<br>

void setup()<br>
{<br>
  // Mengatur semua pin segment sebagai OUTPUT<br>
  for(int i=0; i<8; i++)v
  {<br>
    pinMode(segmentPins[i], OUTPUT);<br>
  }

  // Menampilkan angka awal (F)<br>
  displayDigit(counter);<br>
}<br>

void loop()<br>
{
  // Menampilkan angka saat ini<br>
  displayDigit(counter);<br>

  // Mengurangi nilai counter (F → 0)<br>
  counter--;<br>

  // Jika sudah kurang dari 0, kembali ke 15 (F)<br>
  if(counter < 0) counter = 15;<br>

  // Delay agar perubahan terlihat<br>
  delay(1000);<br>
}<br>

**Pertanyaan 2.6**
1. Gambarkan rangkaian schematic yang digunakan pada percobaan!
<img width="781" height="578" alt="image" src="https://github.com/user-attachments/assets/db62d2d6-c974-40e3-94c9-6c91237259bc" />

2. Mengapa pada push button digunakan mode INPUT_PULLUP pada Arduino Uno?
Apa keuntungannya dibandingkan rangkaian biasa?
Jawab:Agar tidak perlu resistor eksternal dan menghindari noise,ini mengurangi kompleksitas rangkaian dan meningkatkan kestabilan serta keandalan sistem.

3. Jika salah satu LED segmen tidak menyala, apa saja kemungkinan penyebabnya dari
sisi hardware maupun software?
Jawab:
-Kabel salah
-Pin salah
-Program salah
-LED rusak

4. Modifikasi rangkaian dan program dengan dua push button yang berfungsi sebagai
penambahan (increment) dan pengurangan (decrement) pada sistem counter dan
berikan penjelasan disetiap baris kode nya dalam bentuk README.md!
Jawab:Kode Program dan Penjelasan<br>
// Pin 7-Segment (a b c d e f g dp)<br>
// Array ini menyimpan pin Arduino yang terhubung ke masing-masing segmen<br>
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};<br>

// Push button<br>
// buttonUp untuk menambah nilai, buttonDown untuk mengurangi nilai<br>
const int buttonUp = 2;<br>
const int buttonDown = 3;<br>

// Counter untuk menyimpan angka yang ditampilkan<br>
int counter = 0;<br>

// Variabel untuk menyimpan kondisi tombol sebelumnya<br>
// Digunakan untuk mendeteksi perubahan saat tombol ditekan<br>
bool lastUpState = HIGH;<br>
bool lastDownState = HIGH;<br>

// Pola digit 0-F untuk seven segment<br>
// Setiap baris mewakili satu angka (0–F)<br>
// 1 = LED menyala, 0 = LED mati<br>
byte digitPattern[16][8] = {<br>
  {1,1,1,1,1,1,0,0}, //0<br>
  {0,1,1,0,0,0,0,0}, //1<br>
  {1,1,0,1,1,0,1,0}, //2<br>
  {1,1,1,1,0,0,1,0}, //3<br>
  {0,1,1,0,0,1,1,0}, //4<br>
  {1,0,1,1,0,1,1,0}, //5 <br>
  {1,0,1,1,1,1,1,0}, //6<br>
  {1,1,1,0,0,0,0,0}, //7<br>
  {1,1,1,1,1,1,1,0}, //8<br>
  {1,1,1,1,0,1,1,0}, //9<br>
  {1,1,1,0,1,1,1,0}, //A<br>
  {0,0,1,1,1,1,1,0}, //b<br>
  {1,0,0,1,1,1,0,0}, //C<br>
  {0,1,1,1,1,0,1,0}, //d<br>
  {1,0,0,1,1,1,1,0}, //E<br>
  {1,0,0,0,1,1,1,0}  //F<br>
};<br>

// Fungsi untuk menampilkan angka ke seven segment<br>
void displayDigit(int num)<br>
{<br>
  // Loop untuk setiap segmen<br>
  for(int i=0; i<8; i++)<br>
  {<br>
    // Mengirim HIGH atau LOW sesuai pola digit<br>
    digitalWrite(segmentPins[i], digitPattern[num][i]);<br>
  }<br>
}<br>

void setup()<br>
{<br>
  // Mengatur semua pin segment sebagai OUTPUT<br>
  for(int i=0; i<8; i++)<br>
  {<br>
    pinMode(segmentPins[i], OUTPUT);<br>
  }<br>

  // Mengatur tombol sebagai INPUT_PULLUP<br>
  // Artinya default HIGH, ditekan menjadi LOW
  pinMode(buttonUp, INPUT_PULLUP);
  pinMode(buttonDown, INPUT_PULLUP);

  // Menampilkan nilai awal (0)
  displayDigit(counter);
}

void loop()
{
  // Membaca kondisi tombol saat ini
  bool currentUp = digitalRead(buttonUp);
  bool currentDown = digitalRead(buttonDown);

  // Jika tombol increment ditekan
  // (perubahan dari HIGH ke LOW)
  if(lastUpState == HIGH && currentUp == LOW)
  {
    counter++; // tambah nilai

    // Jika lebih dari 15, kembali ke 0
    if(counter > 15) counter = 0;

    displayDigit(counter); // tampilkan ke seven segment
    delay(200); // debounce agar tidak lompat
  }

  // Jika tombol decrement ditekan
  if(lastDownState == HIGH && currentDown == LOW)
  {
    counter--; // kurangi nilai

    // Jika kurang dari 0, kembali ke 15
    if(counter < 0) counter = 15;

    displayDigit(counter); // tampilkan
    delay(200); // debounce
  }

  // Update kondisi tombol sebelumnya<br>
  lastUpState = currentUp;
  lastDownState = currentDown;
}<br>
