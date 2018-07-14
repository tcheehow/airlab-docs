# Installing AIRLAB-specific Libraries

`mkdir ~/catkin_ws/src`

Clone the necessary repository in `~/catkin_ws/src`

```
git clone -b <branch-name> https://github.com/tcheehow/air.git
git clone -b edison https://github.com/tcheehow/mavros.git
git clone https://github.com/ros/geometry.git
git clone https://github.com/Terabee/teraranger_array.git
git clone https://github.com/ros/dynamic_reconfigure.git
git clone https://github.com/wjwwood/serial
```

When completed, run `catkin_make ` to build the package.

add `~/catkin_ws/devel/setup.bash ` into `~/.profile`

