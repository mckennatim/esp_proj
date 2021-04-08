# Blink Example not 5, 2 on devkit1

sdkconfig has an ExampleConfiguration section that takes values fro sdkconfig.defaults and puts them in sdkconfig, but after that changing defaults does nothing.

Starts a FreeRTOS task to blink an LED

See the README.md file in the upper level 'examples' directory for more information about examples.

https://circuits4you.com/2018/02/02/esp32-led-blink-example/

I just got blink to work on esp32 using esp-idf instead of arduino toolchain. Supposedly better since that is what the tensorflow people used when they ported to esp32. I am running using the esp-idf extension on vscode remote  win 10 wsl2 running ubuntu 20.04. Hope the couple of hours to get it working was worth it. I am used to sending data to a server from esp8266 using mqtt and maybe I can get the esp32 to build little data packets of short sound samples and send them to edge impulse as an https post.