# fw-nrf52840-dk-thermometer

Die temperature readout on the nRF52840 DK using the Zephyr sensor API. Zephyr RTOS, nRF Connect SDK.

## What it does

Reads the internal die temperature of the nRF52840 (`TEMP` peripheral) once per second with `sensor_sample_fetch()` and `sensor_channel_get()` on `SENSOR_CHAN_DIE_TEMP`, and prints it to the console. It also sets lower and upper threshold alerts and runs a callback when the temperature leaves them.

Expected output:

```
Thermometer Example (arm)
Temperature device is 0x..., name is temp@4000c000
Temperature is 28.0 C
Temperature is 28.2 C
```

Relevant configuration:

| File | Option | Purpose |
|---|---|---|
| `prj.conf` | `CONFIG_SENSOR=y` | sensor subsystem |
| `prj.conf` | `CONFIG_CBPRINTF_FP_SUPPORT=y` | floating point in printf |
| `boards/nrf52840dk_nrf52840.conf` | `CONFIG_TEMP_NRF5=y` | nRF5 temperature driver |

The `boards/` folder also holds overlays for other boards that come with the Zephyr sample; this project targets the nRF52840 DK.

## Hardware

| Item | Detail |
|---|---|
| Board | nRF52840 DK |
| Sensor | on-die temperature sensor (`TEMP`), no external parts |
| Console | RTT or UART through the J-Link VCOM |

## Build

Requires nRF Connect SDK v3.0.2 or compatible.

```bash
west build -b nrf52840dk/nrf52840 --pristine
```

## Flash

```bash
west flash --runner jlink
```

## Test

Open the serial console (115200 8N1) or RTT. A new temperature line should appear every second. Warming the chip, for example by touching it, should raise the value.

## License

MIT, see [LICENSE](LICENSE).
