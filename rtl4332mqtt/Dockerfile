# Docker file to create an image for a hass.io add-on that contains enough software to listen to events via RTL_SDR/RTL_433 and then publish them to a MQTT broker.
# The script resides in a volume and can be modified to meet your needs.
# This hass.io addon is based on Chris Kacerguis' project here: https://github.com/chriskacerguis/honeywell2mqtt,
# which is in turn based on Marco Verleun's rtl2mqtt image here: https://github.com/roflmao/rtl2mqtt

# IMPORTANT: The container needs privileged access to /dev/bus/usb on the host.

ARG BUILD_FROM=hertzg/rtl_433:alpine-3.12-18.12
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

MAINTAINER Mark Parker

LABEL Description="This image is used to start a script that will monitor for RF events on 433Mhz (configurable) and send the data to an MQTT server"

# Environment variables
ENV \
    HOME="/root" \
    LANG="C.UTF-8" \
    PS1="$(whoami)@$(hostname):$(pwd)$ " \
    TERM="xterm-256color"

RUN apk add --no-cache mosquitto-clients jq  
ADD rtl2mqtt.sh /rtl2mqtt.sh
RUN chmod +x /rtl2mqtt.sh
#
# Define an environment variable
#
# Use this variable when creating a container to specify the MQTT broker host.
ENV MQTT_HOST="hassio.local"
ENV MQTT_USER="guest"
ENV MQTT_PASS="guest"
ENV MQTT_TOPIC="homeassistant/sensor/rtl433"

#CMD cd / && cp rtl2mqtt.sh /rtl2mqtt.sh && chmod +x /rtl2mqtt.sh
#CMD cd / && cp /config/rtl4332mqtt/rtl2mqtt.sh /rtl2mqtt.sh && chmod +x /rtl2mqtt.sh && /rtl2mqtt.sh
ENTRYPOINT ["/rtl2mqtt.sh"]
