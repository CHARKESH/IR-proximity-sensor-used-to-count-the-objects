IR PROXIMITY SENSOR USED TO COUNT THE OBJECTS.

COMPONENTS REQUIRED:

Arduino Uno
IR Proximity Sensor
SSD1306 OLED Display (I2C - 128x64)
LED (External)
220Ω Resistor
Breadboard & Jumper Wires

CIRCUIT CONNECTIONS:

1.IR Sensor

VCC → 5V (Arduino)
GND → GND
OUT → D2 (Arduino)

2.LED + Resistor

Longer leg (anode) of LED → 220Ω resistor → D3 (Arduino)
Shorter leg (cathode) → GND

3.OLED Display

VCC → 5V
GND → GND
SDA → A4
SCL → A5
