#  Lesson 2 — Servo Motor (Keyestudio 4WD BT Car)

##  Objectives

* Understand what a servo motor is and how it works.
* Learn how to control a servo using the `Servo` library in Arduino.
* Use a `for` loop to create a sweeping motion from 0° to 180°.
* Learn how `delay()` affects motion speed and servo behavior.
* Practice timing and smooth control without damaging the motor.

---

##  What Is a Servo Motor?
<img width="713" height="310" alt="image" src="https://github.com/user-attachments/assets/acecd15d-0b2a-4dbf-b168-d0948b2bdaaf" />

A **servo motor** is a special kind of motor that rotates to a **specific angle**, usually between **0° and 180°**. It is often used for precise control of motion in robotics, such as steering, gripping, or sensor positioning.

###  Inside a Servo

A typical servo contains:

* A **small DC motor**
* A **gearbox**
* A **position sensor** (potentiometer)
* Control circuitry

The servo receives **PWM signals** from the Arduino. The signal width determines the target angle:

<img width="535" height="244" alt="image" src="https://github.com/user-attachments/assets/c1fe0d9e-cc57-43a0-ada4-5f06151fb35f" />

* Short pulse (e.g. 1 ms) → 0°
* Medium pulse (1.5 ms) → 90°
* Long pulse (2 ms) → 180°

> ℹ Most servos operate within a PWM signal that repeats every 20 ms (50 Hz).

---

##  Pin Wiring Table

<img width="269" height="251" alt="image" src="https://github.com/user-attachments/assets/c27bf2eb-c8e7-4e62-a981-c70dd455c5f5" />

| Servo Wire   | Connect To |
| ------------ | ---------- |
| Orange/White | **D9**     |
| Red (VCC)    | **5V**     |
| Brown/Black  | **GND**    |

⚠️ **Important:** Servos require more current than typical sensors. Always:

* Use the 5V pin for power.
* Make sure **GND is shared** between servo and Arduino.
* Use **external power** if running multiple servos.

---

##  What Is `delay()`?

The `delay()` function in Arduino pauses the program for a number of **milliseconds (ms)**.

```cpp
// Pause for 15 milliseconds
delay(15);
```

* `delay(1000)` → 1 second
* `delay(15)` → 15 **milliseconds**, or 0.015 seconds

###  Why It Matters for Servos

Servos **don’t instantly jump** to a new angle. They need time to rotate.

* **Smaller delay** → Faster sweep
* **Larger delay** → Slower sweep

If the delay is **too short**, the servo may jitter or get damaged trying to catch up.

 Recommended delay: between **10–30 ms** for 1° steps.

---

##  What Is a `for` Loop?

A `for` loop lets us **repeat code** a specific number of times. Useful for slowly moving a servo through angles.

```cpp
for (int angle = 0; angle <= 180; angle++) {
  myServo.write(angle);  // Move to next angle
  delay(15);             // Wait so servo can catch up
}
```

>  `angle++` means "increase angle by 1 each time"

You can reverse the direction:

```cpp
for (int angle = 180; angle >= 0; angle--) {
  myServo.write(angle);
  delay(15);
}
```

---

##  Arduino Sweep Code (Using Servo Library)

 This code uses the built-in `Servo.h` library.

```cpp
#include <Servo.h>

Servo myServo;  // Create Servo object

void setup() {
  myServo.attach(A3);  // Connect signal wire to pin A3
}

void loop() {
  // Sweep from 0° to 180°
  for (int angle = 0; angle <= 180; angle++) {
    myServo.write(angle);
    delay(15);  // 15 ms wait — safe and smooth
  }

  // Sweep back from 180° to 0°
  for (int angle = 180; angle >= 0; angle--) {
    myServo.write(angle);
    delay(15);
  }
}
```

 Upload this code and observe your servo sweep smoothly!

---

##  Student Challenges

Try modifying the code to explore different behaviors:

*  Make it sweep slowly using `delay(30)`
*  Make it sweep faster using `delay(10)`
*  Try sweeping from 45° to 135° only
*  Bonus: Try jumping to random angles with `random(0, 180)`

 Don't stress about perfection — explore and observe!

---

##  Quick Reference Table

| Concept         | Function            | Description                           |
| --------------- | ------------------- | ------------------------------------- |
| Attach servo    | `myServo.attach(9)` | Connects servo signal to pin A3        |
| Set angle       | `myServo.write(90)` | Moves servo to 90°                    |
| Pause program   | `delay(15)`         | Waits 15 milliseconds (0.015 seconds) |
| For loop (up)   | `for (...) ++`      | Counts upward (0 → 180)               |
| For loop (down) | `for (...) --`      | Counts downward (180 → 0)             |

---

👉 [Next: Lesson 3 - Ultrasonic Sensor](https://github.com/ArtMil86/ECE-IEEE-Summer-Workshop/blob/main/Lesson%203%20Ultrasonic%20Sensor.md))
