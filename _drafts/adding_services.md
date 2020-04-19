# Add a service unit file 

path: /etc/systemd/system/SERVICE-NAME.service

file:

```ini
[Unit]
Description=Hamilton core

[Service]
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=COMMAND
# example:
ExecStart=/usr/bin/python3  /home/pi/src/Hamilton/controller/steam_test.py

[Install]
WantedBy=default.target
```
