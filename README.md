
# Download Anything Using Docker Containers

## TL;DR What Is This?
- Download any TV show or movie that's coming out or is already out
- Easily rebuild, modify or add any component you like
- Download privately through a VPN
- Manage your files with a web file browser UI

## What Do You Mean? Give Me An Example
- It's currently November 2019
- New episodes of the TV series "Rick and Morty" come out each week
- One of my containers runs Sonarr which will automatically download new episodes for me as soon as they are available
- I can check a box and hit search to download past episodes too
- The downloads come from torrents from any indexer I choose

## Requirements
- Linux
- Storage
- Some time
- VPN subscription (Optional but highly reccomended)

## Instalation:
- Install Linux and update
```
apt update -y && apt upgrade -y && reboot
```
## Installing Docker
- [Instructions for this are sourced from this page](https://get.docker.com/)
- Run the below and enter your password when prompted
```
curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh
```
- Run this to add your user account to the Docker group
```
sudo usermod -aG docker $USER
```
- Now reboot
```
sudo reboot
```
- Once rebooted try running hello-world as non-root
```
docker run hello-world
```
- The container should run
- This confirms you can run containers without needing to escalate to root each time

## Installing Portainer For Web Container Management
- [Instructions for this are sourced from this page](https://www.portainer.io/installation/)
- Create the portainer config volume
```
docker volume create portainer_data
```
- Run the below to start the container
```
docker run -d --restart always -p 8000:8000 -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```
- Browse to http://serverip:9000 to confirm it works
- So I'd use http://192.168.0.19:9000
- You will be prompted to set a password to log into Portainer
- Create one
- Now select `local` and `connect` to connect to your server's local Docker instance
- You can now manage containers from the web

## Configuring Filebrowser
- Before we can deploy the containers we need to create the DB file for Filebrowser
- Login to linux
- Run
```
sudo mkdir /var/lib/docker/volumes/filebrowser/
sudo touch /var/lib/docker/volumes/filebrowser/filebrowser.db
```

## Deploying The Containers
- Select `stacks` and `add stack`
- Give the stack a name at the top
- I called mine `Download-Stack`
- [Paste the contents of DeploymentScript into it](https://github.com/Madd2dogg/super-adventure/blob/master/DeploymentScript)
- Deploy the stack
- This will take a while depending on your internet connection as it needs to download each container image
- Once done, you will see all containers running in your containers view

## Configuring JDownloader
- To make JDownloader work you need to configure it with your account
- Click on the JDownloader container from your container view
- Click on `console`
- Change the command to `/bin/sh`
- Type
```
configure myemail@address.com myjdownloaderpassword
```
- Go to https://my.jdownloader.org/ and log in
- Your JDownloader container should appear there

## URLs & Logins For Each Container
- Below is a list of URLs for each container and what they do
- Modify for your IP and bookmark them for easy access

### Filebrowser
- For web management of files
- It's bound to port 80 so no port is specified
- http://192.168.0.19
- Username `admin`
- Password `admin`

### Jackett
- For adding torrent indexers to Sonarr and Radarr
- http://192.168.0.19:9117
- No login by default but you can add one

### JDownloader
- For downloading anything
- https://my.jdownloader.org/
- Login is configured in the container as specified above

### NZBget
- For downloading files from a Usenet provider
- http://192.168.0.19:6789
- Username `nzbget`
- Password `tegbzn6789`

### qBittorrent
- For downloading torrents
- http://192.168.0.19:8080
- Username `admin`
- Password `adminadmin`

### Radarr
- For downloading movies
- http://192.168.0.19:7878
- No login by default but you can add one

### Sonarr
- For downloading TV shows
- http://192.168.0.19:8989
- No login by default but you can add one



