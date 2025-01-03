# Cloudflare Tunnel Client (cloudflared) for MIPS Devices

This repo has a version of cloudflared (Cloudflare Tunnel client) that works on MIPS-based devices running OpenWrt, like the TP-Link MR-3020.

## What’s Inside?

- A cloudflared continues build for MIPS devices.
- Automatic updates using GitHub Actions, so always up-to-date.

## How It Works

- Every day at 00:00, a [GitHub Actions job](https://github.com/notsudoers/cloudflared-openwrt-for-mips/actions) will be checks if there’s a new version of [official Cloudflared releases](https://github.com/cloudflare/cloudflared/releases/latest).  
- If there’s an update, it compiles the new version and drops it in the [Releases](https://github.com/notsudoers/cloudflared-openwrt-for-mips/releases/latest).


## Contents

This repo contains:
- cloudflared binary build for mips based architecture
- cloudflared init (services)

## Installation

**Download latest binary release**

- using aria2

  ```sh
  aria2c $(curl -s https://api.github.com/repos/notsudoers/cloudflared-openwrt-for-mips/releases/latest | jq -r '.assets[] | select(.name | contains ("tar.gz")) | .browser_download_url' | head -n 1) \
  -o cloudflared-mips-latest.tar.gz
  ```

- using wget

  ```sh
  wget $(curl -s https://api.github.com/repos/notsudoers/cloudflared-openwrt-for-mips/releases/latest | jq -r '.assets[] | select(.name | contains ("tar.gz")) | .browser_download_url' | head -n 1) \
  --no-check-certificate -O cloudflared-mips-latest.tar.gz
  ```

- extract archive and change permissions

  ```sh
  tar xvf cloudflared-mips-latest.tar.gz
  mv cloudflared /usr/bin/cloudflared
  chmod +x /usr/bin/cloudflared
  ```

**Download cloudflared services**

- Using aria2

  ```sh
  aria2c https://raw.githubusercontent.com/notsudoers/cloudflared-openwrt-for-mips/main/etc/init.d/cloudflared -o /etc/init.d/cloudflared
  ```

- Using wget

  ```sh
  wget https://raw.githubusercontent.com/notsudoers/cloudflared-openwrt-for-mips/main/etc/init.d/cloudflared --no-check-certificate -O /etc/init.d/cloudflared
  ```

- Change permissions

  ```sh
  chmod +x /etc/init.d/cloudflared
  ```

**Add your token to services**

  - Make sure you have created a Tunnel and got the token or just following this [Guides](https://developers.cloudflare.com/learning-paths/replace-vpn/connect-private-network/cloudflared/)
  - Run command below and replace "your-token" with your owns.
 
  ```sh
  sed -i 's,#token=,token="your-token",' /etc/init.d/cloudflared
  ```

**Start the services**

  ```sh
  /etc/init.d/cloudflared start
  ```

**Enable service on boot**

  ```sh
  /etc/init.d/cloudflared enable
  ```

  other available options:
  ```
  Syntax: /etc/init.d/cloudflared [command]

  Available commands:
          start           Start the service
          stop            Stop the service
          restart         Restart the service
          reload          Reload configuration files (or restart if service does not implement reload)
          enable          Enable service autostart
          disable         Disable service autostart
          enabled         Check if service is started on boot
          running         Check if service is running
          status          Service status
          trace           Start with syscall trace
          info            Dump procd service info
  ```

## Inspiration of this repository

- [Cloudflare-Tunnel-Client-for-Arch-MIPS by aladdinAlmarashly](https://github.com/aladdinAlmarashly/Cloudflare-Tunnel-Client-for-Arch-MIPS)

---
