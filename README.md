# Thrust-Vectoring Drone

I built a thrust-vectoring drone that tilts its motors with small servos so the frame can stay level while moving.  
This is an individual passion project where I learned wiring, PID tuning, and making 3D parts fit together.

> Base firmware: started from **Premium-Resources/583e1** and adapted for my hardware and control logic.  
> https://gitcode.com/Premium-Resources/583e1.git

## What this does
- Reads IMU (gyro/accel) for pitch/roll.
- Moves vectoring servos so each motor tilts slightly to redirect thrust.
- Tries to keep the main frame level under throttle changes.
- Logs data so I can see what went wrong (or right).

## Parts I used
- **Motors/ESCs:** 4 × BLDC + 20A ESCs  
- **Servos:** 4 × high-torque servos (vectoring mounts)  
- **Controller:** STM32F407  
- **Sensor:** MPU6050 (IMU)  
- **Battery:** 6S LiPo  
- **Frame:** 3D-printed PLA parts + carbon rods/plates  
- **Other:** bearings, M3 screws, zip ties, nano tape

## How it works (simple)
1. IMU gives pitch/roll.
2. Target is flat (0°).
3. PID computes how much each vector mount should tilt.
4. Four PWM outputs drive the servos to those angles.
5. ESCs get throttle to spin the props.

---

## Code origin & my changes

The project code is based on a template. I added a **thrust-vectoring control block** to map attitude errors (and a remote “tilt” input) into servo outputs. This lives in:


### My control block (added)
```c
float Tilt_angle, P_remote = 0.5;  // P=0.5

if (switchs.of_flow_on && CH_N[CH7] > 450 && (!switchs.gps_on)) // optical-flow mode
{
    Tilt_angle   = CH_N[CH8] / 25.0f;   // remote tilt input
    Angle_Set_rol = (att_2l_ct.exp_rol / 0.09f) * P_remote;
    Angle_Set_pit = (Tilt_angle / 0.09f) + (att_2l_ct.exp_pit / 0.09f) * P_remote;

    Servo_OUT(Angle_Set_rol, Angle_Set_pit);

    att_2l_ct.exp_rol = 0;
    att_2l_ct.exp_pit = -Tilt_angle;
}
else
{
    TIM_SetCompare1(TIM4, Servo_pos_Init[0]);
    TIM_SetCompare2(TIM4, Servo_pos_Init[1]);
    TIM_SetCompare3(TIM4, Servo_pos_Init[2]);
    TIM_SetCompare4(TIM4, Servo_pos_Init[3]);
}



