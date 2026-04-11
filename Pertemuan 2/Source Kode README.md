**Seven Segment:**
// Pin 7-Segment (a b c d e f g dp)
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};

// Counter mulai dari F (15)
int counter = 15;

// Pola digit 0-F
byte digitPattern[16][8] = {
  {1,1,1,1,1,1,0,0},
  {0,1,1,0,0,0,0,0},
  {1,1,0,1,1,0,1,0},
  {1,1,1,1,0,0,1,0},
  {0,1,1,0,0,1,1,0},
  {1,0,1,1,0,1,1,0},
  {1,0,1,1,1,1,1,0},
  {1,1,1,0,0,0,0,0},
  {1,1,1,1,1,1,1,0},
  {1,1,1,1,0,1,1,0},
  {1,1,1,0,1,1,1,0},
  {0,0,1,1,1,1,1,0},
  {1,0,0,1,1,1,0,0},
  {0,1,1,1,1,0,1,0},
  {1,0,0,1,1,1,1,0},
  {1,0,0,0,1,1,1,0}
};

void displayDigit(int num)
{
  for(int i = 0; i < 8; i++)
  {
    digitalWrite(segmentPins[i], digitPattern[num][i]);
  }
}

void setup()
{
  for(int i = 0; i < 8; i++)
  {
    pinMode(segmentPins[i], OUTPUT);
  }
}

void loop()
{
  displayDigit(counter);
  counter--;

  if(counter < 0) counter = 15;

  delay(1000);
}

**Push Button:**
// Pin 7-Segment (a b c d e f g dp)
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};

// Push button
const int buttonUp = 2;     // increment
const int buttonDown = 3;   // decrement

// Counter
int counter = 0;

// State tombol
bool lastUpState = HIGH;
bool lastDownState = HIGH;

// Pola digit 0-F
byte digitPattern[16][8] = {
  {1,1,1,1,1,1,0,0}, {0,1,1,0,0,0,0,0},
  {1,1,0,1,1,0,1,0}, {1,1,1,1,0,0,1,0},
  {0,1,1,0,0,1,1,0}, {1,0,1,1,0,1,1,0},
  {1,0,1,1,1,1,1,0}, {1,1,1,0,0,0,0,0},
  {1,1,1,1,1,1,1,0}, {1,1,1,1,0,1,1,0},
  {1,1,1,0,1,1,1,0}, {0,0,1,1,1,1,1,0},
  {1,0,0,1,1,1,0,0}, {0,1,1,1,1,0,1,0},
  {1,0,0,1,1,1,1,0}, {1,0,0,0,1,1,1,0}
};

// Fungsi tampil digit
void displayDigit(int num)
{
  for(int i=0; i<8; i++)
  {
    digitalWrite(segmentPins[i], digitPattern[num][i]);
  }
}

void setup()
{
  for(int i=0; i<8; i++)
  {
    pinMode(segmentPins[i], OUTPUT);
  }

  pinMode(buttonUp, INPUT_PULLUP);
  pinMode(buttonDown, INPUT_PULLUP);

  displayDigit(counter);
}

void loop()
{
  bool currentUp = digitalRead(buttonUp);
  bool currentDown = digitalRead(buttonDown);

  // Tombol increment
  if(lastUpState == HIGH && currentUp == LOW)
  {
    counter++;
    if(counter > 15) counter = 0;

    displayDigit(counter);
    delay(200);
  }

  // Tombol decrement
  if(lastDownState == HIGH && currentDown == LOW)
  {
    counter--;
    if(counter < 0) counter = 15;

    displayDigit(counter);
    delay(200);
  }

  lastUpState = currentUp;
  lastDownState = currentDown;
}
