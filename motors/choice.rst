===================================
Choosing the Right Motor Controller
===================================

CAN or PWM
----------
In FRC, there are two ways to control a motor. Either with **P**\ usle **W**\ idth **M**\ odulation, or **C**\ ontroller **A**\ rea **N**\ etwork.

PWM
^^^
PWM works by directly controlling motor output. With PWM, you can't attach sensors directly to the motor controller. Motor controllers that work with PWM are Talons, Victors, VictorSPs, and Talon SRXs (with modification). PWM is useful for controlling simple subsystems that don't require feedback such as intake mechanisms that just spin at 1 speed, drivetrains that don't contain encoders, or a shooter feeder belt that only spins one way.

CAN
^^^
CAN is useful when you want motors to be connected to sensors such as limit switches or encoders. The downside of CAN is that it is harder to set up, can fail more easily, and the Talon SRX motor controllers are more expensive. However, the ability to accurately control actuators with precise movement is generally worth the trade off. Use CAN if you want to make an arm move to a certain position that isn't limited by hardware, drive a certain distance not based on time, or spin a shooter at a certain RPM.
