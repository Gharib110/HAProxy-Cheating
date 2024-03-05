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
- We can add a header called X-Forwarded-For to the incoming request before it's passed along to the Web server `` option forwardfor if-none ``
- Also add Forwarded for Header. Forwarded was officially proposedin RFC 7239 and serves the same purpose as X-Forwarded-For
```bash
option forwardfor if-none
http-request set-header Forwarded for=%[src]
```
## Enabling Stat Page
- Add `` stats enable `` to defaults section to enable it
- Go to `` /haproxy?stats `` of the target frontend