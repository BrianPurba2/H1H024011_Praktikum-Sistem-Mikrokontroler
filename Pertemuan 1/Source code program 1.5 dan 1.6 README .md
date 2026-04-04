**Source code 1.5:**
const int ledPin = 6;<br>
int timeDelay = 100;<br>

void setup() {<br>
  pinMode(ledPin, OUTPUT);<br>
}<br>

void loop() {<br>
  digitalWrite(ledPin, HIGH);<br>
  delay(timeDelay);<br>

  digitalWrite(ledPin, LOW);<br>
  delay(timeDelay);<br>

  if (timeDelay == 100) {<br>
    timeDelay = 500;<br>
  } else if (timeDelay == 500) {<br>
    timeDelay = 1000;<br>
  } else {<br>
    timeDelay = 1000;<br>
  }<br>
}<br>
**Source code 1.6**
void setup() {<br>
  for (int pin = 2; pin <= 7; pin++) {<br>
    pinMode(pin, OUTPUT);<br>
  }<br>
}

void loop() {<br>
  // tiga kiri nyala<br>
  digitalWrite(2, HIGH);<br>
  digitalWrite(3, HIGH);<br>
  digitalWrite(4, HIGH);<br>
  digitalWrite(5, LOW);<br>
  digitalWrite(6, LOW);<br>
  digitalWrite(7, LOW);<br>
  delay(500);<br>

  // tiga kanan nyala<br>
  digitalWrite(2, LOW);<br>
  digitalWrite(3, LOW);<br>
  digitalWrite(4, LOW);<br>
  digitalWrite(5, HIGH);<br>
  digitalWrite(6, HIGH);<br>
  digitalWrite(7, HIGH);<br>
  delay(500);<br>
}<br>
