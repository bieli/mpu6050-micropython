# mpu6050-micropython

Library for IMU - accelerometer & gyroscope - integrated circuit with code "MPU6050" for MicroPython stack

Driver for MPU6050 IMU IC device.

It's usefull device in many IoT and IIoT cases.


## How to install from mip (MicroPython's easy packages manager)

```bash
>>> import mip
>>> mip.install("github:bieli/mpu6050-micropython")
Installing github:bieli/mpu6050-micropython/package.json to /lib
Copying: /lib/mpu6050/__init__.py
Copying: /lib/mpu6050/mpu6050.py
Done
>>> ## you can check on device file system, if exists
>>> import os
>>> os.listdir("/lib/mpu6050")
['__init__.py', 'mpu6050.py']
>>> ## you can use MPU6050 library on device
>>> from mpu6050 import mpu6050
>>> imu = mpu6050.MPU6050(1, False)
```

## How to install from mpremote

```bash
mpremote mip install github:bieli/mpu6050-micropython
```

## Example usage for MPU6050 IMU library in a IoT device (with MicroPython)

```bash
import machine

from mpu6050 import MPU6050

imu = MPU6050(1, False)

start = machine.micros()
cangle = 90.0

# Complementary Filter A = rt/(rt + dt) where rt is response time, dt = period
def compf(fangle, accel, gyro, looptime, A):
    fangle = A * (fangle + gyro * looptime/1000000) + (1-A) * accel
    return fangle

whilte (<condition>):
    # ...
    angle  = imu.pitch()
    rate   = imu.get_gy()
    cangle = compf(cangle, angle, rate, machine.elapsed_micros(start), 0.91)
    start = machine.micros()
    # ...

```

## Known issues

### Reason: Device is not connected or GND is not connected properly to device MPU6050

```bash
>>> from mpu6050 import mpu6050
>>> imu = mpu6050.MPU6050(1, False)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/lib/mpu6050/mpu6050.py", line 66, in __init__
  File "/lib/mpu6050/mpu6050.py", line 87, in _read
  File "/lib/mpu6050/mpu6050.py", line 37, in mem_read
OSError: [Errno 19] ENODEV

```
