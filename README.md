# Pritam Paul

Robotics and Autonomous Systems Software Engineer specializing in Autonomous Mobile Robots (AMRs), navigation stacks (ROS 2 Nav2), real-time control, and low-level embedded firmware.

Focused on the engineering boundary between Linux companion computers running path planning / fleet dispatch and bare-metal microcontroller control running real-time motor loops.

---

## Technical Stack

### Robotics & Autonomy
![ROS 2 Jazzy](https://img.shields.io/badge/ROS_2_Jazzy-%2322314E.svg?style=flat-square&logo=ros&logoColor=white)
![Nav2](https://img.shields.io/badge/Nav2-%2300599C.svg?style=flat-square&logo=ros&logoColor=white)
![Gazebo Sim](https://img.shields.io/badge/Gazebo_Sim-F28D1A?style=flat-square&logo=gazebo&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=opencv&logoColor=white)
![Eigen3](https://img.shields.io/badge/Eigen3-1B365D?style=flat-square)

### Languages & Systems
![C++20](https://img.shields.io/badge/C%2B%2B20-%2300599C.svg?style=flat-square&logo=c%2B%2B&logoColor=white)
![C](https://img.shields.io/badge/C-%2300599C.svg?style=flat-square&logo=c&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![CMake](https://img.shields.io/badge/CMake-064F8C?style=flat-square&logo=cmake&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnu-bash&logoColor=white)

### Embedded & Hardware
![STM32](https://img.shields.io/badge/STM32-03234B?style=flat-square&logo=stmicroelectronics&logoColor=white)
![ARM Cortex-M4](https://img.shields.io/badge/ARM_Cortex--M4-0091BD?style=flat-square&logo=arm&logoColor=white)
![FreeRTOS](https://img.shields.io/badge/FreeRTOS-1A8C82?style=flat-square&logo=c&logoColor=white)
![micro-ROS](https://img.shields.io/badge/micro--ROS-22314E?style=flat-square&logo=ros&logoColor=white)

### Graphics & Tooling
![OpenGL 4.6](https://img.shields.io/badge/OpenGL_4.6-5586A4?style=flat-square&logo=opengl&logoColor=white)
![Dear ImGui](https://img.shields.io/badge/Dear_ImGui-000000?style=flat-square)
![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white)
![GoogleTest](https://img.shields.io/badge/GoogleTest-34A853?style=flat-square)

---

## Featured Repositories

### [fleet-commander](https://github.com/paul-pritam/fleet-commander)
C++20 multi-robot fleet management application and visual console for warehouse AMRs running ROS 2 Nav2.
- Hardware-accelerated occupancy grid visualization in OpenGL 4.6 using `GL_NEAREST` texture filtering.
- Dynamic robot discovery and `/tf` coordinate tracking across active namespaces.
- Non-blocking asynchronous Nav2 `NavigateToPose` action client integration.
- Pluggable goal allocation engine supporting Euclidean and heading-aware cost metrics.
- Mutex-synchronized state machine decoupling ROS 2 callbacks from the 60 FPS Dear ImGui render loop.

<details>
<summary><b>Click to expand Fleet Commander system architecture</b></summary>

```
+-------------------------------------------------------------------+
|                        Fleet Commander GUI                        |
|             (GLFW 3.4 / OpenGL 4.6 / Dear ImGui v1.91.8)          |
+---------------------------------+---------------------------------+
                                  |
                   std::lock_guard<std::mutex>
                                  |
+---------------------------------v---------------------------------+
|                           FleetState                              |
|   - MapData (rasterized pixels, resolution, world origin)         |
|   - std::map<std::string, RobotState>                             |
|   - std::vector<GoalState>                                        |
+---------------------------------^---------------------------------+
                                  |
                   std::lock_guard<std::mutex>
                                  |
+---------------------------------+---------------------------------+
|                            RosBridge                              |
|                     (ROS 2 Jazzy rclcpp Node)                     |
+-------------------+-----------------------+-----------------------+
|  /map Subscriber  |    /tf Subscribers    |  Nav2 Action Clients  |
|  OccupancyGrid    |  Robot Pose Tracking  |   NavigateToPose      |
+-------------------+-----------------------+-----------------------+
```
</details>

### [my_auto_nav_pkg](https://github.com/paul-pritam/my_auto_nav_pkg)
Custom C++ ROS 2 Nav2 plugin package implementing modular navigation components.
- Global path planner plugin using an optimized A* search algorithm on costmaps.
- Local trajectory controller plugin implementing Pure Pursuit path tracking with lookahead horizon adaptation.
- Full integration with Nav2 lifecycle nodes and behavioral tree recovery actions.

### [pure_pursuit](https://github.com/paul-pritam/pure_pursuit)
Standalone C++ ROS 2 path tracking controller node.
- Computes curvature and angular velocity commands based on lookahead distance and goal poses.
- Designed for differential-drive kinematics with heading error clamping.

### [multi_bcr_bot](https://github.com/paul-pritam/multi_bcr_bot)
Multi-robot simulation testbed running warehouse AMR workflows in Gazebo Sim.
- Isolated namespaces for multi-agent AMCL localization and independent Nav2 stacks.
- Validates distributed path execution, collision monitor zones, and fleet dispatching.

---

## Interactive Systems Deep Dive

<details>
<summary><b>Two-Tier AMR Communication Topology (STM32 BlackPill to Companion SBC)</b></summary>

```
Workstation (Asus TUF)
  |  (ROS 2 DDS / Wi-Fi 802.11ac)
Companion SBC (Raspberry Pi 4B)
  - ROS 2 Jazzy / Nav2 Stack
  - AMCL Particle Filter Localization
  - EKF Sensor Fusion (robot_localization)
  |  (High-Speed Wired UART CDC @ 921,600 baud / micro-ROS)
Motor MCU (STM32F411CEU6 BlackPill)
  - Hardware Timers TIM2/TIM3: Quadrature Optical Encoder Decoding
  - Hardware Timer TIM4: Dual H-Bridge 20 kHz Motor PWM
  - 100 Hz Deterministic PID Velocity Control Loop
```
</details>

<details>
<summary><b>Sensor Fusion & Localization Architecture</b></summary>

- **AMCL Particle Filter Tuning**: Differential wheel slip noise modeled via `alpha1: 0.4`; angular convergence threshold set to `update_min_a: 0.08` rad to prevent particle divergence during stationary in-place rotations.
- **EKF Fusion Cardinal Rule**: Set `imu0_differential: true` to fuse relative yaw angular velocity with wheel odometry, preventing absolute yaw jumps between sensor coordinate frames.
- **Regulated Pure Pursuit Controller**: Configured with `use_rotate_to_heading: true` to execute zero-radius in-place turns before linear acceleration, eliminating overshoot on sharp trajectory corners.
</details>

---

## GitHub Activity

<p align="center">
  <img src="https://github-readme-stats.shion.dev/api?username=paul-pritam&theme=tokyonight&show_icons=true&include_all_commits=true&count_private=true" alt="Pritam Paul GitHub Stats" />
  <img src="https://github-readme-stats.shion.dev/api/top-langs/?username=paul-pritam&theme=tokyonight&layout=compact&hide=jupyter%20notebook" alt="Top Languages" />
</p>

---

## Contact & Links

- GitHub: [github.com/paul-pritam](https://github.com/paul-pritam)
- Email: [pritampaulwork7@gmail.com](mailto:pritampaulwork7@gmail.com)
