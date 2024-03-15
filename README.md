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

#### Pi-hole runs on port 53, your ubuntu distro may already occupied that port. To disable 53 port follow below steps.

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
