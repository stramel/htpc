# htpc
Media Server Compose


Tasks are roughly based on https://github.com/IronicBadger/ansible



### Docker Tasks

- Update System
  - `$ sudo apt-get update`
  - `$ sudo apt-get upgrade`
- Install docker and docker-compose
  -
- Create docker group and add current user to it
  -
- Pull docker-compose file
  - `$ git clone https://github.com/stramel/htpc.git`
- Create group for docker appdata
  - `$ sudo groupadd -g 1050 dockergroup`
- Create user for docker appdata
  - `$ sudo useradd -u 1050 -g 1050 -d /dev/null -s /sbin/nologin dockeruser`
- Create docker appdata base directory
  - `$ sudo mkdir -p /opt/appdata`
  - `$ sudo chown -R dockeruser:dockergroup /opt/appdata`
- Set environment variables for docker containers
  - `$ sudo vim /etc/environment`
  - Add the following lines:
    - `PUID="1050"`
    - `PGID="1050"`
- Copy docker-compose to config directory
  - `$ sudo cp htpc/docker-compose.yml /opt/docker-compose.yml`
- Use docker-compose to bring up apps
  - `$ docker-compose -f /opt/docker-compose.yml up -d`


### File Sharing Tasks

- Update System
  - `$ sudo apt-get update`
  - `$ sudo apt-get upgrade`


- Install Samba
  - `$ sudo apt-get install samba`
- Start Samba service
  - `$ sudo service smbd start`
- Setup Samba config
  -


- Install NFS
  - `$ sudo apt-get install nfs-kernel-server`
- Start NFS service
  - `$ sudo service nfs-kernel-server start`
- Setup NFS config
  -
