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
echo 'dns_digitalocean_token = <paste-token-here>' > /home/digital-ocean.ini
```

## Step 4: Configure the CLI

Still with me? Good!

Let's create a configuration file to set some default options for certbot.

Create a file at `/etc/letsencrypt/cli.ini`. I use `nano` for that:

```bash
nano /etc/letsencrypt/cli.ini
```

Paste this in your editor and save it:

```
agree-tos = true
force-renewal = true
authenticator = dns-digitalocean
dns-digitalocean-credentials = /home/digital-ocean.ini
dns-digitalocean-propagation-seconds = 30
register-unsafely-without-email = true
non-interactive = true
```

> If you want to receive emails when a certificate is not renewed, remove `--register-unsafely-without-email` from this configuration. Certbot will ask for your email

## Step 5: Generate certificates

Now we can put all together and generate our certificates:

```bash
certbot certonly -d example.com -d '*.example.com'
```

If everything went well you should see a message like this:

```
IMPORTANT NOTES:
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
systemctl status snap.certbot.renew.timer
```

References for this post:

- [How To Secure Nginx with Let's Encrypt on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04)
- [Certbot with Snap and Nginx](https://certbot.eff.org/lets-encrypt/snap-nginx)
- [Letsencrypt Wildcard SSL with DigitalOcean DNS](https://blog.khophi.co/letsencrypt-wildcard-ssl-with-digitalocean-dns/)
- [Documentation for certbot-dns-digitalocean](https://certbot-dns-digitalocean.readthedocs.io/en/stable/)
- [Certbot: unexpected error](https://blog.bramp.net/post/2018/05/26/certbot-unexpected-error/)
