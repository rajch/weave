---
title: Managing Domains
menu_order: 30
search_type: Documentation
---

The following topics are discussed:

* [Configuring the domain search path](#domain-search-path)
* [Using a different local domain](#local-domain)

## <a name="domain-search-path"></a>Configuring the domain search paths

If you don't supply a domain search path (with `--dns-search=`), Weave
Net (via the [proxy]({{ '/tasks/weave-docker-api/weave-docker-api' | relative_url }}) or via `weave attach`)
tells a container to look for "bare" hostnames, like `pingme`, in its
own domain (or in `weave.local` if it has no domain).

If you want to supply other entries for the domain search path,
e.g. if you want containers in different sub-domains to resolve
hostnames across all sub-domains plus some external domains, you need
*also* to supply the `weave.local` domain to retain the above
behaviour.

```
docker run -ti \
  --dns-search=zone1.weave.local --dns-search=zone2.weave.local \
  --dns-search=corp1.com --dns-search=corp2.com \
  --dns-search=weave.local alpine:latest
```

## <a name="local-domain"></a>Using a different local domain

By default, weaveDNS uses `weave.local.` as the domain for names on the
Weave network. In general users do not need to change this domain, but
you can force weaveDNS to use a different domain by launching it with
the `--dns-domain` argument. For example,

```
$ weave launch --dns-domain="mycompany.local."
```

The local domain should end with `local.`, since these names are
link-local as per [RFC6762](https://tools.ietf.org/html/rfc6762),
(though this is not strictly necessary).


 * [How Weave Finds Containers]({{ '/tasks/weavedns/how-works-weavedns' | relative_url }})
 * [Load Balancing and Fault Resilience with WeaveDNS]({{ '/tasks/weavedns/load-balance-fault-weavedns' | relative_url }})
 * [Managing Domain Entries]({{ '/tasks/weavedns/managing-entries-weavedns' | relative_url }})
