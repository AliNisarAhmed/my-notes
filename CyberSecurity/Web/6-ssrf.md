# Server Side Request Forgery

This is a vulernability that allows a malicious user to cause the webserver to make an additional or edited HTTP request to te resource of the attacker's choosing.

## Types of SSRF

- Regular SSRF
    - where the data is returned to the attacker's screen
- Blind SSRF
    - where an SSRF occurs, but no information is returned to the attacker's screen

## Impact of SSRF

A successful SSRF attack can result in any of the following: 
* Access to unauthorised areas.
* Access to customer/organisational data.
* Ability to Scale to internal networks.
* Reveal authentication tokens/credentials.

## Examples

Below, the first is the way a website is using `url` params to input an endpoint, or ask for a URL, or indicate a subdomain, below it shows how to exploit it

http://website.thm/stock?url=http://api.website.thm/api/stock/item?id=23
- http://website.thm/stock?url=http://api.website.thm/api/user

http://website.thm/stock?url=/item?id=123
- http://website.thm/stock?url=/../user

http://website.thm/stock?server=api&id=123
- http://website.thm/stock?server=api.website.thm/api/user&x=&id=123

http://website.thm/stock?url=http://api.website.thm/api/stock/item?id=123
- http://website.thm/stock?url=http://hacker-domain.thm/

## Finding an SSRF

There are many avenues for SSRF, but 4 common ones shown below: 

1. When a full URL is used in a parameter in the address bar: 

![16acfd5b7d366fbb8f8fa36da880c238.png](../../images/16acfd5b7d366fbb8f8fa36da880c238.png)

2. A hidden field in a form: 

![5dec2507729da3b860b908960f13768e.png](../../images/5dec2507729da3b860b908960f13768e.png)

3. A partial URL such as just the hostname: 

![1d89d95c7929ebd6f91242cf0ebe4d7a.png](../../images/1d89d95c7929ebd6f91242cf0ebe4d7a.png)

4. Or perhaps only the path of the URL: 

![80f53508abf69c80473115537e81bf1b.png](../../images/80f53508abf69c80473115537e81bf1b.png)


## Tools that can help

If working with a blind SSRF where no output is reflected back to you, you'll need to use an external HTTP logging tool to monitor requests such as `requestbin.com`, your own HTTP server or Burp Suite's Collaborator client.


## Defeating common SSRF Defenses

More security-savvy developers aware of the risks of SSRF vulnerabilities may implement checks in their applications to make sure the requested resource meets specific rules. There are usually two approaches to this, either a deny list or an allow list.

### Deny List

A Deny List is where all requests are accepted apart from resources specified in a list or matching a particular pattern. 

A Web Application may employ a deny list to protect sensitive endpoints, IP addresses or domains from being accessed by the public while still allowing access to other locations. 

A specific endpoint to restrict access is the localhost, which may contain server performance data or further sensitive information, so domain names such as localhost and 127.0.0.1 would appear on a deny list. Attackers can bypass a Deny List by using alternative localhost references such as 0, 0.0.0.0, 0000, 127.1, 127.*.*.*, 2130706433, 017700000001 or subdomains that have a DNS record which resolves to the IP Address 127.0.0.1 such as 127.0.0.1.nip.io.

Also, in a cloud environment, it would be beneficial to block access to the IP address 169.254.169.254, which contains metadata for the deployed cloud server, including possibly sensitive information. An attacker can bypass this by registering a subdomain on their own domain with a DNS record that points to the IP Address 169.254.169.254.
  

### Allow List

An allow list is where all requests get denied unless they appear on a list or match a particular pattern, such as a rule that an URL used in a parameter must begin with **https://website.thm.** An attacker could quickly circumvent this rule by creating a subdomain on an attacker's domain name, such as https://website.thm.attackers-domain.thm. The application logic would now allow this input and let an attacker control the internal HTTP request.

  

### Open Redirect

If the above bypasses do not work, there is one more trick up the attacker's sleeve, the open redirect. 

An open redirect is an endpoint on the server where the website visitor gets automatically redirected to another website address. 

Take, for example, the link https://website.thm/link?url=https://tryhackme.com. This endpoint was created to record the number of times visitors have clicked on this link for advertising/marketing purposes. But imagine there was a potential SSRF vulnerability with stringent rules which only allowed URLs beginning with https://website.thm/. An attacker could utilise the above feature to redirect the internal HTTP request to a domain of the attacker's choice.