# Changelog

## 0.9.0-fork.2

- Fix install failing with "exit code: 127": the Supervisor built on its own base image (no Node)
  instead of build.yaml's `build_from`. The Dockerfile now has a fixed `node:24-alpine` base.

## 0.9.0-fork.1

- Fork of the upstream dev add-on, built locally from Nuvolares/ha-eufy-sdk-bridge (upstream `dev` +
  fork changes) with the eufy-sdk pinned to `0.6.0-beta.72`.
- New options: `nvr_extra_media_models` (default `T8172`), `h264_twin`, `h264_width`.
- `go2rtc_enable: false` now actually disables the bundled go2rtc.
