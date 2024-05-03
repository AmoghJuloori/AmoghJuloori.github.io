---
layout: post
title: Wheeled Biped
description: >
  This project is focused on building a real-time system for a biped on wheels. [GitHub Repo](https://github.com/AmoghJuloori/WheeledBiped)
image: /assets/img/blog/whb_fp/28.png
sitemap: false
---

The aim of this project is to build a biped on two wheels with self balance and jumping modes where it should be able to switch between both modes according to the necessity. 

[Check out the GitHub repository](https://github.com/AmoghJuloori/WheeledBiped)


**Team Members :** 
-------------------

Juloori Amogh - 2020AAPS0422H

L Shiva Rudra - 2020A8PS1246H

Tanay Ranjan - 2020A3PS0483H


**Outline**
------------

Outline to achieve the objective:

*   To establish a mathematical model for both modes based on the system kinematics and dynamics and make controller design for both cases and figure out a way to integrate them using sensors such as accelerometer. This is to be done using MATLAB/Simulink which can help in estimating specifications of motors and sensors.
    
*   To estimate the essential values such as robot dimensions, mechanism design, CAD tool Onshape is to be used. 
    
*   Importing the CAD model from Onshape into Simulink allows for establishing the control in the model using MATLAB/Simscape. This also allows for simulating before real-time testing.
    
*   Building the physical robot and performing real-time testing.
    

**Literature Review**
----------------------

A parallel four-bar mechanism is being used as a new type of wheel-legged robot, and two controllers are designed to ensure stable motion - the linear quadratic regulator (LQR) and fuzzy proportion differentiation (PD) jumping controller. The robot is equipped with the ability to jump over obstacles and navigate rough terrain. The height of the jump trajectory adjusts based on changes in the link angle of the parallel four-bar linkage mechanism, enhancing the robot's ability to tackle vertical obstacles. 

The two modes of operation of the system: 

*   **Self-balance:** using LQR controller.
    
    *   To linearize the nonlinear state-space model about the equilibrium point(taking angular conditions as zero). Straight line motion(no steering can be assumed in the initial stage).
        
*   **Jumping:** using a Fuzzy controller.
    
    *   To use Fuzzy conditions to switch between Stance and Flight stages and get the controller output accordingly. 
        

The kinematics of the robot are given as follows:
![](/assets/img/blog/whb_fp/1.png)

**Self-Balance Mode:**

The robot's balance and speed are regulated using the Linear Quadratic Regulator (LQR) technique. The equation for the state of the linear time-invariant system is:

x\_dot(t) = Ax(t) + Bu(t).  The LQR controller works only on linear systems. But, our system is nonlinear with dynamics given as follows:
![](/assets/img/blog/whb_fp/2.png)

On converting it into matrix form by taking 6 states:
![](/assets/img/blog/whb_fp/3.png)
![](/assets/img/blog/whb_fp/4.png)
![](/assets/img/blog/whb_fp/5.png)

where:
![](/assets/img/blog/whb_fp/6.png)

Here, 

*   xb is the forward displacement of the point of contact with the ground
    
*   alpha is the steering angle
    
*   phi is the tilt angle of the robot w.r.t. the vertical axis.
    
*   M is mass of the robot
    
*   L is the length of the robot
    
*   mb is the mass of the wheel
    
*   Ib is the moment of inertia of the robot abt y-axis
    
*   Iw is the moment of inertia of the wheel
    
*   r is the radius of the wheel
    
*   g is the acceleration due to gravity
    
*   D is the wheel to wheel distance 
    
*   It is the moment of inertia of the robot about vertical direction
    

**Jumping mode:**

The robot's jump control utilizes fuzzy Proportional Differentiation (PD) control. Model is to be built based on the dynamic equation of the jump stage:
![](/assets/img/blog/whb_fp/7.png)

**Mathematical Model for Jumping Mode**

The following equations are used to find out the torque of the hip joint as a function of its angle:
![](/assets/img/blog/whb_fp/8.png)

Here :

*   K0 is the kinetic energy of one wheel
    
*   Ki is the kinetic energy of ‘ith’ link of a single 4-bar linkage
    
*   K is the total kinetic energy of on each side(left and right) of the bot
    
*   V is the total potential energy
    
*   L is the Lagrangian 
    
![](/assets/img/blog/whb_fp/9.png)

Where 𝞃 is the torque of the hip joint.

**Methodology**
---------------

**Self-balance Mode**

**Calculating the length of the robot(L):**

From the kinematics schematic of the robot, the following transformation matrices can be found out:
![](/assets/img/blog/whb_fp/10.png)

As coordinate {3} represents the motor’s position and {0} represents the wheel position, so, the length of the robot can be found by using the \[0T3\] matrix.  (Assuming that center of mass of the robot lies near motor)

The fourth column in \[0T3\] represents the coordinates of {3} in {0}. So, using distance formula, 

L=sqrt(sum of squares of first and second value in last column of \[0T3\]).

L2 =l2cos1+l3cos1+22+l2sin1+l3sin1+22

L2 =l22+l32+2l2l3cos2

Maximum value of L will occur at 2 = 0

Lmax=l2+l3=0.28 m

**Calculating the mass of the robot (M):**

The Mass of body is calculated as follows -
![](/assets/img/blog/whb_fp/11.png)

Upper Half of body consists of -

1.  2 [T-Motors](https://robu.in/product/t-motor-antigravity-mn6007ii-kv320/?gclid=Cj0KCQjwlPWgBhDHARIsAH2xdNdmBlSWDCa-FnMQSidATNqJBy6Z23CCBXG3QemWm6l0D_Nc6ZiDKDUaAuH2EALw_wcB) - 180 g 2=360 g
    
2.  [Lipo 6s](https://robu.in/product/orange-5200mah-6s-35c-22-2-v-lithium-polymer-battery-pack-li-po-copy/?gclid=Cj0KCQjwlPWgBhDHARIsAH2xdNcf6yW0m9zF_Tm3WfSjeQWZFHvMOm5AfFRI1WavzTtUaNUPB95x5wkaAumPEALw_wcB) (As T Motors requires 6s Lipo) - 720 g
    
3.  Microcontroller ([Arduino](https://robu.in/product/original-arduino-uno-rev3/?gclid=Cj0KCQjwlPWgBhDHARIsAH2xdNe3PmhveOhLYXz9MI9Dmba6yrB1Ro9K0oK3tkZMzT9ap56_TndC0UwaAvCsEALw_wcB)) - 25 g
    
4.  Microprocessor ([Raspberry Pi](https://robu.in/product/raspberry-pi-compute-module-4-i-o-board/?gclid=Cj0KCQjwlPWgBhDHARIsAH2xdNd3CmR0XshrbriYprA9ruVwi_J0f_g_J2YbqRjjvWpwLVQolWQJ3gQaAoxHEALw_wcB)) - 10 g
    
5.  [Buck Converter](https://robu.in/product/lm2596s-dc-dc-buck-converter-power-supply/?gclid=Cj0KCQjwlPWgBhDHARIsAH2xdNenat-J7fQgTYXHeMhHkrbcOKAkB08sPmwztSxBVZBfbAtEUZwITHYaAvAFEALw_wcB) - 11 g
    
6.  2 [40 A ESC](https://robu.in/product/t-motor-air-40a-esc/?gclid=Cj0KCQjwlPWgBhDHARIsAH2xdNedkovth03AlSLs7dCtZyilwCkITRo02CGn3oMcKFo05d9vw0c_3NQaAhPdEALw_wcB) -30 g 2=60 g
    

Lower Half of body consists of 2 Motors + Wheel (Negligible) = 360 g

Total Mass  M = 1546 g

Lower Body Mass  mb = 360 g

For safety factor, we will consider 2 times of the calculated mass

Therefore M = 3 kg, mb = 0.72 kg

So the values are taken as follows:

*   r=0.03m
    
*   M=3 kg
    
*   mb=0.72 kg
    
*   Ib=M\*L\*L
    
*   Iw=4.5125\*0.0001
    
*   It=0.0225
    
*   b=0.75
    
*   g=9.82ms-2
    
*   D=0.3m
    

The following model is made for the self-balancing mode using LQR controller:
![](/assets/img/blog/whb_fp/12.png)

*   Disturbance(using Pulse Generator) is added to the Plant input.
    
*   The six states are observed in the o/p scope.
    

**Jumping Mode**

**Trajectory Planning:**

![](/assets/img/blog/whb_fp/13.png)

*   We have taken Hmax as 0.5 m. This is used to estimate the vertical velocity of the robot when it is about to take off.
    
*   S is taken as 0.2 m. This is used to estimate the horizontal velocity of the robot throughout the projectile.
    ![](/assets/img/blog/whb_fp/14.png)
    ![](/assets/img/blog/whb_fp/15.png)

*   We used this as the trajectory of the vertical component of the COM(hip joint) during the takeoff.
    

**Control Block Diagram for Jumping Mode:**

![](/assets/img/blog/whb_fp/16.png)

This is the control block diagram which we used to control the hip joint angle and its derivative during the flight phase.

*   qd, qd\_dot and qd\_ddot are the reference inputs. Where q represents the hip joint angle.
    
*   Fuzzy rules are made to tune the delta\_Kp and delta\_Kd values which get added to constant Kp0 and Kd0 values to get PD gains. 
    
*   Torque of the system while performing this control system during the takeoff is given by:
    

**Steps followed to establish this system:**

*   Obtained the Lagrangian from the KE and PE equations. This involves finding the transformation matrices of the COM of all the links wrt the world frame {W}.
    
*   Next, we derived the equation of torque in terms of theta3, theta3\_dot and theta3\_ddot where theta3 is the hip joint angle.
    
*   We segregate the terms in the torque equation to find the following matrices:
    
    *   Inertia matrix: M(q)
        
    *   Coriolis force matric: V(q,q\_dot)
        
    *   Gravitational force matric: G(q)
        
*   We used this information to build the following Simulink models:
![](/assets/img/blog/whb_fp/17.png)
    

This estimates the torque as a function of time taking takeoff time as 0.45 seconds.

**Control Design for q, q\_dot control during takeoff:**

![](/assets/img/blog/whb_fp/18.png)

We built this system to control the q, q\_dot while takeoff where q is the hip joint angle. But due to certain issues like the number of rules and membership functions of the fuzzy controller and due to the lack of proper reference trajectory equations, we could not get proper results for the torque and the control variables: 

**Results**
------------

For testing, we set initial angles of robot as 15°, 30°, and 45°, and then we observed how it recovers by accelerating its bottom wheel

**15° -**
![](/assets/img/blog/whb_fp/19.png)

**30° -**
![](/assets/img/blog/whb_fp/20.png)

**45° -**
![](/assets/img/blog/whb_fp/21.png)

Using these results and aiming that the robot should be able to recover from 45° with max leg length, the specifications of motors for forward translational motion are found as:

*   Max RPM = 254.77 RPM
    
*   Max Torque = 9 N-m
    

**Self balance Mode**

Initial Angle=45 degrees, Hip angle=120 degrees
![](/assets/img/blog/whb_fp/22.png)
![](/assets/img/blog/whb_fp/23.png)

Initial Angle=60 degrees, Hip angle=120 degrees
![](/assets/img/blog/whb_fp/24.png)
![](/assets/img/blog/whb_fp/25.png)

The simulation results can be used to obtain the motor specifications.

**Jumping Mode**

![](/assets/img/blog/whb_fp/26.png)

This is the variation of the torque. We can clearly see that the torque increases in the negative direction while retraction and then increases in the positive while jumping. (Note that the jumping phase involves retraction followed by jumping).
![](/assets/img/blog/whb_fp/27.png)

These are the plots for the q and q\_dot. The parabolic projectile reference is taken into consideration.

**CAD**
--------

The CAD model for the robot is as follows:
![](/assets/img/blog/whb_fp/28.png)

![](/assets/img/blog/whb_fp/29.png)

![](/assets/img/blog/whb_fp/30.png)

**Gearbox**
------------

![](/assets/img/blog/whb_fp/31.png)

**Conclusion**
---------------

*   We learned the mathematical modeling of the wheeled biped and its implementation using LQR and Fuzzy PD controllers.
    
*   We would like to work on the simulation of the landing phase and integrate both the Self balancing and Jumping modes in a more robust manner.
    
*   We also plan to obtain proper values of moment of inertias, and masses of various links to obtain the desired control and minimize the error of trajectory while considering the physical constraints.
    
*   The fabrication and testing of the physical system of the robot would be our target for the future.
    

**References**    
---------------      

[Design and dynamic analysis of jumping wheel-legged robot in complex terrain environment](https://www.frontiersin.org/articles/10.3389/fnbot.2022.1066714/full)

---
<br>
*This post is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/?ref=chooser-v1) by the author.*
