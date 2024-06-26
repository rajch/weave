---
title: Using The Weave Docker API Proxy
menu_order: 10
search_type: Documentation
---


When containers are created via the Weave Net proxy, their entrypoint is
modified to wait for the Weave network interface to become
available.

When they are started via the Weave Net proxy, containers are
[automatically assigned IP addresses]({{ '/tasks/ipam/ipam' | relative_url }}) and connected to the
Weave network.

### Creating and Starting Containers with the Weave Net Proxy

To create and start a container via the Weave Net proxy run:

    host1$ docker run -ti alpine:latest

or, equivalently run:

    host1$ docker create -ti --name=foo alpine:latest
    host1$ docker start foo

Specific IP addresses and networks can be supplied in the `WEAVE_CIDR`
environment variable, for example:

    host1$ docker run -e WEAVE_CIDR=10.2.1.1/24 -ti alpine:latest

Multiple IP addresses and networks can be supplied in the `WEAVE_CIDR`
variable by space-separating them, as in
`WEAVE_CIDR="10.2.1.1/24 10.2.2.1/24"`.


### Returning Weave Network Settings Instead of Docker Network Settings

The Docker NetworkSettings (including IP address, MacAddress, and
IPPrefixLen), are still returned when `docker inspect` is run. If you want
`docker inspect` to return the Weave NetworkSettings instead, then the
proxy must be launched using the `--rewrite-inspect` flag.

This command substitutes the Weave network settings when the container has a
Weave Net IP. If a container has more than one Weave Net IP, then the inspect call
only includes one of them.

    host1$ weave launch --rewrite-inspect

### Multicast Traffic and Launching the Weave Proxy

By default, multicast traffic is routed over the Weave network.
To turn this off, for example, because you want to configure your own multicast
route, add the `--no-multicast-route` flag to `weave launch`.

### Other Weave Proxy options

 * `--without-dns` -- stop telling containers to use [WeaveDNS]({{ '/tasks/weavedns/weavedns' | relative_url }})
 * `--log-level=debug|info|warning|error` -- controls how much
   information to emit for debugging
 * `--no-restart` -- remove the default policy of `--restart=always`, if
   you want to control start-up of the proxy yourself

### Disabling Weave Proxy

If for some reason you need to disable the proxy, but still want to start other Weave Net components (router, weaveDNS), you can do so using:

    weave launch --proxy=false


**See Also**

 * [Setting Up The Weave Docker API Proxy]({{ '/tasks/weave-docker-api/weave-docker-api' | relative_url }})
 * [Securing Docker Communications With TLS]({{ '/tasks/weave-docker-api/securing-proxy' | relative_url }})
