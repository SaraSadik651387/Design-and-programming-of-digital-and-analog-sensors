# Design-and-programming-of-digital-sensor

This project demonstrates how to use an **IR Sensor (Infrared Receiver)** with an **Arduino Uno** to decode signals from an **IR Remote Control** and control devices like LEDs based on the received commands.

---

## **Overview**

The IR sensor receives signals from the remote control, decodes them, and allows the Arduino to interpret which button was pressed. Based on the received signal, specific actions (e.g., turning an LED ON or OFF) are executed.

---

## **Components**

1. **Arduino Uno**
2. **IR Receiver**
3. **IR Remote Control**
4. **LED**
5. **330Ω Resistor**
6. **Jumper Wires**
7. **Breadboard**

---

## **How the IR Sensor Works**

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


### **Connections for the IR Sensor:**
| IR Sensor Pin   | Arduino Pin   |
|------------------|---------------|
| **VCC**          | **5V**        |
| **GND**          | **GND**       |
| **OUT**          | **D3**        |

---

## **Code Explanation**

- **`IRrecv`**: Initializes the receiver on the specified pin.
- **`decode_results`**: Holds the data received from the remote.
- **`IrReceiver.decode()`**: Decodes the signal from the remote.
- **`decodedRawData`**: Provides the raw data for the pressed button.

---

## **How to Use**

1. **Set Up the Circuit:**
   - Connect the **IR Sensor** as shown in the circuit diagram.
   - Connect the LED to pin **D5** with a 330Ω resistor.

2. **Upload the Code:**
   - Use the provided Arduino sketch to upload the program.

3. **Test the Setup:**
   - Open the **Serial Monitor**.
   - Press a button on the IR Remote and observe:
     - The **decoded value** for the button.
     - The LED turning **ON** or **OFF** based on the button pressed.

---

## **Code**

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
