# Zenoh-Pico config.h Customization

This project requires the following changes in `components/picoros/picoros/thirdparty/zenoh-pico/include/zenoh-pico/config.h`:

- `#define Z_TRANSPORT_LEASE 60000`
- `#define Z_TRANSPORT_LEASE_EXPIRE_FACTOR 3`

Make sure these values are set as above. If you update or re-clone the submodule, reapply these changes.
# Launching teleop_twist_joy Node

To control your robot using a joystick launch the `teleop_twist_joy` node with the command:

```bash
ros2 run teleop_twist_joy teleop_node --ros-args --remap cmd_vel:=mcb/cmd_vel -p require_enable_button:=false -p axis_linear.x:=0 -p axis_angular.yaw:=1
```

This command remaps the `cmd_vel` topic and sets the required parameters for joystick operation.
# ESP32 picoros joystick example
- Uses [picoros IDF component](https://github.com/Pico-ROS/picoros-espidf-component) for zenoh and ROS2 support
- Reads 2 axis analogue joystick. Example [KY-023](https://www.amazon.com/Joystick-Sensor-Module-Controller-KY-023/dp/B08681VNJ4).
- Publishes `sensor_msgs/msg/Joy` message on ROS topic

## Building

Use ESP-IDF vscode extension to open & compile & flash the project.

## Configuration

In main.c:

```c
// Wifi Configuration
#define WIFI_SSID                   "mySSID"
#define WIFI_PASS                   "password"
#define WIFI_MAXIMUM_RETRY          5

// Joystick config
#define ADC1_CH_X                   ADC1_CHANNEL_3
#define ADC1_CH_Y                   ADC1_CHANNEL_4
#define DEAD_BAND_PERCENT           5
#define UPDATE_PERIOD_MS            50

// Pico-ROS config
#define TOPIC_NAME                  "picoros/joystick"
#define MODE                        "client"
#define ROUTER_ADDRESS              "tcp/192.168.0.246:7447"

```

In zenoh-pico `config.h` set:
- `Z_TRANSPORT_LEASE=60000`
- `Z_TRANSPORT_LEASE_EXPIRE_FACTOR=2`
To match rmw_zenoh default settings.

## Testing
```sh
# build and run docker container
docker build -t local/ros2-jazzy-zenoh test/
docker run -it --name ros2-jazzy-zenoh --net host local/ros2-jazzy-zenoh
# run in first terminal
ros2 run rmw_zenoh_cpp rmw_zenohd

# second terminal
docker exec -it ros2-jazzy-zenoh /bin/bash
```
