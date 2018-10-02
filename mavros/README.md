# MAVROS

## Frame Convention

The architecture lineage of MAVROS and PX4 led to the adoption of differing frame convention. MAVROS, a MAVLink-ROS interface, adopts the land vehicle ENU frame convention commonly and PX4 adopts the aeronautical NED frame convention. The detailed illustration of the frame convention between ROS and PX4 can be found in [PX4 Developer Wiki](https://dev.px4.io/en/ros/external_position_estimation.html#asserting-on-reference-frames).

### MAVROS Frame \(Kinetic Only\)

Position estimates and setpoints are sent using ROS `PoseStamped`messages. The message contain a header, position and orientation information. Significantly, the position msg follows the earth-fixed ENU reference frame while the orientation follows the body-fixed ENU frame \(i.e. baselink frame, or forward, right up\).

The following two-step conversion is important for transforming between body-fixed NED and body-fixed ENU messages.

Ref: [https://github.com/mavlink/mavros/blob/22d74d997f42140d21b071d6f5ef9dc54ec7a69a/mavros/src/lib/ftf\_frame\_conversions.cpp](https://github.com/mavlink/mavros/blob/22d74d997f42140d21b071d6f5ef9dc54ec7a69a/mavros/src/lib/ftf_frame_conversions.cpp)

```text
/**
 * @brief Static quaternion needed for rotating between ENU and NED frames
 * NED to ENU: +PI/2 rotation about Z (Down) followed by a +PI rotation around X (old North/new East)
 * ENU to NED: +PI/2 rotation about Z (Up) followed by a +PI rotation about X (old East/new North)
 */
static const auto NED_ENU_Q = quaternion_from_rpy(M_PI, 0.0, M_PI_2);

/**
 * @brief Static quaternion needed for rotating between aircraft and base_link frames
 * +PI rotation around X (Forward) axis transforms from Forward, Right, Down (aircraft)
 * Fto Forward, Left, Up (base_link) frames.
 */
static const auto AIRCRAFT_BASELINK_Q = quaternion_from_rpy(M_PI, 0.0, 0.0);
```

Hence, the rotation matrix `qrot = quaternion_multiply(NED_ENU_Q, AIRCRAFT_BASELINK_Q)` can be used to freely transform between frames.

### RVIZ/Cartographer Frame

The /tf and /odom estimates from Cartographer are earth-fixed ENU, that is initalised based on the absolute alignment with the initial body-fixed ENU frame of the rplidar.

Hence, if the r

### Navigation/ Base Local Planner Frame

The /cmd\_vel msg is published in body-fixed \(base\_link\) ENU, and needs to be rotated to earth-fixed ENU before being published to the /mavros/setpoint\_velocity/cmd\_vel topic.

