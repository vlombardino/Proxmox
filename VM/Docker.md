## VM Install
> Ubuntu server 22.04
```
sudo apt update && apt upgrade
```

### Intall Qemu-Guest-Agent
> Options - QEMU Guest Agent - Enabled
```
sudo apt install qemu-guest-agent
```

### Docker Install
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-compose
```

### Check Versions Installed
```
docker -v

docker-compose -v
```

### Latest Docker Compose (If Needed)
```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

### Allow User To Configure Docker
```
sudo usermod -aG docker $USER

sudo reboot
```

### Docker Containers

> [Portainer](https://hub.docker.com/r/portainer/portainer-ce)
```
docker volume create portainer_data
docker run -d \
	-p 9443:9443 \
	-p 8000:8000 \
	--name portainer \
	--restart always \
	-v /var/run/docker.sock:/var/run/docker.sock \
	-v portainer_data:/data portainer/portainer-ce:latest
```
> [Filebrowser](https://hub.docker.com/r/hurlenko/filebrowser)

```
mkdir -p /home/$USER/docker/filebrowser/config
```
```
docker create \
	--name filebrowser \
	--user $(id -u):$(id -g) \
	-p 8080:8080 \
	-v /home:/data \
	-v /home/$USER/docker/filebrowser/config:/config \
	-e FB_BASEURL=/filebrowser \
	--restart unless-stopped \
	hurlenko/filebrowser
```
