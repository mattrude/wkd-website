# OpenPGP Web Key Directory Service Website

This is my [Web Key Directory Service (WKD)](https://tools.ietf.org/html/draft-koch-openpgp-webkey-service) website.  This project is intended to display a small page if a user happens to go to the WKD "advanced method" url (ie. https://openpgpkey.mattrude.com) explaining what the service is and how to access more infromation about WKD.

This site is already build and only needs to be placed into the web directory for your openpgpkey site.  This page auto corrects the site domain when the site loads, via javascript, only the [Contact](https://github.com/mattrude/wkd-website/blob/master/index.html#L26) link requires updating.  All images are imbeaded into the index.html file.

Please remember to update the [Contact](https://github.com/mattrude/wkd-website/blob/master/index.html#L26) link.

## Nginx Config

This site is inteaded to server mulitaple sites at the same time from the same directory.  You may eather create a new nginx config file per site, or add all the sites to the same file.  The below example assumes the root web directory on your webserver is `/var/www/openpgpkey`.

```
#----------------------------------------------------------------------
# openpgpkey.example.com
#----------------------------------------------------------------------

server {
    listen 80;
    listen [::]:80;
    server_name openpgpkey.example.com;

    location '/.well-known/acme-challenge' {
        default_type "text/plain";
        root /var/www/openpgpkey;
    }

    location / {
        return              301 https://$server_name$request_uri;
    }
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name openpgpkey.example.com;
    root /var/www/openpgpkey;

    ssl_certificate         /etc/letsencrypt/live/openpgpkey.example.com/fullchain.pem;
    ssl_certificate_key     /etc/letsencrypt/live/openpgpkey.example.com/privkey.pem;
    ssl_stapling on;

    error_page 404 /index.html;

    location ^~ /.well-known/ {
        expires 5d;
        default_type        "text/plain";
        add_header          'Access-Control-Allow-Origin' '*' always;
    }

    location ^~/.git { return 404; }
    location ^~/.gitignore { return 404; }
    location ^~/LICENSE { return 404; }
    location ^~/README.md { return 404; }
}
```

## Repository License

```
MIT License

Copyright (c) 2020 Matt Rude <matt@mattrude.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
