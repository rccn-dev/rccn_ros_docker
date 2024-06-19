## Build:
#  docker build --rm -f Dockerfile -t gramaziokohler/ros-um-kr60-planner .
#
# Usage:
#  docker pull gramaziokohler/ros-um-kr60-planner

FROM ros-noetic-moveit
LABEL maintainer "Chia Ching Yen <ccyen@umich.edu>"

SHELL ["/bin/bash","-c"]
ENV CATKIN_WS=/root/catkin_ws
RUN mkdir -p $CATKIN_WS/src

# Add robot packages to local catkin workspace
RUN echo "Building Raccoon West Robot Planner for ROS" \
    && source /opt/ros/${ROS_DISTRO}/setup.bash \
    # Update apt-get because its cache is always cleared after installs to keep image size down
    && apt-get update\
    # KUKA KR300 NCKU package
    # && git clone https://github.com/ros-industrial/kuka.git \
    # && git clone https://github.com/ros-industrial/kuka_experimental.git \
    && git clone https://github.com/rccn-dev/kuka_kr300_support.git \
    && git clone https://github.com/rccn-dev/rccn_kuka_robot_cell.git \
    && git clone https://github.com/rccn-dev/rccn_west_robot_moveit_config.git \
    # Install dependencies
    && cd $CATKIN_WS \
    && rosdep install -y --from-paths . --ignore-src --rosdistro ${ROS_DISTRO} \
    # Build catkin workspace
    && catkin_make \
    # Clear apt-cache to reduce image size
    && rm -rf /var/lib/apt/lists/*
