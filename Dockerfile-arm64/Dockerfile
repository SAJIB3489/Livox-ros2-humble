# Use the base ROS2 Humble image for ARM64
FROM arm64v8/ros:humble

# Install required build dependencies
RUN apt update && apt install -y \
    git \
    cmake \
    crossbuild-essential-arm64 \
    python3-colcon-common-extensions \
    libeigen3-dev \
    libpcl-dev \
    libssl-dev \
    libcurl4-openssl-dev \
    libpcl-dev ros-humble-pcl-ros \
    ros-humble-ament-cmake \
    ros-humble-rviz2 && \
    rm -rf /var/lib/apt/lists/*

# Link Volume
VOLUME /home/geoteq/ros2

# Create a ROS2 workspace
RUN mkdir -p /ros2/src

# Set the working directory
WORKDIR /ros2

# Clone the Livox SDK
RUN git clone https://github.com/SAJIB3489/Livox-SDK.git src/Livox-SDK

# Build and install Livox SDK for ARM with -fPIC flag
RUN cd src/Livox-SDK && \
    mkdir -p build && \
    cd build && \
    cmake .. -DCMAKE_SYSTEM_NAME=Linux \
             -DCMAKE_SYSTEM_PROCESSOR=arm64 \
             -DCMAKE_C_COMPILER=aarch64-linux-gnu-gcc \
             -DCMAKE_CXX_COMPILER=aarch64-linux-gnu-g++ \
             -DCMAKE_POSITION_INDEPENDENT_CODE=ON \
             -DCMAKE_CXX_FLAGS="-fPIC" \
             -DCMAKE_C_FLAGS="-fPIC" && \
    make && \
    make install

# Clone the Livox ROS2 Driver
RUN git clone https://github.com/SAJIB3489/livox_ros2_driver.git src/livox_ros2_driver

# Install ROS dependencies
RUN apt update && \
    rosdep update && \
    rosdep install --from-paths src --ignore-src -r -y

# Build the Livox ROS2 driver for ARM64
RUN /bin/bash -c "source /opt/ros/humble/setup.bash && \
    export CMAKE_PREFIX_PATH=/opt/ros/humble && \
    colcon build \
    --cmake-args \
    -DCMAKE_SYSTEM_NAME=Linux \
    -DCMAKE_SYSTEM_PROCESSOR=arm64 \
    -DCMAKE_C_COMPILER=aarch64-linux-gnu-gcc \
    -DCMAKE_CXX_COMPILER=aarch64-linux-gnu-g++ \
    -DCMAKE_POSITION_INDEPENDENT_CODE=ON"

# Source the workspace
RUN /bin/bash -c "source /opt/ros/humble/setup.bash && source install/setup.bash"

# Set entrypoint to run ROS2 commands directly
ENTRYPOINT ["/bin/bash", "-c", "source /opt/ros/humble/setup.bash && source ./install/setup.bash && exec bash"]