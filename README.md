# Obstacle-Detector
Arduino/ESP32-based obstacle detection system
#define trigPin 9
#define echoPin 10
#define buzzer 6
#define motor 5
#define potPin A0

long duration;
int distance;
int threshold;

void setup() {
  Serial.begin(9600);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(motor, OUTPUT);
}

void loop() {

  // 🔁 Read potentiometer (0–1023)
  int potValue = analogRead(potPin);

  // Convert to distance threshold (10cm to 100cm)
  threshold = map(potValue, 0, 1023, 10, 100);

  // 📡 Ultrasonic trigger
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  
  digitalWrite(trigPin, LOW);

  // ⏱️ Read echo
  duration = pulseIn(echoPin, HIGH);

  // 📏 Convert to distance (cm)
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.print(" cm | Threshold: ");
  Serial.println(threshold);

  // 🔔 Condition
  if (distance > 0 && distance < threshold) {
    digitalWrite(buzzer, HIGH);
    digitalWrite(motor, HIGH);
  } else {
    digitalWrite(buzzer, LOW);
    digitalWrite(motor, LOW);
  }

  delay(200);
}
