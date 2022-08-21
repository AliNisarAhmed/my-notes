# IDOR

Insecure Direct Object Reference is a type of access control vulnerability.

This type of vulnerability can occur when a web server receives user-supplied input to retrieve objects (files, data, documents), too much trust has been placed on the input data, and it is not validated on the server-side to confirm the requested object belongs to the user requesting it.

Example: 
- A logged in user with proile link = http://online-service.thm/profile?user_id=1305 is able to view other users by changing the user_id parameter.

IDORs can be found in 
- data encoded with Base64 as that is easily reversible
- Hashed Ids 
    - they may follow a predicatble pattern, such as being the hashed version of an integer value
    - or by putting the hash through the online databse like https://crackstation.net to see if we find any matches

Another method is to create two accounts and swap the ID numbers between them.
- if we can view the other user's content using their id number while still being logged in with a different account (or not logged in at all), we have found a valid IDOR vuln.

The vulnerable endpoints are not necessarily to be seen in the browser address bar
- may also be an AJAX request from browser's Network panel
- may also come from **parameter mining**
    - e.g. a call to `/user/details?user_id=15` can be parameter mined to discover that `/user/details?user_id=16` is also visible