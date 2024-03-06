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
- with ``req.hdr(<Header_Name>)`` can get a header with its name

## Redirecting
- Use `` redirect prefix ``
- By default, HAProxy uses an HTTP response code 302
```text
301 The current and all future requests should use the new URL.

302 The current request should be forwarded to the new URL. Future requests should use the original URL.

303 The current request should be forwarded to the new URL, but as a GET. For example, if a client sends a POST request to a form that submits a shopping cart, they could be redirected to another page that shows details of the purchase, but using a GET request. This page can be bookmarked, refreshed, etc. without resubmitting the form. This is available in HTTP/1.1.

307 The current request should be forwarded to the new URL, but do not change the HTTP verb used. For example, if POST was used before, use POST again. Future requests should use the original URL. This is available in HTTP/1.1.

308 The current and all future requests should use the new URL. Do not change the HTTP verb used. For example, if POST was used before, use POST again. This is available in HTTP/1.1.

```