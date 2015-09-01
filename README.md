# Assignment 2 (100 points) — Operation Knock-Knock

## Description

Your mission, should you choose to accept it, is to write a web application on behalf of the super secret spy agency: CSE591.

You will build a web application that hides in plain sight, with hidden functionality that only our secret agents know exists.

The main functionality of your web application will have user registration, login, logout, as well as, a message board, with users able to post messages.

Each user will have a unique “knock sequence” (described later). Once the secret “knock sequence” of URLs is requested by a logged-in session, your application must switch into “secret” mode. Everything should remain the same, except now secret messages will be shown and posted.

Important: The specification of the URLs must be followed exactly as defined here.

Just as in Assignment 1 Part 3, every HTML page that your web application outputs must be valid HTML 5. Also, none of your form or a elements should have an attribute target with the value of _blank. This is a technique for doing pop-ups and breaks the automated grading. Plus, it is evil.

URL Interface

Here, all URLs are given relative to the root of your web application. Assume that your web application is running at http://example.com:8080, then the URL /user/register described below would be accessed to the web application at http://example.com:8080/user/register.

User Management

Users will have usernames and passwords.

/user/register
Required Page Elements:

Form, name attribute of register Four inputs on the form:

name attribute of username, type of text
name attribute of password, type of password
name attribute of password_confirm, type of password
name attribute of submit, type of submit
Action after submit:

Create a user with the given username and password iff (if and only if): password matches password_confirm and username does not already exist.

/user/login
Required Page Elements:

Form, name attribute of login Three inputs on the form:

name attribute of username, type of text
name attribute of password, type of password
name attribute of submit, type of submit
Action after submit:

If the username and password are of a previously registered user, then the user is logged into the system. Once logged in, the knock sequence starts.

/user/logout
No Required Page Elements

When the user accesses the /user/logout page (GET), then the user will be logged out of the web application. The knock sequence stops.

Message Management

/message/add
Requires logged-in user.

Required Page Elements:

Form, name attribute of create-message Two inputs on the form:

name attribute of title, type of text
name attribute of submit, type of submit
One textarea on the form:

name attribute of message
Action after submit:

If there is a title and message, then the message is added to the list of messages. If the user’s session is in “secret” mode, then the message will be added to the secret messages.

/message/list
Requires logged-in user.

Required Page Elements:

One div with a class attribute of message per message.

Each div must contain the text of the title of the message and the message content.

Messages by all users are shown. If the user’s session is in “secret” mode, then only secret messages must be displayed (everything else on the page remains the same). If the user’s session is not in “secret” mode, then no secret messages should be displayed.

Knock Sequence Algorithm

Each user will have a different knock sequence, which is a function (in the mathematical sense) of their username.

[
  0 : /user/register,
  1 : /user/login,
  2 : /message/add,
  3 : /message/list,
]
Given a username, which is a string, take the md5 hash of the username. Convert it to hexadecimal (there should be 32 hexadecimal digits). The first hexadecimal digit of the hash modulo 4 will be the first element of the knock sequence (using the mapping above), the second hexadecimal digit of the hash modulo 4 will be the second element of the knock sequence, and so on for a knock sequence with total length of 4.

Consider the following example:

For the user who registers with the username “ObMaX” (without quotes), the md5 of this username is “b86ec61e49774117d6ba2b4f183a4a8e” (again, without the quotes). The first four digits of the md5 are [b, 8, 6, e], these digits modulo 4 are [3, 0, 2, 2], so the knock sequence will be [ /message/list, /user/register, /message/add, and /message/add ]

Consider the following example URL accesses (starting as an unknown user):

/message/list
/user/register
/message/add
/message/add
Even though this is a valid knock sequence, the user is not logged in, therefore their application should not be in secret mode. Continuing:

/user/login
Submit form on /user/login with username "ObMaX" and correct password
/message/list
Now, the messages listed are the normal messages. Continuing:

/message/add
Submit form on /message/add with text "Test"
The message with text “Test” just created will be a normal message. Continuing:

/message/list
The message with text “Test” will be shown on this page. Continuing:

/user/register
/message/add
/message/add
The knock sequence has been accessed correctly, and the user is now in “secret” mode. Continuing:

/message/list
The message with text “Test” will not be shown on this page. Continuing:

/message/add
Submit form on /message/add with text "Secret Text"
The message with text “Secret Text” just created will be a secret message. Continuing:

/message/list
This page now contains the message with text “Secret Text”. Continuing:

/user/logout
The user is now no longer in secret mode and is logged out of the web application. Continuing:

/user/login
Submit form on /user/login with username "ObMaX" and correct password
/message/list
There will be a message with text “Test” on this page, and no message with text “Secret Text”.
