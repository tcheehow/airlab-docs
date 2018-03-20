# Cartographer ROS with Gazebo

## Setup

There are various configurations files from the default demo that needs to be modified for our purpose. 

The .launch file, the .lua files. 

## Transform

For cartographer to work with imu information, tf information must be sent. The imu should publish on the `/imu` topic with `/imu_link` frameid. The laser scan should publish on the `/scan` topic with `/rplidar_link`frameid. If the topic name differs, it can be easily remapped in the .launch file. 

