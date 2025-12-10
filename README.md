This project applies the requirements of the first assignment using ROS2 and the turtlesim simulator. The project has two main nodes: one node to control the two turtles using a text interface, and another node to calculate the distance between them and monitor the borders.

First: What was done?
	1.	Creating a new workspace
A special workspace for the project was created using these commands:
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build
source install/setup.bash
	2.	Creating a new package named assignment1_rt
The package was created using Python and the needed dependencies were added:
ros2 pkg create –build-type ament_python assignment1_rt –dependencies rclpy std_msgs geometry_msgs turtlesim
Then a folder for the nodes was created inside the package, and these files were added:
ui_node.py
distance_node.py
	3.	UI Node
This node gives the user a simple text interface. The user can choose the turtle (turtle1 or turtle2) and enter the linear and angular speed.
The node sends the speed for only one second and then stops the robot.
It also publishes the name of the moving robot on the topic /moving_turtle.
	4.	Distance Node
This node subscribes to the topics of the positions of turtle1 and turtle2.
It calculates the distance between the two turtles and publishes it on /turtles_distance.
The node stops the robot if the distance becomes smaller than a certain limit.
It also checks the map borders and stops the robot if it goes outside the allowed area (less than 1.0 or more than 10.0).

Second: How to run the project
	1.	Start the turtlesim simulator
ros2 run turtlesim turtlesim_node
	2.	Spawn the second turtle
ros2 service call /spawn turtlesim/srv/Spawn “{x: 5.0, y: 5.0, theta: 0.0, name: ‘turtle2’}”
	3.	Build the project
cd ~/ros2_ws
colcon build
source install/setup.bash
	4.	Run the distance node
ros2 run assignment1_rt distance_node
	5.	Run the UI node
ros2 run assignment1_rt ui_node

After running the nodes, the user can control the selected turtle, and the speed is sent for only one second. The other node protects both turtles from colliding or leaving the map borders.
