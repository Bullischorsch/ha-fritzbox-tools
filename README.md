# FRITZ!Box Tools

<a href="https://www.buymeacoffee.com/mammuth" target="_blank"><img src="https://bmc-cdn.nyc3.digitaloceanspaces.com/BMC-button-images/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>

Custom component for Home Assistant to control your FRITZ!Box

**Features:**

- Turn on/off guest wifi
- Reconnect your FRITZ!Box / get new IP from provider
- Manage port forwardings for your HomeAssistant device
- Sensor for internet connectivity (with external IP and uptime attributes)

![image](https://user-images.githubusercontent.com/3121306/64920971-d42cb000-d7bd-11e9-8bdf-a21c7ea93c58.png)


## Installation

**Install via HACS**

The custom component is available via [HACS](https://github.com/custom-components/hacs)

**Manual Install**

If you want to install the custom commponent manually, add the folder `fritzbox_tools/` to `YOUR_CONFIG_DIR/custom_components/`.

## Configuration

`configuration.yml`:
```yaml
fritzbox_tools:
  host: "192.168.178.1"
  username: "home-assistant"
  password: "yourfritzboxpassword"
  homeassistant_ip: "192.168.178.42"  # Optional. Needed if you want to control port forwardings for the device running HomeAssistant
```

The custom component registers a switch for controlling the guest wifi and a service for triggering a reconnect. I use the following automation to send the guest wifi password to my wife's and my phones whenever we turn on the guest wifi:
```yaml
automation:
  - alias: "Guests Wifi Turned On -> Send Password To Phone
    trigger:
      platform: state
      entity_id: switch.fritz_box_guest_wifi
      to: 'on'
    action:
      - service: notify.pushbullet_max
        data:
          title: "Guest wifi is enabled"
          message: "Password: ..."
```

**Port forwardings**
It's possible to enable/disable port forwardings for the device which is running HomeAssistant. 

Requirements:
- Set the `homeassistant_ip` in the configuration of `fritzbox_tools`
- On your FRITZ!Box, enable the setting `Selbstständige Portfreigaben für dieses Gerät erlauben.` for the device which runs HA

The port forwards will be exposed as switches in your HA installation (search for `port_forward` in your entity page to find the IDs).

Note: **Currently only port forwards for the device which is running HA are supported!**

## Exposed entities

- `service.reconnect`  Reconnect to your ISP
- `switch.fritz_box_guest_wifi`  Turns on/off guest wifi
- `sensor.fritz_box_connectivity`  online/offline depending on your internet connection
- `switch.port_forward_[description of your forward]` for each of your port forwards for your HA device


## Contributors

- [@mammuth](http://github.com/mammuth)
- [@AaronDavidSchneider](http://github.com/AaronDavidSchneider)
- [@jo-me](http://github.com/jo-me)
