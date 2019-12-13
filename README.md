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
chmod 755 /usr/local/bin/keybase-github-bot
```

Configure the bot for your team
```bash
wget -O /etc/default/keybase-github-bot.env https://raw.githubusercontent.com/wiz/keybase-github-bot/keybase-github-bot.env
chmod 644 /etc/default/keybase-github-bot.env
vi /etc/default/keybase-github-bot.env # modify as needed
```

Install systemd service for bot to start at boot
```bash
wget -O /etc/systemd/system/keybase-github-bot.service https://raw.githubusercontent.com/wiz/keybase-github-bot/keybase-github-bot.service
chmod 644 /etc/systemd/system/keybase-github-bot.service
systemctl daemon-reload
```

Create webhook on your GitHub repo
* Endpoint -> https://webhook.example.com/bot
* JSON encoding
* Secret must match string in bot config
* Repo to team/channel mapping in bot config

Start bot service
```bash
service keybase-github-bot start
```

Check logs for what's going on
```bash
journalctl -eu keybase-github-bot
```

If all went well, you should see this
```
Dec 13 15:26:03 webhook systemd[1]: Started Keybase GitHub Bot.
Dec 13 15:26:03 webhook keybase-github-bot[13723]: Starting...
Dec 13 15:26:07 webhook keybase-github-bot[13723]: Your bot is initialized. It is logged in as bisqcat
Dec 13 15:26:16 webhook keybase-github-bot[13723]: Ready!
```
