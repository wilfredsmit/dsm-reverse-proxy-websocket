# dsm-reverse-proxy-websocket
Configuration fix for Synology DSM 6 reverse proxy to handle websocket

**Starting from DSM 6.2.1, this modification is no longer required, as Application Portail has an option to handle websocket.
It's even not recommended to modify your Portal.mustache file, as it will cause error when adding or edition reverse proxy entries in Application Portal.**

In DSM 6.2.1+, when editing a reverse proxy entry in Application Portal, go to "custom headers" tab and click the arrow on the "Create" button, then "Websocket". This will add the required headers for websocket to this reverse proxy.

**BACKUP YOUR `Portal.mustache` BEFORE MODIFYING IT!**

You need to edit the file `sudo vi /usr/syno/share/nginx/Portal.mustache` to add the followings in the `Location` section:

```
        proxy_set_header        Upgrade $http_upgrade;
        proxy_set_header        Connection "upgrade";
        proxy_read_timeout      86400;
```

Then restart the httpd with: 
```
sudo synoservicecfg --restart nginx
```

*This will restart ALL http service running, not only reverse proxy, as a single instance of NGinX runs everything.*


A modified `Portal.mustache` is provided in this repo (warning: based on DSM 6.2-23739).

# Known DSM upgrade impacts on the `Portal.mustache` file

> In **bold** versions overwriting the `Portal.mustache`

- 6.2-23739-1 : leaves the `Portal.mustache` file unchanged
- **6.2-23739** : overwrites the `Portal.mustache`
- 6.1.6-15266-1 : leaves the `Portal.mustache` file unchanged
- **6.1.6-15266** : overwrites the `Portal.mustache`
- 6.1.3-15152-1 : leaves the `Portal.mustache` file unchanged
- **6.1.3-15152** : overwrites the `Portal.mustache`
- **6.1.2-15132** : overwrites the `Portal.mustache`
- 6.1.1-15101-4 : leaves the `Portal.mustache` file unchanged
- **6.1.1-15101** : overwrites the `Portal.mustache`
