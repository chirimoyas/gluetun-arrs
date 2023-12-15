# gluetun-arrs
Servarr apps routed through a VPN and accessible through Nginx Proxy Manager reverse proxy

This is installed on a Linux instance running Ubuntu 22.04. The docker-compose should be transferable to other linux distros.

Here's how I went about doing this:
1. Be sure docker and docker-compose are installed on your system.
2. Make a directory (e.g. "sudo mkdir servarr") and navigate into the directory ("cd servarr")
3. Create the docker-compose file (sudo nano docker-compose.yml)
4. Paste the text from this repository into your docker-compose.yml file and save.
5. Remaining in the same folder as you docker-compose file, type the following command:

```
docker-compose up -d
```
