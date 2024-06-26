---
title: Feature Overview
menu_order: 20
search_type: Documentation
---

 * [Virtual Ethernet Switch](#virtual-ethernet-switch)
 * [Fast Data Path](#fast-data-path)
 * [Seamless Docker Integration](#docker)
 * [Docker Network Plugin](#plugin)
 * [CNI Plugin](#cniplugin)
 * [Address Allocation (IPAM)](#addressing)
 * [Naming and Discovery](#naming-and-discovery)
 * [Application Isolation](#application-isolation)
 * [Network Policy](#network-policy)
 * [Dynamic Network Attachment](#dynamic-network-attachment)
 * [Security](#security)
 * [Host Network Integration](#host-network-integration)
 * [Service Export](#services)
 * [Service Import](#services)
 * [Service Binding](#services)
 * [Service Routing](#services)
 * [Multi-cloud Networking](#multi-cloud-networking)
 * [Multi-hop Routing](#multi-hop-routing)
 * [Dynamic Topologies](#dynamic-topologies)
 * [Container Mobility](#container-mobility)
 * [Fault Tolerance](#fault-tolerance) 

For step-by-step instructions on how to use Weave Net, see [Launching Weave Net]({{ '/install/using-weave' | relative_url }}).

### <a name="virtual-ethernet-switch"></a>Virtual Ethernet Switch

Weave Net creates a virtual network that connects Docker containers
deployed across multiple hosts.
To application containers, the network established by Weave 
resembles a giant Ethernet switch, where all containers are 
connected and can easily access services from one another. 

Because Weave Net uses standard protocols, your favorite network 
tools and applications, developed over decades, can still 
be used to configure, secure, monitor, and troubleshoot 
a container network. 

Broadcast and Multicast protocols can also be used
over Weave Net.

To start using Weave Net, see [Installing Weave Net]({{ '/install/installing-weave' | relative_url }}) 
and [Launching Weave Net]({{ '/install/using-weave' | relative_url }}).

### <a name="fast-data-path"></a>Fast Datapath

Weave Net automatically chooses the fastest available method to 
transport data between peers. The best performing of these 
(the 'fast datapath') offers near-native throughput and latency.

See [Using Fast Datapath]({{ '/tasks/manage/fastdp' | relative_url }}) and
[How Fast Datapath Works]({{ '/concepts/fastdp-how-it-works' | relative_url }}).

### <a name="docker"></a>Seamless Docker Integration (Weave Docker API Proxy)

Weave Net includes a [Docker API Proxy]({{ '/tasks/weave-docker-api/weave-docker-api' | relative_url }}), which can be 
used to start containers using the Docker [command-line interface](https://docs.docker.com/reference/commandline/cli/) or the [remote API](https://docs.docker.com/reference/api/docker_remote_api/), and attach them to the Weave network before they begin execution.

To use the proxy, run:

    host1$ eval $(weave env)
    
and then start and manage containers with standard Docker commands. 

Containers started in this way that subsequently restart, either
by an explicit `docker restart` command or by Docker restart 
policy, are re-attached to the Weave network by the `Weave Docker API Proxy`.

See [Integrating Docker via the API Proxy]({{ '/tasks/weave-docker-api/weave-docker-api' | relative_url }}).


### <a name="plugin"></a>Weave Network Docker Plugin

Weave Net can also be used as a [Docker plugin](https://docs.docker.com/engine/extend/plugins_network/).  A Docker network 
named `weave` is created by `weave launch`, which is used as follows:

    $ docker run --net=weave -ti alpine:latest

Using the Weave plugin enables you to take advantage of [Docker's network functionality](https://docs.docker.com/engine/extend/plugins_network/).

There are two plugin implementations for Weave Net: the
["Legacy"]({{ '/install/plugin/plugin' | relative_url }}) plugin which doesn't require an external
cluster store, and the ["V2"]({{ '/install/plugin/plugin-v2' | relative_url }}) plugin which
supports Docker swarm mode.

### <a name="cniplugin"></a>Weave Network CNI Plugin

Weave can be used as a plugin to systems that support the [Container Network Interface](https://github.com/appc/cni), such as Kubernetes and Mesosphere.

See [Integrating Kubernetes and Mesos via the CNI Plugin]({{ '/kubernetes' | relative_url }}) for more details.


### <a name="addressing"></a>IP Address Management (IPAM)
 
Containers are automatically allocated a unique IP address. To view the addresses allocated by Weave, run `weave ps`.

Instead of allowing Weave to automatically allocate addresses, an IP address and a network can be explicitly 
specified. See [How to Manually Specify IP Addresses and Subnets]({{ '/tasks/ipam/manual-ip-address' | relative_url }}) for instructions. 

For a discussion on how Weave Net uses IPAM, see [Automatic IP Address Management]({{ '/tasks/ipam/ipam' | relative_url }}). And also review the 
[the basics of IP addressing]({{ '/concepts/ip-addresses' | relative_url }}) for an explanation of addressing and private networks. 


### <a name="naming-and-discovery"></a>Naming and Discovery
 
Containers with a hostname are automatically registered in [weaveDNS]({{ '/tasks/weavedns/weavedns' | relative_url }}), 
and are discoverable by using standard, simple name lookups:

    host1$ docker run -dti --name=service --hostname=service.weave.local alpine:latest
    host1$ docker run -ti alpine:latest
    root@7b21498fb103:/# ping service

WeaveDNS also supports [load balancing]({{ '/tasks/weavedns/load-balance-fault-weavedns' | relative_url }}), [fault resilience]({{ '/tasks/weavedns/load-balance-fault-weavedns' | relative_url }}) and [hot swapping]({{ '/tasks/weavedns/managing-entries-weavedns' | relative_url }}). 

See [Discovering Containers with WeaveDNS]({{ '/tasks/weavedns/weavedns' | relative_url }}).
 
### <a name="application-isolation"></a>Application Isolation

A single Weave network can host multiple, isolated 
applications, with each application's containers being able 
to communicate with each other but not with the containers 
of other applications.

To isolate applications, Weave Net can make use of the 
_isolation-through-subnets_ technique. This common strategy
 is an example of how with Weave many "on metal"
 techniques can be used to deploy your applications to 
 containers.

See [Isolating Applications]({{ '/tasks/manage/application-isolation' | relative_url }}) 
for information on how to use the isolation-through-subnets 
technique with Weave Net.

### <a name="network-policy"></a>Network Policy

The Weave [Kubernetes Addon]({{ '/kubernetes/kube-addon' | relative_url }}) includes a network
policy controller that implements [Kubernetes Network
Policies](http://kubernetes.io/docs/user-guide/networkpolicies/).

### <a name="dynamic-network-attachment"></a>Dynamic Network Attachment

At times, you may not know the application network for a 
given container in advance. In these cases, you can take 
advantage of Weave's ability to attach and detach running 
containers to and from any network. 

See [Dynamically Attaching and Detaching Containers]({{ '/tasks/manage/dynamically-attach-containers' | relative_url }}) 
for details. 


### <a name="security"></a>Security

In keeping with our ease-of-use philosophy, the cryptography 
in Weave Net is intended to satisfy a particular user requirement: 
strong, out-of-the-box security without a complex setup or 
the need to wade your way through the configuration of cipher 
suite negotiation, certificate generation or any of the 
other things needed to properly secure an IPsec or TLS installation.

Weave Net communicates via TCP and UDP on a well-known port, so 
you can adapt whatever is appropriate to your requirements - for 
example an IPsec VPN for inter-DC traffic, or VPC/private network 
inside a data-center. 

For cases when this is not convenient, Weave Net provides a 
secure, [authenticated encryption](https://en.wikipedia.org/wiki/Authenticated_encryption) 
mechanism which you can use in conjunction with or as an 
alternative to any other security technologies you have 
running alongside Weave. 

Weave Net implements encryption and security using the Go version of [Daniel J.  Bernstein's NaCl library](http://nacl.cr.yp.to/index.html),
and, additionally in the case of encrypted fast datapath using [the cryptography framework of the Linux kernel](https://en.wikipedia.org/wiki/Crypto_API_(Linux)).

For information on how to secure your Docker network connections, see [Securing Connections Across Untrusted Networks]({{ '/tasks/manage/security-untrusted-networks' | relative_url }}) and for a more technical discussion on how Weave implements encryption see, [Weave Encryption]({{ '/concepts/encryption' | relative_url }}) and [How Weave Implements Encryption]({{ '/concepts/encryption-implementation' | relative_url }}).


### <a name="host-network-integration"></a>Host Network Integration

Weave Net application networks can be integrated with a host's network, and establish connectivity between the host and 
application containers anywhere.

See [Integrating with the Host Network]({{ '/tasks/manage/host-network-integration' | relative_url }}).

### <a name="services"></a>Managing Services: Exporting, Importing, Binding and Routing
 
 * **Exporting Services** - Services running in containers on a Weave network can be made accessible to the outside world or to other networks.
 * **Importing Services** - Applications can run anywhere, and yet still be made accessible by specific application containers or services.
 * **Binding Services** - A container can be bound to a particular IP and port without having to change your application code, while at the same time will maintain its original endpoint. 
 * **Routing Services** - By combining the importing and exporting features, you can connect to disjointed networks, even when separated by firewalls and where there may be overlapping IP addresses.  

See [Managing Services - Exporting, Importing, Binding and Routing]({{ '/tasks/manage/service-management' | relative_url }}) for instructions on how to manage services on a Weave container network. 

### <a name="multi-cloud-networking"></a>Multi-Cloud Networking

Weave can network containers hosted in different cloud providers 
or data centers. For example, you can run an application consisting 
of containers that run on [Google Compute Engine](https://cloud.google.com/compute/) 
(GCE), [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) 
(EC2) and in a local data centre all at the same time.

See [Enabling Multi-Cloud networking and Muti-hop Routing]({{ '/tasks/manage/multi-cloud-multi-hop' | relative_url }}).


### <a name="multi-hop-routing"></a>Multi-Hop Routing

A network of containers across more than two hosts can be established 
even when there is only partial connectivity between the hosts. Weave Net
routes traffic between containers as long as there is at least one *path* 
of connected hosts between them.

See [Enabling Multi-Cloud networking and Multi-hop Routing]({{ '/tasks/manage/multi-cloud-multi-hop' | relative_url }}).


### <a name="dynamic-topologies"></a>Dynamic Topologies

Hosts can be added to or removed from a Weave network without stopping
or reconfiguring the remaining hosts. See [Adding and Removing Hosts
Dynamically.]({{ '/tasks/manage/finding-adding-hosts-dynamically' | relative_url }})


### <a name="container-mobility"></a>Container Mobility

Containers can be moved between hosts without requiring any 
reconfiguration or, in many cases, restarts of other containers. 
All that is required is for the migrated container to be started 
with the same IP address as it was given originally.

See [Managing Services - Exporting, Importing, Binding and Routing]({{ '/tasks/manage/service-management' | relative_url }}), in particular, Routing Services for more information on container mobility. 


### <a name="fault-tolerance"></a>Fault Tolerance

Weave Net peers continually exchange topology information, and 
monitor and (re)establish network connections to other peers. 
So if hosts or networks fail, Weave can "route around" the problem. 
This includes network partitions, where containers on either side 
of a partition can continue to communicate, with full connectivity 
being restored when the partition heals.

The Weave Net Router container is very lightweight, fast and and disposable. 
For example, should Weave Net ever run into difficulty, one can 
simply stop it (with `weave stop`) and restart it. Application 
containers do *not* have to be restarted in that event, and 
if the Weave Net container is restarted quickly enough, 
may not experience a temporary connectivity failure.

