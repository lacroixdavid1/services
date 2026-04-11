Test

```bash

docker network create -d ipvlan --subnet=192.168.110.0/24 --gateway=192.168.110.1 -o parent=enp6s19 -o ipvlan_mode=l2 iot_vlan110
docker network create -d ipvlan --subnet=192.168.130.0/24 --gateway=192.168.130.1 -o parent=enp6s18 -o ipvlan_mode=l2 services_vlan130
docker network create caddy_proxy
docker network create monitoring
docker network create home_automation
docker network create uptime_kuma
docker network create cloudflare_tunnel

```