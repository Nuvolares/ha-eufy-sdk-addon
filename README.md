# ha-eufy-sdk-addon (Nuvolares fork)

> **Fork of [mega-yfue/ha-eufy-sdk-addon](https://github.com/mega-yfue/ha-eufy-sdk-addon).** All credit for
> the add-on, the [bridge](https://github.com/mega-yfue/ha-eufy-sdk-bridge) and the
> [eufy-sdk](https://github.com/mega-yfue/eufy-sdk) goes to [mega-yfue](https://github.com/mega-yfue) and the
> upstream contributors. It stays under their MIT license.

One add-on, **eufy-sdk bridge (Nuvolares fork)**. Unlike upstream it pulls no prebuilt image: the Supervisor
**builds it on your Home Assistant host** from
[Nuvolares/ha-eufy-sdk-bridge](https://github.com/Nuvolares/ha-eufy-sdk-bridge) at the commit pinned in
[`eufy_sdk_bridge_fork/build.yaml`](eufy_sdk_bridge_fork/build.yaml). The first install takes a few minutes.

What the fork bridge adds on top of upstream `dev`:

- **H.264 copy of every camera stream** (on demand), so browsers without H.265 WebRTC can play H.265
  cameras such as the eufyCam S3 Pro. Pair it with the
  [Nuvolares/ha-eufy-sdk](https://github.com/Nuvolares/ha-eufy-sdk) integration, whose **Live view codec**
  option uses it.
- **More cameras through an S4 NVR**: option `nvr_extra_media_models` (default `T8172`, eufyCam S4).
  Experimental.

## Install

1. **Settings → Add-ons → Add-on Store → ⋮ → Repositories**, add
   `https://github.com/Nuvolares/ha-eufy-sdk-addon`.
2. **Stop** any other eufy-sdk bridge add-on first: they share ports 3000/8554 and the eufy account
   session (eufy allows one session per account).
3. Install **eufy-sdk bridge (Nuvolares fork)**, enter email / password / country, start it.

amd64 and aarch64 only.
