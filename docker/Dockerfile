FROM ros:humble-perception

ENV WS_DIR="/ros2_ws"
WORKDIR ${WS_DIR}

SHELL ["/bin/bash", "-c"]
ARG DEBIAN_FRONTEND=noninteractive

# Base build tools
RUN apt-get update -y \
 && apt-get install -y \
    build-essential \
    cmake \
    git-all \
    software-properties-common \
    apt-transport-https \
    curl \
    gnupg2 \
    lsb-release

# Add Intel RealSense APT repo
RUN curl -s https://librealsense.intel.com/Debian/librealsense.pgp | apt-key add - \
 && add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main"

# Install librealsense2 core + dev packages from Intel repo
RUN apt-get update -y \
 && apt-get install -y \
    librealsense2-utils \
    librealsense2-dev \
    librealsense2-dbg

# ROS deps + realsense-ros source
RUN apt-get install -y python3-rosdep \
 && mkdir src \
 && cd src \
 && git clone https://github.com/IntelRealSense/realsense-ros.git -b ros2-development \
 && cd .. \
 && rm /etc/ros/rosdep/sources.list.d/20-default.list \
 && rosdep init \
 && rosdep update \
 && rosdep install -i --from-path src --rosdistro ${ROS_DISTRO} --skip-keys=librealsense2 -y

# Build
RUN source /opt/ros/${ROS_DISTRO}/setup.bash && colcon build

ARG DEBIAN_FRONTEND=dialog

