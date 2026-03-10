#### Recursive DNS resolver

The recursive resolver is the computer that responds to a recursive request from a client and takes the time to track down the [DNS record](https://www.cloudflare.com/learning/dns/dns-records/). It does this by making a series of requests until it reaches the authoritative DNS nameserver for the requested record (or times out or returns an error if no record is found). Luckily, recursive DNS resolvers do not always need to make multiple requests in order to track down the records needed to respond to a client; [caching](https://www.cloudflare.com/learning/cdn/what-is-caching/) is a data persistence process that helps short-circuit the necessary requests by serving the requested resource record earlier in the DNS lookup.

![DNS Record Request Sequence - DNS Recursive Resolver gets request from client](https://cf-assets.www.cloudflare.com/slt3lc6tev37/3NOmAzkfPG8FTA8zLc7Li8/8efda230b212c0de2d3bbcb408507b1e/dns_record_request_sequence_recursive_resolver.png "DNS Record Request Sequence - Recursive Resolver")

#### Authoritative DNS server

Put simply, an authoritative DNS server is a server that actually holds, and is responsible for, DNS resource records. This is the server at the bottom of the DNS lookup chain that will respond with the queried resource record, ultimately allowing the web browser making the request to reach the IP address needed to access a website or other web resources. An authoritative nameserver can satisfy queries from its own data without needing to query another source, as it is the final source of truth for certain DNS records.

![DNS Record Request Sequence - DNS query reaches authoritative nameserver for cloudflare.com](https://cf-assets.www.cloudflare.com/slt3lc6tev37/6Cxvsc4NOvmU4pPkKbkDmP/a7588a4c8a3c187e9175a40fa1b3d548/dns_record_request_sequence_authoritative_nameserver.png "DNS Record Request Sequence - Authoritative Nameserver")

It’s worth mentioning that in instances where the query is for a subdomain such as foo.example.com or [blog.cloudflare.com](https://blog.cloudflare.com/), an additional nameserver will be added to the sequence after the authoritative nameserver, which is responsible for storing the subdomain’s [CNAME record](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/).

![DNS Record Request Sequence - DNS query to CNAME record for subdomain blog.cloudflare.com](https://cf-assets.www.cloudflare.com/slt3lc6tev37/1O1o3jhs0ztWsD00k8RLIJ/f33c1793a7e21cb92678c1f35ef1b245/dns_record_request_sequence_cname_subdomain.png "DNS Record Request Sequence - CNAME record for subdomain")

There is a key difference between many DNS services and the one that Cloudflare provides. Different DNS recursive resolvers such as [Google DNS](https://www.cloudflare.com/cloudflare-vs-google-dns/), OpenDNS, and providers like Comcast all maintain data center installations of DNS recursive resolvers. These resolvers allow for quick and easy queries through optimized clusters of DNS-optimized computer systems, but they are fundamentally different than the nameservers hosted by Cloudflare.

Cloudflare maintains infrastructure-level nameservers that are integral to the functioning of the Internet. One key example is the [f-root server network](https://blog.cloudflare.com/f-root/) which Cloudflare is partially responsible for hosting. The F-root is one of the root level DNS nameserver infrastructure components responsible for the billions of Internet requests per day. Our [Anycast network](https://www.cloudflare.com/learning/cdn/glossary/anycast-network/) puts us in a unique position to handle large volumes of DNS traffic without service interruption.

## What are the steps in a DNS lookup?

For most situations, DNS is concerned with a domain name being translated into the appropriate IP address. To learn how this process works, it helps to follow the path of a DNS lookup as it travels from a web browser, through the DNS lookup process, and back again. Let's take a look at the steps.

Note: Often DNS lookup information will be cached either locally inside the querying computer or remotely in the DNS infrastructure. There are typically 8 steps in a DNS lookup. When DNS information is cached, steps are skipped from the DNS lookup process which makes it quicker. The example below outlines all 8 steps when nothing is cached.

#### The 8 steps in a DNS lookup:

1. A user types ‘example.com’ into a web browser and the query travels into the Internet and is received by a DNS recursive resolver.
2. The resolver then queries a DNS root nameserver (.).
3. The root server then responds to the resolver with the address of a Top Level Domain (TLD) DNS server (such as .com or .net), which stores the information for its domains. When searching for example.com, our request is pointed toward the .com TLD.
4. The resolver then makes a request to the .com TLD.
5. The TLD server then responds with the IP address of the domain’s nameserver, example.com.
6. Lastly, the recursive resolver sends a query to the domain’s nameserver.
7. The IP address for example.com is then returned to the resolver from the nameserver.
8. The DNS resolver then responds to the web browser with the IP address of the domain requested initially.

Once the 8 steps of the DNS lookup have returned the IP address for example.com, the browser is able to make the request for the web page:

10. The browser makes a [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/) request to the IP address.
11. The server at that IP returns the webpage to be rendered in the browser (step 10).

![Complete DNS Lookup and Webpage Query - 10 steps](https://cf-assets.www.cloudflare.com/slt3lc6tev37/1NzaAqpEFGjqTZPAS02oNv/bf7b3f305d9c35bde5c5b93a519ba6d5/what_is_a_dns_server_dns_lookup.png "Complete DNS Lookup and Webpage Query")

## What is a DNS resolver?

The DNS resolver is the first stop in the DNS lookup, and it is responsible for dealing with the client that made the initial request. The resolver starts the sequence of queries that ultimately leads to a URL being translated into the necessary IP address.

Note: A typical uncached DNS lookup will involve both recursive and iterative queries.

It's important to differentiate between a [recursive DNS](https://www.cloudflare.com/learning/dns/what-is-recursive-dns/) query and a recursive DNS resolver. The query refers to the request made to a DNS resolver requiring the resolution of the query. A DNS recursive resolver is the computer that accepts a recursive query and processes the response by making the necessary requests.

![DNS recursive query goes from DNS client to DNS recursive resolver](https://cf-assets.www.cloudflare.com/slt3lc6tev37/rOXBgctX2gaXNDqP5ktek/7086a97e00525159c6bd9318819c2287/dns_recursive_query.png "DNS Recursive Query")

## What are the types of DNS queries?

In a typical DNS lookup three types of queries occur. By using a combination of these queries, an optimized process for DNS resolution can result in a reduction of distance traveled. In an ideal situation cached record data will be available, allowing a DNS name server to return a non-recursive query.

#### 3 types of DNS queries:

1. **Recursive query** - In a recursive query, a DNS client requires that a DNS server (typically a DNS recursive resolver) will respond to the client with either the requested resource record or an error message if the resolver can't find the record.
2. **Iterative query** - in this situation the DNS client will allow a DNS server to return the best answer it can. If the queried DNS server does not have a match for the query name, it will return a referral to a DNS server authoritative for a lower level of the domain namespace. The DNS client will then make a query to the referral address. This process continues with additional DNS servers down the query chain until either an error or timeout occurs.
3. **Non-recursive query** - typically this will occur when a DNS resolver client queries a DNS server for a record that it has access to either because it's authoritative for the record or the record exists inside of its cache. Typically, a DNS server will cache DNS records to prevent additional bandwidth consumption and load on upstream servers.

## What is DNS caching? Where does DNS caching occur?

The purpose of caching is to temporarily stored data in a location that results in improvements in performance and reliability for data requests. DNS caching involves storing data closer to the requesting client so that the DNS query can be resolved earlier and additional queries further down the DNS lookup chain can be avoided, thereby improving load times and reducing bandwidth/CPU consumption. DNS data can be cached in a variety of locations, each of which will store DNS records for a set amount of time determined by a [time-to-live (TTL)](https://www.cloudflare.com/learning/cdn/glossary/time-to-live-ttl/).

#### Browser DNS caching

Modern web browsers are designed by default to cache DNS records for a set amount of time. The purpose here is obvious; the closer the DNS caching occurs to the web browser, the fewer processing steps must be taken in order to check the cache and make the correct requests to an IP address. When a request is made for a DNS record, the browser cache is the first location checked for the requested record.

In Chrome, you can see the status of your DNS cache by going to chrome://net-internals/#dns.

#### Operating system (OS) level DNS caching

The operating system level DNS resolver is the second and last local stop before a DNS query leaves your machine. The process inside your operating system that is designed to handle this query is commonly called a “stub resolver” or DNS client. When a stub resolver gets a request from an application, it first checks its own cache to see if it has the record. If it does not, it then sends a DNS query (with a recursive flag set), outside the local network to a DNS recursive resolver inside the Internet service provider (ISP).

When the recursive resolver inside the ISP receives a DNS query, like all previous steps, it will also check to see if the requested host-to-IP-address translation is already stored inside its local persistence layer.

The recursive resolver also has additional functionality depending on the types of records it has in its cache:

1. If the resolver does not have the [A records](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/), but does have the [NS records](https://www.cloudflare.com/learning/dns/dns-records/dns-ns-record/) for the authoritative nameservers, it will query those name servers directly, bypassing several steps in the DNS query. This shortcut prevents lookups from the root and .com nameservers (in our search for example.com) and helps the resolution of the DNS query occur more quickly.
2. If the resolver does not have the NS records, it will send a query to the TLD servers (.com in our case), skipping the root server.
3. In the unlikely event that the resolver does not have records pointing to the TLD servers, it will then query the root servers. This event typically occurs after a DNS cache has been purged.

Learn about what differentiates [Cloudflare DNS](https://www.cloudflare.com/dns/) from other DNS providers.

Getting Started

- [Free plans](https://www.cloudflare.com/plans/free/)
- [For enterprises](https://www.cloudflare.com/enterprise/)
- [Compare plans](https://www.cloudflare.com/plans/)
- [Domain name search](https://domains.cloudflare.com/)
- [Get a recommendation](https://www.cloudflare.com/about-your-website/)
- [Request a demo](https://www.cloudflare.com/plans/enterprise/demo/)
- [Contact sales](https://www.cloudflare.com/plans/enterprise/contact/)

About DNS

- [What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [DNS security](https://www.cloudflare.com/learning/dns/dns-security/)
- [DNS over TLS](https://www.cloudflare.com/learning/dns/dns-over-tls/)
- [How to buy a domain name](https://www.cloudflare.com/learning/dns/how-to-buy-a-domain-name/)
- [How to transfer a domain name](https://www.cloudflare.com/learning/dns/how-to-transfer-a-domain-name/)

DNS Servers

- [What's a DNS Server?](https://www.cloudflare.com/learning/dns/what-is-a-dns-server/)
- [DNS Root Server](https://www.cloudflare.com/learning/dns/glossary/dns-root-server/)

DNS Records

- [Overview](https://www.cloudflare.com/learning/dns/dns-records/)
- [DNS A Record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/)
- [DNS AAAA Record](https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/)
- [DNS CNAME Record](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/)
- [DNS MX Record](https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/)
- [DNS TXT Record](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/)
- [DNS NS Record](https://www.cloudflare.com/learning/dns/dns-records/dns-ns-record/)
- [DNS SOA Record](https://www.cloudflare.com/learning/dns/dns-records/dns-soa-record/)
- [DNS SRV Record](https://www.cloudflare.com/learning/dns/dns-records/dns-srv-record/)
- [DNS PTR Record](https://www.cloudflare.com/learning/dns/dns-records/dns-ptr-record/)
- [DNSKEY and DS Records](https://www.cloudflare.com/learning/dns/dns-records/dnskey-ds-records/)
- [DNS SPF Record](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/)
- [DNS DKIM Record](https://www.cloudflare.com/learning/dns/dns-records/dns-dkim-record/)
- [DNS DMARC Record](https://www.cloudflare.com/learning/dns/dns-records/dns-dmarc-record/)

DNS Glossary