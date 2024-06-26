---
title: Load Balancing and Fault Resilience with weaveDNS
menu_order: 20
search_type: Documentation
---



It is permissible to register multiple containers with the same name:
weaveDNS returns all addresses, in a random order, for each request.
This provides a basic load balancing capability.

Expanding the [overview example]({{ '/tasks/weavedns/weavedns' | relative_url }}), let us start an additional `pingme` container on a second host, and then run
some ping tests.

```
host2$ weave launch $HOST1
host2$ eval $(weave env)
host2$ docker run -dti --name=pingme alpine:latest

root@ubuntu:/# ping -nq -c 1 pingme
PING pingme.weave.local (10.32.0.2) 56(84) bytes of data.
...
root@ubuntu:/# ping -nq -c 1 pingme
PING pingme.weave.local (10.40.0.1) 56(84) bytes of data.
...
root@ubuntu:/# ping -nq -c 1 pingme
PING pingme.weave.local (10.40.0.1) 56(84) bytes of data.
...
root@ubuntu:/# ping -nq -c 1 pingme
PING pingme.weave.local (10.32.0.2) 56(84) bytes of data.
...
```

Notice how the ping reaches different addresses.

However, due to
[RFC 3484 address selection](https://tools.ietf.org/html/rfc3484#section-6)
most DNS resolver libraries prefer certain addresses over others, to
the point where in some circumstances the same address is always
chosen. To avoid this behaviour, applications may want to perform
their own address selection, e.g. by choosing a random entry from the
result of
[`getaddrinfo()`](http://pubs.opengroup.org/onlinepubs/9699919799/functions/getaddrinfo.html).

## <a name="fault-resilience"></a>Fault Resilience

WeaveDNS removes the addresses of any container that dies. This offers
a simple way to implement redundancy. E.g. if in our example we stop
one of the `pingme` containers and re-run the ping tests, eventually
(within ~30s at most, since that is the weaveDNS
[cache expiry time]({{ '/tasks/weavedns/managing-entries-weavedns#ttl)' | relative_url }}) we will only be hitting the address of the
container that is still alive.

**See Also**

 * [How Weave Finds Containers]({{ '/tasks/weavedns/how-works-weavedns' | relative_url }})
 * [Managing Domains]({{ '/tasks/weavedns/managing-domains-weavedns' | relative_url }})
 * [Managing Domain Entries]({{ '/tasks/weavedns/managing-entries-weavedns' | relative_url }})
