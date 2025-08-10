# Use the ROS 2 Jazzy desktop image as the base
FROM osrf/ros:jazzy-desktop

# Update and install necessary dependencies
RUN apt-get update && \
    apt-get install -y \
        ros-jazzy-rmw-zenoh-cpp

# Set the environment variable for RMW implementation
ENV RMW_IMPLEMENTATION=rmw_zenoh_cpp

# Source setup scripts globally
RUN echo 'source /opt/ros/jazzy/setup.bash' >> /etc/bash.bashrc

# Expose the Zenoh router port
EXPOSE 7447/udp
EXPOSE 7447/tcp