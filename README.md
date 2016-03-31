# htpc
Media Server Compose


Tasks are roughly based on https://github.com/IronicBadger/ansible

### MergerFS

https://github.com/trapexit/backup-and-recovery-howtos/

- Install MergerFS
  - `$ mkdir mergerfs;cd mergerfs`
  - `$ wget https://raw.githubusercontent.com/IronicBadger/ansible/master/roles/mergerfs/files/build-container`
  - `$ wget https://raw.githubusercontent.com/IronicBadger/ansible/master/roles/mergerfs/files/Docerfile`
  - `$ sudo chmod +x build-container`
  - `$ sudo ./build-container`
  - `$ sudo dpkg -i artifact/mergerfs-from-source.deb`
  - `$ cd ../;sudo rm -rf mergerfs`
- Format the drives
  - `$ sudo mkfs.ext4 -m 0 -T largefile /dev/sd{b,c,d,e,f}`
- Create mount locations
  - `$ sudo mkdir /mnt/disk{0,1,2,3,4}`
  - `$ sudo mkdir /mnt/parity0`
  - `$ sudo mkdir /mnt/storage`
- Add to fstab **USE CAUTION WITH THIS STEP**
  - `$ sudo blkid`
  - `$ sudo vim /etc/fstab`

    ```
    # <file system>  <mount point>      <type>  <options>                              <dump>  <pass>
    /dev/sdb         /mnt/parity0        auto    defaults,nobootwait,errors=remount-ro  0       2
    /dev/sd{c,d,e}   /mnt/disk{0,1,2,3}  auto    defaults,nobootwait,errors=remount-ro  0       2
    ```
- Add to fstab **USE CAUTION WITH THIS STEP**
  - `$ sudo vim /etc/fstab`
  - `$ /mnt/disk* /mnt/storage fuse.mergerfs defaults,allow_other,direct_io,moveonenospc=true 0 0`


### SnapRAID

- Install SnapRAID
  - `$ mkdir snapraid;cd snapraid`
  - `$ wget https://raw.githubusercontent.com/IronicBadger/ansible/master/roles/snapraid/files/build-container`
  - `$ wget https://raw.githubusercontent.com/IronicBadger/ansible/master/roles/snapraid/files/Docerfile`
  - `$ sudo chmod +x build-container`
  - `$ sudo ./build-container`
  - `$ sudo dpkg -i artifact/snapraid-from-source.deb`
  - `$ cd ../;sudo rm -rf snapraid`
- Download snapraid-runner
  - `$ cd /opt/`
  - `$ sudo git clone https://github.com/Chronial/snapraid-runner.git`
- Add snapraid config
  - `$ cd /etc/`
  - `$ sudo wget https://raw.githubusercontent.com/IronicBadger/ansible/master/roles/epsilon/files/etc/snapraid.conf`
  - Modify to fit build
- Setup snapraid-runner config
  - `$ cd /opt/snapraid-runner`
  - `$ sudo wget https://raw.githubusercontent.com/IronicBadger/ansible/master/roles/epsilon/templates/opt/snapraid-runner.j2 -O snapraid-runner.conf`
  - Modify to fit build
- Add snapraid-runner to cron
  - `$ crontab -e`
  - Add `00 03 * * * python2 /opt/snapraid-runner/snapraid-runner.py -c /opt/snapraid-runner/snapraid-runner.conf`
- Sync SnapRAID
  - `$ sudo snapraid sync`

### Docker Tasks

https://gist.github.com/wdullaer/f1af16bd7e970389bad3
docker-compose > 1.4.0

- Update System
  - `$ sudo apt-get update`
  - `$ sudo apt-get upgrade`
- Install docker and docker-compose (https://docs.docker.com/v1.8/installation/ubuntulinux/)
  - `$ sudo apt-get install docker-engine docker-compose`
- Create docker group and add current user to it
  - `$ sudo usermod -aG docker $USER`
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
