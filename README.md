# Docker Compose for Pihole - Unbound - Redis Setup

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
