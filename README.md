# gluetun-arrs
Servarr apps routed through a VPN and accessible through Nginx Proxy Manager reverse proxy

This is installed on a Linux instance running Ubuntu 22.04. The docker-compose should be transferable to other linux distros. [The guide that helped me a lot is here](https://wiki.servarr.com/).

Here's how I went about doing this:
###Setting up Docker
1. Be sure docker and docker-compose are installed on your system. Follow the directions here for installing docker: https://docs.docker.com/engine/install/ubuntu/. Follow the directions here for installing docker-compose: https://docs.docker.com/compose/install/

###Creating your Docker stack for Servarr apps
3. Make a directory and navigate into the directory
```
sudo mkdir servarr
cd servarr
```

4. Create the docker-compose file
```
sudo nano docker-compose.yml
```

5. Paste the text from this repository into your docker-compose.yml file and save.
6. Remaining in the same folder as you docker-compose file, type the following command:

```
docker-compose up -d
```
After this your stack should be running, which you will be able to verify by inspecting any of the containers.
```
docker inspect gluetun
```
You can also confirm that the servarr apps are running on a vpn by execing into the container:
```
docker exec -it prowlarr curl ipinfo.io
```
###Install [Nginx Proxy Manager](https://github.com/NginxProxyManager/nginx-proxy-manager)
7. Next we have to install Nginx Proxy Manager. I like to keep my app folders separate, so I created a new folder:
```
sudo mkdir ~/npm
cd ~/npm
```
8. Make a new docker-compose.yml file in this folder:
```
sudo nano docker-compose.yml
```
9. Paste the contents of the [/nginx-proxy-manager/docker-compose.yml](https://github.com/chirimoyas/gluetun-arrs/blob/main/nginx-proxy-manager/docker-compose.yml) into this file.
10. Fire up the container:
```
docker-compose up -d
```
Now Nginx Proxy Manager (NPM) should be running on your system at port 81.

###Important note for this section: if you are using a remote machine, I suggest installing [Tailscale](https://github.com/tailscale/tailscale) or [Zerotier](https://github.com/zerotier/ZeroTierOne) before continuing. Using NPM also requires you to have a domain name. It's possible to get these for free or for a very small annual fee. I will not walk through the specifics of getting this set up, so [here's a link about how to do it with Cloudlfare](https://developers.cloudflare.com/registrar/get-started/register-domain), which is my tool of choice. **You need to already have a subdomain set up** (e.g. https://my.domain.com)

11. Open up a browser and navigate to port 81 of your machine. If you are on the machine on which your docker container is running, you can go directly to http://localhost:81. If you are on a remote system, use its IP address to access it http://IP-of-remote-machine:81. Tailscale and Zerotier both make this very easy. If you are using [NordVPN](https://nordvpn.com/download/linux/#install-nordvpn)https://nordvpn.com/download/linux/#install-nordvpn, they also have a mesh network that you can use.
12. Log in to NPM. The default credentials for your first login are 'admin@example.com' and 'changeme'. Change your password when you enter and record it in your password manager.
13. Choose "Hosts" from the menu and then "Add Proxy Host"
14. Enter the name of your subdomain in "domain names" (e.g. sonarr.mydomain.com)
15. Enter the gluetun container name followed by the port of the service you are setting up. You're using the gluetun container name because the other containers are subsumed beneath the VPN and can only be referred to by port number. You do not need to open these ports on your router.
```
Sonarr: 8989
Radarr: 7878
Lidarr: 8686
Readarr: 8787
Whisparr: 6969
Prowlarr: 9696
```

16. I toggle all the options on. Click continue.
17. Under SSL
