# Docker Install

### Operating System & Software
- Proxmox CT
- Ubuntu server 22.04
- Docker

---

### Allow Root Login Over SSH
```
nano /etc/ssh/sshd_config

####################ADD TEXT####################
PermitRootLogin yes
################################################

service ssh restart
```

### Docker Install ([repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository))
```
apt update && apt upgrade

apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

apt update

apt install docker-ce docker-ce-cli containerd.io docker-compose docker-compose-plugin
```

### Docker Install ([script](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script))
Download script from [get.docker.com](https://get.docker.com/)
```
curl -fsSL https://get.docker.com -o get-docker.sh
```
Install docker from the downloaded script
```
sudo sh get-docker.sh
```


### Check Installed Version
```
docker -v

docker-compose -v
```

### If Needed Get Latest Docker Compose
```
curl -L "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```

### Docker Containers
> [Portainer](https://hub.docker.com/r/portainer/portainer-ce)
```
docker volume create portainer_data

docker run -d -p 9000:9000 -p 8000:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
