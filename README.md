![luacheck](https://github.com/S-S-X/qos/workflows/luacheck/badge.svg)
![mineunit](https://github.com/S-S-X/qos/workflows/mineunit/badge.svg)
![](https://byob.yarr.is/S-S-X/qos/coverage)

## Minetest HTTP API QoS control queue

### Usage:

Function `QoS` returns HTTP API wrapped within simple request priority manager.
`HttpAPI QoS(http_api, default_priority = 3)`

Simply wrap `minetest.request_http_api` with `QoS` and you're good to go.

```lua
local http = minetest.request_http_api()

http = QoS and QoS(http, 2) or http
```

or alternative if you like:
```lua
local http = QoS and QoS(minetest.request_http_api(), 2) or minetest.request_http_api()
```

### Priorities

QoS comes with 3 priority levels without interesting names, priority levels are 1, 2 and 3.

Lowest number is highest priority and priority 1 requests can cause queue to get clogged.

Priorities 2 and 3 will always leave empty space for priority 1 requests and cannot completely fill queues ever.

Queued requests will be processed in first in, first out order but in a way that queued highest priority requests
will be handled first and lower priorities only if there's still free slots for parallel processing.

### Extras

QoS provides `priority` as last argument for HTTP API functions, this can be used to override default priority.
