#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// OLED setup
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET     -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Pin configuration
const int irPin = 2;    // IR sensor output
const int ledPin = 3;   // External LED pin

int objectCount = 0;
bool objectPreviouslyDetected = false;

void setup() {
  pinMode(irPin, INPUT);
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW); // LED initially OFF

  Serial.begin(9600);

  // OLED initialization
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("OLED init failed!"));
    while (1);
  }

  // Display starting screen
  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(WHITE);
  display.setCursor(10, 20);
  display.print("Starting...");
  display.display();
  delay(2000);
  display.clearDisplay();
  updateOLED();
}

void loop() {
  int irStatus = digitalRead(irPin);

  if (irStatus == LOW && !objectPreviouslyDetected) {
    // Object just detected
    objectCount++;
    objectPreviouslyDetected = true;

    digitalWrite(ledPin, HIGH); // Turn ON external LED
    updateOLED();

    Serial.print("Object Count: ");
    Serial.println(objectCount);
  }

  // Reset detection flag and turn OFF LED when object is gone
  if (irStatus == HIGH) {
    objectPreviouslyDetected = false;
    digitalWrite(ledPin, LOW); // Turn OFF external LED
  }

  delay(50); // debounce delay
}

void updateOLED() {
  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(WHITE);
  display.setCursor(10, 10);
  display.print("Objects:");
  display.setCursor(10, 35);
  display.print(objectCount);
  display.display();
}
