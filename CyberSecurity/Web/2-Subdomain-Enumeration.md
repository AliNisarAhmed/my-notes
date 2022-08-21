# Subdomain Enumeration

Subdomain enumeration is the process of finding valid subdomains for a domain, but why do we do this? We do this to expand our attack surface to try and discover more potential points of vulnerability.  
  
We will explore three different subdomain enumeration methods: Brute Force, OSINT (Open-Source Intelligence) and Virtual Host.

## OSINT - SSL/TLS Certificates

When an SSL/TLS (Secure Sockets Layer/Transport Layer Security) certificate is created for a domain by a CA (Certificate Authority), CA's take part in what's called "Certificate Transparency (CT) logs". 

These are publicly accessible logs of every SSL/TLS certificate created for a domain name. 

The purpose of Certificate Transparency logs is to stop malicious and accidentally made certificates from being used. 

We can use this service to our advantage to discover subdomains belonging to a domain, sites like [https://crt.sh](http://crt.sh) and [https://ui.ctsearch.entrust.com/ui/ctsearchui](https://ui.ctsearch.entrust.com/ui/ctsearchui) offer a searchable database of certificates that shows current and historical results.

Example: we can search `crt.sh` for the domain `tryhackme.com` and find entries listed for variour subdomains, and that way discover hidden subdomains

## OSINT - Search Engines

We can use Google search to discover subdomains. 

One technique is to search like this: `-site:www.tryhackme.com site:*.tryhackme.com`


## DNS Brute Force

Bruteforce DNS (Domain Name System) enumeration is the method of trying tens, hundreds, thousands or even millions of different possible subdomains from a pre-defined list of commonly used subdomains. 

Because this method requires many requests, we automate it with tools to make the process quicker.

Example: `dnsrecon`


## OSINT - Sublist3r

**Automation Using Sublist3r**

To speed up the process of OSINT subdomain discovery, we can automate the above methods with the help of tools like [Sublist3r](https://github.com/aboul3la/Sublist3r) 

## Virtual Hosts

**Virtual hosting** is a method for hosting multiple [domain names](https://en.wikipedia.org/wiki/Domain_name "Domain name") (with separate handling of each name) on a single [server](https://en.wikipedia.org/wiki/Server_%28computing%29 "Server (computing)") (or pool of servers). This allows one server to share its resources, such as memory and processor cycles, without requiring all services provided to use the same host name. The term virtual hosting is usually used in reference to web servers but the principles do carry over to other Internet services.

Some subdomains aren't always hosted in publically accessible DNS results, such as development versions of a web application or administration portals. Instead, the DNS record could be kept on a private DNS server or recorded on the developer's machines in their /etc/hosts file (or c:\\windows\\system32\\drivers\\etc\\hosts file for Windows users) which maps domain names to IP addresses. 

Because web servers can host multiple websites from one server when a website is requested from a client, the server knows which website the client wants from the **Host** header. We can utilise this host header by making changes to it and monitoring the response to see if we've discovered a new website.

Like with DNS Bruteforce, we can automate this process by using a wordlist of commonly used subdomains.

We can use tools like `ffuf`