##  Objectives

* Understand how an 8x16 LED Matrix works and what it can display.
* Learn how to wire and control the LED matrix using two digital pins (SCL and SDA).
* Use Arduino code to display basic images like a smiley face.
* Understand how hexadecimal arrays light up pixels on the matrix.
* Learn how to use a **modulus tool** to create your own patterns.
* Learn how to animate sequences like "start → forward → stop" with delays.

---

##  What Is an LED Matrix?

<img width="933" height="305" alt="image" src="https://github.com/user-attachments/assets/b9b7e387-ab39-4260-b71d-16b530cffe42" />

An **LED matrix** is a grid of small LEDs arranged in rows and columns. The 8x16 matrix has:

* **8 rows** tall
* **16 columns** wide

So, 8 × 16 = **128 LEDs total**!

Each LED can be turned **ON** or **OFF** individually using special data called **binary** or **hexadecimal**.

###  Real-World Examples

* Emoji faces 🤖
* Arrow indicators ↔️
* Animations or icons 🎮

---

##  How It Works Internally

<img width="640" height="302" alt="image" src="https://github.com/user-attachments/assets/f8e45506-98b1-47c3-9206-3f943ab9a79e" />

The display uses a special chip called **AiP1640** to light up the pixels. The Arduino talks to this chip using just **2 pins**:

| Function | Pin |
| -------- | --- |
| Clock    | A5  |
| Data     | A4  |

This is similar to I2C communication but follows its own custom timing. The Arduino sends bytes to the AiP1640 chip, and each **bit** in those bytes turns ON or OFF an LED in a row.

###  How Bits Light Up LEDs

Each LED row is controlled by **1 byte** = 8 bits.

Example:

```cpp
0b10101010 → ON, OFF, ON, OFF, ON, OFF, ON, OFF
```

Or using HEX:

```cpp
0xAA = 0b10101010
```

Each pattern is **16 bytes long**, one for each column!

---

##  Step 1: Wire the LED Matrix

<img width="540" height="223" alt="image" src="https://github.com/user-attachments/assets/a6856ece-154b-4342-96bf-9dff5195267a" />

| LED Matrix Pin | Connect To | Description  |
| -------------- | ---------- | ------------ |
| GND            | GND        | Ground       |
| VCC            | 5V         | Power Supply |
| SCL            | A5         | Clock Pin    |
| SDA            | A4         | Data Pin     |

 **Note:** These two pins are not I2C pins. This module just uses the same pins but with a different protocol.

---

##  Step 2: Design Your Pattern

<img width="405" height="380" alt="image" src="https://github.com/user-attachments/assets/d51d864f-64ee-4f54-8e46-9f01385204cc" />

Go to [dotmatrixtool.com](http://dotmatrixtool.com/#)

### How to use the tool:

1. Set **width to 16** and **height to 8**
2. Select **Big Endian**
3. Draw your face or pattern
4. Click **Generate** → Get a list of **16 HEX numbers**

Example (Smiley Face):

```cpp
unsigned char smile[] = {
  0x00, 0x00, 0x1c, 0x02, 0x02, 0x02, 0x5c, 0x40,
  0x40, 0x5c, 0x02, 0x02, 0x02, 0x1c, 0x00, 0x00
};
```

Each HEX value controls one column of the matrix.

**When Generating design, only copy the HEX value line. For Example:**

<img width="332" height="234" alt="image" src="https://github.com/user-attachments/assets/925f42c8-8719-44f8-8032-eac4da00bc0b" />

The hex values: 

<img width="980" height="252" alt="image" src="https://github.com/user-attachments/assets/c9eb84b7-e8b0-4f53-87c6-2d24c5b8cdf5" />

```cpp
unsigned char smile[] = {
0x00, 0x00, 0x40, 0x20, 0x10, 0x08, 0x04, 0xfa, 0x04, 0x08, 0x10, 0x20, 0x40, 0x00, 0x00, 0x00
};
```


---

##  Step 3: Full Template Example (Smiley Face)

<img width="482" height="441" alt="image" src="https://github.com/user-attachments/assets/f4120c0d-04a4-4f1b-99d8-a61cd79e144c" />


```cpp
unsigned char smile[] = {
  0x00, 0x00, 0x1c, 0x02, 0x02, 0x02, 0x5c, 0x40,
  0x40, 0x5c, 0x02, 0x02, 0x02, 0x1c, 0x00, 0x00
};

unsigned char clear[] = {
  0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
  0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00
};

#define SCL_Pin  A5  // Clock pin
#define SDA_Pin  A4  // Data pin

void setup(){
  pinMode(SCL_Pin, OUTPUT);
  pinMode(SDA_Pin, OUTPUT);
  matrix_display(clear); // Clear matrix on startup
}

void loop(){
  matrix_display(smile);  // Show smile face
  delay(2000);            // Wait for 2 seconds
  matrix_display(clear);  // Clear the screen
  delay(2000);
}

void matrix_display(unsigned char matrix_value[]) {
  IIC_start();            // Start transmission
  IIC_send(0xc0);         // Set address to start from 0
  for(int i = 0; i < 16; i++) {
    IIC_send(matrix_value[i]);  // Send each byte
  }
  IIC_end();              // End transmission
  IIC_start();
  IIC_send(0x8A);         // Set brightness
  IIC_end();
}

void IIC_start() {
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
}

void IIC_send(unsigned char send_data) {
  for(char i = 0; i < 8; i++) {
    digitalWrite(SCL_Pin,LOW);
    delayMicroseconds(3);
    digitalWrite(SDA_Pin, send_data & 0x01);
    delayMicroseconds(3);
    digitalWrite(SCL_Pin,HIGH);
    delayMicroseconds(3);
    send_data >>= 1;
  }
}

void IIC_end() {
  digitalWrite(SCL_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,LOW);
  delayMicroseconds(3);
  digitalWrite(SCL_Pin,HIGH);
  delayMicroseconds(3);
  digitalWrite(SDA_Pin,HIGH);
  delayMicroseconds(3);
}
```

 Upload this and your LED Matrix should show a smile!

---

##  Extension Challenge — Animated LED Patterns

Let’s take it to the next level! What if your robot could *talk* with animations?

###  Goal:

Display multiple patterns **in sequence**, such as:

> 🟢 Start → 🚗 Move Forward → ⛔ Stop

Here is the continued and refined section right after your Extension Challenge — perfect for the end of Lesson 4 on your GitHub Page. This teaches students how to build their own LED animations step-by-step, and prepares them for advanced experimentation.

You can copy this markdown and paste it directly into your `.md` lesson file on GitHub:

---

##  Final Challenge — Custom LED Matrix Animation 🎯

You've learned how to show a smiley or an arrow, but now it’s your turn to be the **creator**!

### 🔍 Your Mission:

Build a 3-frame animation using your own designs.

####  Step-by-Step:

1. Go to [dotmatrixtool.com](http://dotmatrixtool.com/#)
2. Set:

   * **Width = 16**
   * **Height = 8**
   * **Endian = Big Endian**
3. Draw your first symbol (e.g. a waving hand 👋, letter A, car, etc.)
4. Click **Generate**
5. Copy the 16 HEX values and store them like this:

```cpp
unsigned char frame1[] = {
  0x__, 0x__, ..., 0x__  // Replace with your generated values
};
```

🔁 Repeat this for `frame2[]` and `frame3[]`.

---

###  Example Layout:

```cpp
unsigned char frame1[] = { /* First pattern HEX */ };
unsigned char frame2[] = { /* Second pattern HEX */ };
unsigned char frame3[] = { /* Third pattern HEX */ };
unsigned char clear[]  = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
                          0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00};
```

###  Animation Loop:

```cpp
void loop() {
  matrix_display(frame1);
  delay(1000);           // Show first frame for 1 second

  matrix_display(frame2);
  delay(1000);           // Show second frame

  matrix_display(frame3);
  delay(1000);           // Show third frame

  matrix_display(clear); // Clear screen between loops
  delay(500);
}
```

---

###  Stretch Challenge Ideas:

| Challenge                 | What to Try                                    |
| ------------------------- | ---------------------------------------------- |
|  Emoji Expressions      | Make a happy → neutral → sad animation         |
|  Robot Signals          | Add arrows or signals when moving              |
|  Use `for` with frames  | Store frames in an array and loop through them |
|  Use short delays       | Try `delay(200)` for fast flashing animations  |
|  Build a sequence story | Like “HELLO” one letter at a time              |

---

###  Advanced Looping Example:

```cpp
unsigned char* animation[] = {frame1, frame2, frame3};

void loop() {
  for (int i = 0; i < 3; i++) {
    matrix_display(animation[i]);
    delay(800);
  }
  matrix_display(clear);
  delay(400);
}
```

---

###  Reflect & Share

* What symbols did you create?
* How did delay affect your animation speed?
* Can you build a repeating signal, like blink–blink–pause?
* Post your best animation as a `.ino` file in our GitHub submissions folder!

---

 Well done! You now have full creative power over the LED Matrix.
Next, we'll combine this display with robot **movement** and **sensors**.

👉 [Next: Lesson 5 — Obstacle Avoidance Integration](https://github.com/ArtMil86/ECE-IEEE-Summer-Workshop/blob/main/Lesson%205%20-%20Smart%20Obstacle%20-%20Avoiding%20Robot.md)

---


