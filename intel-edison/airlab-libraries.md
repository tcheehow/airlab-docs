# AIRLAB Libraries

`mkdir ~/catkin_ws/src`

Clone the necessary repository in `~/catkin_ws/src`

```text
git clone -b <branch-name> https://github.com/tcheehow/air.git
git clone -b edison https://github.com/tcheehow/mavros.git
git clone https://github.com/ros/geometry.git
git clone https://github.com/Terabee/teraranger_array.git
git clone https://github.com/ros/dynamic_reconfigure.git
git clone https://github.com/wjwwood/serial
```

When completed, run `catkin_make` to build the package.

```text
wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh
./install_geographiclib_datasets.sh
```

add `~/catkin_ws/devel/setup.bash` into `~/.profile`

Using QGroundControl, set the following for companion link

Companion Link, 961200

For remote editting

[http://esthermakes.tech/blog/2015/02/24/remote-text-editing-on-edison-with-atom/](http://esthermakes.tech/blog/2015/02/24/remote-text-editing-on-edison-with-atom/)

For mavros-extras, clone the entire mavros repo and shift only the mavros-extras folder into ~/catkin\_ws/src/

then, edit the CMakeList to remove msg, executable, and build dependencies.

