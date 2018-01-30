# Multiple web vulnerabilities at Conceptronic IP Cameras with 0.61.30.X web firmware.
A CSRF and Denial of Service vulnerabilities have been found at Conceptronic IP Cameras.

Tested on:
| Camera Model| System firmware|Web firmware|
| -------------|-------------| -------------|
| CIPCAMPTIWL|00.30.01.0047P3|0.61.30.21|

##### Cross-Site Request Forgery on users.cgi
Every request is vulnerable to Cross-Site Request Forgery due to lack of a CSRF token or any other CSRF protection. Specially sensitive GET request is:
``` 
/hy-cgi/user.cgi?cmd=edituser&at_username=admin&at_newpassword=new_password&at_newrolename=admin&at_userid=10001
``` 
Which allows to change administrator IP without providing the old password. An attacker can exploit this vulnerability by tricking the victim to visit a page which forces its browser to make a request to this URL.

```
/hy-cgi/user.cgi?cmd=adduser&at_username=admin2&at_password=admin2&at_rolename=admin
```

Allows to create a new administrator user.

Also, in special circustances, an attacker can exploit CSRF on 
```
/hy-cgi/user.cgi?cmd=checkuserinfo
```
to view victims current user password on plaintext.

##### Low-privileged authenticated Denial of Service on device.cgi.
A POST request with huge body to
```
/hy-cgi/device.cgi?cmd=searchlandevice
```
causes a crash on the IP Camera which completley freeze the system, making it unnaccesible.


##### Exploits.
Two exploits are provided in this repo for those vulnerabilities, just for testing purposes.
