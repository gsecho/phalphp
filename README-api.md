phalphp for api
=================

 

Why do this?
http://www.thebuzzmedia.com/designing-a-secure-rest-api-without-oauth-authentication/



Routes
-------------
Routes are stored in `app/config/routes-api.php` as an array. A route has a method (HEAD, GET, POST, PATCH, DELETE, OPTIONS), uri (which can contain regular expressions) and handler/controller to point to.

```php
$routes[] = [
        'method' => 'post',
        'route' => '/ping',
        'handler' => ['Controllers\ExampleController', 'pingAction']
];

$routes[] = [
        'method' => 'get',
        'route' => '/ping',
        'handler' => ['Controllers\ExampleController', 'pingAction']
];

// Example of a route with a parameter (id)
$routes[] = [
        'method' => 'get',
        'route' => '/test/{id}',
        'handler' => ['Controllers\ExampleController', 'testAction']
];

// Example of skipping HMAC authentication on a single page
$routes[] = [
        'method' => 'post',
        'route' => '/skip/{name}',
        'handler' => ['Controllers\ExampleController', 'skipAction'],
        'no-authentication' => FALSE
];
```

Note: For Routes with Paramters, make sure the action you map to has the proper parameters set (in order to read paramters correctly). 
http://docs.phalconphp.com/en/latest/reference/micro.html#defining-routes

Client Requirements
-------------
PHP 5.3+

Required PHP Modules
- Curl (http://php.net/curl)

To check for that module
```bash
$ php -m | grep -i "curl"
curl
```

Server Test
-------------

With `PHP 5.4`, you can use its builtin web server to quickly test functionality. Make sure to be in the public directory when executing the command below.

```bash
cd phalphp/public
php -S localhost:8000 ../htrouter.php
```

Client Test
-------------

Open `client-connect.php` and make sure the host is pointed to the proper url.


When you're ready to test, go ahead and execute it (client application by default points to api.example.com)
```bash
cd phalphp
php client-connect.php
```
Note, if you're using PHP 5.4 built web server (example above) and on the same box, make sure you point the client to the proper server.

```bash
cd phalphp
php client-connect.php localhost:8000
```

Full Example with output from client app
```bash
php client-connect.php localhost:8000

Request: 
POST /ping HTTP/1.1
Host: localhost:8000
Accept: */*
API_ID: 1
API_TIME: 1378703314
API_HASH: de7cd08ab75120791396af887a8b6de7734b211dbe2d443286ed91848f916190
Content-Length: 142
Expect: 100-continue
Content-Type: multipart/form-data; boundary=----------------------------5d9301537cda


Response:
HTTP/1.1 200 OK
Host: localhost:8000
Connection: close
X-Powered-By: PHP/5.4.9-4ubuntu2.3
Content-type: text/html

pong
```

Successful Request
---------------

```http
POST /ping HTTP/1.1
Host: api.example.com
Accept: */*
API_ID: 1
API_TIME: 1377469831
API_HASH: 4cd93cb01ae9a988fbe2922f4ccbc39276ea3626e6016cf80bba32a6447256c5
Content-Length: 143
```

Successful Response
---------------

```http
HTTP/1.1 200 OK
Server: nginx
Date: Sun, 25 Aug 2013 22:27:26 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive

pong
```
