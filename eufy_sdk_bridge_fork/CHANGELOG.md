# Changelog

## 1.3.1

- Bridge `81b5a04`: the H.264 live view no longer fails on a picture whose scaled height is odd. The NVR
  sent the eufyCam S4 as one stitched frame of both lenses (1920x2171), and the encoder refused it.

## 1.3.0

- Bridge `42bc029`: a timed still refresh (`mode=live`) of a battery camera gets a second try, since a
  camera behind the NVR (S3 Pro) can take longer than the SDK's fixed 20 s to wake. Fewer log lines.
  Pair with integration 1.2.0, which also refreshes the second (PTZ) lens.

## 1.2.0

- New option `mains_powered_models` (default `T8214`): those models count as mains-powered although they
  report a battery, e.g. a doorbell on a transformer. They get normal live streams (HLS) without the
  45 s battery limit, and live stills. Their battery level sensor stays. Bridge `7a5e7c0`.

## 1.1.0

- Bridge `287a456`: a live still taken on request (`mode=live`) is kept, so the camera tile shows it.
  Needed by the integration's new "Refresh battery camera stills" option (integration 1.1.0).

## 1.0.0

- Own version numbers from here on (they no longer follow upstream's). Based on upstream add-on
  `dev` and Nuvolares/ha-eufy-sdk-bridge `f7a9ce2` (upstream bridge `dev` + fork changes), SDK `0.6.0-beta.72`.
- Regular add-on icon and logo instead of the dev ones.

## 0.9.0-fork.4

- New option `nvr_extra_dual_lens` (default on): the extra NVR models (T8172, eufyCam S4) also get their
  second, PTZ lens as its own camera, like the S4 Max PoE. Experimental.

## 0.9.0-fork.3

- H.264 copy (`_h264`) now transcodes from the bridge feed itself. Before, an H.265 camera opened
  cold for the first time could load forever: the copy locked onto the H.264 stand-in picture.

## 0.9.0-fork.2

- Fix install failing with "exit code: 127": the Supervisor built on its own base image (no Node)
  instead of build.yaml's `build_from`. The Dockerfile now has a fixed `node:24-alpine` base.

## 0.9.0-fork.1

- Fork of the upstream dev add-on, built locally from Nuvolares/ha-eufy-sdk-bridge (upstream `dev` +
  fork changes) with the eufy-sdk pinned to `0.6.0-beta.72`.
- New options: `nvr_extra_media_models` (default `T8172`), `h264_twin`, `h264_width`.
- `go2rtc_enable: false` now actually disables the bundled go2rtc.
