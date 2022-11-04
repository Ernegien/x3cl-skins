# Xecuter 3 Config Live Skins Database

This project aims to restore the X3 Config Live skin download feature managed via this repo.

## Kernel Patch DIY

WARNING: be sure you can recover in the case of a bad flash, use at your own risk!

1. Use XBTool to unpack the X3 bios image.
2. Use [X3Util](https://github.com/Ernegien/X3Util) to extract the `x3control.xbe` from the `remainder.bin` file; it contains the encrypted/compressed X3 Config Live executable.
3. Use any hex editor to perform a string replace of `www.allxboxskins.com` with `x3cl.xboxarchive.org`.
4. Use X3Util to inject the modified `x3control.xbe` back into the `remainder.bin`.
5. Repack the X3 bios image with XBTool, being sure to select `Multi` as the BIOS Type.

## Hosting

Due to the X3 web server code only supporting basic HTTP and GitHub Pages being HTTPS-only, we utilize the following NGINX proxy configuration to forward back to the contents of this repo.

```
server {
	listen 80;
	server_name x3cl.xboxarchive.org;

	location / {
		proxy_pass              https://raw.githubusercontent.com/Ernegien/x3cl-skins/main/;
		proxy_redirect          default;
		proxy_buffering         off;
		proxy_set_header        Host                    raw.githubusercontent.com;
		proxy_set_header        X-Real-IP               $remote_addr;
		proxy_set_header        X-Forwarded-For         $proxy_add_x_forwarded_for;
		proxy_set_header        X-Forwarded-Protocol    $scheme;
	}
}
```