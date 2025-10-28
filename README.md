# Bramy ROS 2 Stack

![Prototype vehicle](docs/penguine.png)

## Overview
Bramy is a ROS 2 package that fuses perception and drive-by-wire control for an autonomous RC car platform. It captures RGB and depth frames from an Orbbec Astra camera, detects obstacles with an ONNX model, tracks them with OC-SORT, estimates distance from the depth stream, and publishes speed and steering commands over a CAN bus interface.

## Features
- ROS 2 nodes for camera publishing, object detection, multi-object tracking, and CAN control.
- OpenNI2 based driver for Astra RGB-D cameras with image registration into ROS 2 topics.
- ONNXRuntime pipeline for real-time inference and OC-SORT based identity preservation.
- Depth-aware post processing that computes throttle and steering commands.
- SocketCAN bridge that transmits commands to the vehicle firmware.

## Repository Layout
- `rclpy/car/bramy/camera`: Astra camera wrapper and helpers.
- `rclpy/car/bramy/ocsort`: OC-SORT tracker implementation.
- `rclpy/car/bramy/utils`: Pre and post processing utilities for the detector.
- `rclpy/car/bramy`: ROS 2 node implementations (`camera_publisher`, `tracking`, `PostProcess`, `get_control`).
- `rclpy/car/launch`: Launch files (for now `bramy.launch.py`).
- `docs/penguine.png`: Project image used in this README.

## Prerequisites
- ROS 2 Jazzy (other recent distros may work with minor adjustments).
- OpenNI2 runtime and Astra camera drivers (path configured in `camera/astra_camera.py`).
- Python packages: `opencv-python`, `cv_bridge`, `onnxruntime`, `python-can`, `numpy`, `torch`.
- A SocketCAN compatible interface exposed as `can0`.
- An ONNX model file accessible to the deployment target (path configured in `tracking.py`).

Use `rosdep` or `pip` to install any missing Python dependencies:

```bash
rosdep install --from-paths src --ignore-src -y
python3 -m pip install onnxruntime python-can opencv-python numpy torch
```

## Build
```bash
source /opt/ros/jazzy/setup.bash
colcon build
source install/setup.bash
```

## Launch
```bash
ros2 launch bramy bramy.launch.py
```

The launch file starts the four core nodes:

| Node name        | Executable        | Purpose                                                  |
|------------------|-------------------|----------------------------------------------------------|
| `publisher_node` | `camera_publisher`| Publishes `color_image` and `depth_image` from the Astra |
| `tracking`       | `tracking`        | Runs ONNX inference and publishes detections             |
| `postProc`       | `postProcess`     | Filters detections, estimates distance, publishes control|
| `get_control`    | `get_control`     | Bridges control commands to the CAN bus                  |

## Topics
- `/color_image` (`sensor_msgs/Image`): RGB frames from the camera.
- `/depth_image` (`sensor_msgs/Image`): Depth frames aligned to RGB.
- `/cordinates` (`std_msgs/Float32MultiArray`): Flattened detection bounding boxes and confidence scores.
- `/get_control` (`std_msgs/Float32MultiArray`): Target speed and steering angle forwarded to the CAN interface.

## Configuration Notes
- Update the ONNX model path in `rclpy/car/bramy/tracking.py` to match your deployment location.
- Adjust the OpenNI2 library directory in `rclpy/car/bramy/camera/astra_camera.py` if the default `/home/pi/Downloads/libs` is not valid.
- `get_control.py` clamps speed to `[-100, 100]` and steering to `[-90, 90]`. Tweak these limits to match your hardware envelope.
- The CAN interface defaults to `can0`. Set up SocketCAN (for example with `sudo ip link set can0 up type can bitrate 500000`) before launching.

## Running Tests
```bash
colcon test --packages-select bramy
```

## License
Distributed under the terms of the MIT License. See `LICENSE` for details.
