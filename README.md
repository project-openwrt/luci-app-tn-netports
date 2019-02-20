# Network ports status LuCI application

## Description
This package allows you to monitor current state of the
network interfaces specified in configuration file (`/etc/config/luci_netports`).

## Dependencies
This LuCI application developed for LuCI master branch.
Application requires luabitop package to be installed.

## Supported languages
- English
- Russian

## UCI configuration
UCI configuration for this application is stored in `/etc/config/luci_netports` file.

This file contain one `global` section with global application settings and one
or more `port` sections with network ports settings.

### Global settings

The `global` section contains global application settings. Default `global` section configuration:
```
config global 'global'
	option default_additional_info 'false'
	option default_h_mode          'false'
	option hv_mode_switch_button   'false'
```

These options can be set in the `global` section:

| Name                      | Type    | Required | Default | Description                                                                  |
| ------------------------- | ------- | -------- | ------- | ---------------------------------------------------------------------------- |
| `default_additional_info` | boolean | no       | false   | Display additional information in horizontal view mode by default.           |
| `default_h_mode`          | boolean | no       | false   | Use horizontal view mode by default.                                         |
| `hv_mode_switch_button`   | boolean | no       | false   | Show button for manual switching between horizontal and vertical view modes. |

### Ports settings

Sections of the type `port` declare network port settings for displaying in application.

A minimal port declaration consists of the following lines:
```
config port
	option ifname 'eth0'
```
These options can be set in the `port` sections:

| Name                      | Type    | Required | Default | Description                                                                  |
| ------------------------- | ------- | -------- | ------- | ---------------------------------------------------------------------------- |
| `ifname`                  | string  | yes      | (none)  | Network interface name. |
| `name`                    | string  | no       | value of `ifname` | Custom port name for displaying in application. |
| `type`                    | string  | no       | `auto` | Custom port type. Available port types listed in the table below. |

Available port types (`type` setting):

| Type(s) | Disabled icon | Linkdown icon | Linkup icon | Description |
| ----- |:------------:|:--------------:|:------------:| :---------- |
| `auto` | - | - | - | Automatically detect type by interface name (see next table). |
| `copper` | ![copper_disabled](htdocs/luci-static/resources/netports/icons/copper_disabled.svg?sanitize=true) | ![copper_down](htdocs/luci-static/resources/netports/icons/copper_down.svg?sanitize=true) | ![copper_up](htdocs/luci-static/resources/netports/icons/copper_up.svg?sanitize=true) | Wired (RJ45) connection. |
| `fixed` | ![fixed_disabled](htdocs/luci-static/resources/netports/icons/fixed_disabled.svg?sanitize=true) | ![fixed_down](htdocs/luci-static/resources/netports/icons/fixed_down.svg?sanitize=true) | ![fixed_up](htdocs/luci-static/resources/netports/icons/fixed_up.svg?sanitize=true) | Intercircuit fixed link connection. |
| `wifi`, `usb_wifi` | ![wifi_disabled](htdocs/luci-static/resources/netports/icons/wifi_disabled.svg?sanitize=true) | ![wifi_down](htdocs/luci-static/resources/netports/icons/wifi_down.svg?sanitize=true) | ![wifi_up](htdocs/luci-static/resources/netports/icons/wifi_up.svg?sanitize=true) | Wireless connection. |
| `usb_rndis` | ![usb_rndis_disabled](htdocs/luci-static/resources/netports/icons/usb_rndis_disabled.svg?sanitize=true) | ![usb_rndis_down](htdocs/luci-static/resources/netports/icons/usb_rndis_down.svg?sanitize=true) | ![usb_rndis_up](htdocs/luci-static/resources/netports/icons/usb_rndis_up.svg?sanitize=true) | USB RNDIS connection. |
| `usb_stick`, `usb_2g`, `usb_3g`, `usb_4g` | ![usb_stick_disabled](htdocs/luci-static/resources/netports/icons/usb_stick_disabled.svg?sanitize=true) | ![usb_stick_down](htdocs/luci-static/resources/netports/icons/usb_stick_down.svg?sanitize=true) | ![usb_stick_up](htdocs/luci-static/resources/netports/icons/usb_stick_up.svg?sanitize=true) | USB 2G/3G/4G modem connection. |
| `gprs` | ![gprs_disabled](htdocs/luci-static/resources/netports/icons/gprs_disabled.svg?sanitize=true) | ![gprs_down](htdocs/luci-static/resources/netports/icons/gprs_down.svg?sanitize=true) | ![gprs_up](htdocs/luci-static/resources/netports/icons/gprs_up.svg?sanitize=true) | GPRS connection. |
| `vpn` | ![vpn_disabled](htdocs/luci-static/resources/netports/icons/vpn_disabled.svg?sanitize=true) | ![vpn_down](htdocs/luci-static/resources/netports/icons/vpn_down.svg?sanitize=true) | ![vpn_up](htdocs/luci-static/resources/netports/icons/vpn_up.svg?sanitize=true) | VPN connection. |
| `ppp` | ![ppp_disabled](htdocs/luci-static/resources/netports/icons/ppp_disabled.svg?sanitize=true) | ![ppp_down](htdocs/luci-static/resources/netports/icons/ppp_down.svg?sanitize=true) | ![ppp_up](htdocs/luci-static/resources/netports/icons/ppp_up.svg?sanitize=true) | PPP connection. |

Automatic network port type detection (`auto` type):

| Interface name regular expression(s) | Example(s) | Detected type |
| :------------- | :--- | :--- |
| `^eth\d+`,<br />`^sw\d+p\d+`,<br />`^en[a-z]\d+[a-z]\d+` | `eth0`, `eth32`,<br />`sw1p4`,<br />`enp0s4`, `ens9f0` | `copper` |
| `^wlan\d+`,<br />`^wl[a-z]\d+[a-z]\d+` | `wlan0`,<br />`wlp0s4`, `wls1f4` | `wifi` |
| `^usb\d+` | `usb0` | `usb_rndis` |
| `^wwan\d+`,<br />`^ww[a-z]\d+[a-z]\d+` | `wwan0`,<br />`wwp0s4`, `wws1f4` | `usb_stick` |
| `^ppp\d+` | `ppp0` | `ppp` |
| `^tun\d+`,<br />`^tap\d+` | `tun0`, `tap0` | `vpn` |

## Screenshots

### Vertical view mode (default)
![Vertical view mode](screenshots/v-mode-5-ports.png?raw=true "Vertical view mode")

### Horizontal view mode

With additional information:

![Horizontal view mode](screenshots/h-mode-5-ports.png?raw=true "Horizontal view mode")

Without additional information:

![Horizontal view mode (additional information is hidden)](screenshots/h-mode-5-ports-short.png?raw=true "Horizontal view mode (additional information is hidden)")
