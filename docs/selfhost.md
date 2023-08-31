# Self-Hosting

## Without Docker

Install Node and NPM:
```bash
# Debian, Ubuntu
# TODO: Really old version in repos, use nodesource
sudo apt install nodejs npm
# CentOS
sudo yum install nodejs
# Arch
sudo pacman -S nodejs npm
# Alpine
doas apk add nodejs npm
```

Create a user for txtdot, log in:
```bash
# Not Alpine (coreutils)
sudo useradd -r -m -s /sbin/nologin -U txtdot
sudo -u txtdot bash

# Alpine (busybox)
doas addgroup -S txtdot
doas adduser -h /home/txtdot -s /sbin/nologin -G txtdot -S -D txtdot
doas -u txtdot bash
```

Clone the repo: 
```bash
git clone https://github.com/txtdot/txtdot.git src
```

Install packages, compile TS:
```bash
cd src
npm install
npm run build
```

Manually start the server to check if it works (Ctrl+C to exit):
```bash
npm run start
```

Log out from txtdot account: `exit`

### Add txtdot to autostart
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

## With Docker

Docker Engine and Docker Compose are required.

Note that built images are not provided via Docker Hub.
If you can't or don't want to build them on your server
and don't want to setup a CI/CD system,
[let us know](https://github.com/txtdot/txtdot/issues),
we'll consider setting up a GitHub Actions workflow.

```bash
git clone https://github.com/txtdot/txtdot.git
cd txtdot
docker compose build
docker compose up -d
```
