#  Lesson 3 — Ultrasonic Sensor (Keyestudio 4WD BT Car)

##  Objectives

* Understand how an **ultrasonic sensor** measures distance using sound waves.
* Control the **Trigger** and read the **Echo** pin to calculate time-of-flight.
* Use `pulseIn()` and math to convert duration into **cm** and **inches**.
* Display sensor output on the **Serial Monitor**.
* Practice using **`if` statements** to detect nearby obstacles.

---

##  What Is an Ultrasonic Sensor?

The **HC-SR04 ultrasonic sensor** is like sonar for your robot.

* It sends out a **high-frequency sound pulse** (inaudible to humans)
* It waits for the **echo** to bounce off an object
* The Arduino measures how long it takes for the echo to return

Using this **time-of-flight**, you can calculate how far away the object is!

>  It uses the **speed of sound** (343 meters/sec) to convert time into distance.

###  How It Works

<img width="500" height="268" alt="image" src="https://github.com/user-attachments/assets/e8c86b76-aa77-4b8f-9b4b-add5b7c4ceb9" />

* You trigger the sensor with a 10µs pulse on the **Trig** pin
* The sensor responds by sending a pulse on the **Echo** pin
* The width of the echo pulse tells you how far the object is

###  Time and Distance Formulas

```
duration = pulseIn(echoPin, HIGH);
cm = (duration / 2.0) / 29.1;
inches = (duration / 2.0) / 74.0;
```

---

##  Wiring Instructions

<img width="540" height="357" alt="image" src="https://github.com/user-attachments/assets/4c226e52-1172-4454-a67d-2036745b1e43" />

| HC-SR04 Pin | Connect To |
| ----------- | ---------- |
| VCC         | 5V         |
| GND         | GND        |
| TRIG        | D12        |
| ECHO        | D13        |

 **Tip:** The Echo pin must be on a digital pin that supports `pulseIn()`.

📷 Suggested images to add:

```markdown
![Ultrasonic Wiring](/images/ultrasonic_wiring.png)
![Sensor Diagram](/images/hcsr04_diagram.png)
![Pulse Timing](/images/hcsr04_timing.png)
```

---

##  Arduino Program Structure

```cpp
void setup() {
  // Runs once
}

void loop() {
  // Repeats forever
}
```

---

##  Step 1: Setup Sensor Pins

```cpp
int trigPin = 12;
int echoPin = 13;

void setup() {
  Serial.begin(9600);         // Start serial monitor
  pinMode(trigPin, OUTPUT);   // Trig sends signal
  pinMode(echoPin, INPUT);    // Echo receives signal
}
```

📄 Upload this code to confirm the pins are set up correctly.

---

##  Step 2: Trigger the Sensor

To take a measurement:

```cpp
// Trigger pulse sequence

digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
```

 Why so fast? The **10-microsecond pulse** is the signal to start the sonar ping.

---

##  Step 3: Measure and Calculate Distance

```cpp
long duration = pulseIn(echoPin, HIGH);
float cm = (duration / 2.0) / 29.1;
float inches = (duration / 2.0) / 74.0;
```

Now print to the Serial Monitor:

```cpp
Serial.print("Distance: ");
Serial.print(cm);
Serial.print(" cm\t");
Serial.print(inches);
Serial.println(" inches");
```

📄 Upload and open Serial Monitor to see results.

---

## 🖥 How to Use Serial Monitor

* Click the **magnifying glass icon** in the top-right of the Arduino IDE.
* Make sure the baud rate is **9600**.
* Move your hand in front of the sensor and watch the distance change.

 Suggested image:

```markdown
![Serial Monitor Guide](/images/serial_monitor.png)
```

---

##  Full Example Code

```cpp
int trigPin = 12;
int echoPin = 13;

long duration;
float cm, inches;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);

  cm = (duration / 2.0) / 29.1;
  inches = (duration / 2.0) / 74.0;

  Serial.print("Distance: ");
  Serial.print(cm);
  Serial.print(" cm\t");
  Serial.print(inches);
  Serial.println(" inches");

  delay(500);
}
```

📄 Upload and open Serial Monitor to view live sensor data.

---

##  Learn: What Is an `if` Statement?

An `if` statement lets your robot make decisions.

```cpp
if (cm < 10) {
  Serial.println("🚫 Object too close!");
}
```

* If the condition is **true**, the code inside the `{}` runs.
* You can adjust the number to change the trigger distance.

---

## 🌟 Student Challenges

*  Display a message like "Object Detected!" when something is close
*  Modify the math to convert distance differently
*  Measure how far away you can detect your hand
*  Try adding an `if` statement to warn if object is < 10cm

```cpp
if (cm < 10) {
  Serial.println("⚠️ Object Detected!");
}
```

📄 Once you're confident, move on to Lesson 4 (IR Remote Control)!

---

##  Quick Reference Table

| Concept       | Function              | Description                                   |
| ------------- | --------------------- | --------------------------------------------- |
| Setup pins    | `pinMode()`           | Set pin as input or output                    |
| Trigger pulse | `digitalWrite()`      | Controls signal on trigPin                    |
| Delay small   | `delayMicroseconds()` | Short delays in microseconds                  |
| Echo time     | `pulseIn()`           | Measures time of echo pulse (in microseconds) |
| Serial out    | `Serial.print()`      | Displays text or values to computer           |
| Conditional   | `if`                  | Executes code if condition is true            |

---

👉 [Next: Lesson 4 - LED Matrix](https://github.com/ArtMil86/ECE-IEEE-Summer-Workshop/blob/main/Lesson%204%20-%208x16%20LED%20Matrix.md)

