<h3>**Seven Segment:**</h3><br>
// Pin 7-Segment (a b c d e f g dp)<br>
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};<br>

// Counter mulai dari F (15)<br>
int counter = 15;<br>

// Pola digit 0-F<br>
byte digitPattern[16][8] = {<br>
  {1,1,1,1,1,1,0,0},<br>
  {0,1,1,0,0,0,0,0},<br>
  {1,1,0,1,1,0,1,0},<br>
  {1,1,1,1,0,0,1,0},<br>
  {0,1,1,0,0,1,1,0},<br>
  {1,0,1,1,0,1,1,0},<br>
  {1,0,1,1,1,1,1,0},<br>
  {1,1,1,0,0,0,0,0},<br>
  {1,1,1,1,1,1,1,0},<br>
  {1,1,1,1,0,1,1,0},<br>
  {1,1,1,0,1,1,1,0},<br>
  {0,0,1,1,1,1,1,0},<br>
  {1,0,0,1,1,1,0,0},<br>
  {0,1,1,1,1,0,1,0},<br>
  {1,0,0,1,1,1,1,0},<br>
  {1,0,0,0,1,1,1,0}<br>
};<br>

void displayDigit(int num)<br>
{<br>
  for(int i = 0; i < 8; i++)<br>
  {<br>
    digitalWrite(segmentPins[i], digitPattern[num][i]);<br>
  }<br>
}<br>

void setup()<br>
{<br>
  for(int i = 0; i < 8; i++)<br>
  {<br>
    pinMode(segmentPins[i], OUTPUT);<br>
  }<br>
}<br>

void loop()<br>
{<br>
  displayDigit(counter);<br>
  counter--;<br>

  if(counter < 0) counter = 15;<br>

  delay(1000);<br>
}<br>

<h3>**Push Button:**</h3><br>
// Pin 7-Segment (a b c d e f g dp)<br>
const int segmentPins[8] = {7, 6, 5, 11, 10, 8, 9, 4};<br>

// Push button<br>
const int buttonUp = 2;     // increment<br>
const int buttonDown = 3;   // decrement<br>

// Counter<br>
int counter = 0;<br>

// State tombol<br>
bool lastUpState = HIGH;<br>
bool lastDownState = HIGH;<br>

// Pola digit 0-F<br>
byte digitPattern[16][8] = {<br>
  {1,1,1,1,1,1,0,0}, {0,1,1,0,0,0,0,0},<br>
  {1,1,0,1,1,0,1,0}, {1,1,1,1,0,0,1,0},<br>
  {0,1,1,0,0,1,1,0}, {1,0,1,1,0,1,1,0},<br>
  {1,0,1,1,1,1,1,0}, {1,1,1,0,0,0,0,0},<br>
  {1,1,1,1,1,1,1,0}, {1,1,1,1,0,1,1,0},<br>
  {1,1,1,0,1,1,1,0}, {0,0,1,1,1,1,1,0},<br>
  {1,0,0,1,1,1,0,0}, {0,1,1,1,1,0,1,0},<br>
  {1,0,0,1,1,1,1,0}, {1,0,0,0,1,1,1,0}<br>
};<br>

// Fungsi tampil digit<br>
void displayDigit(int num)<br>
{<br>
  for(int i=0; i<8; i++)<br>
  {<br>
    digitalWrite(segmentPins[i], digitPattern[num][i]);<br>
  }<br>
}<br>

void setup()<br>
{<br>
  for(int i=0; i<8; i++)<br>
  {<br>
    pinMode(segmentPins[i], OUTPUT);<br>
  }<br>

  pinMode(buttonUp, INPUT_PULLUP);<br>
  pinMode(buttonDown, INPUT_PULLUP);<br>

  displayDigit(counter);<br>
}<br>

void loop()<br>
{<br>
  bool currentUp = digitalRead(buttonUp);<br>
  bool currentDown = digitalRead(buttonDown);<br>

  // Tombol increment<br>
  if(lastUpState == HIGH && currentUp == LOW)<br>
  {<br>
    counter++;<br>
    if(counter > 15) counter = 0;<br>

    displayDigit(counter);<br>
    delay(200);<br>
  }<br>

  // Tombol decrement<br>
  if(lastDownState == HIGH && currentDown == LOW)<br>
  {<br>
    counter--;<br>
    if(counter < 0) counter = 15;<br>

    displayDigit(counter);<br>
    delay(200);<br>
  }<br>

  lastUpState = currentUp;<br>
  lastDownState = currentDown;<br>
}
