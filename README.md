# HAProxy-Cheating
A cheating Guide for HA Proxy !

## Installation
- `` apt install --no-install-recommends software-properties-common ``
- `` add-apt-repository ppa:vbernat/haproxy-2.8 -y `` It is the latest LTS Version
- `` sudo apt install haproxy=2.4.\* `` Maintain Future Upgrades
## Default Config
- The config file path: `` /etc/haproxy/haproxy.cfg ``
- It contains many lines, do not touch them !

## HTTP Mode
use HTTP Mode if you want to:
- routing traffic to particular servers based on the URL, HTTP headers or body
- modifying the URL and headers
- reading and setting cookies
- determining the health of a server based on its HTTP responses

## Forwarding Client IP Address
```bash
option forwardfor if-none
http-request set-header Forwarded for=%[src]
```
