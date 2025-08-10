# ESP32 picoros joystick example
- Uses [picoros IDF component](https://github.com/Pico-ROS/picoros-espidf-component) for zenoh and ROS2 support
- Reads 2 axis analogue joystick.
- Publishes Twist message on ROS topic

## Building
```
git clone <this_repository>
git submodule update --init recursive 
```

Use ESP-IDF vscode extension to open & compile & flash the project.

## Configuration

In main.c:

```c
#define TOPIC_NAME                  "joystick/twist"
#define ADC1_CH_X                   ADC1_CHANNEL_3
#define ADC1_CH_Y                   ADC1_CHANNEL_4
#define UPDATE_PERIOD_MS            200
#define DEAD_BAND_PERCENT           5
#define FULL_SCALE_LINEAR           1.5 // m/s
#define FULL_SCALE_ANGULAR          2.5 // rad/s
#define EXAMPLE_ESP_WIFI_SSID       CONFIG_ESP_WIFI_SSID
#define EXAMPLE_ESP_WIFI_PASS       CONFIG_ESP_WIFI_PASSWORD
#define MODE                        "client"
#define LOCATOR                     "tcp/192.168.1.10:7447"
#define EXAMPLE_ESP_MAXIMUM_RETRY   CONFIG_ESP_MAXIMUM_RETRY
```

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
