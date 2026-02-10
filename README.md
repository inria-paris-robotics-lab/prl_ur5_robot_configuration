# prl_ur5_robot_configuration

This repo holds the current configuration of the bimanual UR5 robot.

## Contents

* [Setups](#setups)
* [Arm and Gripper configuration](#arm-and-gripper-configuration)
* [Network configuration](#network-configuration)
* [Cameras configuration](#cameras-configuration)

  * [dataset_collection.yaml](#dataset_collectionyaml)
  * [cameras_config.yaml](#cameras_configyaml)
* [Custom gripper URDF](#custom-gripper-urdf)
* [Quick start](#quick-start)

---

## Setups

For different experiments, the robot can be equipped with different grippers and sensors.
To simplify modification, changeable parameters are placed in separate setup files in the [config/](config/) directory.
Each setup file contains a list of used devices and calibration parameters.
The calibration parameters include the positions of the arms, grippers, sensors, and kinematic parameters of UR5 arms.

---

## Quick start

1. Choose or duplicate a setup file in `config/`
2. Update arm poses and grippers to match your hardware
3. Configure network IPs and ports
4. Configure cameras if used
5. Launch the robot with the selected setup

---

## Arm and Gripper configuration

To configure your setup, edit the `prl_ur5_robot_configuration/config/standard_setup.yaml` file. Update the following parameters to match your hardware:

| Parameter            | Description                                 | Example Value       |
| -------------------- | ------------------------------------------- | ------------------- |
| `arm_pose.x`         | X position of the arm in meters             | -0.765              |
| `arm_pose.y`         | Y position of the arm in meters             | -0.09               |
| `arm_pose.z`         | Z position of the arm in meters             | 0.01                |
| `arm_pose.roll`      | Roll rotation in radians                    | 0                   |
| `arm_pose.pitch`     | Pitch rotation in radians                   | 0                   |
| `arm_pose.yaw`       | Yaw rotation in radians                     | 0                   |
| `gripper`            | Gripper type and hand configuration (enum)  | `custom:no_gripper` |
| `gripper_controller` | Controller name for the gripper (enum)      | `none`              |
| `gripper_pose.x`     | X position of the gripper offset in meters  | 0.0                 |
| `gripper_pose.y`     | Y position of the gripper offset in meters  | 0.0                 |
| `gripper_pose.z`     | Z position of the gripper offset in meters  | 0.01                |
| `gripper_pose.roll`  | Gripper roll rotation in radians            | 0.0                 |
| `gripper_pose.pitch` | Gripper pitch rotation in radians           | 0.0                 |
| `gripper_pose.yaw`   | Gripper yaw rotation in radians             | 0.0                 |
| `camera`             | End effector camera model identifier (enum) | `realsense-d435i`   |
| `camera_pose.x`      | X position of the camera in meters          | -1.82e-05           |
| `camera_pose.y`      | Y position of the camera in meters          | 0.0213              |
| `camera_pose.z`      | Z position of the camera in meters          | 0.173               |
| `camera_pose.roll`   | Camera roll rotation in radians             | 0.284               |
| `camera_pose.pitch`  | Camera pitch rotation in radians            | -1.561              |
| `camera_pose.yaw`    | Camera yaw rotation in radians              | -1.868              |

---

### Supported `gripper` values

| Gripper                | Supported controllers | Description                                                                     |
| ---------------------- | --------------------- | ------------------------------------------------------------------------------- |
| `onrobot-rg2`          | `onrobot-rg`          | OnRobot RG2 parallel gripper                                                    |
| `onrobot-rg6`          | `onrobot-rg`          | OnRobot RG6 parallel gripper                                                    |
| `onrobot-rg6-v2`       | `onrobot-rg`          | OnRobot RG6 parallel gripper (version 2)                                        |
| `weiss-wsg50`          | `weiss-gripper`       | Weiss WSG 50 parallel gripper                                                   |
| `custom:no_gripper`    | `none`                | Custom gripper configuration with no specific controller                        |
| `custom:cylinder_tool` | `none`                | Custom gripper configuration for a cylindrical tool with no specific controller |
| `allegro-hand:right`   | `allegro-hand`        | Allegro Hand – right hand configuration                                         |
| `allegro-hand:left`    | `allegro-hand`        | Allegro Hand – left hand configuration                                          |

---

## Network configuration

This file defines the network settings used to communicate with robot arms.
Each arm is described by a user-defined arm name (e.g., `left_network`, `right_network`).

```yaml
<arm_name>:
  ip: <ip_address>
  ...
```

Each arm must use **unique ports**. IP and ports must match the robot configuration.

### Network parameters

| Parameter             | Type   | Description                         |
| --------------------- | ------ | ----------------------------------- |
| `<arm_name>`          | string | User-defined arm network identifier |
| `ip`                  | string | Robot controller IP address         |
| `script_command_port` | string | Port used to send script commands   |
| `reverse_port`        | string | Reverse communication port          |
| `script_sender_port`  | string | Script sender communication port    |
| `trajectory_port`     | string | Trajectory streaming port           |

---

## Cameras configuration

### dataset_collection.yaml

This file defines the **physical placement of cameras** in the environment.
Each camera entry is an item in a list and represents one mounted camera.

```yaml
- type: <camera_model>
  name_prefix: <camera_name>
  ...
```

* The order of entries is not significant
* `name_prefix` must be unique
* The camera name is derived from `name_prefix`

### Common camera parameters

| Parameter             | Type   | Description                                     |
| --------------------- | ------ | ----------------------------------------------- |
| `type`                | enum   | Camera model (`realsense-d435i`, `femto-mega`)  |
| `name_prefix`         | string | Camera identifier prefix                        |
| `bounding_box`        | object | Physical camera footprint (meters)              |
| `pose`                | object | Camera pose in world frame                      |
| `fixture_orientation` | enum   | Mount orientation relative to fixture           |
| `offset`              | object | Offset from fixture tip to camera optical frame |

#### Bounding box parameters

| Parameter             | Type  | Description             |
| --------------------- | ----- | ----------------------- |
| `bounding_box.width`  | float | Camera width in meters  |
| `bounding_box.height` | float | Camera height in meters |

> The bounding box can be shared between cameras using YAML anchors.

#### Pose parameters

| Parameter    | Type  | Description               |
| ------------ | ----- | ------------------------- |
| `pose.x`     | float | X position in meters      |
| `pose.y`     | float | Y position in meters      |
| `pose.z`     | float | Z position in meters      |
| `pose.roll`  | float | Roll rotation in radians  |
| `pose.pitch` | float | Pitch rotation in radians |
| `pose.yaw`   | float | Yaw rotation in radians   |

#### Fixture orientation

| Value | Description                           |
| ----- | ------------------------------------- |
| `+y`  | Camera mounted facing positive Y axis |
| `-y`  | Camera mounted facing negative Y axis |
| `+x`  | Camera mounted facing positive X axis |
| `-x`  | Camera mounted facing negative X axis |

#### Offset parameters

| Parameter  | Type  | Description        |
| ---------- | ----- | ------------------ |
| `offset.x` | float | X offset in meters |
| `offset.y` | float | Y offset in meters |
| `offset.z` | float | Z offset in meters |

> Offset is applied from the fixture tip to the camera optical frame.

---

### cameras_config.yaml

Each camera is defined by a user-defined camera name (e.g., `alpha_camera`, `front_cam`, `camera_1`).
This name is used as a key and can be freely chosen by the user.

```yaml
cameras:
  <camera_name>:
    type: <camera_type>
    activate: <true|false>
    ...
```

> [!NOTE]
> `type` refers to the camera driver (`realsense`, `femto-mega`), while the model (e.g., `realsense-d435i`) is defined in the setup file.

#### RealSense camera parameters (type: realsense)

| Parameter       | Type   | Description                                         |
| --------------- | ------ | --------------------------------------------------- |
| `serial_no`     | string | RealSense device serial number                      |
| `enable_depth`  | bool   | Enable depth stream                                 |
| `enable_infra`  | bool   | Enable infrared streams (requires infra1 or infra2) |
| `enable_infra1` | bool   | Enable infrared stream 1                            |
| `enable_infra2` | bool   | Enable infrared stream 2                            |
| `enable_color`  | bool   | Enable RGB stream                                   |
| `enable_gyro`   | bool   | Enable gyroscope stream                             |
| `enable_accel`  | bool   | Enable accelerometer stream                         |
| `enable_rgbd`   | bool   | Enable RGB-D combined stream                        |
| `pointcloud`    | bool   | Enable point cloud generation                       |
| `enable_sync`   | bool   | Enable hardware synchronization                     |
| `align_depth`   | bool   | Align depth to color frame                          |

#### Femto Mega camera parameters (type: femto-mega)

| Parameter                    | Type   | Description                                            |
| ---------------------------- | ------ | ------------------------------------------------------ |
| `ip`                         | string | Camera IP address                                      |
| `port`                       | string | Camera communication port                              |
| `sync_mode`                  | enum   | Synchronization mode (`standalone`, `master`, `slave`) |
| `enable_colored_point_cloud` | bool   | Enable colored point cloud                             |
| `depth_registration`         | bool   | Register depth to color frame                          |

---

## Custom gripper URDF

In the `*_setup.yaml` file, the type of gripper used for each arm is specified. Standard types include `'onrobot-rg2'`, `'onrobot-rg6'`, `'onrobot-rg6-v2'`, etc.

To attach **custom tools**, create a custom URDF in the [custom_gripper/](custom_gripper/) directory.

### Usage

To create a custom tool/gripper named `my_gripper`, `prl_ur5_description` expects:

1. `prl_ur5_robot_configuration/urdf/my_gripper.urdf.xacro` containing:

```xml
<?xml version="1.0"?>
<robot xmlns:xacro="http://wiki.ros.org/xacro">
  <xacro:macro name="my_gripper" params="parent *origin prefix">
      ...
  </xacro:macro>
</robot>
```
with the first link being the child of `${parent}` with a transform of `*origin` and all the link and joint having the prefix `${prefix}` in front of their names.

2. `prl_ur5_robot_configuration/urdf/my_gripper.srdf.xacro` containing:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<robot xmlns:xacro="http://wiki.ros.org/xacro">
    <xacro:macro name="cylinder_tool_group_define" params="prefix">
        ...$1...
    </xacro:macro>

    <xacro:macro name="cylinder_tool_autocol" params="prefix">
        ...$2...
    </xacro:macro>

    <xacro:macro name="cylinder_tool_col_with" params="prefix link">
        ...$3...
    </xacro:macro>
</robot>
```
with `$1` being `<link>` and `<joint>` tags defining the gripper, `$2` being `<disable_collision>` tags to disable all the necessary autocollisions inside the gripper, and `$3` being `<disable_collision>` tags to disable the collisions between all the gripper links and an arbitrary `${link}` link.

Then in `*_setup.yaml` set:
```
...
  gripper: "custom:my_gripper"
...
```

**Note**: It is strongly recommended to create a link named `${prefix}_grasp_frame` in the gripper URDF (corresponding to the tip/grasp point of the gripper/tool) to keep consistency with the other grippers.