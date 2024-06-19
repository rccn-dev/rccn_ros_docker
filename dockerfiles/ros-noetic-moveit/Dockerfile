# Build:
#  docker build --rm -f Dockerfile -t gramaziokohler/ros-moveit .
#
# Usage:
#  docker pull gramaziokohler/ros-moveit

FROM ros-noetic-base
LABEL maintainer "Chia Ching Yen <ccyen@umich.edu>"

SHELL ["/bin/bash","-c"]

# Install packages
RUN apt-get update && apt-get install -y \
    # ROS MoveIt
    ros-${ROS_DISTRO}-moveit \
    ros-${ROS_DISTRO}-moveit-resources-prbt-moveit-config \
    --no-install-recommends \
    --fix-missing \
    # Clear apt-cache to reduce image size
    && rm -rf /var/lib/apt/lists/*