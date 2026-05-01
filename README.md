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



















# Hospital Delivery Robot (ROS 2 + Gazebo)

## Overview
Autonomous hospital delivery robot simulation using ROS 2 and Gazebo.

## Demo Video
https://studio.youtube.com/video/OtlEBUTG7Do/edit
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
