# Using Nginx with Angular Router and deep linking

Angular Router uses the browser's history API to change the current URL based on the route a user access without reloading the entire page.

That's very handy to keep state, but the backend won't be as happy if we reload the page.
URL's that only exist in Angular lands won't work of course, so we need to redirect the URL's to the `index.html` file and reload the app.

This can be done using only an extra line in Nginx rules.

Add this to your Nginx server configuration (or `/etc/nginx/conf.d/default.conf` for the default one):

```
try_files $uri $uri/ /index.html;
```

A simple http server it would look like this:

```
server {
  listen 80;
  server_name localhost;

  location / {
    try_files $uri $uri/ /index.html;
    root /usr/share/nginx/html;
    index index.html;
  }

  include conf/*.conf;
}

```
