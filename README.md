# Automatic Irrigation System (APIS)

An Arduino-based automatic irrigation system that monitors soil moisture in real time and switches a water pump on or off accordingly — removing the need for manual irrigation monitoring.

## Overview

Agriculture suffers significant crop losses due to improper or inconsistent irrigation, and manual monitoring of soil conditions is impractical for farmers managing large areas. This project automates that process: a soil moisture sensor continuously checks the water content of the soil, and an Arduino Uno uses that reading to control a relay-driven DC water pump — supplying water only when the soil is actually dry, and stopping automatically once adequate moisture is restored.

## Motivation

Built in the context of agriculture-dependent regions facing water scarcity, the goal was to minimize manual intervention by farmers, reduce water wastage from unplanned/manual watering, and free up labor time that would otherwise go into manually switching pumps on and off.

## How It Works

1. The Arduino Uno powers up and continuously reads the soil moisture sensor's digital output.
2. If the sensed threshold voltage is **below 2.5 mV** (dry soil), the Arduino drives the relay to switch the DC pump **ON**, and water is supplied until adequate moisture is reached.
3. If the threshold voltage is **above 2.5 mV** (sufficiently moist soil), the relay is switched **OFF**, stopping the pump.
4. This loop runs continuously and requires no manual trigger after setup.

## Hardware Components

| Component | Purpose |
|---|---|
| Arduino UNO (ATmega328P) | Microcontroller — reads sensor, controls relay |
| Soil Moisture Sensor | Detects volumetric water content in soil |
| 5V Relay Module | Switches the higher-current DC pump circuit using the Arduino's low-current digital signal |
| DC Water Pump + Pipe | Delivers water to the plant |
| 9V Battery | Powers the DC pump |
| Jumper Wires | Circuit connections |
| USB-A to B Cable | Programming/power connection to Arduino |

## Software

- **Arduino IDE** — used to write, compile, and upload the control sketch (C/C++) to the Arduino Uno.

## Circuit Connections

- Soil Moisture Sensor `D0` → Arduino pin 6; sensor `Vcc` → Arduino `Vin`
- Relay Module `IN` → Arduino pin 3; relay `Vcc` → Arduino `5V`
- All component ground terminals → Arduino `GND`
- DC motor positive → Relay `NC`; Battery positive → Relay `COM`
- DC motor negative → Battery negative
- Soil moisture sensor probes inserted directly into the plant's soil

## Code

```cpp
int water; // stores sensor reading

void setup() {
  pinMode(3, OUTPUT); // output pin for relay board
  pinMode(6, INPUT);  // input pin from soil sensor
}

void loop() {
  water = digitalRead(6); // read signal from soil sensor

  if (water == HIGH) {
    digitalWrite(3, LOW);  // soil sufficiently moist -> cut relay (pump off)
  } else {
    digitalWrite(3, HIGH); // soil dry -> activate relay (pump on)
  }

  delay(100);
}
```

## Results

The system was tested and confirmed to:
- Automatically activate the pump when soil moisture dropped below the 2.5 mV threshold
- Automatically stop the pump once sufficient moisture was restored
- Run continuously without manual intervention after initial setup

## Advantages

- Faster than manual irrigation monitoring
- Simple, low-power, and portable circuit
- Reduces water wastage from unplanned/manual watering
- Saves labor and water (up to ~70% in comparable large-scale irrigation systems)
- Lets non-experts manage irrigation that would otherwise require expert oversight

## Applications

- Rooftop and home gardens with limited monitoring availability
- Large-scale agricultural fields
- Public lawns and park irrigation
- Pisciculture (maintaining consistent water depth in fish farming tanks)

## Team

- Batana Dedeepya
- Chowdada Bhargavi
- Korada Harika
- Mandala Lavanya

**Guide:** Mrs. Ch. Sirisha, Assistant Professor  
Department of Electronics and Communication Engineering, GVP College of Engineering for Women (JNTU Kakinada), 2020–2024

## Conclusion

The Automatic Plant Irrigation System (APIS) was designed, built, and tested successfully, integrating a soil moisture sensor, relay module, and DC pump into a single self-regulating circuit. The system reliably switches irrigation on and off based on real soil conditions, demonstrating a low-cost, scalable approach to automated irrigation.
