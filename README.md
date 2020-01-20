# Keybase GitHub Bot
Notify your Keybase channels when stuff happens on GitHub!

<img width="655" alt="Screen Shot 2019-12-14 at 12 09 32" src="https://user-images.githubusercontent.com/232186/70842755-b75e5200-1e6a-11ea-8454-411da04cbd7b.png">

## How to install on Ubuntu 18

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
wget -O /usr/local/bin/keybase-github-bot https://raw.githubusercontent.com/wiz/keybase-github-bot/master/keybase-github-bot
chmod 755 /usr/local/bin/keybase-github-bot
```

Configure the bot for your team
```bash
wget -O /etc/default/keybase-github-bot.env https://raw.githubusercontent.com/wiz/keybase-github-bot/master/keybase-github-bot.env
chmod 644 /etc/default/keybase-github-bot.env
vi /etc/default/keybase-github-bot.env # modify as needed
```

Install systemd service for bot to start at boot
```bash
wget -O /etc/systemd/system/keybase-github-bot.service https://raw.githubusercontent.com/wiz/keybase-github-bot/master/keybase-github-bot.service
chmod 644 /etc/systemd/system/keybase-github-bot.service
systemctl daemon-reload
```

Start bot service, check logs
```bash
service keybase-github-bot start
journalctl -eu keybase-github-bot
```

If all went well, you should see this
```
Dec 13 15:26:03 webhook systemd[1]: Started Keybase GitHub Bot.
Dec 13 15:26:03 webhook keybase-github-bot[13723]: Starting...
Dec 13 15:26:07 webhook keybase-github-bot[13723]: Your bot is initialized. It is logged in as bisqcat
Dec 13 15:26:16 webhook keybase-github-bot[13723]: Ready!
```

Configure nginx to proxy /bot to your now running bot service
```bash
wget -O /etc/nginx/nginx.conf https://raw.githubusercontent.com/wiz/keybase-github-bot/master/nginx.conf
vi /etc/nginx/nginx.conf # edit as necessary for your domain (i.e. example.com -> your domain)
service nginx restart # restart nginx
```

Finally, configure the webhook on your GitHub repo
* Set endpoint -> `https://webhook.example.com/bot`
* Set content-type to use `application/json` encoding
* Set secret to match your string in bot config
* Map full git repo name in bot config to channel name
  i.e. `wiz/keybase-github-bot=general`)

### Ubuntu cleans /tmp

If ubuntu erases your Keybase session cookies in /tmp you might want to do this:
```bash
sudo systemctl mask systemd-tmpfiles-clean.timer
```
