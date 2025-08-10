# ESP32 picoros joystick example
- Uses [picoros IDF component](https://github.com/Pico-ROS/picoros-espidf-component) for zenoh and ROS2 support
- Reads 2 axis analogue joystick (TODO)
- Publishes on cmd_vel topic

## Building
```
git clone <this_repository>
git submodule update --init recursive 
```

Use ESP-IDF vscode extension to open & compile & flash the project.


## Testing
```sh
# build and run docker container
docker build -t local/ros2-jazzy-zenoh test/
docker run -it --name ros2-jazzy-zenoh -p 7447:7447/tcp -p 8000:8000/tcp local/ros2-jazzy-zenoh
# run in first terminal
ros2 run rmw_zenoh_cpp rmw_zenohd

# second terminal
docker exec -it ros2-jazzy-zenoh /bin/bash
```