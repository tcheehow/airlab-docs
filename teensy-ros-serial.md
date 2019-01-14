---
description: >-
  This page covers the instructions to set-up an arduino/teensy to function as a
  ROS node over serial communication.
---

# Teensy ROS-Serial

{% hint style="info" %}
References:

1. [http://wiki.ros.org/rosserial\_arduino/Tutorials](http://wiki.ros.org/rosserial_arduino/Tutorials)
{% endhint %}

## Build ROS-Lib Library for Arduino

TODO:

## Forward ROS-Serial MSG to ROS

```text
roscore
rosrun rosserial_python serial_node.py /dev/ttyACM0

# to see the msgs from teensy, run
rostopic echo chatter
```







