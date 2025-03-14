# About MellonPass

This project is open source, so everybody can do security reviews, propose improvements, and fork MellonPass.

## Key features

- **End-to-end encryption**: Passwords and data are encrypted and authenticated using a 512-bit symmetric key: AES CTR 256-bit for confidentiality and HMAC 256-bit for integrity.
- **Secure master password generation**: The master password is salted and hashed via the Password-Based Key Derivation Function 2 (SHA-256) and is stretched using the HMAC-based Extract-and-Expand Key Derivation Function (SHA-512). The master password and stretched master passwords are not sent to the server.
- **Zero-knowledge encryption**: Users' vault items are encrypted locally before they are sent to the server. There's no way for MellonPass to see the data, and only you can decrypt them using your master password.
- **DB Column-level encryption**: Each database column that stores cipher texts is encrypted using Fernet (AES-CBC 128-bit, HMAC 256-bit, IV generated from a cryptographic secure random number generator).
- **Supported Vault Items**: Logins and Secure notes only for now. I will add more types in the future.
- **Organization Vaults**: These will be supported in the future!

## MellonPass Repos

### MellonPass Server

A Django-powered API server for serving and storing information securely. It also provides database-level encryption at rest to protect data.

### MellonPass Web

The front-end application is built on top of SvelteKit using TypeScript and has a security feature for safely storing decrypted data and users' encryption/decryption keys.

## Usage

### Docker compose

For docker compose deployment, you need to create your own `docker-compose.yml` file. Below also contains the configuration for the server image.

```yml
networks:
    mp-network:
        driver: bridge

volumes:
    static_cdn_volume:
        driver: local
    server_tmp_volume:
        driver: local

services:
    server: &server
        image: ghcr.io/mellonpass/server:latest
        restart: always
        command: /start_server
        # To avoid duplication, you can move these into .env file
        # and use the `env_file: path/to/.env` directive.
        environment:
            - DOMAIN=example.com
            - DJANGO_SECRET_KEY=your-secure-key
            - DATABASE_URL=postgres://
        volumes:
            - server_tmp_volume:/tmp
            # You can share this volume with your web server e.g. nginx.
            - static_cdn_volume:/code/static_cdn
        ports:
            - 8000:8000
        networks:
            - mp-network

    huey_consumer:
        <<: *server
        restart: always
        # To avoid duplication, you can move these into .env file
        # and use the `env_file: path/to/.env` directive.
        environment:
            - DOMAIN=example.com
            - DJANGO_SECRET_KEY=your-secure-key
            - DATABASE_URL=postgres://
        volumes:
            - server_tmp_volume:/tmp
        command: /start_huey_consumer
        ports: []
        depends_on:
            - server

    web:
        # Use your own web image here.
        # For more information, see: https://github.com/mellonpass/web?tab=readme-ov-file#deployment
        image: your-custom-mp-web-image:tag
        restart: always
        ports:
            - 5173:80
        networks:
            - mp-network
```

You need to persist `server_tmp_volume:/tmp` and share the volume for the server and [Huey](https://huey.readthedocs.io/en/latest/index.html) consumer for async task executions.

The `static_cdn_volume:/code/static_cdn` is relevant only on production setup for your web server to discover your static folder. See nginx example below:

```yml
nginx:
    image: nginx:alpine
    restart: always
    volumes:
        - ./path/to/nginx/conf.d/:/etc/nginx/conf.d/:ro
        - static_cdn_volume:/static_cdn
    ports:
        - 80:80
        - 443:443
    depends_on:
        - server
        - web
    links:
        - server
        - web
    networks:
        - mp-network
```

Then on your nginx `/path/to/nginx/conf.d/app.conf` file:

```
server {

    listen 80 ssl;
    server_name example.com;

    # The Django static_root url.
    location /static/ {
        alias /static_cdn/;
    }

    location / {
        proxy_pass http://localhost:8000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Get in touch

If you have questions or improvements in mind, don't hesitate to reach out to me at eikz.dev@protonmail.com.

Reported bugs are highly appreciated. Please [create an issue]([url](https://github.com/mellonpass/.github/issues)) 🙏
