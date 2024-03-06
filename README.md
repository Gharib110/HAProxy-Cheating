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

## HAProxy Algorithms
- Round-Robin
- Weighted Round-Robin
- Weighted/Normal LeastConn
- Weighted/Normal Hash URI
- First Available

## Content Switching on URL Path
Fetch methods and What they do
- path exact string match
- path_beg URL begins with string
- path_dir subdirectory match
- path_end suffix match
- path_len length match
- path_reg regex match
- path_sub substring match

## Content Switching on URL Params
- ``acl iswest url_param(region) -i -m str west`` Exact match `` http://mysite.com/?region=west `` and `` -i `` means case-insensitive
- Match multiple strings : `` acl iswest url_param(region) -i -m str west westcoast wc ``
- Regex Matching : `` acl iswest url_param(west) -m reg .+ ``

## Content Switching on HTTP Headers
- `` acl ismobile req.hdr(User-Agent) -i -m reg (android|iphone) `` Filter the Mobile User-Agents and use a seperate backend for them. reg says contains android or iphone
- Filter based on the Host Header : `` acl ischeapshoes req.hdr(Host) -i -m str www.cheapshoes.com ``, `` acl isexpensiveshoes req.hdr(Host) -i -m str www.expensiveshoes.com ``
- Match more host on on ACL Rule : `` acl ischeapshoes req.hdr(Host) -i -m str cheapshoes.com www.cheapshoes.com ``
- 