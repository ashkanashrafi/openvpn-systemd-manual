۱️⃣ README.md
# OpenVPN Systemd Manual Control

Simple systemd service to manage OpenVPN manually, with easy connect/disconnect scripts.

## Usage
- Connect VPN:
  ```bash
  sudo vpn-on


Disconnect VPN:

sudo vpn-off


Check status:

sudo systemctl status openvpn-htb.service

Notes

If .ovpn requires username/password, add auth-user-pass /path/to/auth.txt inside .ovpn

Optional .ovpn tweaks: keepalive 10 60, ping 10, ping-restart 60

Installation

Copy systemd/openvpn-htb.service to /etc/systemd/system/

Copy bin/vpn-on and bin/vpn-off to /usr/local/bin/ and chmod +x

Reload systemd: sudo systemctl daemon-reload

Enable service: sudo systemctl enable openvpn-htb.service


---

### ۲️⃣ `systemd/openvpn-htb.service`
```ini
[Unit]
Description=OpenVPN for HackTheBox (Manual Control)
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
ExecStart=/usr/sbin/openvpn --config "/home/ashkan/Downloads/starting_point_ashkan1383 (1).ovpn"
User=root

[Install]
WantedBy=multi-user.target

۳️⃣ bin/vpn-on
#!/usr/bin/env bash
sudo systemctl start openvpn-htb.service && echo "VPN connected."

۴️⃣ bin/vpn-off
#!/usr/bin/env bash
sudo systemctl stop openvpn-htb.service && echo "VPN disconnected."

۵️⃣ install.sh
#!/usr/bin/env bash
sudo cp systemd/openvpn-htb.service /etc/systemd/system/
sudo cp bin/vpn-on bin/vpn-off /usr/local/bin/
sudo chmod 755 /usr/local/bin/vpn-on /usr/local/bin/vpn-off
sudo systemctl daemon-reload
sudo systemctl enable openvpn-htb.service
echo "Use 'sudo vpn-on' to connect and 'sudo vpn-off' to disconnect."

۶️⃣ uninstall.sh
#!/usr/bin/env bash
sudo systemctl stop openvpn-htb.service || true
sudo systemctl disable openvpn-htb.service || true
sudo rm -f /etc/systemd/system/openvpn-htb.service
sudo rm -f /usr/local/bin/vpn-on /usr/local/bin/vpn-off
sudo systemctl daemon-reload
echo "Uninstalled."
