# Description: This Dockerfile creates a ROS Noetic base image with the following packages:
# 1. ros-noetic-rosbridge-server
# 2. ros-noetic-moveit
# 3. ros-noetic-moveit-resources-prbt-moveit-config

# Build:
#  docker buildx build --rm -f Dockerfile -t rccn-ros-noetic-base . 

FROM ros:noetic
LABEL maintainer "Chia-Ching Yen <ccyen@umich.edu>"

SHELL ["/bin/bash","-c"]

# All apt-get commands start with an update, then install
# and finally, a cache cleanup to keep the image size small.

# Install packages
RUN apt-get update \
    && apt-get install -y \
    # Basic utilities
    git \
    net-tools \ 
    iputils-ping \
    dnsutils \
    # ROS catkin tools
    python3-catkin-tools \
    ros-${ROS_DISTRO}-catkin \
    # ROS bridge server and related packages
    ros-${ROS_DISTRO}-rosbridge-server \
    # ROS MoveIt
    ros-${ROS_DISTRO}-moveit \
    ros-${ROS_DISTRO}-moveit-resources-prbt-moveit-config \
    # ros-${ROS_DISTRO}-tf2-web-republisher   # NOT yet published?   \
    --fix-missing \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Create local catkin workspace
ENV CATKIN_WS=/root/catkin_ws
RUN mkdir -p $CATKIN_WS/src
WORKDIR $CATKIN_WS/src

# Initialize local catkin workspace
RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
    # && apt-get update  # Seems like this is not necessary \
    # ROS File Server
    && git clone https://github.com/gramaziokohler/ros_file_server.git \
    # Install dependencies
    && cd $CATKIN_WS \
    && rosdep install -y --from-paths . --ignore-src --rosdistro ${ROS_DISTRO} \
    # Build catkin workspace
    && catkin build

# Always source ros_catkin_entrypoint.sh when launching bash (e.g. when attaching to container)
RUN echo "source /usr/local/bin/ros_catkin_entrypoint.sh" >> /root/.bashrc

COPY roscore_check.py /usr/local/bin/roscore_check.py
COPY ros_catkin_entrypoint.sh /usr/local/bin/ros_catkin_entrypoint.sh

RUN chmod +x /usr/local/bin/roscore_check.py
RUN chmod +x /usr/local/bin/ros_catkin_entrypoint.sh

ENTRYPOINT ["/usr/local/bin/ros_catkin_entrypoint.sh"]
CMD ["bash"]