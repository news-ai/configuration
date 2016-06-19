```
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install tcl8.5
wget http://download.redis.io/releases/redis-stable.tar.gz
tar xzf redis-stable.tar.gz
cd redis-stable
make
make test
sudo make install
cd utils
sudo ./install_server.sh
sudo service redis_6379 start
sudo service redis_6379 stop
redis-cli
```

### Securing Redis

Open the Redis configuration file for editing:

`sudo nano /etc/redis/6379.conf`

Locate this line and make sure it is uncommented (remove the `#` if it exists):

`bind 127.0.0.1`
