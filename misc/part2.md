# Part 2


Learning Objectives

By the end of this session, students will be able to:

- Identify and use ROS topics for basic drone control

- Use direct topic publishing to take off, land, and move a drone

- Implement keyboard teleoperation for real-time control

- Understand and switch between velocity and position control modes

- Develop a basic ROS service to command predefined flight trajectories

Section 1: Direct Topic Publishing for Drone Control
Topics:

- /cmd_vel for velocity commands (geometry_msgs/Twist)

- /drone/takeoff and /drone/land for flight control (std_msgs/Empty)

Exercise 1:

Check available topics:
```
rostopic list
```
Take off:
```
rostopic pub /drone/takeoff std_msgs/Empty "{}"
```
Move forward in a circle:
```
rostopic pub /cmd_vel geometry_msgs/Twist '{linear: {x: 0.5}, angular: {z: 0.5}}'
```
Stop the drone:
```
rostopic pub /cmd_vel geometry_msgs/Twist '{}'
```
Land:
```
rostopic pub /drone/land std_msgs/Empty "{}"
```
Section 2: Keyboard Teleoperation
Installation & Setup:

Clone and build the teleop package:
```
cd ~/catkin_ws/src
git clone https://github.com/ros-teleop/teleop_twist_keyboard.git
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```
Run the teleoperation node:
```
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```
Extension (Class Activity):

Modify the script to bind keys 1 and 2 for takeoff and landing by:

- Importing Empty from std_msgs.msg

- Adding publishers: pub_takeoff and pub_land

- Binding new keys in the while loop for triggering takeoff/land

Section 3: Switching to Position Control Mode
Concepts:

- Velocity Mode: Default mode, commands in terms of velocity

- Position Mode: Allows specifying spatial goals (x, y, z)

Exercise 2:

Enable position mode:
```
rostopic pub /drone/posctrl std_msgs/Bool "data: true"
```
Send position goals via /cmd_vel:
```
rostopic pub /cmd_vel geometry_msgs/Twist '{linear: {x: 1.0, y: 1.0, z: 1.0}}'
```
Return to velocity mode:
```
rostopic pub /drone/vel_mode std_msgs/Bool "data: true"
```
Section 4: Predefined Trajectories via ROS Services
Task:

Implement a ROS service that executes one of three trajectories: circle, square, or triangle.
Steps:

- Create package: drone_trajectories

-Define service message in srv/DroneTrajectory.srv:
```
string label
---
bool navigation_successfull
string message
```
Write Service Server:

- Handles takeoff, movement commands, and landing

- Executes trajectory logic based on the label string

Example logic (in Python):

- For "square": sequence of forward moves and turns

- For "circle": combine linear and angular velocities

- For "triangle": combine timed straight segments and turns

Wrap-Up & Discussion

- Differences between direct command publishing and service-based control

- Benefits of switching control modes

- Q&A: Exploring extensions (e.g., obstacle avoidance, SLAM integration)