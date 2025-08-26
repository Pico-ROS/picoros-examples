idf_component_register(SRCS "wifi_connect.c" "main.c"
                    INCLUDE_DIRS
                        "."
                    REQUIRES
                        esp_wifi
                        nvs_flash
                        picoros
                    )

# custom types
idf_build_set_property(COMPILE_OPTIONS -DUSER_TYPE_FILE="my_types.h" APPEND)
idf_build_set_property(COMPILE_OPTIONS -I${CMAKE_CURRENT_SOURCE_DIR} APPEND )

# match rmw_zenoh default lease duration setting
idf_build_set_property(COMPILE_OPTIONS  -DZ_TRANSPORT_LEASE=60000 APPEND)
idf_build_set_property(COMPILE_OPTIONS  -DZ_TRANSPORT_LEASE_EXPIRE_FACTOR=2 APPEND)
