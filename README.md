## How to deploy pi-hole in ubuntu

## Steps To Install Docker

#### You can choose any machine type you like

- can be bare-bone Ubuntu
- can be on Virtual box, virtual machine
- can be on [Multipass](https://multipass.run/install)

#### After choosing right machine, you need to install Docker

- Step 1 Set up Docker's apt repository. Copy and paste below code:

```js
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

- Step 2 install Docker & Docker Compose

```js
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Running Pi-hole

#### Disable 53 port: Pi-hole runs on port 53, your ubuntu distro may already occupied that port. To disable 53 port follow below steps.

- Copy and paste below code

```js
sudo systemctl stop systemd-resolved && sudo systemctl disable systemd-resolved
```

- Use the command below to begin modifying the configuration file.

```js
sudo nano /etc/resolv.conf
```

- ou will want to find and replace the following line within this file contains "nameserver 127.0.0.53" and replace that with the below line:

```js
nameserver 1.1.1.1
```

#### Make a directory and paste code

- Create a folder

```js
mkdir pihole
```

- CD to that folder

```js
cd pihole
```

- nano a new file (for pasting the code)

```js
nano docker-compose.yml
```

- Paste the below code to text editor

```js
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "8080:80/tcp"
    dns:
      - 127.0.0.1
      - 8.8.8.8
    environment:
      TZ: "Asia/Dhaka"
      WEBPASSWORD: "r@nd0mpa$$"
    volumes:
      - "./etc-pihole:/etc/pihole"
      - "./etc-dnsmasq.d:/etc/dnsmasq.d"
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

- Save and exit (ctrl + x, y, enter)

- run docker composer

```js
docker compose up -d
```
