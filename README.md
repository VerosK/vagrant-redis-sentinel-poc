# Redis Sentinel with HAProxy. Proof of concept

This is proof of concept only. This Vagrantfile sets up three node Redis
Sentinel setup on port 6378.  There is also HAProxy serving on port 6379
which connects you to master Redis.

Based on https://discuss.pivotal.io/hc/en-us/articles/205309388

## how to start

    vagrant up

## Connect to writeable readis

    redis-cli -h 192.168.9.41
    redis-cli -h 192.168.9.42
    redis-cli -h 192.168.9.40

## Connect to read-only readis

    redis-cli -h 192.168.9.41 -p 6378
    redis-cli -h 192.168.9.42 -p 6378
    redis-cli -h 192.168.9.40 -p 6378

## Stop one redis for 20 seconds

    redis-cli -h 192.168.9.41 -p 6378 debug sleep 20

License: WTFPL || 2BSD

