# Nginx Traffic Filter Proof of Concept

This is a proof of concept that shows how to use Nginx to filter HTTP and HTTPS traffic by domain without breaking SSL.

## What It Does

This setup has three main parts:

1. Router: Forwards traffic and handles DNS.
2. Nginx: Filters traffic based on allowed domains.
3. Client: Used for testing.

These parts run in Docker containers.

## How It Works

1. Router:
   - Acts as a gateway.
   - Sends HTTP and HTTPS traffic to Nginx.
   - Provides DNS for the client.

2. Nginx:
   - Filters HTTP traffic using a list of allowed domains.
   - Filters HTTPS traffic without decrypting it.

3. Client:
   - Acts like a computer on the internal network.
   - Uses the router for internet access and DNS.

## How to Set It Up

1. Install Docker and Docker Compose on your computer.
2. Clone the repo
3. Run this command to start:
   ```
   docker-compose up -d
   ```

## How to Test

1. Get access to the client container:
   ```
   docker-compose exec client sh
   ```

2. In the client container, try these tests:
   - Check DNS:
     ```
     nslookup example.com
     ```
   - Try an allowed website (HTTP):
     ```
     curl -v http://ifconfig.me
     ```
   - Try an allowed website (HTTPS):
     ```
     curl -v https://ifconfig.me
     ```
   - Try a blocked website (should not work):
     ```
     curl -v http://example.com
     ```
   - Try a blocked https website (should not work):
     ```
     curl -v https://example.com
     ```

## Allowed Websites

These websites are allowed:
- ifconfig.me

To change this list, edit the `nginx.conf` file and restart the containers.