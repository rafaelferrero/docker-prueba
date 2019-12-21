December 21, 2019
* First of all we gonna follow this [post](http://pawamoy.github.io/2018/02/01/docker-compose-django-postgres-nginx.html)
and his [repository](https://github.com/Pawamoy/docker-nginx-postgres-django-example)
that is updated... Thanks to [@Pawamoy](https://github.com/Pawamoy) !!!

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

# Install Django and create a little project with a little app in it for use a Database
There are several tutorial for this, we use virtualenv and Pycharm as IDE for this but you can use what ever you want.
```
sudo apt-get install virtualenv
sudo apt-get install git
```
* Then install Pycharm Community version from his [web](https://www.jetbrains.com/es-es/pycharm/download/#section=linux)
and create a project with Pycharm, using a virtual environment and indicate use Python3.6 that originally came with Ubuntu and use
Git to manage versions. 
* In Pycharm Terminal Console, install Django and GUnicorn using PIP, freeze the pip installation and create a Django Project. Test it!.
* For now I'll use plain pip and requirements.txt file. I don't feel confortable with Pipenv
* We will use PostgreSQL as database we need psycopg2
```
pip install django
pip install gunicorn
psycopg2-binary
pip freeze > requirements.txt
django-admin startproject djdocker
./djdocker/manage.py startapp polls
gunicorn --chdir djdocker --bind :8000 djdocker.wsgi:application
```

# Create a Dockerfile and a Compose file too for use with Nginx and PostgreSQL
* Create a Dockerfile, docker-compose.yml
* Create config files for Nginx and PostgreSQL
* Edit settings.py from Django Project
* Build and Test docker!!
```
sudo docker-compose build
sudo docker-compose run --rm djdocker /bin/bash -c "./djdocker/manage.py makemigrations"
sudo docker-compose run --rm djdocker /bin/bash -c "./djdocker/manage.py migrate"
sudo docker-compose run djdocker ./djdocker/manage.py collectstatic --no-input
sudo docker-compose up
```
