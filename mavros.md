# MAVROS and PX4 Flight Stack

## Frame Convention

The architecture lineage of MAVROS and PX4 led to the adoption of differing frame convention. MAVROS, a MAVLink-ROS interface, adopts the land vehicle ENU frame convention commonly and PX4 adopts the aeronautical NED frame convention. The detailed illustration of the frame convention between ROS and PX4 can be found in [PX4 Developer Wiki](https://dev.px4.io/en/ros/external_position_estimation.html#asserting-on-reference-frames).

### MAVROS Frame

Position estimates and setpoints are sent using ROS `PoseStamped`messages. The message contain a header, position and orientation information. \(?\) Weirdly, the pose follows the earth-fixed ENU reference frame while the orientation follows the body-fixed NED.

### RVIZ/Cartographer Frame

The /tf and /odom estimates are earth-fixed ENU, that is initalised based on the absolution alignment with the initial body-fixed ENU frame of the rplidar. 

### Navigation/ Base Local Planner Frame

The /cmd\_vel msg is published in body-fixed \(base\_link\) ENU, and needs to be rotated to earth-fixed ENU before being published to the /mavros/setpoint\_velocity/cmd\_vel topic.

