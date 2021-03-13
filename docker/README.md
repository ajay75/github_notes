Create the docker group.
sudo groupadd docker
Add your user to the docker group.
sudo usermod -aG docker ${USER}
You would need to loog out and log back in so that your group membership is re-evaluated or type the following command:
su -s ${USER}
docker run hello-world
you may see the following error, which indicates that your ~/.docker/ directory was created with incorrect permissions due to the sudo commands.
To fix this problem, either remove the ~/.docker/ directory (it is recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
sudo systemctl restart docker
sudo chmod 666 /var/run/docker.sock