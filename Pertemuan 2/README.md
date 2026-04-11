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
// Pin 7-Segment (a b c d e f g dp)
// Menentukan pin Arduino yang terhubung ke masing-masing segmen
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};

// Counter dimulai dari 15 (F dalam heksadesimal)
int counter = 15;

// Pola digit 0-F untuk seven segment
// 1 = LED menyala, 0 = LED mati
byte digitPattern[16][8] = {
  {1,1,1,1,1,1,0,0}, //0
  {0,1,1,0,0,0,0,0}, //1
  {1,1,0,1,1,0,1,0}, //2
  {1,1,1,1,0,0,1,0}, //3
  {0,1,1,0,0,1,1,0}, //4
  {1,0,1,1,0,1,1,0}, //5 
  {1,0,1,1,1,1,1,0}, //6
  {1,1,1,0,0,0,0,0}, //7
  {1,1,1,1,1,1,1,0}, //8
  {1,1,1,1,0,1,1,0}, //9
  {1,1,1,0,1,1,1,0}, //A
  {0,0,1,1,1,1,1,0}, //b
  {1,0,0,1,1,1,0,0}, //C
  {0,1,1,1,1,0,1,0}, //d
  {1,0,0,1,1,1,1,0}, //E
  {1,0,0,0,1,1,1,0}  //F
};

// Fungsi untuk menampilkan angka ke seven segment
void displayDigit(int num)
{
  // Mengatur setiap segmen sesuai pola
  for(int i=0; i<8; i++)
  {
    digitalWrite(segmentPins[i], digitPattern[num][i]);
  }
}

void setup()
{
  // Mengatur semua pin segment sebagai OUTPUT
  for(int i=0; i<8; i++)
  {
    pinMode(segmentPins[i], OUTPUT);
  }

  // Menampilkan angka awal (F)
  displayDigit(counter);
}

void loop()
{
  // Menampilkan angka saat ini
  displayDigit(counter);

  // Mengurangi nilai counter (F → 0)
  counter--;

  // Jika sudah kurang dari 0, kembali ke 15 (F)
  if(counter < 0) counter = 15;

  // Delay agar perubahan terlihat
  delay(1000);
}

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
Jawab:Kode Program dan Penjelasan
// Pin 7-Segment (a b c d e f g dp)
// Array ini menyimpan pin Arduino yang terhubung ke masing-masing segmen
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};

// Push button
// buttonUp untuk menambah nilai, buttonDown untuk mengurangi nilai
const int buttonUp = 2;
const int buttonDown = 3;

// Counter untuk menyimpan angka yang ditampilkan
int counter = 0;

// Variabel untuk menyimpan kondisi tombol sebelumnya
// Digunakan untuk mendeteksi perubahan saat tombol ditekan
bool lastUpState = HIGH;
bool lastDownState = HIGH;

// Pola digit 0-F untuk seven segment
// Setiap baris mewakili satu angka (0–F)
// 1 = LED menyala, 0 = LED mati
byte digitPattern[16][8] = {
  {1,1,1,1,1,1,0,0}, //0
  {0,1,1,0,0,0,0,0}, //1
  {1,1,0,1,1,0,1,0}, //2
  {1,1,1,1,0,0,1,0}, //3
  {0,1,1,0,0,1,1,0}, //4
  {1,0,1,1,0,1,1,0}, //5 
  {1,0,1,1,1,1,1,0}, //6
  {1,1,1,0,0,0,0,0}, //7
  {1,1,1,1,1,1,1,0}, //8
  {1,1,1,1,0,1,1,0}, //9
  {1,1,1,0,1,1,1,0}, //A
  {0,0,1,1,1,1,1,0}, //b
  {1,0,0,1,1,1,0,0}, //C
  {0,1,1,1,1,0,1,0}, //d
  {1,0,0,1,1,1,1,0}, //E
  {1,0,0,0,1,1,1,0}  //F
};

// Fungsi untuk menampilkan angka ke seven segment
void displayDigit(int num)
{
  // Loop untuk setiap segmen
  for(int i=0; i<8; i++)
  {
    // Mengirim HIGH atau LOW sesuai pola digit
    digitalWrite(segmentPins[i], digitPattern[num][i]);
  }
}

void setup()
{
  // Mengatur semua pin segment sebagai OUTPUT
  for(int i=0; i<8; i++)
  {
    pinMode(segmentPins[i], OUTPUT);
  }

  // Mengatur tombol sebagai INPUT_PULLUP
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

  // Update kondisi tombol sebelumnya
  lastUpState = currentUp;
  lastDownState = currentDown;
}
