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
