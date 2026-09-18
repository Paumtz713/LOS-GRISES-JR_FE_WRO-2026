<div align="center">

# LOS GRISES JR

## World Robot Olympiad 2026
### Future Engineers

[INSERT MAIN ROBOT PHOTO]

Mexico

</div>

---

# Our Robot

This repository documents the autonomous vehicle developed by Los Grises Jr for the WRO 2026 Future Engineers category.

Our robot is based on a LEGO Mindstorms EV3 running ev3dev. We use two LEGO EV3 Medium Motors, one for movement and one for steering. The mechanical system includes LEGO gears, a LEGO differential and Ackermann steering.

For navigation, the robot uses five ultrasonic sensors, a HuskyLens camera and an AbsoluteIMU. An Arduino Nano works with the ultrasonic sensor system, while the EV3 is responsible for the main navigation and movement decisions.

We did not build the final robot only from calculations. Most of the values used now came from testing it on the track, seeing what happened and adjusting it again.

We have performed around 80 tests during development. Our normal three-lap time is close to 1:50, and our best recorded time is approximately 1:30.

---

# Robot Photos

The final robot is documented from different sides so its mechanical construction and sensor positions can be seen.

## Front View

[INSERT FRONT PHOTO]

## Rear View

[INSERT REAR PHOTO]

## Left Side

[INSERT LEFT-SIDE PHOTO]

## Right Side

[INSERT RIGHT-SIDE PHOTO]

## Top View

[INSERT TOP PHOTO]

## Bottom View

[INSERT BOTTOM PHOTO]

More robot photographs are available in:

[`Photos/`](Photos/)

---

# Challenge Videos

## Open Challenge

https://youtu.be/L_cxqAxT0rg

## Obstacle Challenge

https://youtu.be/ZGV2QgN1YLQ

---

# Robot Architecture

The EV3 is the main controller of the vehicle. It receives information from the different sensors and uses it to control the drive and steering motors.

The Arduino Nano is used with the ultrasonic sensor system. During development, communication between the Nano and EV3 was first tested using I2C. Later, the Nano-EV3 communication was moved to USB serial because this worked better for our final system.

The HuskyLens is responsible for detecting the colored pillars during the Obstacle Challenge, while the AbsoluteIMU is used to measure the rotation of the robot and count corners.

| System | Component |
|---|---|
| Main controller | LEGO Mindstorms EV3 |
| Operating system | ev3dev |
| Main language | Python |
| Ultrasonic controller | Arduino Nano |
| Drive motor | LEGO EV3 Medium Motor |
| Steering motor | LEGO EV3 Medium Motor |
| Transmission | LEGO gears and LEGO differential |
| Steering | Ackermann |
| Distance sensing | 5 ultrasonic sensors |
| Vision | HuskyLens |
| Rotation / heading | AbsoluteIMU |
| Electronics | Custom PCB |
| Current Nano-EV3 communication | USB serial |

The idea is that each sensor has a specific job, but the EV3 makes the final movement decisions.

---

# Mechanical Design

## Drive System

The robot uses a LEGO EV3 Medium Motor for movement. The transmission uses LEGO gears and a LEGO differential.

We kept the same main mechanical mechanism during development. Instead of rebuilding the transmission, most of our work was focused on finding a speed where the mechanical system and the control program worked correctly together.

We tested motor speed values from 0 to 100.

The robot normally runs with values around 40 to 50. When we tried to run it much faster, the robot started oscillating more and the steering reacted too late.

This showed us that the highest motor value was not necessarily the fastest way to complete the challenge.

At a lower speed the sensors and steering have more time to correct the trajectory. At a very high speed, the robot can travel too far before the next correction has enough effect.

Because of this, we preferred a speed range that gave us a more stable robot instead of using maximum motor power.

Our normal three-lap runs are close to 1:50. The best time we have recorded is around 1:30.

---

# Ackermann Steering

The robot uses Ackermann steering controlled by another LEGO EV3 Medium Motor.

The steering and drive systems have different functions. The drive motor moves the vehicle while the steering motor changes the direction of the front wheels.

One important part of the steering system is finding its center correctly.

During testing, we discovered that detecting the mechanical limits was not as simple as moving the steering motor until it stopped.

In one test, at 40% duty the steering moved around 28 degrees and jammed before reaching its real mechanical limit. An early calibration interpreted this as the end of the steering range.

Because of this, it measured only about 26 degrees of travel when the real range was around 111 degrees. The calculated center ended almost 50 degrees away from the real center.

We changed the calibration routine so it checks the complete power ladder before accepting a position as the mechanical stop.

We also compared forced and free steering travel:

| Test | Steering travel |
|---|---:|
| Forced travel | 153 degrees |
| Free travel | 119 degrees |
| Difference | 34 degrees |

The difference showed that the mechanism flexes when it is pushed hard against its limits.

For this reason, the normal steering range is based on free travel instead of the maximum forced measurement. A safety margin is also used so the steering does not constantly push against its physical stops.

This was important because what first looked like a programming problem was also related to the mechanical behavior of the steering.

---

# Power System

The robot uses the LEGO EV3 battery as its main power source.

The EV3 system supplies the components used by the robot. We do not currently have a complete measured current budget for every component, so we do not include estimated current values as if they were measurements.

However, battery voltage became an important part of our testing.

We noticed that when the battery voltage drops, control values that worked correctly before can produce different reactions from the robot.

This is especially important when we are tuning steering and speed. A value tested with a charged battery does not always feel exactly the same when the voltage is lower.

For this reason, charging the battery became part of our preparation before testing.

We also check the transmission before a run because another problem we found was that the transmission could become stuck.

Our pre-test routine therefore includes:

1. Check battery condition.
2. Check the transmission.
3. Check steering movement.
4. Reset the IMU.
5. Check sensor operation.
6. Start the challenge program.

---

# Ultrasonic Sensor System

The robot has five ultrasonic sensors.

They are not all pointing in the same direction because each position has a different purpose.

Two sensors are positioned at approximately 90 degrees to the sides of the robot. Their main job is to measure the walls and help prevent the robot from getting too close to them.

Two more sensors are positioned at approximately 45 degrees. We think of these as the "shoulders" of the robot. Their position allows the robot to receive information about the track before the completely lateral sensors do, which helps when approaching curves.

The fifth ultrasonic sensor points forward and works as a safety brake. It gives the robot information about something directly in front of it.

This configuration gives us three types of information:

| Position | Purpose |
|---|---|
| Left and right at 90 degrees | Wall distance and centering |
| Left and right at 45 degrees | Anticipating curves |
| Front | Safety / obstacle in front |

The ultrasonic system is connected around an Arduino Nano and our custom PCB.

During development we had a problem with one Arduino Nano and had to replace it. This was one of the hardware failures we had to diagnose instead of trying to correct the problem only by changing the program.

---

# Custom PCB

The custom PCB was designed by Paulina using EasyEDA.

We made the PCB to organize the ultrasonic sensor system around the Arduino Nano and the connections used in the project.

The board was useful because it gave the ultrasonic system a more organized connection point instead of leaving all of the sensor wiring separated.

The design includes the Arduino Nano and the connections used for the I2C system developed during the project.

The PCB design is documented in:

[`Electronics/PCB design.md`](Electronics/PCB%20design.md)

During development, the communication architecture changed. We originally worked with I2C communication between the Nano system and EV3, but the final Nano-EV3 communication was moved to USB serial.

We kept the previous work in the repository because it shows the development of the electronic system instead of showing only its final version.

---

# HuskyLens

The HuskyLens is mounted in an elevated position on the robot.

Its main job is to detect the colored pillars during the Obstacle Challenge.

The camera does not replace the normal centering system.

The main behavior of the robot is still to stay centered using the distance sensors. When the HuskyLens detects a pillar, the program temporarily changes its behavior and performs the avoidance maneuver.

For a green pillar, the robot passes on the left.

For a red pillar, the robot passes on the right.

While this happens, the ultrasonic sensors continue working.

The camera keeps track of the detected block during the maneuver. After the avoidance is completed, the robot returns to its normal centering behavior.

This means the obstacle behavior is temporary:

```text
Normal centering
      |
Pillar detected
      |
Identify red / green
      |
Avoid pillar
      |
Continue reading ultrasonic sensors
      |
Finish avoidance
      |
Return to normal centering
