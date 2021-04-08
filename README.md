# esp-idf 

Install it in linux not windows, set it up from a remote ubuntu 20.04 window

Tools and some includes) go in /home/tim/.espressif by default.
You can choose where to put esp-idf. I put it in /home/tim/esp 

Installing issues: You need venv in ubuntu20.04 which gives you virtualenv

        apt-get update
        apt-get upgrade
        /usr/bin/python3 -m pip --version
        sudo apt install python3-venv
        cmake --version
        ninja --version


vscode extension is here: but also in Extensions        
https://github.com/espressif/vscode-esp-idf-extension  

some help on errors here https://www.esp32.com/viewtopic.php?t=12481

it will download esp-idf choice of release candidates

## .vscode/c_cpp_properties.json

doesn't do shit for compiling or finding tools it just is used by visual code for squiggly error stuff. Making it work keeps fake problems from masking real problems

a typical file in /home/tim/esp/proj/blink_wifi looks like

    {
        "configurations": [
            {
                "name": "Linux",
                "includePath": [
                    "${workspaceFolder}/**",
                    "/home/tim/esp/esp-idf/**",
                    "${config:idf.espIdfPath}/components/",
                    "${config:idf.espIdfPathWin}/components/",
                    "/home/tim/.espressif/tools/riscv32-esp-elf/1.24.0.123_64eb9ff-8.4.0/riscv32-esp-elf/riscv32-esp-elf/include/"
                ],
                "defines": [],
                "compilerPath": "/usr/bin/gcc",
                "cStandard": "gnu17",
                "cppStandard": "gnu++14",
                "intelliSenseMode": "linux-gcc-x64",
                "configurationProvider": "ms-vscode.cmake-tools"
            }
        ],
        "version": 4
    }

## https://github.com/tonyp7/esp32-wifi-manager

brilliant work plus introduces components in a good way

### [esp-wifi-manager as a component](https://github.com/tonyp7/esp32-wifi-manager#adding-esp32-wifi-manager-to-your-code)

In order to use esp32-wifi-manager effectively in your esp-idf projects, copy the whole esp32-wifi-manager repository (or git clone) into a components subfolder.

A typical top level CmakeLists.txt file should look like this:

    cmake_minimum_required(VERSION 3.5)
    set(EXTRA_COMPONENT_DIRS components/)
    include($ENV{IDF_PATH}/tools/cmake/project.cmake)
    project(name_of_your_project)

NOT AS SIMPLE AS All you need to do now is to call wifi_manager_start()

Once this is done, you can now in your user code add the header:

    #include <esp_wifi.h>
    #include "wifi_manager.h"

    void cb_connection_ok(void *pvParameter){
        printf("Connected - Yaay\n");
    }

    void app_main(void){
	    wifi_manager_start();
        wifi_manager_set_callback(WM_EVENT_STA_GOT_IP, &cb_connection_ok);
        //do your thing
        gpio_reset_pin(BLINK_GPIO);
        gpio_set_direction(BLINK_GPIO, GPIO_MODE_OUTPUT);
        while(1) {
            gpio_set_level(BLINK_GPIO, 0);
            vTaskDelay(1000 / portTICK_PERIOD_MS);
            gpio_set_level(BLINK_GPIO, 1);
            vTaskDelay(1000 / portTICK_PERIOD_MS);
        }


