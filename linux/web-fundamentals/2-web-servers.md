# Web Servers

A web server is a software that listens for incoming connections and then utilises the HTTP protocol to deliver web content to its clients. The most common web server software you'll come across is Apache, Nginx, IIS and NodeJS. 

A Web server delivers files from what's called its root directory, which is defined in the software settings. For example, Nginx and Apache share the same default location of /var/www/html in Linux operating systems, and IIS uses C:\\inetpub\\wwwroot for the Windows operating systems. So, for example, if you requested the file [http://www.example.com/picture.jpg](http://www.example.com/picture.jpg), it would send the file /var/www/html/picture.jpg from its local hard drive.

## Virtual Hosts

Web servers can host multiple websites with different domain names; to achieve this, they use virtual hosts. The web server software checks the hostname being requested from the HTTP headers and matches that against its virtual hosts (virtual hosts are just text-based configuration files). If it finds a match, the correct website will be provided. If no match is found, the default website will be provided instead.

Virtual Hosts can have their root directory mapped to different locations on the hard drive. For example, [one.com](http://one.com) being mapped to /var/www/website_one, and [two.com](http://two.com) being mapped to /var/www/website_two

There's no limit to the number of different websites you can host on a web server.

## Order of a request

1. Request website in your browser
2. Check browser's local cache for IP Address
3. Check your recursive DNS Server for Address
4. recursive DNS queries root server to find authoritative DNS Server
5. Authoritative DNS server advises the IP address for the website
6. Request passes through a "Web Application Firewall" (WAF)
7. Request passes through a Load Balancer
8. Connect to the web server on Port 80 or 443
9. Web App talks to the Database, prepares and sends the response
10. The browser receives the response and renders the HTML page