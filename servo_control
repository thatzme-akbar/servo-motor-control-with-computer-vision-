#include <Servo.h>

Servo myServo;

void setup() {
  Serial.begin(9600);
  myServo.attach(9);
  myServo.write(0);  // Start in OFF position
}

void loop() {
  if (Serial.available() > 0) {
    char command = Serial.read();
    myServo.write(command == '1' ? 180 : 0);
  }
}
