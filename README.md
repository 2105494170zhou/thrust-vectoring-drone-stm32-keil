# Thrust-Vectoring Drone (Student Project)

I built a thrust-vectoring drone that tilts its motors with small servos so the frame can stay level while moving.  
This is a individual passion project where I learned a lot about wiring, tuning PID, and making 3D parts fit together.

> Base firmware reference: this project’s code started from **Premium-Resources/583e1** and I adapted it for my hardware and control logic.  
> Link: https://gitcode.com/Premium-Resources/583e1.git

## What this does

- Reads the IMU (gyro/accel) to know the drone’s pitch/roll.
- Moves **vectoring servos** to angle each motor slightly so the thrust points where we need it.
- Tries to keep the main frame level while I change throttle.
- Logs some data so I can see what went wrong (or right).

## Parts I used

- **Motors/ESCs:** 4 × BLDC + 20A ESCs  
- **Servos:** 4 × high torque servos for vectoring mounts  
- **Controller:** STM32F407  
- **Sensor:** MPU6050 (IMU)  
- **Battery:** 6S LiPo  
- **Frame:** mix of 3D-printed parts (PLA) + carbon rods + carbon plates  
- **Other:** bearings, M3 screws, zip ties, nano tape

## 🧩 How It Works (Simple Explanation)

1. The IMU gives the pitch and roll angles.  
2. The code compares these angles with the target (flat = 0°).  
3. The PID controller calculates how much each motor mount should tilt.  
4. Four PWM signals drive the servos to move the vectoring brackets.  
5. The ESCs receive throttle signals to spin the propellers.

