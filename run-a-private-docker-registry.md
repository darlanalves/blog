# How to run a private Docker registry

## Step by step

The official documentation [is here](https://docs.docker.com/registry/deploying/) with a good step by step to begin with.

Try following the steps there. The steps below worked for me, using Nginx for SSL termination and LetsEncrypt for certificates.

Or just try the following commands:

```
mkdir registry && cd registry
mkdir auth certs storage

// replace USERNAME and PASSWORD here with credentials for your registry
docker run --entrypoint htpasswd registry:2 -Bbn "USERNAME" "PASSWORD" > auth/htpasswd

```

## Securing the registry

You can use [`letsencrypt`](https://letsencrypt.org/) to get a valid SSL password and secure your registry.
You need to set up SSL to make it work.

After getting valid SSL certificates, they will probably be available at `/etc/letsencrypt/live/yourdomain.com`.
So let's copy them to our registry folder:

```
cp /etc/letsencrypt/live/yourdomain.com/fullchain.pem certs/docker.crt
cp /etc/letsencrypt/live/yourdomain.com/privkey.pem certs/docker.key
```

## Running the registry

Now that SSL certificates are ready and we created a password to protect our registry, let's run it!

```
export REGISTRY_HTTP_SECRET="some-very-long-random-string"

docker run --rm -d \
  -p 3000:443 \
  --name registry \
  -v $(pwd)/storage:/var/lib/registry \
  -v "$(pwd)"/auth:/auth \
  -e REGISTRY_LOG_LEVEL=debug \
  -e "REGISTRY_AUTH=htpasswd" \
  -e REGISTRY_HTTP_SECRET \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -v "$(pwd)"/certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/docker.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/docker.key \
  registry:2
```


