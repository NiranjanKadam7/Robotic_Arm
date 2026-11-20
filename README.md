
# Simple_robotic_arm: ROS 2 4-DOF Robotic Arm

A 4-DOF robotic arm (rotating waist, shoulder, elbow, wrist/gripper) described in URDF/Xacro with STL mesh visuals, and visualised in RViz and Gazebo.


<img width="1920" height="1017" alt="Screenshot (42)" src="https://github.com/user-attachments/assets/7837313e-42e8-4972-af06-8fc5b9339445" />
<img width="1920" height="1017" alt="Screenshot (43)" src="https://github.com/user-attachments/assets/330f4676-210b-4bfb-ae29-63cb30861965" />


## Features

- Xacro-based robot description with reusable inertia macros (`cylinder_inertia`, `box_inertia`)
- Mesh-based visuals (STL) with simplified collision geometry (spheres and boxes)
- Physically specified links: mass, inertia and joint limits for every link
- Fixed to the world through a `virtual_joint`
- One launch file starts robot_state_publisher, Gazebo, RViz and spawns the arm

## Packages

| Package | Contents |
|---|---|
| `my_robot_description` | Xacro files (`my_robot.urdf.xacro`, `common_properties.xacro`), STL meshes and the RViz config |
| `my_robot_bringup` | Launch file for Gazebo, RViz and robot spawning |

## Robot

| Link | Mass | Collision shape |
|---|---|---|
| `base_link` | 15 kg | Sphere, r = 0.35 m |
| `Waist` | 8 kg | Sphere, r = 0.35 m |
| `arm_1` | 5 kg | Box 0.40 × 1.40 × 0.20 m |
| `arm_2` | 5 kg | Box 0.40 × 1.40 × 0.20 m |
| `gripper` | 2 kg | Box 0.35 × 0.20 × 0.30 m |

| Joint | Type | Axis | Limits |
|---|---|---|---|
| `Joint_1` (base → waist) | revolute | Z | ±3.14 rad |
| `joint_2` (waist → arm_1) | revolute | X | ±π/2 rad |
| `joint_3` (arm_1 → arm_2) | revolute | X | 0 to π rad |
| `joint_4` (arm_2 → gripper) | revolute | X | 0 to π rad |

Frames: `world` → `base_link` → `Waist` → `arm_1` → `arm_2` → `gripper`

## Run with
Terminal 1: start the simulation

bash
cd ~/robotic_arm
source install/setup.bash
ros2 launch my_robot_bringup my_robot_gazebo.launch.xml

Terminal 2: open the sliders

bash
cd ~/robotic_arm
source install/setup.bash
ros2 run rqt_joint_trajectory_controller rqt_joint_trajectory_controller --force-discover
