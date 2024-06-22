## Build:
#  docker buildx build --rm -f Dockerfile -t rccn-ros-kuka-cv . 

FROM rccn-ros-noetic-base
LABEL maintainer "Chia Ching Yen <ccyen@umich.edu>"

SHELL ["/bin/bash","-c"]
ENV CATKIN_WS=/root/catkin_ws
RUN mkdir -p $CATKIN_WS/src

# Add robot packages to local catkin workspace
RUN echo "Building Raccoon West Robot Computer Vision for ROS" \
    && source /opt/ros/${ROS_DISTRO}/setup.bash \
    # Update apt-get because its cache is always cleared after installs to keep image size down
    && apt-get update \
    && apt-get install -y \
    # rqt utilities
    ros-${ROS_DISTRO}-rqt-* \
    # KUKA KR300 NCKU package
    # && git clone https://github.com/ros-industrial/kuka.git \
    # && git clone https://github.com/ros-industrial/kuka_experimental.git \
    && git clone https://github.com/rccn-dev/kuka_kr300_support.git \
    && git clone https://github.com/rccn-dev/rccn_robot_cell \
    && git clone https://github.com/rccn-dev/compas_fab_backend.git \
    && git clone https://github.com/rccn-dev/kuka_robot_driver_interfaces.git \

    # CV packages
    && git clone https://github.com/ros-planning/moveit_calibration.git \
    && git clone -b noetic-devel https://github.com/introlab/rtabmap_ros.git \
    # Install dependencies
    && cd $CATKIN_WS \
    && rosdep install -y --from-paths . --ignore-src --rosdistro ${ROS_DISTRO} \
    # Build catkin workspace
    && catkin build \
    # Clear apt-cache to reduce image size
    && rm -rf /var/lib/apt/lists/*
