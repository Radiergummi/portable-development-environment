Setup with externally terminated HTTPS
======================================
This setup relies on externally terminated HTTPS, for example using Cloudflare. It serves all endpoints on port 80, using unencrypted HTTP. If you go down this route,  
be careful to not expose your server to any requests apart from your HTTPS terminating proxy!
