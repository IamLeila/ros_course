FROM ros:noetic

RUN apt-get update && \
    apt-get install -y \
        libusb-dev \
        libusb-1.0-0-dev \
        python3-pip \
        python3-catkin-tools \
        git \
        curl

RUN python3 -m pip install --upgrade odrive

RUN mkdir -p /root/ros_ws/src && \
    cd /root/ros_ws/src && \
    git clone https://github.com/YDLIDAR/ydlidar_ros && \
    cd ydlidar_ros && \
    git checkout master && \
    cd ../.. && \
    . /opt/ros/noetic/setup.sh && \
    catkin build

RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    apt-get install -y nodejs

RUN python3 -m pip install \
    paho-mqtt

RUN apt-get update && \
    apt-get install -y \
        ros-noetic-joy \
        ros-noetic-teleop-twist-joy \
        ros-noetic-twist-mux \
        ros-noetic-robot-localization \
        ros-noetic-gmapping \
        ros-noetic-move-base \
        ros-noetic-cv-bridge \
        ros-noetic-image-transport \
        ros-noetic-compressed-image-transport \
        ros-noetic-realsense2-camera \
        ros-noetic-imu-filter-madgwick \
        ros-noetic-xacro \
        ros-noetic-robot-state-publisher \
        ros-noetic-tf-conversions

COPY /web /root/web
RUN cd /root/web && \
    npm ci

COPY /ros/src /root/ros_ws/src
RUN . /opt/ros/noetic/setup.sh && \
    cd /root/ros_ws && \
    catkin build

COPY /docker/docker_entrypoint.bash /

ENTRYPOINT ["/docker_entrypoint.bash"]
