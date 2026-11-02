# Physical AI & Robot Learning

*Learning transferable robot skills from human demonstrations, reinforcement learning and physical interaction.*

Robot learning from people and from simulation, executed on real and simulated robots: human demonstrations recorded
with hand-worn grippers and converted into training data for a low-cost dual-arm robot (reBot B601-DM), and
reinforcement learning for humanoid locomotion. Each project lives in its own repository.

| Garment manipulation | Cube stacking | G1 humanoid navigation |
|---|---|---|
| ![human demo vs robot replay](https://github.com/physical-ai-vla/egocentric-garment-manipulation/raw/main/media/human_vs_robot_replay_large.gif) | ![cube stacking demos](https://github.com/physical-ai-vla/egocentric-cube-stacking/raw/main/media/demo_three_orders.gif) | ![G1 reaching an instructed object](https://github.com/physical-ai-vla/g1-target-walking/raw/main/videos/seed0_go-to-the-orange-cylinder.gif) |

## Projects

| Project | Period | Task | What the repository contains | Status | Repository |
|---|---|---|---|---|---|
| **Cube stacking** | Sep – Oct 2026 | bimanual three-cube stacking in 6 instructed orders | 349 recorded demonstrations → pose (wrist SLAM + IMU scale), gripper and gate filtering → robot training data (joint space, then relative Cartesian); X-VLA pretraining on human data and fine-tuning on robot data; one 15-page technical report; a script that re-checks every reported number (736 claims) | offline results; no robot success rate | [egocentric-cube-stacking](https://github.com/physical-ai-vla/egocentric-cube-stacking) |
| **Garment manipulation** | Oct 2026 – | T-shirt flattening and folding | the complete project: recorder, calibration, pose (fixed-camera point tracking + markers + IMU), servo-encoder jaw labels, label and dataset builders, policy training and the receding-horizon robot deploy, configs, technical report and videos; open-loop replay as an end-to-end test of the data; 300 flattening demonstrations; first policies observed on the robot (trials not counted); first real-robot grasps in a one-sleeve pinching test on 2 November (trials not counted); small reimplemented utilities with tests on synthetic data | in progress; no policy success rate yet | [egocentric-garment-manipulation](https://github.com/physical-ai-vla/egocentric-garment-manipulation) |
| **G1 humanoid navigation** | Jun 2026 | instructed target reaching with a Unitree G1 in MuJoCo / MJX | a PPO walking policy trained from scratch (154.8 M environment steps, 77 min; 0.52 m/s for a 0.6 m/s command) driven by a hand-written Navigator; three seeded scenes reached within 0.55 m without falling, with keyword target selection and the simulator's object position; checkpoints, videos and a claims check that re-measures every number on CPU | finished simulation prototype; no hardware, no success rate | [g1-target-walking](https://github.com/physical-ai-vla/g1-target-walking) |

## What the demonstration projects share (garment, cube)

- **Hardware.** Two hand-worn grippers built from **HandUMI**, an open-source design by **BrikHMP18**
  ([murobotics-ai/handumi-hw](https://github.com/murobotics-ai/handumi-hw), Apache-2.0) — **not our design**. We printed
  and assembled it and made small changes: a wrist IMU instead of VR tracking, and (garment project) AprilTag markers on
each gripper. A fixed camera watches the table: a head-mounted webcam (cube) or an RGB-D camera (garment). The jaw opening
  is read from the gripper's **Feetech jaw servo used as an encoder** (cube data; garment demonstrations since 29 October),
  and from the wrist video while the servos were removed (13–28 October).
- **One repository layout.** `src/` importable packages, `configs/`, `scripts/` by pipeline stage, `tests/`, `docs/`, CI
  on every push, a README per component, and a "current vs. superseded" table so that dropped approaches stay visible.
- **Conventions.** Poses in the robot base frame; actions as relative end-effector motion `inv(T_t)·T_{t+k}`; 1 = open
  for the gripper. The cube CART20 action uses a continuous aperture; the garment action uses the binary jaw state with a
  0.1 s ramp (the continuous aperture is stored separately).

## How the approach changed (cube → garment)

| | cube stacking | garment manipulation |
|---|---|---|
| pose source | wrist-camera SLAM + IMU scale | fixed RGB-D camera: point tracking anchored by markers, IMU inside gaps (wrist SLAM lost tracking at grasps) |
| robot data | retargeted to joints, later relative Cartesian | Cartesian labels taken before inverse kinematics, no clamps or table lift (IK does not change or mask them) |
| data test | offline metrics | open-loop replay of human motion on the robot, then policies on the robot |

Details, numbers and their limits are in each project's README and technical report.

## Author

**Jeong Hwan Lee** — Robotics Engineer, Physical AI / Robot Learning · <https://github.com/physical-ai-vla>

## License

Each repository states its own terms (the project repositories use the PolyForm Noncommercial License 1.0.0:
**commercial use is not permitted**). Third-party work keeps its own licence and credit.
