# Network UPS Tools server

**Work in Progress** Docker image for Network UPS Tools server.

Issues:
- Still crashes on auto start. Circumvented by adjusting the entrypoint to an eternal sleep, and manually starting the docker-entrypoint script by attaching to the container
- Need to figure out how to use a upsmon user without password

## Usage

This image provides a complete UPS monitoring service (USB driver only).

Start the container:

```
version: "3.8"

services:
  nut-upsd:
    container_name: nut-upsd
    image: martijndierckx/nut-upsd:latest
    restart: unless-stopped
    ports:
      - "3493:3493"
    #environment:
      #- UPS_NAME=UPS
      #- UPS_DESC=APC BX1600MI
      #- API_USER=upsmon
      #- API_PASSWORD=
    volumes:
      - /dev/bus/usb/002:/dev/bus/usb/002
    privileged: true
```

## Auto configuration via environment variables

This image supports customization via environment variables.

### UPS_NAME

*Default value*: `ups`

The name of the UPS.

### UPS_DESC

*Default value*: `Eaton 5SC`

This allows you to set a brief description that upsd will provide to clients that ask for a list of connected equipment.

### UPS_DRIVER

*Default value*: `usbhid-ups`

This specifies which program will be monitoring this UPS.

### UPS_PORT

*Default vaue*: `auto`

This is the serial port where the UPS is connected.

### API_USER

*Default vaue*: `upsmon`

This is the username used for communication between upsmon and upsd processes.

### API_PASSWORD

*Default vaue*: `secret`

This is the password for the upsmon user.

### SHUTDOWN_CMD

*Default vaue*: `echo 'System shutdown not configured!'`

This is the command upsmon will run when the system needs to be brought down. The command will be run from inside the container.

