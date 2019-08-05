# OpenPGP Web Key Directory Service Website

This is my [Web Key Directory Service (WKD)](https://tools.ietf.org/html/draft-koch-openpgp-webkey-service) website.  This project is intended to display a small page if a user happens to go to the WKD "advanced method" url (ie. https://openpgpkey.mattrude.com) explaining what the service is and how to access more infromation about WKD.

This site is alredy build and only needs to be placed into the webdirectory for your openpgpkey site.  This page auto corrects the site domain when the site loads, via javascript.  All images are imbeaded into the index.html file.

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
    location ^~/README.md { return 404; }
}
```

## Repository License

```
              GNU GENERAL PUBLIC LICENSE
                Version 3, 29 June 2007

OpenPGP Web Key Directory Service website (wkd-website)
Copyright (C) 2019 Matt Rude <matt@mattrude.com>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
```
