FROM osrf/ros:melodic-desktop-full-bionic

SHELL ["/bin/bash", "-c"]

# Install GTSAM (Georgia Tech Smoothing and Mapping library) 
RUN apt-get update \
    && apt install -y software-properties-common \
    && add-apt-repository ppa:borglab/gtsam-release-4.0 \
    && apt-get update \
    && apt install -y libgtsam-dev libgtsam-unstable-dev \
    && rm -rf /var/lib/apt/lists/*

# Install ceres solver
RUN apt-get update \
    && apt-get install -y libgoogle-glog-dev \
    && apt-get install -y libatlas-base-dev \ 
    && mkdir -p ~/Downloads/ \
    && cd ~/Downloads/ \
    && apt-get install wget \
    && apt-get install zip unzip \
    && wget -O ~/Downloads/ceres.zip https://github.com/ceres-solver/ceres-solver/archive/1.14.0.zip \
    && cd ~/Downloads/ \
    && \unzip ceres.zip -d ~/Downloads/ \ 
    && cd ~/Downloads/ceres-solver-1.14.0 \
    && mkdir ceres-bin && cd ceres-bin \
    && cmake .. \
    && sudo make install -j4

# RoboSense driver
RUN cd ~/Downloads/ \
    && apt install libpcap-dev libeigen3-dev libboost-dev libpcl-dev -y \
    && git clone https://github.com/RoboSense-LiDAR/rs_driver.git \
    && cd rs_driver \
    && mkdir build \
    && cd build \
    && cmake .. \
    && make -j4 \
    && make install

# Build local ROS packages
RUN mkdir -p ~/catkin_ws/src \
    && cd ~/catkin_ws/src \
    && git clone https://github.com/RoboSense-LiDAR/rslidar_sdk.git \
    && git clone https://github.com/RobustFieldAutonomyLab/LeGO-LOAM.git \
    && git clone https://github.com/HKUST-Aerial-Robotics/A-LOAM.git \
    
    # Correction error rslidar_sdk
    && cd ~/catkin_ws/src/rslidar_sdk/src \
    && git clone https://github.com/RoboSense-LiDAR/rs_driver.git \
    
    && cd ~/catkin_ws \
    && source /opt/ros/melodic/setup.bash \
    && catkin_make -j1

RUN echo "source /opt/ros/melodic/setup.bash" >> /root/.bashrc \
    && echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc

WORKDIR /root/catkin_ws
