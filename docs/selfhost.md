# Self-Hosting

If you prefer hosting with Docker, see [Docker](docker.md) instead.

## Install nodejs and npm

For Debian, Ubuntu: packages in the repository are so old,
consider installing them with [NodeSource](https://github.com/nodesource/distributions#installation-instructions).
Minimal required version is NodeJS 18.

Other distros:
```bash
# CentOS
sudo yum install nodejs
# Arch
sudo pacman -S nodejs npm
# Alpine
doas apk add nodejs npm
```

## Create a user for txtdot

Almost all distros except Alpine:
```bash
sudo useradd -r -m -s /sbin/nologin -U txtdot
sudo -u txtdot bash
```

Alpine Linux with busybox and doas:
```bash
doas addgroup -S txtdot
doas adduser -h /home/txtdot -s /sbin/nologin -G txtdot -S -D txtdot
doas -u txtdot bash
```

## Build, config and launch

Clone the git repository, cd into it:
```bash
git clone https://github.com/txtdot/txtdot.git src
cd src
```

Copy and modify the sample config file (see the [Configuring](env.md) section):
```bash
cp .env.example .env
nano .env
```

Install packages, compile TS:
```bash
npm install
npm run build
```

Manually start the server to check if it works (Ctrl+C to exit):
```bash
npm run start
```

Log out from the txtdot account:
```bash
exit
```

## Add txtdot to autostart
Either using systemd unit file:
```bash
wget https://raw.githubusercontent.com/TxtDot/txtdot/main/config/txtdot.service
sudo chown root:root txtdot.service
sudo chmod 644 txtdot.service
sudo mv txtdot.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable txtdot
sudo systemctl start txtdot
```

Or using OpenRC script:
```bash
wget -O txtdot https://raw.githubusercontent.com/TxtDot/txtdot/main/config/txtdot.init
doas chown root:root txtdot
doas chmod 755 txtdot
doas mv txtdot /etc/init.d/
doas rc-update add txtdot
doas rc-service txtdot start
```

Or using crontab:
```bash
sudo crontab -u txtdot -e
# The command will open an editor
# Add this line to the end of the file:
@reboot sleep 10 && cd /home/txtdot/src && npm run start
# Save the file and exit
```
