# Welcome to my LiveServer Portfolio Deployment

## Frameworks Used:
* Bootstrap (for styling)

## Setting Up The Server
This server was created through [Digital Ocean](https://wwww.digitalocean.com)
The server is created by making a droplet through this service.

####Settings chose for this server are:

*Ubuntu 12.04.5 x64
*Smallest size Digital Ocean offers
*Created in New York
*No SSH key added

Server is running Apache2 by using the following code.

```
sudo apt-get install apache2
sudo service apache2 restart
sudo pico /etc/apache2/conf.d/security

```
After opening the nano, the following was added in the file.
*ServerName localhost
Then the server must be restarted.

```
sudo service apache2 restart
```

Then access was restricted by uncommenting <Directory/> and adding Options *FollowSymLinks*

```
sudo pico /etc/apache2/conf.d/security
```

Then the server is restarted and ownership of the /www/ file is changed.

```
sudo service apache2 restart
sudo chown -R UserName /var/www
```

GitHub was also installed on the server by doing the following.

```
sudo apt-get install git-core
git config --global user.name “NAME”
git config --global user.email Your@Email.com
git config --list
ssh-keygen -t rsa -C ”YourEmail@example.com”
less ~/.ssh/id_rsa.pub
```

After everything was installed I created my own user by doing the following.

```
adduser Username
adduser Username sudo
```

The key generated above was added to this GitHub.

##Getting the page from desktop to server.
In the server I created a directory for my repository.

```
sudo mkdir /var/repos/
sudo chown MyUserName /var/repos
cd /var/repos && mkdir SiteName.git && cd SiteName.git
git init --bare
```

With the repo created I moved to the hooks folder and configured it so the files would
move to the correct place.

```
cd /var/repos/SiteName.git/hooks/
pico post-receive
```
In the pico file just created I entered the following.

```
#!/bin/sh
GIT_WORK_TREE=/var/www git checkout -f
```
And then after saving I get to get permission of it

```
chmod +x post-receive
```

Now the default index file had to be deleted

```
cd /var/www/
sudo rm -R index.html
```

And that is all for the server side.

##In the local Dev Environment
A local folder must be created for the files to be sent to the server and it must be
set up for github

```
mkdir ~/Desktop/Live/
mkdir ~/Desktop/Live/SiteName.com && cd ~/Desktop/Live/SiteName.com
git init
```
Next the remote must be added for pushing to the server. The first commit must also be done
and pushed.

```
git remote add LiveServer ssh://usernamt@104.236.112.187/var/repos/SiteName.git
git add -A
git commit 'some commit description'
git push LiveServer Master
```

Then this repo must be added to the GitHub. In GitHub, a repo must be created and this will be added
to it by following the instructions from the creation page and this:

```
git remote add github https://github.com/Username/RepoName.git
git push github master
```
And that's it!
