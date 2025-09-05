# ESP32 picoros joystick example
- Uses [picoros IDF component](https://github.com/Pico-ROS/picoros-espidf-component) for zenoh and ROS2 support
- Reads 2 axis analogue joystick. Example [KY-023](https://www.amazon.com/Joystick-Sensor-Module-Controller-KY-023/dp/B08681VNJ4).
- Publishes `sensor_msgs/msg/Joy` message on ROS topic

## Building

Use ESP-IDF vscode extension to open & compile & flash the project.

### Zenoh-Pico config.h Customization

This project requires the following changes in `components/picoros/picoros/thirdparty/zenoh-pico/include/zenoh-pico/config.h`:

- `#define Z_TRANSPORT_LEASE 60000`
- `#define Z_TRANSPORT_LEASE_EXPIRE_FACTOR 2`

This is to support rmw_zenoh default configuration.

### Project Configuration

In `main/main.c`:

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

### Using Serial port
Connection to zenohd can be done through serial port. On **ESP32-DevKit-C1** there are two usb connectors. The one marked with **UART** is connected to `UART_NUM_0` on esp32. The one marked **USB** is connected to esp32 USB-CDC pins. 

If you want to use serial connection with USB cable and on-board serial to USB converter, the only option is to use connector marked **UART**. 

Unfortunately, by default, esp32 has console output on this port.

To make it work you need to:
- Set `#define USE_SERIAL 1` in main.c
- Set `Z_FEATURE_LINK_SERIAL 1` in zenoh-pico config.h
- Set sdkconfig [settings](docs/DevKIT-C1-Serial.png) to move console output to USB-CDC port
- In zenoh-pico/src/system/espidf/network.c - `_z_open_serial_from_dev` change `tx_pin = 43`, `rx_pin = 44`

After setting all this up you can flash and get console output through port marked **USB**. Port marked **UART** is used for communication with the zenohd router.


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

### Launching teleop_twist_joy Node

To control your robot using a joystick launch the `teleop_twist_joy` node with the command:

```bash
ros2 run teleop_twist_joy teleop_node --ros-args --remap cmd_vel:=mcb/cmd_vel -p require_enable_button:=false -p axis_linear.x:=0 -p axis_angular.yaw:=1
```

This command remaps the `cmd_vel` topic and sets the required parameters for joystick operation.
