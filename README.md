# Legacy_RS232_DripFeeder

> [!WARNING]
> This repository is a prototype. The supplied implementation has not been established as production-ready or physically validated end to end.

FileLink Discovery is a Windows WPF application and ESP32 firmware pair for discovering and managing FileLink devices on a local network. The device exposes an HTTP API for SD-card operations, RS232 transport, device settings, and OTA updates; the Windows application discovers devices through mDNS.

## What is implemented

- mDNS discovery with `_filelink._tcp.local.`
- Ethernet-first startup, Wi-Fi station mode, and provisioning fallback
- SD-card file operations with CRC32 transfer checks
- RS232 configuration and raw transport
- Declarative HP-GL and HP 7550A software definitions
- OTA partition update flow

## Important limitations

- Communication is plain HTTP; HTTPS is not implemented.
- The provisioning access point is open.
- The password feature is used by the desktop Connect flow, but it is not a global authorization layer for ordinary HTTP endpoints.
- Physical hardware compatibility and project-owned automated test results are not established by the supplied evidence.

## Documentation

The detailed documentation is maintained in the project Wiki:

- [Home](../../wiki/Home)
- [Setup](../../wiki/Setup)
- [Operating a device](../../wiki/Operating-a-Device)
- [Architecture](../../wiki/Architecture)
- [Security boundaries](../../wiki/Security-Boundaries)
- [Troubleshooting](../../wiki/Troubleshooting)

For the Wiki source pages and publishing notes, see [`github-wiki/`](github-wiki/).

## Source-layout reference

| Area | Location |
|---|---|
| ESP32 firmware | `esp32-firmware/` |
| Windows application | `windows-app/FileLinkDiscovery/` |
| Wiki source pages | `github-wiki/` |

Shield: [![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg
