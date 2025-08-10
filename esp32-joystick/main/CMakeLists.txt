idf_component_register(SRCS "main.c"
                    INCLUDE_DIRS
                        "."
                    REQUIRES
                        esp_wifi
                        nvs_flash
                        picoros
                    )

idf_build_set_property(COMPILE_OPTIONS -DUSER_TYPE_FILE="my_types.h" APPEND)
idf_build_set_property(COMPILE_OPTIONS -I${CMAKE_CURRENT_SOURCE_DIR} APPEND)