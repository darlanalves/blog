# Automated HTTPS certificates with Digital Ocean and Let's Encrypt on Ubuntu 20+

> Important: `sudo` is not used here, I'm assuming you are doing this as root.
>
> Otherwise, prefix all commands with `sudo`

## Step 1: Make sure Snap is installed

Follow [this page on Snap](https://snapcraft.io/docs/installing-snapd) to install it if needed.

## Step 2: Install Certbot

We will follow the steps from [Certbot website](https://certbot.eff.org/lets-encrypt/snap-nginx)

```bash
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
```

## Step 3: Install and configure Digital Ocean DNS plugin

Let's [install a DNS plugin](https://certbot-dns-digitalocean.readthedocs.io/en/stable/) for Certbot that will automatically configure the DNS entries for renewal.

> Side note: I don't know exaclty why we need to explicitly install the plugin as a python dependency, but it won't work otherwise

```bash
apt install python3-pip
pip3 install certbot-dns-digitalocean
snap set certbot trust-plugin-with-root=ok
snap install certbot-dns-digitalocean
```

You also need to [create an API token](https://cloud.digitalocean.com/account/api/tokens?i=d7938a) in your Digital Ocean settings.
This token is used to configure the DNS entries. Go to settings and copy your token.

Ok, now let's create a file and paste your token there:

```bash
echo 'dns_digitalocean_token = <paste-token-here>' > ~/digital-ocean.ini
```

## Step 4: Generate certificates

Still with me? Good!

Now we can put all together and generate our certificates:

```bash
certbot certonly \
  --agree-tos \
  --register-unsafely-without-email \
  --dns-digitalocean \
  --dns-digitalocean-credentials ~/digital-ocean.ini \
  --dns-digitalocean-propagation-seconds 30 \
  -d example.com \
  -d '*.example.com'
```

> If you want to receive emails when a certificate is not renewed, remove `--register-unsafely-without-email` from this command. Certbot will ask for your email

If everything went well you should see a message like this:

```
IMPORTANT NOTES:s
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on [date]. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
 ```
 
 ## Testing the automatic renewal

Then you can do a dry-run of your settings with the following:

 ```bash
 certbot renew --dry-run
 ```
If you see no errors, everything is ready and fully automated.

> Note: I saw some issues due to my _acme* DNS entries not changing fast enough. I can't figure out why, but my renewals work just fine 🤷‍♂️

Also check if there's a cronjob schedule to auto renew your certificates:

```bash
systemctl status certbot.timer
```
