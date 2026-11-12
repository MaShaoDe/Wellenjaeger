# Wellenjäger: DIY Shortwave SDR Receiver

[Deutsch](README.md) | [English](README.en.md)

*Build your own software defined receiver, watch the bands as a waterfall and hunt the signals yourself.*

**Status: concept.** No hardware and no firmware yet. The main documentation ([README](README.md) and [docs/konzept.md](docs/konzept.md)) is in German. This page is a short summary.

* **What:** a workshop project for radio amateur exam candidates. We build a real SDR receiver for the HF bands from open source software. Receive only, so no licence is needed to use it.
* **Signal chain:** antenna, band filter, Tayloe quadrature mixer, Si5351 local oscillator, I2S audio codec, ESP32-S3.
* **Software:** FFT, waterfall and SSB/AM/CW demodulation on the ESP32-S3.
* **Bands:** start with 40 m and 20 m, later 15 m and 10 m, one band filter per band.
* **Display:** waterfall on the device and as a web page served by the ESP32 (own Wi-Fi, no internet). Bluetooth LE only as a remote control.
* **Sound:** built in speaker, 3.5 mm jack with line out, optional Bluetooth speaker.
* **Case:** 3D printed, with room for a shield over the RF section.
* **Rugged:** acceptance test is a 1 m drop onto wood or tiles.
* **Software base:** [michalin/easysdr](https://github.com/michalin/easysdr) (GPL-3.0). The authors of ESP32-SDR and ESP32-A1S-Tayloe are asked about their licences first, since their repositories have none.
* **No FM broadcast:** a Tayloe receiver covers about 1 to 30 MHz. For FM and MW see [Funkfüürli](https://github.com/MaShaoDe/Funkfuerli).

Licenses: hardware CERN-OHL-P-2.0, documentation CC-BY-4.0, firmware follows its upstream. See [LICENSE.md](LICENSE.md).

73 de Marcel (MaShaoDe)
