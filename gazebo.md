---
description: details critical steps and instructions for modelling a monocopter on gazebo.
---

# Monocopter Setup on Gazebo

The instruction is based on Gazebo 7.8.1, and PX4 running on LPE.

### Building a Monocopter Model

The `standard_vtol` model is used as a template for building the `monocopter` model. 

#### 1. Fake Base Link

As the rotary motion is a key dynamics for monocopter flight, an invisible `fakebase_link` is essential to eliminate the surface friction between the `base_link` and the floor. As all the other links are already mounted on the base\_link, using `fakebase_link` as an intermediate link is the simplest method. The `<transparency>` sdf command can be used to hide the `fakebase_link`.

#### 2. Mount the Base Link

Then, the `base_link` is mounted on the previously created `fakebase_link` using a revolute joint. This is the same concept as mounting the propellers on a revolute joint for seamless rotation.

#### 3. Customizing the Base Link

Most importantly, the `<mass>` and `<inertia>` tensor must be modified in `base_link` for accurate monocopter dynamics.

The visual of the `base_link` can be modified by amending the `<uri>` in `base_link_visual` to direct to the file path of the monocopter stl. Then, unnecessary link, visual, and joint can be commented/ removed, leaving on those relating to `rotor_0` and `right_elevon`.

#### 4. ROS Plugins

Unnecessary ROS plugins interface can be commented/ removed, leaving only those relating to `rotor_0` and `right_elevon`, which should be `front_right_motor_model` and `right_wing` respectively.

The aerodynamics descriptions of the airfoil, and the propeller can be modified in `right_wing` and `front_right_motor_model` which is based on Gazebo's `LiftDragPlugin` and RotorS's `Motor` plugins.

