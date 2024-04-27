---
layout: post
title: Design and fabrication of a Stewart Platform
description: >
  This project is focuses on the design and fabrication of a Stewart platform and the implementation of certain motions using an Arduino UNO.
image: /assets/img/blog/stew_rob/14.png
sitemap: false
---

Stewart Platform is a parallel-plate manipulator which has a wide range of use-cases in fields of surgery, flight simulators, robotics etc. The idea behind this project is to understand the working of such manipulators and to work with the Kinematics of these manipulators.

**Components List**
<p style="text-align: center;">1. Arduino UNO with cable</p>
![](/assets/img/blog/stew_rob/1.png)

![](/assets/img/blog/stew_rob/2.png)

<p style="text-align: center;">2. MG995 Servo x 6</p>
![](/assets/img/blog/stew_rob/3_.png)

<p style="text-align: center;">3. Servo Horn x 6</p>
![](/assets/img/blog/stew_rob/4_.png)

<p style="text-align: center;">4. Rod End Bearing Spherical Joints - 6mm x 12</p>
![](/assets/img/blog/stew_rob/5.png)

<p style="text-align: center;">5. Metal shafts with threading - 6mm x 6</p>
![](/assets/img/blog/stew_rob/6_.png)

<p style="text-align: center;">6. LM2596S DC-DC Buck Converter x 3</p>
![](/assets/img/blog/stew_rob/7.png)

<p style="text-align: center;">7. 3D Printed Platform</p>
![](/assets/img/blog/stew_rob/8.png)

<p style="text-align: center;">8. Wooden Base - 40x30 cm</p>


<p style="text-align: center;">9. Power Source AC-DC 12V-2A supply</p>
![](/assets/img/blog/stew_rob/9.png)

<p style="text-align: center;">10. Screws and nuts- M3 with 3D printed 6mm->3mm connectors x 6</p>
![](/assets/img/blog/stew_rob/10.png)

![](/assets/img/blog/stew_rob/11_.png)

<p style="text-align: center;">11. Jumper Wires</p>
![](/assets/img/blog/stew_rob/12.png)

**Circuit Diagram**



![](/assets/img/blog/stew_rob/13.png)

**Types of Motion**

Using the Arduino IDE, we have been able to establish fourfour different types of motion:1. Vertical Motion2. Twisting Motion3. Circular Motion in a horizontal plane4. Phase Difference or Sine wave motion

**Vertical Motion**

Code:
```
// To include the Servo library
#include <Servo.h>
Servo Servo1;
Servo Servo2;
Servo Servo3;
Servo Servo4;
Servo Servo5;
Servo Servo6;

// Servo Pin numbers
int servoPin1 = 5;
int servoPin2 = 6;
int servoPin3 = 9;
int servoPin4 = 11;
int servoPin5 = 10;
int servoPin6 = 3;

void setup() {
  // Servo pins setup
  Servo1.attach(servoPin1);
  Servo2.attach(servoPin2);
  Servo3.attach(servoPin3);
  Servo4.attach(servoPin4);
  Servo5.attach(servoPin5);
  Servo6.attach(servoPin6);
}

void loop() {
   //To move the platform vertically up and down
  int i = 0;
  int j = 0;
  while(i <= 90) {
    j =180 - i;
    Servo1.write(i);
    Servo2.write(j);
    Servo3.write(i);
    Servo4.write(j);
    Servo5.write(i);
    Servo6.write(j);

    delay(25);

    i += 1;
  }
//  Servo1.write(45);
//  Servo2.write(135);
//  Servo3.write(45);
//  Servo4.write(135);
//  Servo5.write(45);
//  Servo6.write(135);
  while(i >= 0) {
    j =180 - i;
    Servo1.write(i);
    Servo2.write(j);
    Servo3.write(i);
    Servo4.write(j);
    Servo5.write(i);
    Servo6.write(j);

    delay(25);

    i -= 1;
  }

}
```

Video:
![](/assets/img/blog/stew_rob/Vertical Motion.MOV)

**Twisting Motion**

Code:
```
#include <Servo.h>

Servo Servo1;
Servo Servo2;
Servo Servo3;
Servo Servo4;
Servo Servo5;
Servo Servo6;

int servoPin1 = 5;
int servoPin2 = 6;
int servoPin3 = 9;
int servoPin4 = 11;
int servoPin5 = 10;
int servoPin6 = 3;

void setup() {
  // put your setup code here, to run once:
  Servo1.attach(servoPin1);
  Servo2.attach(servoPin2);
  Servo3.attach(servoPin3);
  Servo4.attach(servoPin4);
  Servo5.attach(servoPin5);
  Servo6.attach(servoPin6);
}

void loop() {
  // put your main code here, to run repeatedly:
  int i = 45;
  int theta = 15;
  while(i <= 45+theta) {
    Servo1.write(i);
    Servo2.write(90+i);
    Servo3.write(i);
    Servo4.write(90+i);
    Servo5.write(i);
    Servo6.write(90+i);

    delay(75);

    i++;
  }

  while(i >= 45-theta) {
    Servo1.write(i);
    Servo2.write(90+i);
    Servo3.write(i);
    Servo4.write(90+i);
    Servo5.write(i);
    Servo6.write(90+i);

    delay(75);

    i--;
  }

  while(i <= 45) {
    Servo1.write(i);
    Servo2.write(90+i);
    Servo3.write(i);
    Servo4.write(90+i);
    Servo5.write(i);
    Servo6.write(90+i);

    delay(75);

    i++;
  }
}
```
Video:
![](/assets/img/blog/stew_rob/Twisting motion.MOV)

**Circular Motion in a horizontal plane**

Code:
```
#include <Servo.h>

Servo Servo1;
Servo Servo2;
Servo Servo3;
Servo Servo4;
Servo Servo5;
Servo Servo6;

int servoPin1 = 5;
int servoPin2 = 6;
int servoPin3 = 9;
int servoPin4 = 11;
int servoPin5 = 10;
int servoPin6 = 3;

void setup() {
  // put your setup code here, to run once:
  Servo1.attach(servoPin1);
  Servo2.attach(servoPin2);
  Servo3.attach(servoPin3);
  Servo4.attach(servoPin4);
  Servo5.attach(servoPin5);
  Servo6.attach(servoPin6);
}

void loop() {
  int i = 45;
  while(i <= 90) {
    Servo1.write(90-i);
    Servo2.write(90+i);
    Servo3.write(45);
    Servo4.write(135);
    Servo5.write(i);
    Servo6.write(180-i);

    delay(75);
    i++;
  }

  int counter = 0;
  while(counter < 5) {
    i = 0;
    while(i <= 45) {
      Servo1.write(i);
      Servo2.write(180-i);
      Servo3.write(45+i);
      Servo4.write(135-i);
      Servo5.write(90-2*i);
      Servo6.write(90+2*i);

      delay(75);
      i++;
    }

    i = 0;
    while(i <= 45) {
      Servo5.write(i);
      Servo6.write(180-i);
      Servo1.write(45+i);
      Servo2.write(135-i);
      Servo3.write(90-2*i);
      Servo4.write(90+2*i);

      delay(75);
      i++;
    }

    i = 0;
    while(i <= 45) {
      Servo3.write(i);
      Servo4.write(180-i);
      Servo5.write(45+i);
      Servo6.write(135-i);
      Servo1.write(90-2*i);
      Servo2.write(90+2*i);

      delay(75);
      i++;
    }

    counter++;
  }

  i = 0;
  while(i <= 45) {
    Servo1.write(i);
    Servo2.write(180-i);
    Servo3.write(45);
    Servo4.write(135);
    Servo5.write(90-i);
    Servo6.write(90+i);

    delay(75);
    i++;
  }
}
```

Video:
![](/assets/img/blog/stew_rob/Circular Motion in a Horizontal Plane.MOV)

**Phase Difference or Sine wave motion**

Code:
```
#include <Servo.h>

Servo Servo1;
Servo Servo2;
Servo Servo3;
Servo Servo4;
Servo Servo5;
Servo Servo6;

int servoPin1 = 5;
int servoPin2 = 6;
int servoPin3 = 9;
int servoPin4 = 11;
int servoPin5 = 10;
int servoPin6 = 3;

void setup() {
  // put your setup code here, to run once:
  Servo1.attach(servoPin1);
  Servo2.attach(servoPin2);
  Servo3.attach(servoPin3);
  Servo4.attach(servoPin4);
  Servo5.attach(servoPin5);
  Servo6.attach(servoPin6);
}

void loop() {
  int i = 45;
  while(i <= 90) {
    Servo1.write(90-i);
    Servo6.write(90+i);
    Servo3.write(45);
    Servo2.write(135);
    Servo5.write(i);
    Servo4.write(180-i);

    delay(50);
    i++;
  }

  int counter = 0;
  while(counter < 5) {
    i = 0;
    while(i <= 45) {
      Servo1.write(i);
      Servo6.write(180-i);
      Servo3.write(45+i);
      Servo2.write(135-i);
      Servo5.write(90-2*i);
      Servo4.write(90+2*i);

      delay(50);
      i++;
    }

    i = 0;
    while(i <= 45) {
      Servo5.write(i);
      Servo4.write(180-i);
      Servo1.write(45+i);
      Servo6.write(135-i);
      Servo3.write(90-2*i);
      Servo2.write(90+2*i);

      delay(50);
      i++;
    }

    i = 0;
    while(i <= 45) {
      Servo3.write(i);
      Servo2.write(180-i);
      Servo5.write(45+i);
      Servo4.write(135-i);
      Servo1.write(90-2*i);
      Servo6.write(90+2*i);

      delay(50);
      i++;
    }

    counter++;
  }

  i = 0;
  while(i <= 45) {
    Servo1.write(i);
    Servo6.write(180-i);
    Servo3.write(45);
    Servo2.write(135);
    Servo5.write(90-i);
    Servo4.write(90+i);

    delay(50);
    i++;
  }
}
```

Video:
![](/assets/img/blog/stew_rob/Phase Difference Motion.mp4)

**Images of the setup**


![](/assets/img/blog/stew_rob/14.png)

![](/assets/img/blog/stew_rob/15.png)

![](/assets/img/blog/stew_rob/18.png)

![](/assets/img/blog/stew_rob/19.png)
