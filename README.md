# Keybase GitHub Bot - notifies keybase team channel on various GitHub events

## Setup for Ubuntu 18

Install dependencies NodeJS + NPM, nginx + letsencrypt, Keybase + keybase-bot
```base
sudo apt-get install -y nodejs npm nginx-core python-certbot-nginx
curl --remote-name https://prerelease.keybase.io/keybase_amd64.deb
sudo apt install -y ./keybase_amd64.deb
sudo npm install -g keybase-bot
```

Obtain SSL certificate for your server's hostname
```bash
sudo certbot --nginx --agree-tos --non-interactive -m ssl@example.com -d webhook.example.com
```

Install this bot code from GitHub
```bash
wget -O /usr/local/bin/keybase-github-bot https://raw.githubusercontent.com/wiz/keybase-github-bot/keybase-github-bot
wget -O /usr/local/bin/keybase-github-bot https://raw.githubusercontent.com/wiz/keybase-github-bot/keybase-github-bot
chmod 755 /usr/local/bin/keybase-github-bot
```

Configure the bot for your team
```bash

```

