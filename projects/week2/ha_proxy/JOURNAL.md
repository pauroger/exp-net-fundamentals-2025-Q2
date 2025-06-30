# Journal

## HAProxy

**HAProxy** stands for High Availability Proxy.

It is a software-based load balancer and reverse proxy commonly used to
distribute network traffic across multiple backend servers.

### Why is HAProxy used?

#### Load Balancing

Distributes incoming traffic across multiple backend servers to ensure:

- No single server gets overwhelmed.
- Better performance, higher concurrency.

#### High Availability (HA)

- If one backend server fails, HAProxy automatically reroutes traffic to healthy
servers.
- Ensures service uptime and reliability.

#### Reverse Proxy

- Acts as a middleman between clients and backend servers.
- Hides the internal architecture from the outside world.

### Security Layer

- Filters bad traffic.
- Can handle SSL/TLS termination — clients connect via HTTPS to HAProxy, and
  HAProxy forwards traffic to backend servers.

#### Session Persistence (Sticky Sessions)

Optionally ensures a user keeps being routed to the same backend server (useful
for session-based applications).

#### Traffic Management

Rate limiting, connection throttling, access control, and monitoring.

## Lab

I had to copy the HA files into my Ubuntu box with the command, from my local
machine:

```sh
scp -r -i ~/.ssh/azure-ubuntu-vm-key-pair.pem \
<ha-proxy-path> azureuser@<publiciip>:/home/azureuser/
```

Then Created the web1 and 2 folders with their respective HTML files.

Then I intalled and ran Docker:

```sh
root@ubuntu-vm:/home/azureuser/ha-proxy# containerlab deploy
17:30:20 INFO Containerlab started version=0.68.0
17:30:20 INFO Parsing & checking topology file=haproxylab.clab.yaml
17:30:20 INFO Creating docker network name=clab IPv4 subnet=172.20.20.0/24 IPv6 subnet=3fff:172:20:20::/64 MTU=1500
17:30:21 INFO Pulling docker.io/haproxytech/haproxy-ubuntu:latest Docker image
17:30:33 INFO Done pulling docker.io/haproxytech/haproxy-ubuntu:latest
17:30:33 INFO Pulling docker.io/ubuntu/apache2:latest Docker image
17:30:45 INFO Done pulling docker.io/ubuntu/apache2:latest
17:30:45 INFO Creating lab directory path=/home/azureuser/ha-proxy/clab-basic-haproxylab1
17:30:46 INFO Creating container name=web2
17:30:46 INFO Creating container name=web1

(...)

17:33:38 INFO Adding host entries path=/etc/hosts
17:33:38 INFO Adding SSH config for nodes path=/etc/ssh/ssh_config.d/clab-basic-haproxylab1.conf
╭────────────────────────────────┬───────────────────────────────────┬─────────┬───────────────────╮
│              Name              │             Kind/Image            │  State  │   IPv4/6 Address  │
├────────────────────────────────┼───────────────────────────────────┼─────────┼───────────────────┤
│ clab-basic-haproxylab1-haproxy │ linux                             │ running │ 172.20.20.4       │
│                                │ haproxytech/haproxy-ubuntu:latest │         │ 3fff:172:20:20::4 │
├────────────────────────────────┼───────────────────────────────────┼─────────┼───────────────────┤
│ clab-basic-haproxylab1-web1    │ linux                             │ running │ 172.20.20.2       │
│                                │ ubuntu/apache2:latest             │         │ 3fff:172:20:20::2 │
├────────────────────────────────┼───────────────────────────────────┼─────────┼───────────────────┤
│ clab-basic-haproxylab1-web2    │ linux                             │ running │ 172.20.20.3       │
│                                │ ubuntu/apache2:latest             │         │ 3fff:172:20:20::3 │
╰────────────────────────────────┴───────────────────────────────────┴─────────┴───────────────────╯
```

In the Windows box. Add a route to the Ubuntu Box via:

```sh
route add 172.20.20.0 mask 255.255.255.0 10.100.0.17 IF 2
```

I also had to add a new row to the Route table within Azure:

![Route Table](assets/routing_in_azure.png)

Next I SSH'd into `clab-basic-haproxylab1-haproxy` via:

```sh
root@ubuntu-vm:/home/azureuser/ha-proxy# ssh demo@172.20.20.4

(...)

demo@haproxy:~$
```

Proof of work:

![POW](assets/ha_proxy_ssh.png)

Finally, I was then able to get the html files from web1 and 2:

```sh
demo@haproxy:~$ curl http://172.20.20.2
<HTML>
<p>This is Web1</p>
</HTML>

demo@haproxy:~$ curl http://172.20.20.3
<HTML>
<p>This is the second web</p>
</HTML>
demo@haproxy:~$
```
