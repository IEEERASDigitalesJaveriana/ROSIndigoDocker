# This is an auto generated Dockerfile for ros:desktop-full
# generated from docker_images/create_ros_image.Dockerfile.em
FROM ubuntu:trusty

ENV DEBIAN_FRONTEND=noninteractive 
ENV DEBIAN_FRONTEND teletype

RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        ca-certificates \
        build-essential \
        g++ \
        libxinerama-dev \
        libxext-dev \
        libxrandr-dev \
        libxi-dev \
        libxcursor-dev \
        libxxf86vm-dev && \
    rm -rf /var/lib/apt/lists/*

# Requirements
# BEGIN INSTALL packages
RUN apt-get update
RUN apt-get install -q -y --no-install-recommends     dirmngr     gnupg2     && rm -rf /var/lib/apt/lists/*
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 4B63CF8FDE49746E98FA01DDAD19BAB3CBF125EA
RUN echo "deb http://snapshots.ros.org/indigo/final/ubuntu trusty main" > /etc/apt/sources.list.d/ros1-snapshots.list

# python tools
RUN apt-get update && apt-get install --no-install-recommends -y   python-rosdep    
RUN apt-get update && apt-get install --no-install-recommends -y   python-rosinstall 
RUN apt-get update && apt-get install --no-install-recommends -y   python-vcstools   

ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8


ENV ROS_DISTRO=indigo
RUN apt-get update && apt-get install -y     ros-indigo-ros-core=1.1.6-0*    

ENTRYPOINT ["/ros_entrypoint.sh"]

RUN apt-get update && apt-get install -y     ros-indigo-ros-base=1.1.6-0*   
RUN apt-get update && apt-get install -y     ros-indigo-robot=1.1.6-0*    
RUN apt-get update && apt-get install -y     ros-indigo-desktop=1.1.6-0*   
RUN apt-get update && apt-get install -y     ros-indigo-desktop-full=1.1.6-0*    
# install ros nao
RUN apt-get install -y ros-indigo-driver-base ros-indigo-move-base-msgs ros-indigo-octomap
RUN apt-get install -y ros-indigo-octomap-msgs ros-indigo-humanoid-msgs ros-indigo-humanoid-nav-msgs
RUN apt-get install -y  ros-indigo-camera-info-manager ros-indigo-camera-info-manager-py
RUN apt-get install -y ros-.*-nao-robot
RUN apt-get install -y ros-.*-nao-extras


#END INSTALL PACKAGES
RUN rm -rf /var/lib/apt/lists/*

# Setup workspace ROS
RUN /bin/bash -c "source /opt/ros/$ROS_DISTRO/setup.bash"
RUN rosdep init && rosdep update --rosdistro $ROS_DISTRO

# Add user
RUN adduser --quiet --disabled-password qtuser && usermod -a -G audio qtuser

# Setup the shell
# Edit /root/.bashrc
RUN echo "\n# Set ROS Indigo" >> /root/.bashrc
RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> /root/.bashrc
RUN echo "\n# Set ROS Network" >> /root/.bashrc
RUN echo "export ROS_HOSTNAME=localhost" >> /root/.bashrc
RUN echo "export ROS_MASTER_URI=http://localhost:11311" >> /root/.bashrc

#RUN echo "export GAZEBO_MODEL_PATH=/usr/share/gazebo-2.2/worlds" >> /root/.bashrc

RUN sed "2d" -i /usr/share/gazebo-2.2/setup.sh
RUN sed "2d" -i /usr/share/gazebo/setup.sh


#RUN echo "export GAZEBO_MODEL_DATABASE_URI=http://models.gazebosim.org" >> /usr/share/gazebo-2.2/setup.sh
#RUN echo "export GAZEBO_MODEL_DATABASE_URI=http://models.gazebosim.org" >> /usr/share/gazebo/setup.sh

RUN echo "export GAZEBO_MODEL_DATABASE_URI=""" >> /usr/share/gazebo-2.2/setup.sh
RUN echo "export GAZEBO_MODEL_DATABASE_URI=""" >> /usr/share/gazebo/setup.sh

RUN echo "source /usr/share/gazebo-2.2/setup.sh" >> /root/.bashrc
RUN echo "source /usr/share/gazebo/setup.sh" >> /root/.bashrc

RUN echo "source /workspaces/ROSIndigoDocker/Nao/devel/setup.bash" >> /root/.bashrc
RUN echo  "export GAZEBO_MODEL_PATH=/workspaces/ROSIndigoDocker/ModelsGazebo" >> /root/.bashrc

# Nao Packages
RUN apt-get update
RUN yes |  apt-get install -y  ros-indigo-nao-meshes
RUN apt-get install -y  ros-indigo-ros-control ros-indigo-ros-controllers ros-indigo-gazebo-ros-control ros-indigo-gazebo-ros-pkgs
RUN apt-get install -y ros-indigo-effort-controllers ros-indigo-joint-state-controller ros-indigo-joint-trajectory-controller ros-indigo-gazebo-plugins
RUN apt-get install -y  libdc1394-22-dev libdc1394-22 libdc1394-utils
RUN echo  "ln /dev/null /dev/raw1394" >> /root/.bashrc

# Pepper Package
RUN apt-get install -y ros-indigo-driver-base ros-indigo-move-base-msgs ros-indigo-octomap ros-indigo-octomap-msgs ros-indigo-humanoid-msgs ros-indigo-humanoid-nav-msgs ros-indigo-camera-info-manager ros-indigo-camera-info-manager-py
RUN yes | apt-get install -y ros-indigo-pepper-.*
RUN apt-get install -y ros-indigo-moveit-*

#END INSTALL PACKAGES
RUN rm -rf /var/lib/apt/lists/*

RUN groupadd --gid 5000 Sebastian \
	&& useradd  --home-dir /home/Sebastian --create-home --uid 5000 \
	--gid 5000 --shell /bin/sh --skel /dev/null Sebastian

RUN echo "Sebastian:Sebastian" | chpasswd

RUN usermod -aG sudo Sebastian

RUN chsh -s /bin/bash Sebastian

USER Sebastian

CMD ["/bin/bash"]