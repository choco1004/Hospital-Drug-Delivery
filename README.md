source /opt/ros/jazzy/setup.bash
colcon build
ros2 launch simulation.launch.py

<WSL>

</>bash   
mkdir -p ~/ws/src/drug_delivery_hospital/worlds
mkdir -p ~/ws/src/drug_delivery_hospital/launch
mkdir -p ~/ws/src/drug_delivery_hospital/models/drugbot
cd ~/ws/src/drug_delivery_hospital

1. package.xml
   
</>bash
nano package.xml

<?xml version="1.0"?>
<package format="3">
  <name>drug_delivery_hospital</name>
  <version>0.0.1</version>
  <description>Hospital delivery robot simulation using ROS 2 and Gazebo</description>
  <maintainer email="your_email@example.com">Seohee Park</maintainer>
  <license>MIT</license>

  <buildtool_depend>ament_cmake</buildtool_depend>

  <exec_depend>ros_gz_sim</exec_depend>
  <exec_depend>ros_gz_bridge</exec_depend>
  <exec_depend>geometry_msgs</exec_depend>

  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>

2. CMakeLists.txt
</>Bash
CMakeLists.txt

cmake_minimum_required(VERSION 3.8)
project(drug_delivery_hospital)

find_package(ament_cmake REQUIRED)

install(DIRECTORY
  launch
  worlds
  models
  DESTINATION share/${PROJECT_NAME}
)

ament_package()

3. world/ hospital_world.sdf
</>bash
nano worlds/hospital_world.sdf
<?xml version="1.0" ?>
<sdf version="1.9">
  <world name="hospital_world">
    <physics name="1ms" type="ignored">
      <max_step_size>0.001</max_step_size>
      <real_time_factor>1.0</real_time_factor>
    </physics>

    <plugin filename="gz-sim-physics-system" name="gz::sim::systems::Physics"/>
    <plugin filename="gz-sim-user-commands-system" name="gz::sim::systems::UserCommands"/>
    <plugin filename="gz-sim-scene-broadcaster-system" name="gz::sim::systems::SceneBroadcaster"/>

    <light type="directional" name="sun">
      <cast_shadows>true</cast_shadows>
      <pose>0 0 10 0 0 0</pose>
      <diffuse>0.8 0.8 0.8 1</diffuse>
      <specular>0.2 0.2 0.2 1</specular>
      <direction>-0.5 0.1 -0.9</direction>
    </light>

    <model name="ground_plane">
      <static>true</static>
      <link name="link">
        <collision name="collision">
          <geometry><plane><normal>0 0 1</normal><size>20 20</size></plane></geometry>
        </collision>
        <visual name="visual">
          <geometry><plane><normal>0 0 1</normal><size>20 20</size></plane></geometry>
          <material><ambient>0.8 0.8 0.8 1</ambient></material>
        </visual>
      </link>
    </model>

    <!-- Hospital corridor walls -->
    <model name="left_wall">
      <static>true</static>
      <pose>0 4 1 0 0 0</pose>
      <link name="link">
        <collision name="collision"><geometry><box><size>16 0.2 2</size></box></geometry></collision>
        <visual name="visual"><geometry><box><size>16 0.2 2</size></box></geometry></visual>
      </link>
    </model>

    <model name="right_wall">
      <static>true</static>
      <pose>0 -4 1 0 0 0</pose>
      <link name="link">
        <collision name="collision"><geometry><box><size>16 0.2 2</size></box></geometry></collision>
        <visual name="visual"><geometry><box><size>16 0.2 2</size></box></geometry></visual>
      </link>
    </model>

    <!-- Obstacles -->
    <model name="hospital_bed">
      <static>true</static>
      <pose>2 1.5 0.4 0 0 0</pose>
      <link name="link">
        <collision name="collision"><geometry><box><size>1.8 0.8 0.8</size></box></geometry></collision>
        <visual name="visual"><geometry><box><size>1.8 0.8 0.8</size></box></geometry></visual>
      </link>
    </model>

    <model name="medical_cart">
      <static>true</static>
      <pose>-2 -1.3 0.5 0 0 0</pose>
      <link name="link">
        <collision name="collision"><geometry><box><size>0.8 0.6 1.0</size></box></geometry></collision>
        <visual name="visual"><geometry><box><size>0.8 0.6 1.0</size></box></geometry></visual>
      </link>
    </model>

    <include>
      <uri>model://drugbot</uri>
      <pose>-6 0 0.2 0 0 0</pose>
    </include>
  </world>
</sdf>

4. models/drugbot/model.config
 </>bash 
   nano models/drugbot/model.config

</> XML 
   <?xml version="1.0"?>
<model>
  <name>drugbot</name>
  <version>1.0</version>
  <sdf version="1.9">model.sdf</sdf>
  <author>
    <name>Seohee Park</name>
  </author>
  <description>Simple differential-drive hospital delivery robot</description>
</model>

5. models/drugbot/model.sdf
nano models/drugbot/model.sdf

<?xml version="1.0" ?>
<sdf version="1.9">
  <model name="drugbot">
    <pose>0 0 0.2 0 0 0</pose>

    <link name="base_link">
      <inertial>
        <mass>5.0</mass>
        <inertia>
          <ixx>0.2</ixx><iyy>0.2</iyy><izz>0.2</izz>
          <ixy>0</ixy><ixz>0</ixz><iyz>0</iyz>
        </inertia>
      </inertial>
      <collision name="base_collision">
        <geometry><box><size>0.8 0.5 0.25</size></box></geometry>
      </collision>
      <visual name="base_visual">
        <geometry><box><size>0.8 0.5 0.25</size></box></geometry>
      </visual>
    </link>

    <link name="left_wheel">
      <pose>0 0.32 0 1.5708 0 0</pose>
      <inertial><mass>0.5</mass></inertial>
      <collision name="collision"><geometry><cylinder><radius>0.12</radius><length>0.08</length></cylinder></geometry></collision>
      <visual name="visual"><geometry><cylinder><radius>0.12</radius><length>0.08</length></cylinder></geometry></visual>
    </link>

    <link name="right_wheel">
      <pose>0 -0.32 0 1.5708 0 0</pose>
      <inertial><mass>0.5</mass></inertial>
      <collision name="collision"><geometry><cylinder><radius>0.12</radius><length>0.08</length></cylinder></geometry></collision>
      <visual name="visual"><geometry><cylinder><radius>0.12</radius><length>0.08</length></cylinder></geometry></visual>
    </link>

    <joint name="left_wheel_joint" type="revolute">
      <parent>base_link</parent>
      <child>left_wheel</child>
      <axis><xyz>0 0 1</xyz></axis>
    </joint>

    <joint name="right_wheel_joint" type="revolute">
      <parent>base_link</parent>
      <child>right_wheel</child>
      <axis><xyz>0 0 1</xyz></axis>
    </joint>

    <plugin filename="gz-sim-diff-drive-system" name="gz::sim::systems::DiffDrive">
      <left_joint>left_wheel_joint</left_joint>
      <right_joint>right_wheel_joint</right_joint>
      <wheel_separation>0.64</wheel_separation>
      <wheel_radius>0.12</wheel_radius>
      <topic>/cmd_vel</topic>
      <odom_topic>/odom</odom_topic>
      <frame_id>odom</frame_id>
      <child_frame_id>base_link</child_frame_id>
    </plugin>
  </model>
</sdf>

6. launch/gz_sim.launch.py
nano launch/gz_sim.launch.py
</> python
from launch import LaunchDescription
from launch.actions import ExecuteProcess, SetEnvironmentVariable
from ament_index_python.packages import get_package_share_directory
import os


def generate_launch_description():
    pkg_share = get_package_share_directory('drug_delivery_hospital')
    world_file = os.path.join(pkg_share, 'worlds', 'hospital_world.sdf')
    model_path = os.path.join(pkg_share, 'models')

    return LaunchDescription([
        SetEnvironmentVariable(
            name='GZ_SIM_RESOURCE_PATH',
            value=model_path
        ),
        ExecuteProcess(
            cmd=['gz', 'sim', '-r', world_file],
            output='screen'
        )
    ])

7. Commit

   cd ~/ws
source /opt/ros/jazzy/setup.bash
colcon build
source install/setup.bash
ros2 launch drug_delivery_hospital gz_sim.launch.py

8. robot moving

   <wsl> 

   gz topic -t /cmd_vel -m gz.msgs.Twist -p 'linear: {x: 0.5}, angular: {z: 0.0}'

 rotation 
   gz topic -t /cmd_vel -m gz.msgs.Twist -p 'linear: {x: 0.0}, angular: {z: 0.5}'

stop 

gz topic -t /cmd_vel -m gz.msgs.Twist -p 'linear: {x: 0.0}, angular: {z: 0.0}'

# Hospital-Drug-Delivery
Hospital Drug Delivery Mobile Robot Simulation  
import numpy as np
import matplotlib.pyplot as plt

# Target position (robot goal)
target_x = 5.0
target_y = 5.0

# Simulated robot position (example)
robot_x = 5.1
robot_y = 4.9

# Distance check (arrival condition)
distance = np.sqrt((robot_x - target_x)**2 + (robot_y - target_y)**2)

threshold = 0.3  # arrival tolerance

if distance < threshold:
    print("Drug delivery completed")

    # Pharmacokinetic model (simple exponential decay)
    t = np.linspace(0, 24, 100)
    k = 0.2  # elimination rate
    C0 = 10  # initial concentration

    C = C0 * np.exp(-k * t)

    # Plot result
    plt.plot(t, C)
    plt.xlabel("Time (hours)")
    plt.ylabel("Drug Concentration")
    plt.title("Post-Delivery Drug Concentration")
    plt.grid()

    plt.savefig("concentration_plot.png")
    plt.show()
else:
    print("Robot is still navigating...")



















# Hospital Delivery Robot (ROS 2 + Gazebo)

## Overview
Autonomous hospital delivery robot simulation using ROS 2 and Gazebo.

## Demo Video
https://studio.youtube.com/video/OtlEBUTG7Do/edit

https://www.youtube.com/watch?v=HY1NJFGmMUg

https://studio.youtube.com/video/GycP0SJv3eI/edit
## Features
- Differential drive robot
- Obstacle avoidance
- Hospital environment simulation

## How to Run
```bash
source /opt/ros/jazzy/setup.bash
colcon build
ros2 launch simulation.launch.py


# Hospital-Drug-Delivery
Hospital Drug Delivery Mobile Robot Simulation  
import numpy as np
import matplotlib.pyplot as plt

# Target position (robot goal)
target_x = 5.0
target_y = 5.0

# Simulated robot position (example)
robot_x = 5.1
robot_y = 4.9

# Distance check (arrival condition)
distance = np.sqrt((robot_x - target_x)**2 + (robot_y - target_y)**2)

threshold = 0.3  # arrival tolerance

if distance < threshold:
    print("Drug delivery completed")

    # Pharmacokinetic model (simple exponential decay)
    t = np.linspace(0, 24, 100)
    k = 0.2  # elimination rate
    C0 = 10  # initial concentration

    C = C0 * np.exp(-k * t)

    # Plot result
    plt.plot(t, C)
    plt.xlabel("Time (hours)")
    plt.ylabel("Drug Concentration")
    plt.title("Post-Delivery Drug Concentration")
    plt.grid()

    plt.savefig("concentration_plot.png")
    plt.show()
else:
    print("Robot is still navigating...")
