# **WiFi Watchdog – Bash Script**

This project is a **WiFi watchdog for Linux** that automatically checks your WiFi dongle and reconnects to a network if the internet goes down.
It also keeps logs of every event, so you can debug when and why your connection dropped.

---

## 🔧 How It Works

* Checks if the WiFi interface exists (dongle plugged in).
* Pings `8.8.8.8` to verify connectivity.
* If connection is lost:

  * Tries reconnecting **up to 3 times** using `nmcli`.
  * If still fails → restarts the WiFi interface.
* Logs every action into `./wifi-info/logwifi.txt`.
* Resets the log file automatically if older than **7 days**.
* Marks each reboot with `[REBOOT] Service start`.

---

## 📜 Script Example

```bash
SSID="ryznxx-xpon-kh6c"
PASS="12345678"
IFACE="wlx2023511faf6d"
LOGDIR="./wifi-info"
LOGFILE="$LOGDIR/logwifi.txt"
```

Change:

* `SSID` → Your WiFi network name
* `PASS` → WiFi password
* `IFACE` → Your WiFi dongle interface (check with `ip link`)

---

## 🚀 Usage

1. Copy the script to your system:

   ```bash
   cp wifi-watchdog.sh /usr/local/bin/wifi-watchdog.sh
   chmod +x /usr/local/bin/wifi-watchdog.sh
   ```

2. Run manually:

   ```bash
   ./wifi-watchdog.sh
   ```

3. Or run as a background service (systemd recommended). Example `wifi-watchdog.service`:

   ```ini
   [Unit]
   Description=WiFi Watchdog Service
   After=network.target

   [Service]
   ExecStart=/usr/local/bin/wifi-watchdog.sh
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

   Then:

   ```bash
   sudo cp wifi-watchdog.service /etc/systemd/system/
   sudo systemctl daemon-reload
   sudo systemctl enable --now wifi-watchdog
   ```

---

## 📂 Logs

All logs are stored in:

```
./wifi-info/logwifi.txt
```

Example log entries:

```
2025-09-27 10:00:00 -> [REBOOT] Service start
2025-09-27 10:05:12 -> [WARN] Koneksi terputus, coba reconnect...
2025-09-27 10:05:20 -> [OK] WiFi berhasil reconnect ke XPON-KH6c (percobaan 2)
```

---

## 📡 Notes

* Requires `nmcli` (NetworkManager).
* Works best with WiFi dongles (tested on USB WiFi adapters).
* Adjust the `sleep` duration if you want faster/slower checks.

---

## 📜 License

No need license all is yours

⚡ Mau gw bikinin juga **badge status (systemd running/stopped)** buat README biar keliatan lebih pro kalau diliat di GitHub?
