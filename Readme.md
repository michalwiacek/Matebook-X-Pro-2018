# Huawei Matebook X Pro (2018 Edition)
[![release](https://img.shields.io/badge/download-release-blue.svg)](https://github.com/profzei/Matebook-X-Pro-2018/releases) [![wiki](https://img.shields.io/badge/support-wiki-green.svg)](https://github.com/profzei/Matebook-X-Pro-2018/wiki)

macOS on Huawei Matebook X Pro 2018
![Alt text](https://ivanov-audio.com/wp-content/uploads/2014/01/Hackintosh-Featured-Image.png)

This repo is currently compatible with macOS Mojave 10.14.6 (18G87)


## Configuration

| Specifications      | Details                                                       |
| ------------------- | --------------------------------------------------------------|
| Computer model      | Huawei Matebook X Pro 2018 Space Gray                         |
| Processor           | Intel Core i7-8550U Processor @ 1.8 GHz                       |
| Memory              | 8 GB LPDDR3 2133 MHz                                          |
| Hard Disk           | LiteON SSD PCIe NVMe 512 GB                                   |
| Integrated Graphics | NVIDIA GeForce MX150 with 2 GB GDDR5 / Intel UHD Graphics 620 |
| Screen              | JDI 3k Display @ 3000 x 2000 (13.9 inch)                      |
| Sound Card          | Realtek ALC295 ???                                            |
| Wireless Card       | Intel Dual Band Wireless-AC 8265-8275                         |
| Bluetooth Card      | Intel Bluetooth 8265-8275                                     |

This repository is for personal purposes: it is heavily based on the hard work done by [gnodipac886](https://github.com/gnodipac886/MatebookXPro-hackintosh), but with some significant personal improvements in `CLOVER/ACPI/patched` and `CLOVER/kexts/Other` sections.


Changelog:   	see [Changelog.md](https://github.com/profzei/Matebook-X-Pro-2018/blob/master/Changelog.md)


## Device Firmware
- Bios version: `1.28`

## Bootloader Firmware
- Default bootloader: Clover `r5103` [Dids release](https://github.com/Dids/clover-builder/releases)

## SMBIOS
- Default SMBIOS settings of this repo is `MacBookPro14,1`

## Power management: CPUFriend
CPU power management is done by `CPUFriend.kext` while `CPUFriendDataProvider.kext` defines how it should be done. `CPUFriendDataProvider.kext` is generated for a specific CPU and power setting. The one supplied in this repository was made for `i7-8550U` and is optimized for balanced performance.
- The kexts and SSDT for `i7-8550U` are [here](https://github.com/profzei/Matebook-X-Pro-2018/tree/master/CPUFriend/1.2.0).
- `CPUFriendDataProvider.kext` is generated for SMBIOS `MacBookPro15,2` because of Kaby Lake R architecture.
- `CPUFriend.kext` and `CPUFriendDataProvider.kext` need to be put in `CLOVER/kexts/Other`
- Furthermore, you also need to put `SSDT-XCPM.aml` in `CLOVER/ACPI/patched` for working as normal after awake.

## Power management: Hibernation
Hibernation (suspend to disk or S4 sleep) is not supported on a Hackintosh: so it's recommended to disable it.
```
sudo pmset -a hibernatemode 0
sudo rm -f /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
```
Also, it's important to disable the other hibernation related functions.
```
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
```
Disabling additional features prevents random wakeups while the lid is closed.
```
sudo pmset -a powernap 0
sudo pmset -a proximitywake 0   [optional]
sudo pmset -b tcpkeepalive 0    [optional]
```
After every update, ALL these settings should be reapplied manually.

You can verify yuor power settings by typing in terminal `sudo pmset -g live` . If you ever want to reset these settings: `sudo pmset -a restoredefaults`


## USB port mapping

A proper `SSDT-UIAC.aml` has been created for USB Host Controller (XHCI-Device-ID: `<2f 9d 00 00>`) with only the necessary ports (tested with IOReg) and the correct connector type.

| Port      | Address               | Physical Location                                         | Internal/External |
| --------- | --------------------- | --------------------------------------------------------- | ----------------- |
| HS01/SS01 | `00000001`/`0000000D` | Left Port type-C (Power Source) - next to 3.5mm jack port | E                 |
| HS02/SS02 | `00000002`/`0000000E` | Right Port type-A                                         | E                 |
| HS03      | `00000003`            | Left Port type-C Thunderbolt                              | E                 |
| HS05      | `00000005`            | Bluetooth USB Port                                        | I                 |
| HS07      | `00000007`            | Integrated HD Camera module                               | I                 |









