# Personal Platform Services

- [Introduction](README.md)
- [Build a server](build_a_server.md)
- [Decommission a server](decommission_a_server.md)

# Build a publicly accessible server like AWS

This document helps build a server on your local machine so that you can use this as a Cloud server like AWS, Google Cloud or Azure and host as many applications as you want from the server. And this can all be done for **FREE** without any expensive third party intervention. The only hidden costs are the power bill, the internet bill and the cost for static IP which will have to be paid for running the local machine.

Note: The below steps are for MacBook (Intel/M1/M2 chip)

## Prerequisites:

Static IP: You can request for this from your Internet Provider who will provide it to you for a monthly fee.
Domain: You must have a domain of your own of which we will be making subdomains.

In the below steps, you will see a lot of use of <domain> and <subdomain name>. <domain> refers to the domain you have purchased for yourself. And <subdomain name> refers to each subdomain that you are going to create to host each of your applications on.

For example: Say your domain is `buyguru.in` and your subdomain name is `app1`. If the document says `https://<subdomain name>.<domain>`, it means `https://app1.buyguru.in` for you.

## Step 1: DNS Configuration

You need to tell the internet that your subdomains point to your specific hardware.
Access your Registrar: Log in to the dashboard where you bought your domain.
- Create "A" Records: For every application you want to host, add a new "A" record:
- Host/Name: <subdomain name> (like app2, blog, etc.).
- Value/Points to: Enter your Static Public IP address.TTL: Leave as "Auto" or "3600".

## Step 2: Install and Configure Nginx

Use Homebrew to manage Nginx on your Mac.

**Install Nginx:**
```
brew install nginx
brew services start nginx
```

**Locate Configuration:** On modern Macs (Apple Silicon), the config is at `/opt/homebrew/etc/nginx/nginx.conf`. On Intel Macs, it is at `/usr/local/etc/nginx/nginx.conf`.
- Create Server Blocks: Instead of editing the main file, create a new file for your app:
- Create a directory if it doesn't exist: mkdir -p /opt/homebrew/etc/nginx/servers/
- Create a file: nano /opt/homebrew/etc/nginx/servers/<subdomain name>.conf

Paste this Reverse Proxy template:
```
server {
    listen 80;
    server_name <subdomain name>.<domain>;

    location / {
        proxy_pass http://localhost:3000; # Replace 3000 with your app's port
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
**Test and Reload:**

sudo nginx -t     # Checks for syntax errors
sudo nginx -s reload

## Step 3: Router Port Forwarding

Since you have a static IP, you must "open the doors" on your router to allow web traffic through to your MacBook.
- Login to Router: Usually at `192.168.1.1` or `192.168.0.1` in your browser.
- Find Port Forwarding: Look for "Virtual Server," "NAT Forwarding," or "Port Forwarding".

Add Rules:
- Service: HTTP | Port: 80 | Protocol: TCP | Device: Your MacBook's Local IP.
- Service: HTTPS | Port: 443 | Protocol: TCP | Device: Your MacBook's Local IP.
- Set a "DHCP Reservation" in your router for your MacBook so its local IP never changes.

## Step 4: Secure with SSL (HTTPS)Use Certbot to get free certificates.

Install Certbot:
```
brew install certbot
```

Generate Certificate: Run the following and follow the prompts. It will automatically update your Nginx files to handle HTTPS:
```
sudo certbot --nginx -d <subdomain name>.<domain>
```

**Auto-Renewal:**

Certificates last 90 days. Certbot usually sets up a timer, but you can test it with sudo certbot renew --dry-run.

# Decommissioning your server

## Close the "Front Door" (Router)

This is the most important step for security.
- Log into your router and Delete the Port Forwarding rules for ports 80 and 443.
- This ensures that no one on the internet can reach your MacBook, even if Nginx is still running.

## Stop and Disable Nginx

You want to stop the background processes from consuming CPU and memory.

- Stop the service:
```
sudo brew services stop nginx
```
- Verify it's stopped: Run `sudo lsof -i :443` again; it should return nothing.
- If you want to delete the files completely: `brew uninstall nginx`.

## Update your DNS Records

To prevent people from trying to connect to your home IP address:
- Log into your domain registrar (where you manage buyguru.in).
- Delete the A Records for mapguessr and any other subdomains you created.
- It may take up to 24–48 hours for these changes to "propagate" across the internet.

## Remove SSL Certificates

If you aren't using them, it's best to remove the local copies of your security keys.
- Revoke and Delete:
```
sudo certbot delete --cert-name mapguessr.buyguru.in
```
- This removes the files from `/etc/letsencrypt/`.

## Re-enable macOS Firewall

If you turned it off earlier, turn it back on to protect your Mac while you use it normally.
- System Settings > Network > Firewall > ON.

## Power Settings

Go to System Settings > Battery (or Energy Saver) and re-enable "Automatic sleeping" so your MacBook doesn't stay awake 24/7 and waste electricity/battery.
