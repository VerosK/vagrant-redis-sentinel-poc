# Redis Sentinel with HAProxy. Proof of concept

This is proof of concept only. This Vagrantfile sets up three nodes 
192.168.9.41, 192.168.9.42 a 192.168.9.40.  On all of these nodes
there is Redis Sentinel setup wit Redis on port 6378 and Redis Sentinel
on port 5001. 

There is also HAProxy serving on port 6379 which connects you to 
master Redis. 

For easier debugging, you can also use floating adress 192.168.9.64 which 
is guaranteed to be online.

HAProxy+Redis setup based on https://discuss.pivotal.io/hc/en-us/articles/205309388


## how to start

    git clone --recursive https://github.com/VerosK/vagrant-redis-sentinel-poc redis-cluster
    cd redis-cluster; vagrant up 

## Connect to writeable readis

    redis-cli -h 192.168.9.64

## Connect to read-only readis

Two of these are read-only, third should be writeable.

    redis-cli -h 192.168.9.41 -p 6378
    redis-cli -h 192.168.9.42 -p 6378
    redis-cli -h 192.168.9.40 -p 6378

## Stop one redis for 20 seconds

    redis-cli -h 192.168.9.41 -p 6378 debug sleep 20

License: WTFPL || 2BSD

