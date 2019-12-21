December 21, 2019
* First of all we gonna follow this [post](http://pawamoy.github.io/2018/02/01/docker-compose-django-postgres-nginx.html)

# Base Server
* OS: ubuntu-18.04.2-live-server-amd64.iso
* Basic Setup, only OpenSSH

# Install Docker
Following this [post](https://medium.com/@sh.tsang/installation-of-docker-3b18d9e70bea)
```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
```

# Install Compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

# Install Django and create a little project
There are several tutorial for this, we use virtualenv and Pycharm as IDE for this but you can use what ever you want.
```
sudo apt-get install virtualenv
sudo apt-get install git
```
* Then install Pycharm Community version from his [web](https://www.jetbrains.com/es-es/pycharm/download/#section=linux)
and create a project with Pycharm, using a virtual environment and indicate use Python3.6 that originally came with Ubuntu and use
Git to manage versions. 
* In Pycharm Terminal Console, install Django and GUnicorn using PIP, freeze the pip installation and create a Django Project. Test it!.
* For now I'll use plain pip and requirements.txt file.
```
pip install django
pip install gunicorn
pip freeze > requirements.txt
django-admin startproject djdocker
gunicorn --chdir djdocker --bind :8000 djdocker.wsgi:application
```

# Create a Dockerfile
* Create a Dockerfile and Build and Test docker!!
```
sudo docker build . -t djdocker
sudo docker run -p 8000:8000 djdocker
```
