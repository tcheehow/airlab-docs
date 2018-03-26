# Cartographer ROS with Gazebo

## Setup

There are various configurations files from the default demo that needs to be modified for our purpose.

The .launch file, the .lua files.

## TF and Topic Remapping

For cartographer to work with imu information, tf information must be sent. The imu should publish on the `/imu` topic with `/imu_link` frameid. The laser scan should publish on the `/scan` topic with `/rplidar_link`fram1eid. If the topic name differs, it can be easily remapped in the .launch file.

### Tuning Cartographer

Due to the sparsity of the feature, there is a need to tune the cartographer parameters for real-time localisation and quality SLAM. The detailed description of each parameters can be found in [Cartographer Configuration Wiki](http://google-cartographer.readthedocs.io/en/latest/configuration.html).

Based on the Gazebo-MAVROS SITL testing, tuning the following parameters \(non-exhaustive\) improves SLAM performance,

* TRAJECTORY\_BUILDER\_2D.num\_accumulated\_range\_data = 5
* TRAJECTORY\_BUILDER\_2D.ceres\_scan\_matcher.translation\_weight = 5
* TRAJECTORY\_BUILDER\_2D.ceres\_scan\_matcher.translation\_weight = 
* POSE\_GRAPH.optimize\_every\_n\_data = 1
* POSE\_GRAPH.optimizattion\_problem.acceleration\_weight = 0.1 

and reduces latency,

* TRAJECTORY\_BUILDER\_2D.voxel\_filter\_size = 0.1
* TRAJECTORY\_BUILDER\_2D.submap\_resolution = 0.1

The full list of tunable parameters for low latency can be found on [Cartographer ROS Wiki](http://google-cartographer-ros.readthedocs.io/en/latest/tuning.html#low-latency)

### Interpreting Cartographer Output

![](/assets/gazebo-cartographer-px4-rqt.png)Cartographer subscribes to /imu and /scan for map building and trajectory generation. The trajectory is published in /trajectory\__node\_list and the robot pose at a /tf with parent\_frame_\_id: map and child\_frame\_id: base\_link. The robot pose is extracted by listening to the latest /tf.

### Tuning PX4 Parameters

Due to the latency and drift from the pose estimates, LPE often develop faults. The fault can be overcomed by setting the std of VIS\_XY to the maximum of 1, and tuning the propogation parameters to track the vision measurements more accurately

