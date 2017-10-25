# rpi-rtl433

[![Build Status](https://travis-ci.org/sofwerx/rpi-rtl433.svg)](https://travis-ci.org/sofwerx/rpi-rtl433)

This is the Dockerfile behind [sofwerx/rpi-rtl433](https://hub.docker.com/r/sofwerx/rpi-rtl433/) on Docker Hub, setup to autobuild through travis.

Note: the base image of the Dockerfile is `FROM multiarch/debian-debootstrap:armhf-jessie`, which is to allow the x86_64 travis-ci servers to run qemu when building the ARM contents of this resultant image.

This is a raspberry-pi docker container to acquire Tire Pressure Management System (TPMS) data from an rtl433 and transmit to MQTT.

The parent project to this is [sofwerx/rpi-tpms](https://github.com/sofwerx/rpi-tpms). There you will find the docker-compose that uses this container.

Here is the docker-compose snippet of this container image in use:

  rtl433:
    image: sofwerx/rpi-rtl433:latest
    container_name: rtl433
    hostname: rtl433
    restart: always
    privileged: true
    links:
      - mqtt
    devices:
      - "/dev/bus/usb"
    command: rtl_433 -F json -U | mosquitto_pub -h mqtt -p 1883 -t home/rtl_433 -l
    depends_on:
      - multiarch
      - mqtt
    logging:
      driver: json-file
      options:
        max-size: "20k"


