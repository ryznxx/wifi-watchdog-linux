# WiFi Watchdog (Linux)

`wifi-watchdog` is a simple **systemd-based service** that automatically **restores WiFi connectivity** when your internet goes down.
Instead of manually running `nmcli` every time the connection drops, this watchdog keeps your WiFi alive and reconnects automatically.

## ✨ Features

* Periodically checks internet connectivity (default: every 30s).
* Pings a target host (default: `8.8.8.8`) to verify connection.
* Automatically runs `disconnect` + `connect` on the WiFi interface if connection is lost.
* Runs as a **systemd service**, auto-starts on boot.

## ⚙️ Installation

```bash
# clone the repo
git clone https://github.com/ryznxx/wifi-watchdog-linux.git
cd wifi-watchdog

# copy script
sudo cp wifi-watchdog.sh /usr/local/bin/
sudo chmod +x /usr/local/bin/wifi-watchdog.sh

# copy systemd service
sudo cp wifi-watchdog.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now wifi-watchdog
```

## 🔧 Configuration

Edit the variables inside `wifi-watchdog.sh` as needed:

```bash
WIFI_INTERFACE="wlan0"   # change this to your WiFi interface
PING_TARGET="8.8.8.8"    # target host to ping (can be your router/gateway)
SLEEP_INTERVAL=30        # check interval in seconds
```

## 📡 Check service status

```bash
systemctl status wifi-watchdog
journalctl -u wifi-watchdog -f
```

## 📜 License

MIT License – free to use, modify, and share.
