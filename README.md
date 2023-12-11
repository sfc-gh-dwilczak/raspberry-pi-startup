# raspberry-pi-startup
Start up code to get your raspberry pi to ssh via ngrok after it has connected to the internet.

## Setup
Create a file on your desktop:
```bash
cd Desktop/
nano startup.sh
```

Add whatever bash code you want here. In my case it's ssh via ngrok:
```bash
#!/bin/bash
ngrok config add-authtoken <NGROK KEY>
ngrok tcp --remote-addr=9.tcp.ngrok.io:22830 22
```

Next make it executable:
```bash
chmod +x startup.sh
```

Edit your system file to set startup procedure:
```
sudo systemctl edit --force --full startup.service
```

Add the config to your script:
```
[Unit]
Description=My run after internet script
After=network-online.target

[Service]
ExecStart=/home/larry/Desktop/startup.sh

[Install]
WantedBy=network-online.target
```

Now enable the service to run:
```
sudo systemctl daemon-reload
sudo systemctl enable startup.service
```

Now reboot and see if it works by connection to your ssh:
```
sudo reboot
```

Connect from your other pc:
```
ssh -p 22830 larry@9.tcp.ngrok.io
```
