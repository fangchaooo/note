# 彻底删除docker

```shell
he uninstallation step mentions:

sudo apt-get purge -y docker-engine
sudo apt-get autoremove -y --purge docker-engine
sudo apt-get autoclean
Note: chasmani adds in the comments:

I had to also add docker-ce, docker.io and docker to the first two lines to completely get rid of
sudo apt-get purge -y docker-engine docker docker.io docker-ce
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce
It adds:

The above commands will not remove images, containers, volumes, or user created configuration files on your host. If you wish to delete all images, containers, and volumes run the following command:
sudo rm -rf /var/lib/docker
Remove docker from apparmor.d:
sudo rm /etc/apparmor.d/docker
Remove docker group:
sudo groupdel docker
As Micah Smith adds in the comments:

You may (as I did) also need to remove runtime files such as /var/run/docker.sock.
Try find / -name '*docker*' to see all of these files. (This searches your entire filesystem, look in at least /var, /run, /etc separately if you prefer not to do this.)
Now, You have successfully deleted docker.


```

