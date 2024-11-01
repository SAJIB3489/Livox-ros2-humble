# Livox ROS 2 Humble for aarch64 / arm64

I have test the following Dockerfile on Raspberry PI 5. The image built sucessfully. The Dockerfile contains following:
1. [ROS Humble](https://hub.docker.com/layers/arm64v8/ros/humble/images/sha256-b0b9bb05b0dceb08acc0e640c5b1c8a205da350369881d61b4ef42715ee42a63?context=explore) and packages.
2. [Livox SDK](https://github.com/SAJIB3489/Livox-SDK.git)
3. [Livox ros2 driver](https://github.com/SAJIB3489/livox_ros2_driver.git)

**Use the following steps to build the Docker Image without any error on the aaech64/ arm64 devices.**


```
git clone https://github.com/SAJIB3489/Livox-ros2-humble.git
cd ~/Dockerfile-arm64/
```

**Remove all unused images (If you have previous cache)**

```
docker system prune -a
```
**Build the Docker Image**

```
docker build --no-cache -t livox_ros2_humble .
```

It will takes some times while the Docker image size is around 3.1GB. After sucessfully build the image, go into the image.

```
docker run --rm  -it livox_ros2_humble:latest
```

> [!WARNING]
> You might get an warning but just ignore it.
> 
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
>


### Visulization using Livox avia lidar

#### Option 1: Raspberry PI with GUI 

1. Download the rosbag file [balcony_5th_floor_avia.bag](https://drive.google.com/file/d/1fD2oRkFPBf_JG3S95ZxVBLpNm6kcRxvw/view?usp=drive_link). It is pretty big file. 

2. Now run the following command on the terminal

```
docker run -it \
    --network=host \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /home/rosuser/Downloads/:/rosbag \
    livox_ros2_humble:latest
```

Replace ``/home/rosuser/Downloads/`` with the actual path to your rosbag file.

3. Inside the container, you can play the rosbag:

```
source /opt/ros/humble/setup.bash
source /ros2/install/setup.bash
cd /rosbag
ros2 bag play balcony_5th_floor_avia.bag
```

4. On your host PC (in a new terminal), run RViz2:

```
rviz2
```

In RViz:

Add a ``PointCloud2`` display
Set the topic to the Livox point cloud topic (typically ``/livox/lidar``)
Set the Fixed Frame to match your bag data (usually ``livox_frame`` or ``base_link``)

If you have permission issues with X11:
```
xhost +local:docker
```

#### Option 2: Raspberry PI without GUI

If you do not have GUI access for your Raspberry PI, then go to the Raspberry PI using **SSH**, for example. ``ssh -X rosuser@192.168.0.1``.

1. Open a terminal on Raspberry PI and Download the rosbag file using the following steps. Whereas we will use the terminal to download the rosbag file from google drive, we have to go through some additional steps:

```
pip install gdown
gdown https://drive.google.com/uc?id=1fD2oRkFPBf_JG3S95ZxVBLpNm6kcRxvw -O balcony_5th_floor_avia.bag
```
**OR** You can download the [rosbag file](https://drive.google.com/file/d/1fD2oRkFPBf_JG3S95ZxVBLpNm6kcRxvw/view?usp=drive_link) file on your host PC and then copy the file to the Raspberry PI.

```
scp -r balcony_5th_floor_avia.bag rosuser@192.168.0.1:/home/rosuser/Downloads/
```

Here replace ``rosuser@192.168.0.1`` with your Raspberry PI username and IP address. REplace the directory ``/home/rosuser/Downloads/`` with the actual directory of Raspberry PI.


2. Now run the following command on the Raspberry PI terminal

```
docker run -it \
    --network=host \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /home/rosuser/Downloads/:/rosbag \
    livox_ros2_humble:latest
```

Replace ``/home/rosuser/Downloads/`` with the actual path to your rosbag file.

3. Inside the container, you can play the rosbag:

```
source /opt/ros/humble/setup.bash
source /ros2/install/setup.bash
cd /rosbag
ros2 bag play balcony_5th_floor_avia.bag
```

4. On your host PC (in a new terminal), run RViz2:

```
rviz2
```

In RViz:

Add a ``PointCloud2`` display
Set the topic to the Livox point cloud topic (typically ``/livox/lidar``)
Set the Fixed Frame to match your bag data (usually ``livox_frame`` or ``base_link``)

If you have permission issues with X11:
```
xhost +local:docker
```