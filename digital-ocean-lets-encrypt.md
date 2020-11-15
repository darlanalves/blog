# Automated HTTPS certificates with Digital Ocean and Let's Encrypt on Ubuntu 20+

> NOTE: `sudo` is not used here, I'm assuming you are doing this as root.
>
> Otherwise, prefix all commands with `sudo`

## Step 1: make sure Snapd is installed

Follow [this page on Snap](https://snapcraft.io/docs/installing-snapd) to install it if needed.

## Step 2: Install Certbot

We will follow the steps from [Certbot website](https://certbot.eff.org/lets-encrypt/snap-nginx)

```bash
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
```

## Step 3: Install and configure Digital Ocean DNS plugin

Let's [install a DNS plugin](https://certbot-dns-digitalocean.readthedocs.io/en/stable/) for Certbot that will automatically configure the DNS entries for renewal.

> Note: I don't know why we need to explicitly install

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

## Step 4: generate certificates

Still with me? Good!

Now we can put all together and generate our certificates:

```bash
certbot certonly \
  --register-unsafely-without-email \
  --dns-digitalocean \
  --dns-digitalocean-credentials ~/digital-ocean.ini \
  -d example.com \
  -d '*.example.com'
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
 
 ```bash
 certbot renew --dry-run
 ```
