# WIE-are-Makers RobotCars — Final Challenges (Instructions Only)

> **Read first:** Use the **same pin map and helper functions from your Lessons** (e.g.,  
> `M1_DIR/M1_SPD`, `M2_DIR/M2_SPD`, `stopAll()`, `driveForward()`, `driveBackward()`, `turnLeft()`, `turnRight()`) and the examples from:
> - **Lesson 1 – Servo Motor**
> - **Lesson 2 – Ultrasonic Sensor**
> - **Lesson 3 – Motors (Forward/Backward/Turns)**
> - **Lesson 4 – 8×16 LED Matrix**
>
> Do **not** invent new pins or names—reuse your lesson templates so your code stays consistent.


---

## Challenge A — Roaming Obstacle Avoider (with LED Matrix Reaction)

**Goal:** Robot roams forward. When an obstacle is near, it **reacts on the LED matrix**, **backs up**, **turns away**, and **continues forward**. It repeats forever.

### Required behaviors
- Drive forward until an object is detected **closer than `10cm`**.
- When too close:
  - Show a **reaction image** on the **8×16 LED matrix** (for example an X) — from **Lesson 4**.
  - **Back up** briefly.
  - **Turn** left **or** right (your choice: random, alternate, or based on last turn).
  - Resume **forward** drive.
- Keep roaming and avoiding continuously.

### Build notes (use your Lesson code)
- **Motors:** call your `driveForward(pwm)`, `driveBackward(pwm)`, `turnLeft(pwm)`, `turnRight(pwm)`, `stopAll()`.
- **Ultrasonic:** reuse your **Lesson 2** trigger/echo read to get distance in **cm**.
- **LED Matrix:** reuse your **Lesson 4** helper to draw one simple icon or message.

## Challenge B — Follow-Me Tracker (Servo Sweep + Ultrasonic)

> **Use your existing lesson scaffolding.** Reuse the **same pins** and **helper functions** from your previous lessons:
> - **Servo control** from *Lesson 1 – Servo Motor*  
> - **Ultrasonic read** from *Lesson 2 – Ultrasonic Sensor*  
> - **Motor helpers** (`driveForward()`, `turnLeft()`, `turnRight()`, `stopAll()`, etc.) from *Lesson 3 – Motors*  
> - (Optional) **LED/Matrix status** from *Lesson 4 – 8×16 LED Matrix*  

###  Goal
Make the robot **track a nearby object (you!)** by sweeping the ultrasonic on a **servo** to find the closest direction, **turn toward it**, then **approach** and **stop** at a safe distance (≈ **5–10 cm**). If the object moves, the robot **re-scans** and follows again.

---

### Required Behaviors
- Perform a **servo sweep** across your chosen angles and **measure distance** at each step.
- Pick the **best angle** (closest valid reading).
- **Turn** the robot toward that direction until it’s roughly centered.
- **Drive forward** until the object is within **STOP distance**, then **stop**.
- If the object moves away, **repeat**: sweep → re-aim → approach → stop.

---

###  Build Notes (tie back to lessons)
- **Servo sweep**: `Servo.write(angle)` with a **small delay** (≈ **10–25 ms**) after each move so readings settle.  
- **Ultrasonic**: take **2–3 readings** per angle and keep the **minimum** (simple de-noise).  
- **Motors**: use **short nudges** (brief turns/forward bursts) rather than long moves, so you can correct often.  
- **Safety**: keep a hand near the switch; avoid driving into people/walls.



