# Add a service unit file 

path: /etc/systemd/system/SERVICE-NAME.service

file: 
```
[Unit]
Description=Hamilton core

[Service]
Type=simple
Restart=on-failure
ExecStart=COMMAND
# example:
ExecStart=/usr/bin/python3  /home/pi/src/Hamilton/controller/steam_test.py

[Install]
WantedBy=default.target
```
