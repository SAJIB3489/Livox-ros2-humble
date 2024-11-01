# Livox ROS 2 Humble for x86_64 / amd64


I have test the following Dockerfile on amd64 PC. The image built sucessfully. The Dockerfile contains following:
1. [ROS Humble](https://hub.docker.com/layers/arm64v8/ros/humble/images/sha256-b0b9bb05b0dceb08acc0e640c5b1c8a205da350369881d61b4ef42715ee42a63?context=explore) and packages.
2. [Livox SDK](https://github.com/SAJIB3489/Livox-SDK.git)
3. [Livox ros2 driver](https://github.com/SAJIB3489/livox_ros2_driver.git)

**Use the following steps to build the Docker Image without any error on the x86_64 / amd64 devices.**


```
git clone https://github.com/SAJIB3489/Livox-ros2-humble.git
cd ~/Dockerfile-amd64/
```

**Remove all unused images (If you have previous cache)**

```
docker system prune -a
```
**Build the Docker Image**

```
docker build --no-cache -t livox_ros2_humble .
```

> [!WARNING]
> You might get an warning, just ignore it. Something like this.

```
--- stderr: livox_ros2_driver
CMake Warning (dev) at CMakeLists.txt:31 (find_package):
  Policy CMP0074 is not set: find_package uses <PackageName>_ROOT variables.
  Run "cmake --help-policy CMP0074" for policy details.  Use the cmake_policy
  command to set the policy and suppress this warning.

  CMake variable PCL_ROOT is set to:

    /usr

  For compatibility, CMake is ignoring the variable.
This warning is for project developers.  Use -Wno-dev to suppress it.

/usr/include/apr-1.0
apr-1
---
```


After sucessfully build the image, go into the image. 

```
docker run --rm  -it livox_ros2_humble:latest
```

### Play rosbag file (Optional)


1. Rrun the following command on the terminal

```
docker ps
xhost +local:docker


docker run --name livox_ros2 -it --rm --privileged -e DISPLAY=$DISPLAY --net=host -v $XSOCK -v $XAUTH --volume="$HOME/.Xauthority:/root/.Xauthority:rw" \
-v ~/Music/github/Github/Livox-ros2-humble/bagfiles:/ros2/src/bagfiles/ -v /tmp/.X11-unix/:/tmp/.X11-Unix  livox_ros2_humble  bash
```


Replace ``~/Music/github/Github/Livox-ros2-humble/bagfiles:/ros2/src/bagfiles/`` with the actual path to your rosbag file.

2. Inside the container, you can play the rosbag:

```
source /opt/ros/humble/setup.bash
source /ros2/install/setup.bash
cd /src/bagfiles/
ros2 bag play <rosbag file> --clock
```

4. Go to another newly open terminal and look the rostopic:

```
  ros2 topic list #Shows all the published rostopics
  ros2 topic echo <topicname> #shows data of specific rostopic <topicname>
```

**Thank you. If you have any error please create an issue.**

## References

- [Livox SDK](https://github.com/SAJIB3489/Livox-SDK.git)
- [Livox ROS2 Driver](https://github.com/Duna-System/livox_ros2_driver)
