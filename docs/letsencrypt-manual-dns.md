# Get a letsencrypt certificate manually when you can edit the DNS records

This is a guide to getting a letsencrypt certificate manually when you have access to edit the DNS records.

## Prerequisites

* [Docker](https://docs.docker.com/get-docker/)

1. Create a folder to store the certificate files. For example

    ```bash
    mkdir -p ~/certs/etc/letsencrypt
    mkdir -p ~/certs/var/lib/letsencrypt
    ```

2. Get the plugin

    ```bash
    > curl -o /etc/letsencrypt/acme-dns-auth.py https://raw.githubusercontent.com/joohoi/acme-dns-certbot-joohoi/master/acme-dns-auth.py
    
    > chmod 0700 /etc/letsencrypt/acme-dns-auth.py
    ```

3. Start Certbot

    ```bash
    docker run -it --rm --name certbot \
    -v "/path_to/etc/letsencrypt:/etc/letsencrypt" \
    -v "/path_to/var/lib/letsencrypt:/var/lib/letsencrypt" \
    certbot/certbot certonly --manual \
    --manual-auth-hook /etc/letsencrypt/acme-dns-auth.py \
    --preferred-challenges dns \
    --debug-challenges -d my.webaddress.com
    ```

4. Follow the prompts and create the appropriate cname record in your DNS to authorize the request.

5. The certificates will be stored in `/path_to/etc/letsencrypt:/etc/letsencrypt`