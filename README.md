# Design and programming of digital and analog sensors

This project demonstrates how to use an IR Sensor (Infrared Receiver) with an Arduino Uno to decode signals from an IR Remote Control and control devices like LEDs based on the received commands. Additionally, it explores the use of an Analog Temperature Sensor (TMP36) to read and respond to temperature changes.

---

## Overview

The IR sensor receives signals from the remote control, decodes them, and allows the Arduino to interpret which button was pressed. Based on the received signal, specific actions (e.g., turning an LED ON or OFF) are executed.

In the Analog Sensor project, the TMP36 temperature sensor measures the ambient temperature, and the Arduino interprets this data to control LEDs, providing a visual representation of the temperature range.

---

## Components

- Arduino Uno
- IR Receiver
- IR Remote Control
- TMP36 Temperature Sensor
- LEDs (Red, Green, Blue)
- 330Ω Resistors
- Jumper Wires
- Breadboard

---

## How the IR Sensor Works

1. **Signal Transmission:**
   - The **IR Remote Control** emits infrared light when a button is pressed.

2. **Signal Reception:**
   - The **IR Sensor (Receiver)** detects the modulated infrared signal.
   - The signal contains a unique **binary code** for each button.

3. **Decoding:**
   - The **IRremote library** decodes the signal and converts it into a readable number (unique for each button).

4. **Action Execution:**
   - Based on the decoded value, the Arduino executes predefined actions like turning an LED ON or OFF.

---

## How the TMP36 Temperature Sensor Works

1. **Signal Generation:**
   - The TMP36 outputs a voltage proportional to the ambient temperature.
   - The Arduino reads this voltage as an analog input.

2. **Temperature Conversion:**
   - The analog voltage is converted to a temperature in Celsius using a mathematical formula.

3. **LED Control:**
   - LEDs are turned ON or OFF based on the temperature range.

---

## How to Use

### IR Sensor Project

1. **Set Up the Circuit:**
   - Connect the IR Sensor as shown in the circuit diagram.
   - Connect the LED to pin D5 with a 330Ω resistor.

2. **Upload the Code:**
   - Use the provided Arduino sketch to upload the program.

3. **Test the Setup:**
   - Open the Serial Monitor.
   - Press a button on the IR Remote and observe:
     - The decoded value for the button.
     - The LED turning ON or OFF based on the button pressed.

### TMP36 Project

1. **Set Up the Circuit:**
   - Connect the TMP36 Sensor and LEDs as shown in the circuit diagram.
   - Use 330Ω resistors with each LED.

2. **Upload the Code:**
   - Use the provided Arduino sketch to upload the program.

3. **Test the Setup:**
   - Observe the LEDs turning ON or OFF based on the temperature:
     - Below 40°C: All LEDs OFF.
     - 40-50°C: Red LED ON.
     - 50-60°C: Red and Blue LEDs ON.
     - 60°C and Above: All LEDs ON.

---

## Code

### IR Sensor Code

```cpp
#include <IRremote.hpp>

const int rcvPin = 3; // Pin for IR Sensor
IRrecv irrecv(rcvPin);
decode_results results;

void setup()
{ 
  Serial.begin(9600);
  irrecv.enableIRIn(); // Start the receiver
  pinMode(5, OUTPUT); // Pin for LED
}

void loop() {
  if (IrReceiver.decode()) {
    auto value = IrReceiver.decodedIRData.decodedRawData;
    switch (value)
    {        
        case 4010852096: // Button "1"
            Serial.println("1");
            digitalWrite(5, HIGH); // Turn LED ON
            break;
          
        case 3994140416: // Button "2"
            Serial.println("2");
            digitalWrite(5, LOW); // Turn LED OFF
            break;

        default:
            Serial.println(value); // Print the raw value
    }
    IrReceiver.resume(); // Ready for the next value
  }
}
```
##**TMP36 Temperature Sensor Code**
```cpp
int baselineTemp = 40;
int celsius = 0;
int fahrenheit = 0;

void setup()
{
  pinMode(A0, INPUT);
  Serial.begin(9600);

  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
}

void loop()
{
  celsius = map(((analogRead(A0) - 20) * 3.04), 0, 1023, -40, 125);
  fahrenheit = ((celsius * 9) / 5 + 32);

  Serial.print(celsius);
  Serial.print(" C, ");
  Serial.print(fahrenheit);
  Serial.println(" F");

  if (celsius < baselineTemp) {
    digitalWrite(2, LOW);
    digitalWrite(3, LOW);
    digitalWrite(4, LOW);
  } else if (celsius >= baselineTemp && celsius < baselineTemp + 10) {
    digitalWrite(2, HIGH);
    digitalWrite(3, LOW);
    digitalWrite(4, LOW);
  } else if (celsius >= baselineTemp + 10 && celsius < baselineTemp + 20) {
    digitalWrite(2, HIGH);
    digitalWrite(3, HIGH);
    digitalWrite(4, LOW);
  } else {
    digitalWrite(2, HIGH);
    digitalWrite(3, HIGH);
    digitalWrite(4, HIGH);
  }

  delay(1000); 
}
