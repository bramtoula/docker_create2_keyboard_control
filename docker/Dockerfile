FROM ros:melodic-ros-core

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Install catkin tools
RUN apt-get update && \
    apt-get install -y wget && \
    sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list' && \
    wget http://packages.ros.org/ros.key -O - | apt-key add - && \
    apt-get install -y python-rosdep python-catkin-tools && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get remove -y wget

# Install ROS packages
RUN source /ros_entrypoint.sh && \
    apt-get update && \ 
    apt-get install -y ros-melodic-tf2-tools ros-melodic-tf ros-melodic-joy ros-melodic-joy-teleop ros-melodic-roslint ros-melodic-xacro ros-melodic-robot-state-publisher ros-melodic-key-teleop && \
    rm -rf /var/lib/apt/lists/*

# Install libcreate
RUN apt-get update && \
    apt-get install -y build-essential cmake libboost-system-dev libboost-thread-dev && \
    cd ~ && \
    git clone https://github.com/AutonomyLab/libcreate.git && \
    cd libcreate && \
    mkdir build && cd build && \
    cmake .. && \
    make -j && \
    make install && \
    rm -rf /var/lib/apt/lists/*

# Make workspace
RUN source /ros_entrypoint.sh && \
    mkdir -p ~/roomba_ws/src && \
    cd ~/roomba_ws/src  && \
    git clone https://github.com/AutonomyLab/create_autonomy.git && \
    cd .. && \
    catkin build && \
    echo "source ~/roomba_ws/devel/setup.bash" >> ~/.bashrc && \
    source ~/.bashrc && \
    usermod -a -G dialout root

# Install vim
# RUN apt-get install -y vim

# Run drivers for the roomba
RUN echo "#!/bin/bash" > ~/launch.sh && \
    echo "source ~/roomba_ws/devel/setup.bash" >> ~/launch.sh && \
    echo "roslaunch ca_driver create_2.launch &" >> ~/launch.sh && \
    echo "rosrun key_teleop key_teleop.py key_vel:=/cmd_vel" >> ~/launch.sh && \
    chmod +x ~/launch.sh

ENTRYPOINT bash ~/launch.sh
