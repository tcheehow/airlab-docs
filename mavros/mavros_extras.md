---
description: installing a lean version of mavros_extras for edison
---

# MAVROS\_EXTRAS

## Install MAVROS\_EXTRAS

```text
# change to an unimportant directory to clone the mavros repo, we will copy the mavros_extras folder out only
cd ~
git clone https://github.com/mavlink/mavros.git
# copy to catkin_ws
cp -r mavros/mavros_extras ~/catkin_ws/src/ 
# you can remove the rest of the downloaded files
rm -r mavros
```

In the `CMakeList.txt` and `mavros_plugins.xml`, comment out irrelevant packages, leaving only

```text
px4flow
vision_pose_estimate
vision_speed_estimate
distance_sensor
rangefinder

the following packages/ executables can also be comment out
# visulization_msgs
# urdf 
# copter_visualisation
# servo_state_publisher

```

Once done, run `catkin_make`

Remove distance\_sensor and rangefinder from the blacklist

