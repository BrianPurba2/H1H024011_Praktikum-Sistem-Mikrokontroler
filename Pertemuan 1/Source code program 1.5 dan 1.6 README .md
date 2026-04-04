**Source code 1.5:**
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
}
**Source code 1.6**
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
