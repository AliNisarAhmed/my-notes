# Authentication Bypass

## Username Enumeration

We can use website error messages to check if a username guess exists in the system.

e.g. entering username = admin and seeing the result: An account with this username already exists.

Example using `ffuf`

```bash
$ ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/signup -mr "username already exists"
```

In the above example: 
- the `-w` argument selects the file's location on the computer that contains the list of usernames that we're going to check exists. 
- The `-X` argument specifies the request method, this will be a GET request by default, but it is a POST request in our example. 
- The `-d` argument specifies the data that we are going to send. In our example, we have the fields username, email, password and cpassword. We've set the value of the username to **FUZZ**. In the ffuf tool, the FUZZ keyword signifies where the contents from our wordlist will be inserted in the request. 
- The `-H` argument is used for adding additional headers to the request. In this instance, we're setting the `Content-Type` to the webserver knows we are sending form data. 
- The `-u` argument specifies the URL we are making the request to. 
- The `-mr` argument is the text on the page we are looking for to validate we've found a valid username.


## Brute Force

If we have a list of valid usernames for a website, we can now use this list to brute force attack on the login page using the same tool `ffuf`

```bash
$ ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/login -fc 200
```

we've chosen `W1` for our list of valid usernames and `W2` for the list of passwords we will try. The multiple wordlists are again specified with the `-w` argument but separated with a comma.  For a positive match, we're using the `-fc` argument to check for an HTTP status code other than 200.

## Logical Flaw

Sometimes authentication processes contain logic flaws. A logic flaw is when the typical logical path of an application is either bypassed, circumvented or manipulated by a hacker. Logic flaws can exist in any area of a website, including Authentication.

Consider a password reset scenario, which asks for the email, and then the username, and then informs the user that password reset link is now in their support tickets/inbox on this web-app. 

To attack: What we can do is create an account on that webapp, get our email to act as the email that receives the password reset link, then use that link to login/change password of the target. 
- The vulnerability works because the code accepts the user's email both as query param as well as POST data, but while the app itself uses the query param, it gives preference to POST data which can be used to insert our own email.