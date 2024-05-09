# Docker Compose for Pihole - Unbound - Redis Setup
## About
This project is designed to support a docker compose deployment of Pihole using Unbound DNS upstream with a persistent Redis cache for Unbound. 

The default configuration is tuned for performance:
- Unbound is configured as a forwarder, see forward-records.conf
- Unbound is configured to serve expired, min ttl = 300/max ttl = 86400, and prefetch on
  
## Getting Started
To run, execute the following command
` docker compose -f "pihole-compose.yml" up -d --build `

## Tuning

### Run these on your host machine to update without restarting
- `sudo sysctl vm.overcommit_memory=1`
- `sudo sysctl -w net.core.wmem_max=8388608`
- `sudo sysctl -w net.core.rmem_max=8388608`

### Edit to host machine sysctl.conf to persist
- `sudo nano /etc/sysctl.conf `
- `net.core.rmem_max=8388608`
- `net.core.wmem_max=8388608`


## Enabling Unbound DNS Remote Control

Run the following on the host machine:
```
    $ mkdir /etc/unbound/keys
    $ unbound-control-setup -d /etc/unbound/keys
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
