# Home Assistant Add-on: APC UPS Daemon

## Installation

Follow these steps to get the add-on installed on your system:

1. Navigate in your Home Assistant frontend to **Settings** -> **Add-ons** -> **Add-on store**.
2. Find the "APC UPS Daemon" add-on and click it.
3. Click on the "INSTALL" button.

## How to use

1. In the configuration section, set the **UPS Cable**, **UPS Type**, and
   **Device** options. The default values for the remaining options are suitable
   for most people.
2. Save the configuration.
3. Start the add-on.
4. Check the add-on log output to see the result.

## Configuration

Add-on configuration:

```yaml
kill_on_powerfail: false
dumb_mode: false
upscable: smart
upstype: apcsmart
device: '/dev/ttyS0'
polltime: 60
onbatterydelay: 6
batterylevel: 5
minutes: 3
timeout: 0
```

### Option: `kill_on_powerfail` (required)

Instructs the add-on to command the UPS to power down in hibernate mode just
before it starts the system shutdown. This relies on the grace shutdown delay of
a Smart-UPS being long enough to allow the system to shutdown completely before
the UPS shuts off the power to the system and goes into hibernate mode. This
shutdown grace delay is a programmable value stored in a Smart-UPS EEPROM. In
hibernate mode, the UPS will again supply power to the system when the utility
power returns. 

### Option: `dumb_mode` (required)

Put a UPS which runs in smart signalling mode by default (e.g. a Smart-UPS) into
simple signalling mode.

### Option: `upscable` (required)

The type of cable used to connect the UPS to Home Assistant.

### Option: `upstype` (required)

The type of APC UPS that you have.

### Option: `device` (optional)

The name of the device used for communication between the UPS and Home
Assistant. For a USB UPS, you should leave this option blank and the add-on will
figure out where the device is located.

UPS Type   | Device                           | Connection
--         | --                               | --
`apcsmart` | `/dev/tty**`                     | Serial
`usb`      | (blank)                          | USB
`net`      | `hostname:port`                  | Network Information Server
`snmp`     | `hostname:port:vendor:community` | SNMP
`dumb`     | `/dev/tty**`                     | Serial
`pcnet`    | `ipaddr:username:passphrase`     | PowerChute (SmartSlot cards)
`modbus`   | `/dev/tty**`                     | Serial
`modbus`   | (blank)                          | USB

### Option: `polltime` (required)

The rate in seconds that the add-on polls the UPS for status. This rate is
automatically set to 1 second when the UPS goes on battery and reset to the
specified value when the utility power returns. This setting applies both to
directly-attached UPSes and networked UPSes. A low setting will improve the
add-on's responsiveness to certain events at the cost of higher CPU utilisation.

### Option: `onbatterydelay` (required)

The number of seconds from when a power failure is detected until the add-on
reacts with an `onbattery` event.

### Option: `batterylevel` (required)

The add-on will shutdown Home Assistant during a power failure when the
remaining battery charge falls below the specified percentage.

### Option: `minutes` (required)

The add-on will shutdown Home Assistant during a power failure when the
remaining runtime on batteries as internally calculated by the UPS falls below
the specified minutes.

### Option: `timeout` (required)

After a power failure occurs, the add-on will shutdown Home Assistant after the
specified number of seconds have expired. For a Smart-UPS, this should normally
be set to zero so that the shutdown time will be determined by the battery level
or remaining runtime (see above). This option is, however, useful for a Back-UPS
or other simple signalling UPS which does not report battery level or the
remaining runtime. It is also useful for testing the add-on because you can
force a rapid shutdown by setting a small value (e.g. 60) and turning off the
power to the UPS.

The `timeout`, `batterylevel`, and `minutes` options can all be set without
problems. The add-on will initiate a shutdown when the first of these conditions
becomes valid.

## Support

If a problem is encountered using this add-on, please file an issue on
[GitHub][issues].

[issues]: https://github.com/sstallion/hassio-addons/issues
