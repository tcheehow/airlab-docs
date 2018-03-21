# Cartographer ROS with Gazebo

## Setup

There are various configurations files from the default demo that needs to be modified for our purpose.

The .launch file, the .lua files.

## Transform

For cartographer to work with imu information, tf information must be sent. The imu should publish on the `/imu` topic with `/imu_link` frameid. The laser scan should publish on the `/scan` topic with `/rplidar_link`fram1eid. If the topic name differs, it can be easily remapped in the .launch file.

### Tuning Cartographer 

Due to the sparsity of the feature, there is a need to tune the cartographer parameters for real-time localisation and quality SLAM. 

It includes translation\_weight

rotation\_weight

resolution

refer to the full list at Cartographer ROS Wiki

### Tuning PX4 Parameters

Due to the latency and drift from the pose estimates, LPE often develop faults. The fault can be overcomed by setting the std of VIS\_XY to the maximum of 1, and tuning the propogation parameters to track the vision measurements more accurately



