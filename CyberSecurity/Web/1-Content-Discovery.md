# Content Discovery

There are three main ways of discovering content on a website which we'll cover. Manually, Automated and OSINT (Open-Source Intelligence).

## Manual Discovery 

### Robots.txt file 

The robots.txt file is a document that tells search engines which pages they are and aren't allowed to show on their search engine results or ban specific search engines from crawling the website altogether. 

It can be common practice to restrict certain website areas so they aren't displayed in search engine results. 

These pages may be areas such as administration portals or files meant for the website's customers. 

This file gives us a great list of locations on the website that the owners don't want us to discover as penetration testers.

Usually can be found at `http://website/robots.txt`
- e.g.:https://twitter.com/robots.txt

### Favicon

Sometimes when frameworks are used to build a website, a favicon that is part of the installation gets leftover, and if the website developer doesn't replace this with a custom one, this can give us a clue on what framework is in use. 

OWASP host a database of common framework icons that you can use to check against the targets favicon [https://wiki.owasp.org/index.php/OWASP\_favicon\_database](https://wiki.owasp.org/index.php/OWASP_favicon_database). 

Once we know the framework stack, we can use external resources to discover more about it (see next section).

### Sitemap.xml

Unlike the `robots.txt` file, which restricts what search engine crawlers can look at, the `sitemap.xml` file gives a list of every file the website owner wishes to be listed on a search engine. 

These can sometimes contain areas of the website that are a bit more difficult to navigate to or even list some old webpages that the current site no longer uses but are still working behind the scenes.


### HTTP Headers

When we make requests to the web server, the server returns various HTTP headers. These headers can sometimes contain useful information such as the webserver software and possibly the programming/scripting language in use. In the below example, we can see the webserver is NGINX version 1.18.0 and runs PHP version 7.4.3. Using this information, we could find vulnerable versions of software being used.

### Framework Stack

Once you've established the framework of a website, either from the above favicon example or by looking for clues in the page source such as comments, copyright notices or credits, you can then locate the framework's website. From there, we can learn more about the software and other information, possibly leading to more content we can discover.

## OSINT 

There are also external resources available that can help in discovering information about your target website; these resources are often referred to as OSINT or (Open-Source Intelligence) as they're freely available tools that collect information:


### Google Hacking / Dorking

Google hacking / Dorking utilizes Google's advanced search engine features, which allow you to pick out custom content. You can, for instance, pick out results from a certain domain name using the **site:** filter, for example (site:[tryhackme.com](http://tryhackme.com)) you can then match this up with certain search terms, say, for example, the word admin (site:tryhackme.com admin) this then would only return results from the [tryhackme.com](http://tryhackme.com) website which contain the word admin in its content. You can combine multiple filters as well. Here is an example of more filters you can use:

![4beff656c0697bcc61c7bab084f6b0c9.png](../../images/4beff656c0697bcc61c7bab084f6b0c9.png)

### Wappalyzer

Wappalyzer ([https://www.wappalyzer.com/](https://www.wappalyzer.com/)) is an online tool and browser extension that helps identify what technologies a website uses, such as frameworks, Content Management Systems (CMS), payment processors and much more, and it can even find version numbers as well.

### Wayback Machine

The Wayback Machine ([https://archive.org/web/](https://archive.org/web/)) is a historical archive of websites that dates back to the late 90s. You can search a domain name, and it will show you all the times the service scraped the web page and saved the contents. This service can help uncover old pages that may still be active on the current website.

### Github

 You can use GitHub's search feature to look for company names or website names to try and locate repositories belonging to your target. Once discovered, you may have access to source code, passwords or other content that you hadn't yet found.
 
### S3 Buckets

S3 Buckets are a storage service provided by Amazon AWS, allowing people to save files and even static website content in the cloud accessible over HTTP and HTTPS. 

The owner of the files can set access permissions to either make files public, private and even writable. Sometimes these access permissions are incorrectly set and inadvertently allow access to files that shouldn't be available to the public. 

The format of the S3 buckets is http(s)://**{name}.**[**s3.amazonaws.com**](http://s3.amazonaws.com/) where {name} is decided by the owner, such as [tryhackme-assets.s3.amazonaws.com](http://tryhackme-assets.s3.amazonaws.com). 

S3 buckets can be discovered in many ways, such as finding the URLs in the website's page source, GitHub repositories, or even automating the process. 

One common automation method is by using the company name followed by common terms such as **{name}**-assets, **{name}**-www, **{name}**-public, **{name}**-private, etc.

## Automated Discovery

**What is Automated Discovery?**

Automated discovery is the process of using tools to discover content rather than doing it manually. This process is automated as it usually contains hundreds, thousands or even millions of requests to a web server. These requests check whether a file or directory exists on a website, giving us access to resources we didn't previously know existed. This process is made possible by using a resource called wordlists.

**What are wordlists?**

Wordlists are just text files that contain a long list of commonly used words; they can cover many different use cases. For example, a password wordlist would include the most frequently used passwords, whereas we're looking for content in our case, so we'd require a list containing the most commonly used directory and file names. An excellent resource for wordlists that is preinstalled on the THM AttackBox is [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists) which Daniel Miessler curates.

**Automation Tools**

Examples: `ffuf`, `dirb` and `gobuster`.

https://github.com/ffuf/ffuf
https://github.com/OJ/gobuster
https://www.kali.org/tools/dirb/

![29a1e166cb96e4e5f89ae3aab464ac5f.png](../../images/29a1e166cb96e4e5f89ae3aab464ac5f.png)

![f0814e413020433575cc70fa1407b6d4.png](../../images/f0814e413020433575cc70fa1407b6d4.png)
