FROM ros:noetic-ros-core-focal

RUN apt-get update && apt-get install -y sudo

ARG USERNAME=user
ARG GROUPNAME=user
ARG UID=1000
ARG GID=1000
ARG PASSWORD=user
RUN groupadd -g $GID $GROUPNAME && \
    useradd -m -s /bin/bash -u $UID -g $GID -G sudo $USERNAME && \
    echo $USERNAME:$PASSWORD | chpasswd && \
    echo "$USERNAME   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER $USERNAME
WORKDIR /home/$USERNAME/

RUN sudo apt-get update && sudo apt-get install -y \
    wget curl \
    git build-essential clang\
    xorg-dev x11-apps \
    python3-vcstool \
    python3-pip \
    python3-rosdep \
    python3-catkin-tools \
    && sudo apt-get upgrade -y \
    && sudo rosdep init && rosdep update
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
RUN echo "if [ -f /home/$USERNAME/mujoco_ws/devel/setup.bash ]; then source /home/$USERNAME/mujoco_ws/devel/setup.bash; fi" >> ~/.bashrc