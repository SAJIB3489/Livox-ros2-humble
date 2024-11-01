# Livox ROS 2 Humble

This repository provides Dockerfiles and instructions to set up a development environment for Livox LiDAR sensors using ROS 2 Humble. The setup supports both AMD64 and ARM64 architectures, making it versatile for various hardware platforms, including standard PCs and ARM-based devices like Raspberry Pi.

## Overview

Livox LiDAR sensors are known for their high performance and cost-effectiveness, making them suitable for a wide range of applications, including autonomous driving, robotics, and mapping. Integrating these sensors with ROS 2 allows for advanced data processing, visualization, and control in robotic systems.

This repository includes:

- **Dockerfiles**: Pre-configured Dockerfiles for both AMD64 and ARM64 architectures to simplify the setup process.
- **Livox SDK**: The official Livox Software Development Kit for interfacing with Livox LiDAR sensors.
- **Livox ROS 2 Driver**: A ROS 2 package that provides drivers and utilities for using Livox LiDAR sensors within the ROS 2 ecosystem.


## Getting Started

To get started, follow the instructions provided in the respective `README.md` files within the `Dockerfile-amd64` and `Dockerfile-arm64` directories. These guides will walk you through the process of building and running the Docker images, as well as playing back rosbag files for testing and development purposes.

## References

- [Livox SDK](https://github.com/SAJIB3489/Livox-SDK.git)
- [Livox ROS2 Driver](https://github.com/Duna-System/livox_ros2_driver)
