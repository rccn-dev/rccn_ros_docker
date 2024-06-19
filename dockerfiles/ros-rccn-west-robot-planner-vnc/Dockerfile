FROM ros-rccn-west-robot-planner
LABEL maintainer "Chia Ching Yen <ccyen@umich.edu>"

EXPOSE 5900 6099 11311
ENV \
  DEBIAN_FRONTEND="nonintractive" \
  X11VNC_PASSWORD="password"
RUN apt-get update -y
RUN apt-get install -y xvfb x11vnc fluxbox gnome-terminal --fix-missing
ADD ./entrypoint.sh /opt/entrypoint.sh
ENTRYPOINT /opt/entrypoint.sh