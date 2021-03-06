# ROS_tutorial

## First Example
### Brief Description
![image](https://user-images.githubusercontent.com/62916482/148050456-924a0ab0-9360-4029-925f-c9c4e5a7deba.png)
![image](https://user-images.githubusercontent.com/62916482/148050467-6a40bf14-ede7-4356-9f53-fccba759aed9.png)


We give radius, linear velocity and direction of turtle's trajectory(uniform circular) to ["/rvd_publisher"](https://github.com/windust7/ROS_tutorial/blob/main/uniform_circle_turtle/uniform_circle_turtle/rvd_publisher.py) node.

Then ["/rvd_publisher"](https://github.com/windust7/ROS_tutorial/blob/main/uniform_circle_turtle/uniform_circle_turtle/rvd_publisher.py) node publishs "/uniform_circular_velocity" topic msg (["rvd_msg_example/msg/RVD"](https://github.com/windust7/ROS_tutorial/blob/main/rvd_msg_example/msg/RVD.msg) type) to ["/rvd_subscriber"](https://github.com/windust7/ROS_tutorial/blob/main/uniform_circle_turtle/uniform_circle_turtle/rvd_subscriber.py) node. This node transforms "/uniform_circular_velocity" topic msg (["rvd_msg_example/msg/RVD"](https://github.com/windust7/ROS_tutorial/blob/main/rvd_msg_example/msg/RVD.msg) type) to "/turtle1/cmd_vel" topic msg ("geometry_msgs/msg/Twist" type) and publishs to "/turtlesim" node.

[Launch file](https://github.com/windust7/ROS_tutorial/blob/main/uniform_circle_turtle/launch/rvd_to_twist.launch.py) includes ["/rvd_subscriber"](https://github.com/windust7/ROS_tutorial/blob/main/uniform_circle_turtle/uniform_circle_turtle/rvd_subscriber.py) node and "/turtlesim_node" (turtlesim package). We give "/uniform_circular_velocity" topic msg (["rvd_msg_example/msg/RVD"](https://github.com/windust7/ROS_tutorial/blob/main/rvd_msg_example/msg/RVD.msg) type) to ["/rvd_subscriber"](https://github.com/windust7/ROS_tutorial/blob/main/uniform_circle_turtle/uniform_circle_turtle/rvd_subscriber.py) node.

### Requirements
Ubuntu 18.04, ROS2 dashing version

To see results, follow:
```
git clone https://github.com/windust7/ROS_tutorial
```
Move [rvd_msg_example](https://github.com/windust7/ROS_tutorial/tree/main/rvd_msg_example) and [uniform_circle_turtle](https://github.com/windust7/ROS_tutorial/tree/main/uniform_circle_turtle) folders to src folder(which is at your workspace)

```
cd ~/(your workspace)
colcon build --symlink-install --packages-select rvd_msg_example
colcon build --symlink-install --packages-select uniform_circle_turtle
. ~/(your workspace)/install/local_setup.bash
```
To run each nodes, follow below at different terminal window:
```
ros2 run turtlesim turtlesim_node
ros2 run uniform_circle_turtle rvd_publisher -r 2 -v 2 -c 1
ros2 run uniform_circle_turtle rvd_subscriber 
```
To run launch file, follow below at different terminal window:

```
ros2 launch uniform_circle_turtle rvd_to_twist.launch.py
ros2 topic pub --once /uniform_circular_velocity rvd_msg_example/msg/RVD "{ccwdirection: False, radius: 1.5, lin_vel: 5.0}"
```


## Second Example
### Brief Description
![image](https://user-images.githubusercontent.com/62916482/148576671-307a83f6-638b-43f0-8e89-3644751406a8.png)

[/agent_spawner](https://github.com/windust7/ROS_tutorial/blob/main/dwa_turtle/dwa_turtle/agent_spawner.py) node spawns a new turtle(/agent). 

An original turtle(/turtle1) will be an obstacle and [the agent](https://github.com/windust7/ROS_tutorial/blob/main/dwa_turtle/dwa_turtle/dwa_planner.py) has to move to targets in order without bumping into the obstacle. You can decide which points will be targets with [config file](https://github.com/windust7/ROS_tutorial/blob/main/dwa_turtle/param/config.yaml).

### Requirements
Ubuntu 18.04, ROS2 dashing version

To see results, follow:
```
git clone https://github.com/windust7/ROS_tutorial
```
Move [dwa_turtle](https://github.com/windust7/ROS_tutorial/tree/main/dwa_turtle) folders to src folder(which is at your workspace)
```
cd ~/(your workspace)
colcon build --symlink-install --packages-select dwa_turtle
. ~/(your workspace)/install/local_setup.bash
```
To run each nodes, follow below at different terminal window.

  * -x means initial x coordinate of /agent, -y means initial y coordinate of /agent, -t means initial theta of /agent. 

  * If you don't give x, y, t or direction of .yaml file, default values are given.

  * If you give .yaml file's directory, you have to run last line at your workspace.
```
ros2 run turtlesim turtlesim_node
ros2 run dwa_turtle agent_spawner -x 1.0 -y 1.0 -t 0.0
ros2 run dwa_turtle dwa_planner __params:=src/dwa_turtle/param/config.yaml
```
At [config.yaml](https://github.com/windust7/ROS_tutorial/blob/main/dwa_turtle/param/config.yaml), you can decide num_target, pick_target and etc.

If you give 3 to num_target, there will be 3 * 3 = 9 grids.

If you give 325 to pick_target, 325 = 0b101000101 and 1st, 3rd, 7th, and 9th grids will be targets(Consider each digit in a binary number). 

### Citation([Dynamic Window Planner](https://github.com/windust7/ROS_tutorial/blob/main/dwa_turtle/dwa_turtle/dwa_planner.py))

* [Author - Atsushi Sakai](https://github.com/AtsushiSakai)
* [Original Repository](https://github.com/AtsushiSakai/PythonRobotics/blob/master/PathPlanning/DynamicWindowApproach/dynamic_window_approach.py)

### Problem
I tried to run a [launch file](https://github.com/windust7/ROS_tutorial/blob/main/dwa_turtle/launch/dwa_turtle.launch.py) but when I tried:
```
ros2 launch dwa_turtle dwa_turtle.launch.py
```
The following error message appeared:

[ERROR] [launch]: Caught exception in launch (see debug for traceback): ``__init__()`` missing 1 required keyword-only argument: 'node_executable'

If someone find the reason or solution, please let me know(via e-mail or etc,,)

(Looks like my local environment is something wrong,, Maybe you will be able to run the launch file without that error message)
