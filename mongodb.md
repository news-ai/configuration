1. `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927`
2. `echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list`
3. `sudo apt-get update`
4. `sudo apt-get install -y mongodb-org`
5. `sudo service mongod start`
