V0 default ackerman bot 

Terminal 1:
source /opt/ros/humble/setup.bash
source ~/gz_ros2_control_ws/install/setup.bash
ros2 launch gz_ros2_control_demos ackermann_drive_example.launch.py

Terminal 2:
source /opt/ros/humble/setup.bash
source ~/gz_ros2_control_ws/install/setup.bash
ros2 run gz_ros2_control_demos example_ackermann_drive


