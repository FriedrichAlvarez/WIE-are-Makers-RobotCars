#  Lesson 1 — Servo Motor (Keyestudio 4WD BT Car)

##  Objectives

* Understand what a servo motor is and how it works.
* Learn how to control a servo in TinkerCAD.
* Use a `for` loop to create a sweeping motion from 0° to 180°.
* Learn how `delay()` affects motion speed and servo behavior.

---

##  Part 1: What Is a Servo Motor?
<img width="713" height="310" alt="image" src="https://github.com/user-attachments/assets/acecd15d-0b2a-4dbf-b168-d0948b2bdaaf" />

A **servo motor** is a special kind of motor that rotates to a **specific angle**, usually between **0° and 180°**. It is often used for precise control of motion in robotics, such as steering, gripping, or sensor positioning.

Another way to think about it: A regular DC motor is like a **fan** because it has continuous spinning. On the other hand, a servo motor is like a **steering wheel** because it has precise control over the degree of rotation.

###  Inside a Servo

A typical servo contains:

* A **small DC motor**
* A **gearbox**
* A **position sensor** (potentiometer)
* Control circuitry

---

##  Pin Wiring Table

<img width="269" height="251" alt="image" src="https://github.com/user-attachments/assets/c27bf2eb-c8e7-4e62-a981-c70dd455c5f5" />

In TinkerCAD, ensure that the circuit design template is connected like this:

| Servo Port   | Connect To |
| ------------ | ---------- |
| Ground       | **GND**    |
| Power        | **5V**     |
| Signal       | **4**      |

## Basic Rotation

We can do some basic rotating using this block:

<img width="565" height="141" alt="image" src="https://github.com/user-attachments/assets/8ecf10e5-5c25-4e6f-adc1-8fb1b54ef1a3" />

The block code is provided in the design. Make sure that the pin is **4**.

##  Part 2: What Is `delay()`?


The `delay()` function in Arduino/TinkerCAD pauses the program for a number of **milliseconds (ms)**.

```cpp
// Pause for 15 milliseconds
delay(15);
```
The block version of this command is known as the **wait** block in the Control tab.

<img width="379" height="109" alt="image" src="https://github.com/user-attachments/assets/17009fb8-1e0a-4957-8614-66e48a0447ef" />

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

A `for` loop lets us **repeat code** a specific number of times. It is useful for slowly moving a servo through angles.

Here's what the code looks like in TinkerCAD:

<img width="627" height="469" alt="image" src="https://github.com/user-attachments/assets/0e09fb1e-584a-452d-bf70-f9668f95ee28" />

### What is this code doing?

In this code, we created a Variable called angle and set it 180. This represents the number of degrees we want the **Repeat** block to rotate.

So, this code is saying "Repeat this piece of code for 180 times"

Then, we rotate the servo to that angle, and include our delay to help the motor catch up.

##  Student Challenges

Try modifying the code to explore different behaviors:

*  Make it rotate slowly using `delay(30)`
*  Make it rotate faster using `delay(10)`
*  Bonus: Try jumping to random angles with `random(0, 180)`

 Don't stress about perfection — explore and observe!

---

##  Quick Reference Table for Arduino Text code

| Concept         | Function            | Description                           |
| --------------- | ------------------- | ------------------------------------- |
| Attach servo    | `myServo.attach(9)` | Connects servo signal to pin A3        |
| Set angle       | `myServo.write(90)` | Moves servo to 90°                    |
| Pause program   | `delay(15)`         | Waits 15 milliseconds (0.015 seconds) |
| For loop (up)   | `for (...) ++`      | Counts upward (0 → 180)               |
| For loop (down) | `for (...) --`      | Counts downward (180 → 0)             |

---

