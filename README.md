# Docker Compose for Pihole - Unbound - Redis Setup
## About
This project is designed to support a docker compose deployment of Pihole using Unbound DNS upstream with a persistent Redis cache for Unbound. 

The default configuration is tuned for performance:
- Unbound is configured as a forwarder, see forward-records.conf
- Unbound is configured to serve expired, min ttl = 300/max ttl = 86400, and prefetch= on
  
## Getting Started
To run, execute the following command
` docker compose -f "pihole-compose.yml" up -d --build `

## Accessing the Pihole Web UI
Access at 
`http://{$HOSTNAME}/admin`

If you are running Pihole for the first time you will need to set a password to access the Web UI. 

Attach to the pihole container shell and run the following 

`pihole -a -p`

## Tuning

### Host OS Tuning

These settings should be setup when you first get started so that Redis and Unbound behave as expected.

#### Run these on your host machine to update without restarting (will not persist across restart)
```
sudo sysctl vm.overcommit_memory=1
sudo sysctl -w net.core.wmem_max=8388608
sudo sysctl -w net.core.rmem_max=8388608
```

#### Edit to host machine sysctl.conf to persist
`sudo nano /etc/sysctl.conf `
```
vm.overcommit_memory=1
net.core.rmem_max=8388608
net.core.wmem_max=8388608
```


### Unbound Configuration Tuning

Custom Unbound configuration can be stored at `/unbound-config/custom/*.conf`

Examples can be found at `unbound-config/examples`\

#### Enabling Unbound DNS Remote Control
Run the following on the host machine:
```
chown 1500:1500 unbound-keys
```

Run the following attached to the container:
```
unbound-control-setup -d /etc/unbound/keys
```

Create config/remote-control.conf with the following:

```
remote-control:
    control-enable: yes 
    server-key-file: "/etc/unbound/keys/unbound_server.key"
    server-cert-file: "/etc/unbound/keys/unbound_server.pem"
    control-key-file: "/etc/unbound/keys/unbound_control.key"
    control-cert-file: "/etc/unbound/keys/unbound_control.pem"
```
Once added, restart the container for unbound for the changes to take effect.

### Redis Configuration Tuning

Custom Redis configuration can be stored at `/redis-config/custom/*.conf`

Examples can be found at `redis-config/examples`

Redis and Unbound communicate via unix sockets to reduce overhead

In redis.conf, sockets are configured:

```
unixsocket /tmp/docker/redis.sock
unixsocketperm 777
```
