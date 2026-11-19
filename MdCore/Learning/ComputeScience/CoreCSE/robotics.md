
# 26 · ROBOTICS & AUTONOMOUS SYSTEMS

## 26.1 Perception & Control

### Perception

**Definition:** Process sensor data to understand environment.

**Sensors:**
- **Lidar:** 3D point cloud; precise distance
- **Camera:** RGB, depth, stereo
- **IMU:** Accelerometer, gyroscope
- **GPS:** Global position
- **Radar:** Distance, velocity; works in bad weather
- **Ultrasonic:** Short-range distance

**Tasks:**
- **Object detection:** Identify objects
- **Tracking:** Follow objects over time
- **Localization:** Determine position
- **Mapping:** Build map of environment

**Sensor fusion:** Combine multiple sensors (Kalman filter, particle filter).

**Recall:** *Sensors → understanding.*

### SLAM (Simultaneous Localization and Mapping)

**Definition:** Build map while simultaneously localizing in it.

**Problem:** Chicken-and-egg — need map to localize, need localization to map.

**Approaches:**
- **EKF-SLAM:** Extended Kalman filter; landmarks
- **Graph SLAM:** Pose graph optimization
- **Visual SLAM:** ORB-SLAM, LSD-SLAM
- **Visual-inertial:** VINS-Mono, OKVIS

**Loop closure:** Recognize previously visited place; correct drift.

**Recall:** *Map + locate at once.*

### Control

**PID Controller:**
```
u(t) = Kp e(t) + Ki ∫e(τ)dτ + Kd de/dt
```
- **P:** Proportional to error
- **I:** Eliminates steady-state error
- **D:** Dampens oscillations

**LQR (Linear Quadratic Regulator):** Optimal control for linear systems; minimizes quadratic cost.

**MPC (Model Predictive Control):** Optimize over finite horizon; receding horizon.

**Adaptive control:** Adjust parameters online.

**Recall:** *PID, LQR, MPC.*

### State Estimation

**Kalman filter:** Optimal linear estimator for Gaussian noise.
```
Predict: x̂ = Fx + Bu; P = FPFᵀ + Q
Update: K = PHᵀ(HPHᵀ + R)⁻¹; x̂ = x̂ + K(z − Hx̂); P = (I − KH)P
```

**Extended Kalman filter (EKF):** Linearize nonlinear systems.

**Particle filter:** Monte Carlo; handles non-Gaussian, multimodal.

**Recall:** *Kalman, EKF, particle filter.*

### Kinematics & Dynamics

**Forward kinematics:** Joint angles → end-effector pose.

**Inverse kinematics:** End-effector pose → joint angles. May have 0, 1, or many solutions.

**Jacobian:** Relates joint velocities to end-effector velocities.

**Dynamics:** Equations of motion (Newton-Euler, Lagrangian).

**Recall:** *Forward/inverse kinematics, dynamics.*

---

## 26.2 Planning & Manipulation

### Motion Planning

**Definition:** Find collision-free path from start to goal.

**Approaches:**
- **A*:** Grid-based; heuristic
- **RRT (Rapidly-exploring Random Tree):** Sampling-based; high-dimensional
- **RRT*:** Asymptotically optimal
- **PRM (Probabilistic Roadmap):** Build roadmap; query
- **Trajectory optimization:** Optimize smooth trajectory

**Constraints:** Kinematic, dynamic, collision.

**Recall:** *RRT, PRM, trajectory optimization.*

### Task Planning

**Definition:** Sequence actions to achieve goal.

**STRIPS:** States, actions, preconditions, effects.

**PDDL:** Planning Domain Definition Language.

**HTN (Hierarchical Task Network):** Decompose tasks into subtasks.

**Recall:** *Sequence actions.*

### Manipulation

**Grasping:** Determine grasp points; force closure.

**Dexterous manipulation:** Multi-finger; in-hand manipulation.

**Grasp synthesis:** Analytical (force closure) vs data-driven (learning).

**Recall:** *Grasping, dexterous manipulation.*

### Locomotion

**Types:**
- **Wheeled:** Simple, efficient on flat terrain
- **Legged:** Adaptable to rough terrain
- **Aerial:** Drones, flying robots
- **Underwater:** Submersibles

**Legged locomotion:** Bipedal, quadrupedal; balance, gait generation.

**Recall:** *Wheeled, legged, aerial.*

---

## 26.3 Systems & Applications

### ROS (Robot Operating System)

**Definition:** Middleware framework for robotics.

**Components:**
- **Nodes:** Processes
- **Topics:** Publish/subscribe
- **Services:** Request/response
- **Actions:** Long-running tasks
- **Messages:** Data types

**Tools:** RViz (visualization), Gazebo (simulation), rosbag (logging).

**ROS 2:** DDS-based; real-time, security, multi-robot.

**Recall:** *Robot Operating System.*

### Autonomous Vehicles

**Levels (SAE):**
| Level | Description |
|-------|-------------|
| 0 | No automation |
| 1 | Driver assistance |
| 2 | Partial automation |
| 3 | Conditional automation |
| 4 | High automation |
| 5 | Full automation |

**Stack:**
- **Perception:** Cameras, lidar, radar
- **Localization:** GPS, IMU, HD maps
- **Planning:** Route, behavior, motion
- **Control:** Steering, throttle, brake

**Companies:** Waymo, Tesla, Cruise, Zoox.

**Recall:** *Self-driving, drones.*

### Human-Robot Interaction (HRI)

**Definition:** Study of interactions between humans and robots.

**Topics:** Safety, trust, communication, collaboration.

**Safety:** ISO 10218, ISO/TS 15066 (collaborative robots).

**Recall:** *Safety, collaboration.*

### Sim-to-Real

**Definition:** Transfer policies learned in simulation to real world.

**Challenges:** Reality gap (physics, sensing, actuation).

**Techniques:**
- **Domain randomization:** Randomize simulation parameters
- **Domain adaptation:** Align features
- **System identification:** Match simulation to reality

**Recall:** *Domain randomization, transfer learning.*

### Multi-Robot Systems

**Definition:** Coordination of multiple robots.

**Topics:** Swarm robotics, formation control, task allocation.

**Challenges:** Communication, scalability, robustness.

**Recall:** *Swarm, coordination.*

### Soft Robotics

**Definition:** Robots made from compliant materials.

**Benefits:** Safe interaction, adaptable, robust.

**Materials:** Silicone, hydrogels, shape memory alloys.

**Use:** Grasping fragile objects, medical, exploration.

**Recall:** *Compliant actuators, materials.*

### Humanoids

**Definition:** Robots with human-like form.

**Challenges:** Bipedal balance, whole-body control, dexterous manipulation.

**Examples:** Atlas (Boston Dynamics), ASIMO (Honda), Optimus (Tesla).

**Recall:** *Bipedal locomotion, whole-body control.*

**SOTA:** Foundation models for robotics, embodied AI, RT-2, PaLM-E, sim-to-real, soft robotics.

---
