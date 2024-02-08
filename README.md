# Docker Nginx Headers SSL

This repository provides a quick and easy way to build a custom Nginx container with secure headers and SSL support using Certbot. The included Docker Compose configuration simplifies the process of building and deploying the Nginx server.

## Getting Started

Follow these steps to set up and run the custom Nginx container:

1. **Clone the Repository:**
    ```bash
    git clone https://github.com/matiasvelizb/docker-nginx-headers-ssl.git
    cd docker-nginx-headers-ssl
    ```

2. **Copy the Environment File:**
    ```bash
    cp .env_example .env
    ```

3. **Build the Docker Image:**
    ```bash
    docker-compose build --no-cache
    ```

4. **Start the Nginx Server:**
    ```bash
    docker-compose up
    ```

The Nginx server with secure headers and SSL support will now be up and running.

## Configuration

The configuration files for Nginx, including headers and SSL setup, can be found in the `containers/nginx/configuration` directory.

### Secure Headers

This repository includes the [headers-more-nginx-module](https://github.com/openresty/headers-more-nginx-module), a powerful Nginx module that extends the standard `headers` module. This module provides additional features for manipulating HTTP headers, allowing you to enhance the security of your web application.

The Nginx configuration in `containers/nginx/configuration/nginx.conf` incorporates the headers-more module, providing you with the ability to customize and secure your headers. Use this module to hide sensitive information from the request for improved security.

### ❌ [WIP] SSL with Certbot

SSL certificates are automatically obtained and renewed using Certbot. The Certbot configuration is present in the `containers/nginx/configuration/certbot.conf` file. Ensure that your domain and email information are correctly set.

## Notes

- The `--no-cache` flag in the build command ensures a fresh build without using the cache.
- Make sure to update the SSL configuration with your specific domain and email information.
- Additional Nginx configuration options can be added or modified in the `nginx-config/nginx.conf` file.

## Contributing

Feel free to contribute to this project by submitting issues or pull requests.
