# Personal Platform Services

- [Introduction](README.md)
- [Build a server](build_a_server.md)
- [Decommission a server](decommission_a_server.md)

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
