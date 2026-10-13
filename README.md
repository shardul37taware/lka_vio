# Lane Keeping Assist (LKA) with Visual-Inertial Odometry

A ROS 2-based lane-keeping assist project using an Ackermann-steered vehicle, camera, and IMU. The project is being developed in stages, beginning with the robot description and visualization.

## 1. Current Development Environment

- **OS:** Ubuntu 22.04
- **ROS 2:** Humble
- **Simulator planned:** Gazebo Harmonic
- **Visualization:** RViz2
- **Robot model:** `lka_car`
- **Workspace:** `~/lka_ws`

## 2. Current Workspace Structure

```text
lka_ws/
├── README.md
└── src/
    └── lka_car_description/
        ├── urdf/
        │   └── car.urdf.xacro
        ├── meshes/
        ├── config/
        └── launch/
            └── display.launch.py
```

The Xacro file defines the chassis, steering links, and four wheels. The launch file starts the robot state publisher and joint-state publisher GUI.

## 3. Install Required Packages

Run these commands if the required packages are not already installed:

```bash
sudo apt update
sudo apt install ros-humble-xacro \
    ros-humble-robot-state-publisher \
    ros-humble-joint-state-publisher-gui \
    ros-humble-rviz2 \
    liburdfdom-tools
```

## 4. Validate the Robot Description

Open a terminal and run:

```bash
source /opt/ros/humble/setup.bash

xacro ~/lka_ws/src/lka_car_description/urdf/car.urdf.xacro \
    -o /tmp/lka_car.urdf

check_urdf /tmp/lka_car.urdf
```

A successful validation should report the robot name as `lka_car` and indicate that the XML was successfully parsed.

## 5. Launch the Current Setup

Open a terminal and run:

```bash
source /opt/ros/humble/setup.bash

ros2 launch ~/lka_ws/src/lka_car_description/launch/display.launch.py
```

This starts:

- `robot_state_publisher` — publishes the robot's link transforms and robot description.
- `joint_state_publisher_gui` — provides controls for inspecting movable joints.

Keep this terminal open while using RViz.

## 6. Open RViz2

Open a second terminal and run:

```bash
source /opt/ros/humble/setup.bash

rviz2
```

Configure RViz as follows:

1. Under **Global Options**, set **Fixed Frame** to `base_link`.
2. Click **Add** and select **RobotModel**.
3. Expand the RobotModel properties.
4. Set **Description Source** to `Topic`, if that setting is available.
5. Set **Description Topic** to `/robot_description`.
6. Set **Durability Policy** to `Transient Local`, if available.
7. Ensure **Visual Enabled** is checked.

The chassis and wheels should now be visible. Use the joint-state GUI to inspect movable joints.

Save the RViz configuration if desired using **File → Save Config As**.

## 7. Verify ROS 2 Topics

To check that the robot description is being published, open another terminal:

```bash
source /opt/ros/humble/setup.bash

ros2 topic list
ros2 topic info /robot_description --verbose
```

The robot description topic should have a publisher from `robot_state_publisher`.

## 8. Troubleshooting

### RViz does not show the robot

- Confirm that `robot_state_publisher` and the joint-state GUI are running.
- Set the Fixed Frame to `base_link`.
- Check the RobotModel description topic and QoS settings.
- Inspect the RobotModel status for errors.

### Xacro parsing error

Run:

```bash
source /opt/ros/humble/setup.bash

xacro ~/lka_ws/src/lka_car_description/urdf/car.urdf.xacro \
    -o /tmp/lka_car.urdf
```

Resolve any reported XML or Xacro errors before launching.

### Check the available transforms

```bash
source /opt/ros/humble/setup.bash

ros2 topic echo /tf --once
```

## 9. Planned Development Stages

1. Improve and validate the Ackermann vehicle model.
2. Add collision geometry and Gazebo-compatible physics.
3. Spawn the vehicle in Gazebo Harmonic.
4. Add a camera and verify the image topic.
5. Create a road environment with lane markings.
6. Implement lane detection using OpenCV.
7. Develop the lane-keeping controller.
8. Integrate visual-inertial odometry and IMU data.
9. Deploy the system to Raspberry Pi hardware and integrate the physical motors and steering servo.

**Current status:** The Xacro model parses successfully, and the robot is visible in RViz. Gazebo physics, lane detection, and control are still pending. 
