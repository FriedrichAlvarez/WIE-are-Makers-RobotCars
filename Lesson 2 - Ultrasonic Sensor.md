#  Lesson 2 — Ultrasonic Sensor (Keyestudio 4WD BT Car)

##  Objectives

* Understand how an **ultrasonic sensor** measures distance using sound waves.
* Control the **Trigger** and read the **Echo** pin to calculate time-of-flight.
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


You can think of it just like a **bat's echolocation**. An ultrasonic sensor is like a bat that shouts and listens for the echo to figure out how far away things are.

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
| VCC/Power   | 5V         |
| GND/Ground  | GND        |
| TRIG        | 4          |
| ECHO        | 5          |

 **Tip:** The Echo pin must be on a digital pin that supports `pulseIn()`.

---

##  Step 1: Setup Sensor Pins

In TinkerCAD, we are going to use this block from the **Input** tab.

<img width="963" height="157" alt="image" src="https://github.com/user-attachments/assets/e4b21970-4566-4dd0-9223-452711850eda" />

This block will get us a distance reading in centimeters. We can then set this to a variable.

Create a variable called "distance".

<img width="1192" height="114" alt="image" src="https://github.com/user-attachments/assets/1221c34c-d46b-4ab3-ae61-5232b0494cb3" />

Now, we can print this variable "distance" to the **Serial Monitor** to see the measurements print out.

<img width="1254" height="329" alt="image" src="https://github.com/user-attachments/assets/37091f79-5b0d-4d04-9ece-380c6ab22425" />


**Here is this code in text:**

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

To take a measurement, click on the Ultrasonic Sensor and then move the circle around.

<img width="795" height="744" alt="image" src="https://github.com/user-attachments/assets/8c2ffadb-7257-4a94-a02f-750b34c4a068" />

**Here is the code in text**

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

## 🖥 How to Use Serial Monitor

* Click the **Code** button in the top-right of the TinkerCAD interface.
* Below the block menu, find the **Serial Monitor** tab.
* When you run your simulation, you see the numbers being actively printed out.

---

##  Learn: What Is an `if` Statement?

An `if` statement lets your robot/code make decisions.

<img width="1216" height="422" alt="image" src="https://github.com/user-attachments/assets/e57ab759-44f2-4477-b6bd-7bc8d26ccec4" />


```cpp
if (cm < 10) {
  Serial.println("🚫 Object too close!");
}
```

* If the condition is **true**, the code inside the block runs.
* You can adjust the number to change the trigger distance.

---

## 🌟 Student Challenges

*  Display a message like "Object Detected!" when something is close
*  Display a message like "All clear!" when nothing is close
*  Change your circuit to have an LED serve as a warning light that turns on when the distance is too close

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
---

##  Quick Reference Table for Arduino Text code

| Concept       | Function              | Description                                   |
| ------------- | --------------------- | --------------------------------------------- |
| Setup pins    | `pinMode()`           | Set pin as input or output                    |
| Trigger pulse | `digitalWrite()`      | Controls signal on trigPin                    |
| Delay small   | `delayMicroseconds()` | Short delays in microseconds                  |
| Echo time     | `pulseIn()`           | Measures time of echo pulse (in microseconds) |
| Serial out    | `Serial.print()`      | Displays text or values to computer           |
| Conditional   | `if`                  | Executes code if condition is true            |

---
