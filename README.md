# ublox_f9r

This repository contains packages for ublox f9r.


## Quick ROS2 Install

Instant install script for ROS2 on various versions of Ubuntu (20.04 LTS)Linux

### Install ROS2 on Ubuntu


put the ublox_f9r folder from the repo in the src folder of workspace
```
cd <ros_workspace>
rosdep update
rosdep install --from-paths src --ignore-src -r -y
colcon build
source install/setup.bash
```

# Open other terminal for ntrip client

    ROS node that will communicate with an NTRIP server to receive RTCM connections and publish them on a ROS topic.

    ros2 launch ntrip_client ntrip_client_launch.py


    Published Topics

    rtcm (mavros_msgs/RTCM)
    
    RTCM corrections received from NTRIP server. Only messages whose checksums have passed validation are sent. The messages contain the raw RTCM bytes including the checksum



```

docker run -it --privileged --gpus all --net=host --ipc=host \
    -e "DISPLAY=$DISPLAY" \
    -e "QT_X11_NO_MITSHM=1" \
    -e NVIDIA_DRIVER_CAPABILITIES=all \
		--device /dev:/dev \
    -v "/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    mintcart:v1.0

