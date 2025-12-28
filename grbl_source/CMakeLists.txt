



add_subdirectory(eeprom)
add_subdirectory(fans)
add_subdirectory(grbl)
add_subdirectory(keypad)
add_subdirectory(laser)
# add_subdirectory(littlefs)
add_subdirectory(trinamic)
add_subdirectory(motors)
add_subdirectory(openpnp)
add_subdirectory(plasma)
add_subdirectory(plugins)
add_subdirectory(sdcard)
add_subdirectory(spindle)


target_include_directories(trinamic INTERFACE
    grbl/
)


target_link_libraries(grbl INTERFACE motors trinamic)
