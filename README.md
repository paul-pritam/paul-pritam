# Pritam Paul

Robotics and Autonomous Systems Software Engineer specializing in Autonomous Mobile Robots (AMRs), navigation stacks (ROS 2 Nav2), real-time control, and low-level embedded firmware.

Focused on the engineering boundary between Linux companion computers running path planning / fleet dispatch and bare-metal microcontroller control running real-time motor loops.

## Core Technical Focus

- **Motion Planning & Navigation**: Custom Nav2 plugin development (A* global planners, Pure Pursuit / Regulated Pure Pursuit controllers), costmap layers, dynamic collision avoidance.
- **State Estimation & Sensor Fusion**: Extended Kalman Filtering (`robot_localization`), AMCL probabilistic localization, wheel odometry calibration, and transform trees (`/tf`).
- **Fleet Coordination & Tooling**: Multi-AMR asynchronous goal dispatching, OpenGL 4.6 / Dear ImGui telemetry dashboards, and thread-safe ROS 2 action clients.
- **Embedded & Real-Time Firmware**: Bare-metal C on ARM Cortex-M4 (STM32F411), FreeRTOS task scheduling, hardware timer quadrature encoder decoding, and micro-ROS serial transport.

## Technical Skills

- **Languages**: C++20, C (Bare-Metal / FreeRTOS), Python, Bash, CMake
- **Robotics & Middleware**: ROS 2 (Jazzy, Humble), Nav2, Gazebo Sim (Harmonic / Fortress), TF2, micro-ROS
- **Estimation & Planning**: Regulated Pure Pursuit, A*, AMCL, Extended Kalman Filter (EKF), Costmap2D
- **Embedded Systems**: STM32F411CEU6, FreeRTOS, PWM motor control, optical quadrature encoders, I2C/SPI sensors
- **Graphics & Telemetry**: OpenGL 4.6 (Core Profile), Dear ImGui, GLFW
- **Development & Verification**: Linux (Ubuntu, Arch), Git, GoogleTest, GDB, Valgrind

## Featured Repositories

### [fleet-commander](https://github.com/paul-pritam/fleet-commander)
C++20 multi-robot fleet management application and visual console for warehouse AMRs running ROS 2 Nav2.
- Hardware-accelerated occupancy grid visualization in OpenGL 4.6 using `GL_NEAREST` filtering.
- Dynamic robot discovery and `/tf` coordinate tracking across active namespaces.
- Non-blocking asynchronous Nav2 `NavigateToPose` action client integration.
- Pluggable goal allocation engine supporting Euclidean and heading-aware cost metrics.
- Mutex-synchronized state machine decoupling ROS 2 callbacks from the 60 FPS Dear ImGui render loop.

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

## Contact & Links

- GitHub: [github.com/paul-pritam](https://github.com/paul-pritam)
- Email: [pritampaulwork7@gmail.com](mailto:pritampaulwork7@gmail.com)
