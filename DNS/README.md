Never be confused about DNS again.

Hi! I’m Ruurtjan Pul, a full-stack developer who used to suck at DNS.

I’ve had many side projects over the years, and never completely understood why my DNS records didn’t update. 
Or how long to wait until they do. Worse yet, I’ve had my emails bounce because I misconfigured SPF, DKIM and DMARC.

I decided to learn DNS by making Nslookup.io, a web interface for DNS. I’ve read the RFCs to implement a custom 
DNS client and I now run my own DNS server. Nslookup has become popular enough to replace my income, so I’m working 
on it full-time since early 2022.

Now that I’ve spent so much time learning DNS, I’m sharing everything I’ve learned in this comprehensive video course. 
It will teach you to reason about DNS from first principles and guard you against the mistakes I made. When you finish this 
course, you will be able to manage and debug DNS with confidence.

Learn DNS once and for all.

They say you should learn the fundamentals, and the rest will fall into place. DNS is right at the foundation 
of the internet. Having a clear understanding of DNS will help you configure and debug DNS many times throughout your career.

DNS hasn’t changed that much since its introduction in 1983 and it’s unlikely to start now. DNS isn’t going anywhere. 
The earlier you learn DNS, the longer you’ll benefit.

The great thing about DNS is that all the information is public. You can have perfect clarity if you know where to look.

```ascii
Table of Contents
1 DNS is a database
2 The DNS is organized as a tree
3 Zone delegation
4 The root zone
5 Authoritative DNS servers
6 Zone transfer
7 Top-level domains
8 Second and third-level domains
9 Registries, registrars & registrants
10 ICANN-s oversight
11 WHOIS & RDAP
12 International domain names
13 The domain name lifecycle
14 Domain transfer
15 Recursive queries
16 Glue records
17 DNS caching
18 Negative caching
19 The DNS protocol
20 EDNS
21 Transport protocols
22 Public DNS resolvers
23 DDNS
24 Dynamic DNS responses
25 An overview of DNS record types
26 An overview of DNS record types
27 CNAME records
28 TXT records
29 SRV records
30 PTR records
31 DNSSEC-related record types
32 An overview of email configuration
33 Receiving email Resources and references
34 DANE Resources and references
35 MTA-STS Resources and references
36 SPF Resources and references
37 DKIM Resources and references
38 DMARC Resources and references
39 BIMI Resources and references
40 Cache invalidation
41 DNSSEC issues
42 Graceful DNS changes
43 Identifying root causes Resources and references
44 Inspecting DNS traffic Resources and references
45 Lame delegation Resources and references
46 Simulate slow or failing DNS
```


𝗗𝗡𝗦 𝗹𝗼𝗼𝗸𝘂𝗽 𝗽𝗿𝗼𝗰𝗲𝘀𝘀 𝗲𝘅𝗽𝗹𝗮𝗶𝗻𝗲𝗱 𝗶𝗻 𝘀𝗶𝗺𝗽𝗹𝗲 𝘁𝗲𝗿𝗺𝘀.

DNS lookup is the process of translating human-readable domain names like "www. example .com" into IP addresses like "172 .217 .22 .14". It's essentially how a web browser converts a URL into an IP address.

𝗧𝗼 𝗴𝗲𝘁 𝗮 𝗰𝗹𝗲𝗮𝗿 𝗽𝗶𝗰𝘁𝘂𝗿𝗲 𝗼𝗳 𝗵𝗼𝘄 𝗗𝗡𝗦 𝗹𝗼𝗼𝗸𝘂𝗽 𝘄𝗼𝗿𝗸𝘀, 𝗹𝗲𝘁'𝘀 𝘄𝗮𝗹𝗸 𝘁𝗵𝗿𝗼𝘂𝗴𝗵 𝘁𝗵𝗲 𝗽𝗿𝗼𝗰𝗲𝘀𝘀:

𝟭) 𝗨𝘀𝗲𝗿 𝗶𝗻𝗽𝘂𝘁

It all begins when you enter a URL into your web browser, such as "www. example .com".

𝟮) 𝗕𝗿𝗼𝘄𝘀𝗲𝗿 𝗰𝗮𝗰𝗵𝗲

Before involving DNS servers, the browser first looks in its own cache. If it already knows the IP address for the domain, then the process stops here.

𝟯) 𝗢𝗦 𝗰𝗮𝗰𝗵𝗲

If the browser doesn't have the IP address stored, the operating system's cache is then examined. The OS maintains its own cache of DNS lookups.

𝟰) 𝗥𝗲𝗰𝘂𝗿𝘀𝗶𝘃𝗲 𝗿𝗲𝘀𝗼𝗹𝘃𝗲𝗿

If the IP address cannot be retrieved via the OS cache, then the request is forwarded to the recursive resolver, which is typically managed by your internet service provider (ISP). To find the IP address, this server will act on behalf of the user.

𝟱) 𝗥𝗼𝗼𝘁 𝗗𝗡𝗦 𝘀𝗲𝗿𝘃𝗲𝗿

The recursive resolver first checks its cache. If it doesn't have the IP address, it then queries a root DNS server. Although the root server normally does not know the IP address for each domain, it knows where to direct the query next.

𝟲) 𝗧𝗼𝗽-𝗹𝗲𝘃𝗲𝗹 𝗱𝗼𝗺𝗮𝗶𝗻 (𝗧𝗟𝗗) 𝘀𝗲𝗿𝘃𝗲𝗿

Based on the domain extension (such as .com, .net, or .org), the root server will direct the resolver to a TLD server. This server keeps track of which nameservers are responsible for handling domains under that TLD.

𝟳) 𝗗𝗼𝗺𝗮𝗶𝗻'𝘀 𝗻𝗮𝗺𝗲𝘀𝗲𝗿𝘃𝗲𝗿

From here the TLD server points the resolver to the authoritative nameserver for the specific domain (e.g., "example .com"). The IP address for the domain is known by this nameserver. This nameserver returns the IP address for the domain to the resolver.

𝟴) 𝗥𝗲𝘀𝗽𝗼𝗻𝘀𝗲 𝘁𝗼 𝗰𝗹𝗶𝗲𝗻𝘁

Now that the resolver has finally retrieved the IP address, it sends it back to the OS. The OS then sends the IP address to the browser.

𝟵) 𝗪𝗲𝗯𝘀𝗶𝘁𝗲 𝗮𝗰𝗰𝗲𝘀𝘀

We've reached the final stage, the browser has the IP address for the domain that we originally entered. The browser can now fetch the website by making a request to the web server associated with that IP address.

As we've seen, DNS lookup can be a quick process or it can involve several steps and components to locate the IP address. Caching is a key strategy for DNS lookup. It occurs at multiple levels throughout the process so that future requests for 