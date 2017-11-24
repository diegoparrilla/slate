---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
#  - javascript
#  - java
#  - ruby
#  - python
#  - php
#  - objective_c
#  - csharp
#  - c

toc_footers:
  - <a href='https://dashboard.apility.io/#/register'>Sign Up for a free account</a>
  - <a href='https://dashboard.apility.io/#/login'>Log in</a>
  - <a href='https://apility.io'>Apility.io Homepage</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# What is Apility.io

[Apility.io](https://apility.io) can be defined as a Look up as a Service for developers and product companies that want to know in realtime if their existing or potential users have been classified as 'abusers' by one or more of these lists.

Automatic extraction processes extracts all the information in realtime, keeping the most up to date data available, saving yourself the hassle of extract and update regularly all these lists and the data.

We have several language bindings! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and submit pull requests if you want to improve this documentation.

## What does Apility.io offer?

Apility.io offers an extremely simple and minimalistic REST style API to access in realtime to these lists and do the following simple question about the resource?

__Is this IP, domain or email stored in any blacklist?__

The answers to this question can be:

* YES: The resource can be found in an abusers' list. This is a __bad__ resource.
* NO: The resource cannot be found in any abusers' list. This is a __clean__ resource.

A bad resource implies some kind of action from developers' side. A clean resource does not need any action from their side.

<aside class="notice">
The API will also return HTTP error codes
</aside>

## What resources are listed?

There are several different type of resources that will grow in time:

* __IP__: IP version 4 that has been used in any abusing activities like spam, attacks, hacking activities and others.
* __Domains__: Domain names used as email registration, source of attacks and others.
* __Emails__: Emails used in spam and fraudulent activities.
* __IP Geolocation__: For Geolocation activities, access to a Geolocation Rest API based in MaxMind GeoIP Lite database is also available.
* __Autonomous Systems__: Get the Autonomous System and network by the IP or the number.

## What blacklists are in use?

Blacklists change on a daily basis. Please follow the links in the [main page](https://apility.io/blacklists).

## Where are the API endpoints servers deployed?

There are multiple servers deployed worldwide in different Cloud Providers, scaling up and down automatically to handle peaks and valleys usage. If you don't care about using a non secure connection then the api can be reached at

<aside class="notice">
http://api.apility.io
</aside>


But if you care about non using a secured connection you can use SSL-terminated endpoints at:

<aside class="notice">
https://api.apility.io
</aside>

When you perform a request to this endpoint, thanks to the magic of DNS Anycast and latency-based resolution you will be served with the closest server available.

# How to use the API

## Design principles

The access to the API has been developed to be simple, minimalistic and fast. We have followed the Keep it Simple (KISS) approach, with several different ways to access the data.

To use the API you will need a API Key or Token that you will pass along your request. [You need to sign up to get it](https://dashboard.apility.io/#/register), and yes, there is a free plan you can use as long as you want.

API is restricted by this API Key. Each subscription plan is limited to a number of API requests per day. If you exceed that limit in a 24 hour period the service will return a 429 HTTP status code. If you need to make more requests you should consider a paid plan with more quota. You can know in real time the quota consumed daily from the Dashboard.


## Authentication

> To authorize with a header parameter, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.apility.io"
  -H "X-Auth-Token: UUID"
```

> To authorize with a query string parameter, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.apility.io/?token=UUID"
```

> Make sure to replace `UUID` with your API key.

Apility.io uses API keys to allow access to the API. [You need to sign up to get an API Key.](https://dashboard.apility.io/#/register)

Apility.io expects for the API Key to be included in all API requests made. It can be included as a header or as a query string parameter.

`X-Auth-Token: UUID`

<aside class="notice">
You must replace <code>UUID</code> with your own API key.
</aside>

## Simple Model: GET requests and HTTP response codes

> ___Example: Is IP 1.2.3.4 in any blacklist?___

> We will use curl from the command line.

```shell
$ curl -I -X GET api.moocher.io/badip/1.2.3.4 -H "X-Auth-Token: UUID"
```

> The response is:

```shell
HTTP/1.1 200 OK
CONTENT-TYPE: text/plain; charset=utf-8
CONTENT-LENGTH: 7
DATE: Tue, 21 Jun 2016 08:52:14 GMT
SERVER: Python/3.5 aiohttp/0.21.6
```

> __The HTTP 200 (OK) code means the IP 1.2.3.4 belongs to any blacklist.__


> ___Example: Is IP 8.8.8.8 in any blacklist?___

> We will use curl from the command line.

```shell
$ curl -I -X GET api.moocher.io/badip/8.8.8.8 -H "X-Auth-Token: UUID"
```

>The response is:

```shell
HTTP/1.1 404 Not Found
CONTENT-TYPE: text/plain; charset=utf-8
CONTENT-LENGTH: 18
DATE: Sun, 26 Jun 2016 15:41:40 GMT
SERVER: Python/3.5 aiohttp/0.21.6
```

> __The HTTP 404 (Not Found) code means the IP 8.8.8.8 does not belong to any blacklist (8.8.8.8 is a public DNS by Google, which makes a lot of sense to not be in any blacklist of abusers...)__

The easiest way to use the API get advantage of GET requests and responses with standard HTTP codes. In this model, a _HTTP 200 (OK)_ response code means that the value passed in the request belongs to any lists and therefore is a bad resource. If the answer is a _HTTP 404 (Not found)_ code that means it does not belong to any lists and therefore is a clean resource.

Developers just need to check the status code returned in the response. If code is 200 (OK), then the resource was found in any list and it's a bad resource. If the code is 404 (Not found), the resource was not found in any list.

## Advanced Model: GET requests, JSON and HTTP response codes

> ___Example: Is domain gmail.com in any blacklist?___

```shell
$ curl -H "Content-Type: application/json" -H "X-Auth-Token: UUID" -X GET https://api.apility.io/baddomain/gmail.com
```

> The response is:

```shell
{  
   "type":"baddomain",
   "response":{  
      "domain":{  
         "mx":[  
            "alt4.gmail-smtp-in.l.google.com",
            "alt2.gmail-smtp-in.l.google.com",
            "gmail-smtp-in.l.google.com",
            "alt1.gmail-smtp-in.l.google.com",
            "alt3.gmail-smtp-in.l.google.com"
         ],
         "ns":[  
            "ns3.google.com.",
            "ns4.google.com.",
            "ns1.google.com.",
            "ns2.google.com."
         ],
         "blacklist_mx":[ ],
         "blacklist_ns":[ ],
         "blacklist":[  
            "FREEMAIL"
         ],
         "score":-1
      },
      "score":-1,
      "source_ip":{  
         "score":0,
         "geo":{},
         "is_quarantined":false,
         "blacklist":[ ]
      },
      "ip":{  
         "score":0,
         "geo":{  
            "country":"US",
            "continent":"NA",
            "hostname":"mad06s25-in-f5.1e100.net.",
            "city":"Mountain View",
            "as":{  
               "country":"US",
               "asn":"15169",
               "name":"GOOGLE - Google Inc."
            },
            "region":"California",
            "postal":"94043",
            "address":"216.58.201.133",
            "longitude":-122.0574,
            "latitude":37.419200000000004
         },
         "is_quarantined":false,
         "blacklist":[ ]
      }
   }
}
```

> Gmail is in the blackist FREEMAIL. It contains a catalog of Free Email services worldwide. You can remove FREEMAIL from the blacklists from the dashboard if you don't want to restrict access to Free Email services.

This model follows the approach of the simple model, but allows to know the blacklists the requested resource is. This model still follow that if the response is an _HTTP code 200 (OK)_ means that the value passed in the request is part of the lists and therefore is a bad resource. But this answer returns a set of list names the resource belongs to.

And if the answer is an _HTTP 404 (Not found)_ code means that the value cannot be found in the lists and therefore is a clean resource.

To use Advanced Model, the developer needs to add to the header of the requests the __application/json__ response __Content-Type__:

Developers will need to parse and analyze the JSON object returned in their applications.

## JSONP support

> ___Example: Is email marketing@moocher.io in any blacklist?___

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/bademail/marketing@apility.io?callback=myfunction"
```

>The response is:

```shell
myfunction(
{"error":
    { "message": "Resource not found",
      "status": 404
    }
});
```

> The marketing email from Apility.io is not in any blacklist, and the error comes as a JSON payload inside a function call as JSONP needs.

> ___Example: Is domain mailinator.com in any blacklist?___

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/baddomain/mailinator.com?callback=myfunction"
```

> The response is:

```shell
myfunction(
{"blacklists": ["IVOLO-DED", "DEA"]}
);
```

> Mailinator is a very well known Disposible Email Address provider, and the response returns as a JSON payload inside a function call as JSONP needs.

JSONP (JSON with Padding or JSON-P) is a technique used by web developers to overcome the cross-domain restrictions imposed by browsers' same-origin policy that limits access to resources retrieved from origins other than the one the page was served by.  [Read more details in the wikipedia](https://en.wikipedia.org/wiki/JSONP)

The developer does not  need to add to the header of the requests the __application/json__ response __Content-Type__, it will be added implicit when the parameter __callback__ is passed:

With JSONP requests some client side libraries can't manage HTTP codes properly, so the error responses are returned as JSON payload as follows:

## CORS restrictions

[Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a mechanism that uses additional HTTP headers to let a user agent gain permission to access selected resources from a server on a different origin (domain) than the site currently in use. A user agent makes a cross-origin HTTP request when it requests a resource from a different domain, protocol, or port than the one from which the current document originated.

Paid plans can configure the allowed referers site to prevent unauthorized use and quota theft. Key restriction lets you specify which web sites, IP addresses, or apps can use this key. You can test this feature during the 30 days trail period.

## IP Address restrictions

Paid plans can restrict the source IP of the requests to prevent unauthorized use and quota theft. You can test this feature during the 30 days trail period.

# IP Check

Apility.io tracks multiple abuse blacklists and consolidates them in a single database you can look up with our minimalist API.

## Check if an IP belongs to any abusers' blacklist

```shell
$ curl -I -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/badip/<IP>"
```

>The response can be:

```shell
HTTP/1.1 200 OK
CONTENT-TYPE: text/plain; charset=utf-8
CONTENT-LENGTH: 7
DATE: X
SERVER: Python/3.5 aiohttp/0.21.6
```

>or

```shell
HTTP/1.1 404 Not Found
CONTENT-TYPE: text/plain; charset=utf-8
CONTENT-LENGTH: 18
DATE: X
SERVER: Python/3.5 aiohttp/0.21.6
```


This endpoint returns if the IP belongs to any list with the HTTP status code in the response.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/badip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP | The IP to look up in the system.

### Response

The response can be a 200 or 404 HTTP code if everything is ok:

* __HTTP/1.1 200 OK__ : 200 (OK) means that the IPV4 is in any blacklist and then it is a __bad__ IP
* __HTTP/1.1 404 Not Found__: and 404 (OK) means that the IPV4 is a clean IP.

<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

## Get all abusers' blacklist an IP belongs to

```shell
$ curl -H "Content-Type: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/badip/<IP>"
```

>The response can be:

```shell
{
    "blacklists": ["STOPFORUMSPAM-180", "STOPFORUMSPAM-30", "STOPFORUMSPAM-7", "STOPFORUMSPAM-365", "STOPFORUMSPAM-90"]
}
```

>or

```shell
Resource Not found
```

If a developer wants to know what blacklists contain the IP passed as argument, she needs to pass to curl an extra argument with information about the Content type as __application/json__:

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/badip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Content-Type | Yes | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP | The IP to look up in the system.

### Response

The status code of the response can be a 200 or 404 HTTP code if everything is ok, but it will also return the following JSON structure if status code is 200:

Parameter | Description
--------- | -----------
blacklists | Array with a list with the Identifiers of each blacklist.


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

# Domain Check

Domain check implements an algorithm that assigns a score to the domain based on several checks done by the system:

* Is the domain in any of the domain blacklists?
* Is the domain MX records in any of the domain blacklists?
* Is the domain NS records in any of the domain blacklists?
* Is the IP of domain in any of the IP blacklists?

By default if some of these tests success, a -1 score is added to the overall score of the domain. So an overall score of -3 means the chances of being a bad domain are higher than -1.

## Check if a domain scores a negative or neutral score.

> Check a "clean" domain

```shell
$ curl -I -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/baddomain/google.com"
```

>The response:

```shell
HTTP/1.1 404 Not Found
Server: nginx/1.10.3 (Ubuntu)
Date: Fri, 24 Nov 2017 13:59:42 GMT
Content-Type: text/plain; charset=utf-8
Content-Length: 18
Connection: keep-alive
```

> Check a "bad" domain

```shell
$ curl -I -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/baddomain/mailinator.com"
```

```shell
HTTP/1.1 200 OK
Server: nginx/1.10.3 (Ubuntu)
Date: Fri, 24 Nov 2017 14:00:50 GMT
Content-Type: text/plain; charset=utf-8
Content-Length: 7
Connection: keep-alive
Strict-Transport-Security: max-age=63072000; includeSubdomains
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
```

This endpoint returns in the HTTP status code of the response if the domain has a neutral or a negative score. A negative score means that the domain has been classified as 'bad' by the algorithm.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/baddomain/<DOMAIN>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
DOMAIN    | The Domain name to look up in the system.

### Response

The response can be a 200 or 404 HTTP code if everything is ok:

* __HTTP/1.1 200 OK__ : 200 (OK) means that the score calculated for the domain is less than zero and then a __bad__ Domain
* __HTTP/1.1 404 Not Found__: and 404 (OK) means that the score calculated for the domain is zero or higher and it is a clean Domain.

<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

## Get individual scores of a domain

> Get all information from a "clean" domain

```shell
$ curl -H "Content-Type: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/baddomain/google.com"
```

>The response can be:

```shell
{
   "response":{
      "domain":{
         "score":0,
         "blacklist_ns":[],
         "mx":[
            "alt2.aspmx.l.google.com",
            "alt1.aspmx.l.google.com",
            "aspmx.l.google.com",
            "alt4.aspmx.l.google.com",
            "alt3.aspmx.l.google.com"
         ],
         "blacklist_mx":[],
         "blacklist":[],
         "ns":[
            "ns2.google.com.",
            "ns3.google.com.",
            "ns1.google.com.",
            "ns4.google.com."
         ]
      },
      "score":0,
      "ip":{
         "score":0,
         "is_quarantined":false,
         "address":"74.125.133.102",
         "blacklist":[]
      },
      "source_ip":{
         "score":0,
         "is_quarantined":false,
         "address":"2.137.249.73",
         "blacklist":[]
      }
   },
   "type":"baddomain"
}
```

> Get all information from a "bad" domain

```shell
$ curl -H "Content-Type: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/baddomain/mailinator.com"
```

>The response can be:

```shell
{
   "response":{
      "domain":{
         "score":-1,
         "blacklist_ns":[

         ],
         "mx":[
            "mail2.mailinator.com",
            "mail.mailinator.com"
         ],
         "blacklist_mx":[

         ],
         "blacklist":[
            "IVOLO-DED",
            "DEA"
         ],
         "ns":[
            "betty.ns.cloudflare.com.",
            "james.ns.cloudflare.com."
         ]
      },
      "score":-1,
      "ip":{
         "score":0,
         "is_quarantined":false,
         "address":"104.25.198.31",
         "blacklist":[

         ]
      },
      "source_ip":{
         "score":0,
         "is_quarantined":false,
         "address":"2.137.249.73",
         "blacklist":[

         ]
      }
   },
   "type":"baddomain"
}
```

This endpoint returns a full JSON structure with individual details about the score of the different domains and IP analyzed by the algorithm. The request always returns the status code 200 (HTTP OK) no matter if the domain is bad or clean.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/baddomain/<DOMAIN>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Content-Type | Yes | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
DOMAIN    | The Domain name to look up in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
response  | JSON structure containing the '[Domain](#domain)' object.
type      | String describing the response type: baddomain | badip | bademail


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

# Email Check

Email check implements an algorithm as a superset of the used for the domain analysis that assigns a score to the email based on several checks done by the system:

* Is the domain in any of the domain blacklists?
* Is the domain MX records in any of the domain blacklists?
* Is the domain NS records in any of the domain blacklists?
* Is the IP of domain in any of the IP blacklists?
* Is the Email in any of the Email blacklists?
* Is the domain in a Free Email Provider list?
* Is the domain in a Disposable Email Address Provider list?
* Does the email inbox really exists?
* Is the email well formed?

By default if some of these tests success, a -1 score is added to the overall score of the email. So an overall score of -3 means the chances of being a bad email are higher than -1.

## Check if an email scores a negative or neutral score.

> Check a "clean" email

```shell
$ curl -I -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/bademail/support@apility.io"
```

>The response:

```shell
HTTP/1.1 404 Not Found
Server: nginx/1.10.3 (Ubuntu)
Date: Fri, 24 Nov 2017 15:42:53 GMT
Content-Type: text/plain; charset=utf-8
Content-Length: 18
Connection: keep-alive
```

> Check a "bad" email

```shell
$ curl -I -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/bademail/test@mailinator.com"
```

```shell
HTTP/1.1 200 OK
Server: nginx/1.10.3 (Ubuntu)
Date: Fri, 24 Nov 2017 15:43:55 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 809
Connection: keep-alive
Strict-Transport-Security: max-age=63072000; includeSubdomains
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
```

This endpoint returns in the HTTP status code of the response if the email has a neutral or a negative score. A negative score means that the email has been classified as 'bad' by the algorithm.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/bademail/<EMAIL>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
EMAIL     | The Email name to look up in the system.

### Response

The response can be a 200 or 404 HTTP code if everything is ok:

* __HTTP/1.1 200 OK__ : 200 (OK) means that the score calculated for the email is less than zero and then a __bad__ Email
* __HTTP/1.1 404 Not Found__: and 404 (OK) means that the score calculated for the email is zero or higher and it is a clean Email.

<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

## Get individual scores of an email

> Get all information from a "clean" email

```shell
$ curl -H "Content-Type: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/bademail/ceo@apility.io"
```

>The response can be:

```shell
{
   "response":{
      "score":0,
      "address":{
         "score":0,
         "is_role":false,
         "is_well_formed":true
      },
      "ip":{
         "score":0,
         "is_quarantined":false,
         "address":"52.218.49.2",
         "blacklist":[

         ]
      },
      "smtp":{
         "exist_mx":true,
         "score":0,
         "exist_address":false,
         "exist_catchall":false
      },
      "freemail":{
         "score":0,
         "is_freemail":false
      },
      "domain":{
         "score":0,
         "blacklist_ns":[

         ],
         "mx":[
            "aspmx.l.google.com",
            "aspmx2.googlemail.com",
            "aspmx3.googlemail.com",
            "alt1.aspmx.l.google.com",
            "alt2.aspmx.l.google.com"
         ],
         "blacklist_mx":[

         ],
         "blacklist":[

         ],
         "ns":[
            "ns-1395.awsdns-46.org.",
            "ns-1635.awsdns-12.co.uk.",
            "ns-213.awsdns-26.com.",
            "ns-614.awsdns-12.net."
         ]
      },
      "email":{
         "score":0,
         "blacklist":[

         ]
      },
      "source_ip":{
         "score":0,
         "is_quarantined":false,
         "address":"2.137.249.73",
         "blacklist":[

         ]
      },
      "disposable":{
         "score":0,
         "is_disposable":false
      }
   },
   "type":"bademail"
}
```

> Get all information from a "bad" email

```shell
$ curl -H "Content-Type: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/bademail/test@mailinator.com"
```

>The response can be:

```shell
{
   "response":{
      "score":-3,
      "address":{
         "score":0,
         "is_role":false,
         "is_well_formed":true
      },
      "ip":{
         "score":0,
         "is_quarantined":false,
         "address":"104.25.199.31",
         "blacklist":[

         ]
      },
      "smtp":{
         "exist_mx":true,
         "score":0,
         "exist_address":false,
         "exist_catchall":false
      },
      "freemail":{
         "score":0,
         "is_freemail":false
      },
      "domain":{
         "score":-1,
         "blacklist_ns":[

         ],
         "mx":[
            "mail2.mailinator.com",
            "mail.mailinator.com"
         ],
         "blacklist_mx":[

         ],
         "blacklist":[
            "IVOLO-DED",
            "DEA"
         ],
         "ns":[
            "betty.ns.cloudflare.com.",
            "james.ns.cloudflare.com."
         ]
      },
      "email":{
         "score":-1,
         "blacklist":[
            "STOPFORUMSPAM-90",
            "STOPFORUMSPAM-180",
            "STOPFORUMSPAM-365"
         ]
      },
      "source_ip":{
         "score":0,
         "is_quarantined":false,
         "address":"2.137.249.73",
         "blacklist":[

         ]
      },
      "disposable":{
         "score":-1,
         "is_disposable":true
      }
   },
   "type":"bademail"
}
```

This endpoint returns a full JSON structure with individual details about the score of the different domains, IP, SMTP and other parameters analyzed by the algorithm. The request always returns the status code 200 (HTTP OK) no matter if the domain is bad or clean.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/bademail/<EMAIL>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Content-Type | Yes | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
EMAIL     | The Email name to look up in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
response  | JSON structure containing the '[Email](#email)' object.
type      | String describing the response type: baddomain | badip | bademail


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

# Geo IP look up

The Geo IP service cannot be classified as __blacklists__ as the lists above, but are really helpful to analyze an IP and create a profile of your users. Just like blacklists they can be accessed with our simple API and they are rate limited the same way. So if you get 429 error (Too many requests) please consider upgrading to a paid plan.

This service returns the geo location of any IP with relevant information like:

* Continent
* Country
* Region
* City
* Postal code
* Latitude and Longitude
* Autonomous System
* Reverse Hostname

This API request will always return a JSON structure with the information and a 200 HTTP code if the IP can be found in the database. If not, then it will return a 404 HTTP code as usual.


## Get IP geo location.

> Get the geo location data of an IP:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/geoip/9.9.9.9"
```

>The response can be:

```shell
{
   "ip":{
      "longitude":2.3387000000000002,
      "postal":"",
      "hostname":"dns.quad9.net",
      "as":{
         "networks":"['9.9.9.0/24', '149.112.112.0/24', '149.112.149.0/24']",
         "asn":"19281",
         "name":"QUAD9-AS-1 - Quad9",
         "country":"US"
      },
      "latitude":48.8582,
      "country":"FR",
      "region":"",
      "address":"9.9.9.9",
      "continent":"EU",
      "city":""
   }
}
```

This endpoint returns a JSON structure with individual details about the geographical location of the IP, and information about the Autonomous System and its networks. The request always returns the status code 200 (HTTP OK) if the IP exists. If the IP is malformed it will return a 400 (HTTP Bad Request) code.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/geoip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Content-Type | Yes | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP        | The IP to geo locate in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
ip  | JSON structure containing the '[geoip](#geoip)' object.


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

# Autonomous System look up

The Autonomous System look up services cannot be classified as __blacklists__ too, but are really helpful to deep analyze an IP and create a profile of your users. Just like blacklists they can be accessed with our simple API and they are rate limited the same way. So if you get 429 error (Too many requests) please consider upgrading to a paid plan.

This service returns the Autonomous System information of an IP or ASN:

* ASN
* Country
* Name
* Networks

This API request will always return a JSON structure with the information and a 200 HTTP code if the IP or AS number can be found in the system. If not, then it will return a 404 HTTP code as usual.


## Get the AS information from an IP.

> Get the AS data of an IP:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/as/ip/9.9.9.9"
```

>The response can be:

```shell
{
   "as":{
      "networks":"['9.9.9.0/24', '149.112.112.0/24', '149.112.149.0/24']",
      "asn":"19281",
      "name":"QUAD9-AS-1 - Quad9",
      "country":"US"
   }
}
```

This endpoint returns a JSON structure with details about the Autonomous System that owns the IP, and information about its networks. The request always returns the status code 200 (HTTP OK) if the IP exists. If the IP is malformed it will return a 400 (HTTP Bad Request) code.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/as/ip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Content-Type | Yes | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP        | The IP to look up the AS in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
as  | JSON structure containing the ''[as](#as)'' object.


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>


## Get the AS information from its number.

> Get the AS data from its number:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.io/as/num/19281"
```

>The response can be:

```shell
{
   "as":{
      "networks":"['9.9.9.0/24', '149.112.112.0/24', '149.112.149.0/24']",
      "asn":"19281",
      "name":"QUAD9-AS-1 - Quad9",
      "country":"US"
   }
}
```

This endpoint returns a JSON structure with details about the Autonomous System with the number, and information about its networks. The request always returns the status code 200 (HTTP OK) if the IP exists. If the IP is malformed it will return a 400 (HTTP Bad Request) code.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.io/as/num/<NUMBER>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Content-Type | Yes | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
NUMBER    | The Number of the AS to look up in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
as  | JSON structure containing the '[as](#as)' object.


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>


# Objects

## domain

The domain object contains the information used in the scoring algorithm.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' domain. Neutral or positivo means it's a 'clean' domain.
domain | JSON structure containing the '[domainname score](#domainname-score)' object as result of the analysis of the domains.
ip | JSON structure containing the '[ip score](#ip-score)' object as result of the analysis of the IP of the domain.
source_ip | JSON structure containing the '[ip score](#ip-score)' object as result of the analysis of the IP origin of the request.

## email

The email object contains the information used in the scoring algorithm.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' domain. Neutral or positive means it's a 'clean' domain.
domain | JSON structure containing the '[domainname score](#domainname-score)' object as result of the analysis of the domains.
ip | JSON structure containing the '[ip score](#ip-score)' object as result of the analysis of the IP of the domain.
source_ip | JSON structure containing the '[ip score](#ip-score)' object as result of the analysis of the IP origin of the request.
address | JSON structure containing the '[address score](#address-score)' object as result of the analysis of the email.
smtp  | JSON structure containing the '[smtp score](#smtp-score)' object as result of the analysis of the email service.
freemail | JSON structure containing the '[freemail score](#freemail-score)' object as result of the analysis of the email provider.
email | JSON structure containing the '[email-blacklist score](#email-blacklist-score)' object as result of the look up in the email blacklists.
disposable | JSON structure containing the '[disposable score](#disposable-score)' object as result of the analysis of the email provider.

## geoip

The geoip object contains the information used in Geolocation of an IP.

Parameter     | Description
------------- | -----------
longitude | Longitude where the IP has been found
latitude | Latitude where the IP has been found
hostname | Name of the host resolved from the IP
address | IPv4 or IPv6 address of the request
continent | 2 letter code of the continent
country | ISO 3166-1 Country code
region | Name of the region
city | Name of the city
postal | Postal code or Zip code
as | JSON structure containing the '[as](#as)' object.

## as

The AS object contains the information of Autonmous System

Parameter     | Description
------------- | -----------
asn | AS number
name | name of the AS
country | ISO 3166-1 Country code
networks | Array with the lists of networks of the AS

## domainname score

The domainname score contains the information of testing different subdomains of the main root domain: NS records, MX records and domain blacklists.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' domain. Neutral or positive means it's a 'clean' domain.
blacklist_ns | Array containing the blacklists where the NS domains were found.
blacklist_mx | Array containing the blacklists where the MX domains were found.
blacklist | Array containing the blacklists where the domain was found.
mx | Array with the hosts found in the MX records.
ns | Array with the hosts found in the NS records.

## ip score

The ip score contains the information of looking up the IP in the blacklists.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' IP. Neutral or positive means it's a 'clean' IP.
blacklist | Array containing the blacklists where the IP was found.
is_quarantined | If the IP has been added by the user to the quarantine lists.
address | IPv4 or IPv6 resolved.

## address score

The address score contains the information of checking the format of the email.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' email. Neutral or positive means it's a 'clean' email.
is_role | The email has the format of a role-based-address. It's not common to allow registration with role-based-addresses.
is_well_formed | The email is compliant or not with the standards and could cause issues in some systems.

## smtp score

The smtp score contains the information obtained after testing the remote inbox where the email is hosted.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' email. Neutral or positive means it's a 'clean' email.
exist_mx | The SMTP service is reachable using the hosts in the MX records.
exist_address | The SMTP service recognizes the email address.
exist_catchall | The SMTP service implements a catch-all email feature.

## freemail score

The freemail score contains the information of looking up the domain in the lists of Free Email Service Providers.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' domain. Neutral or positive means it's a 'clean' domain.
is_freemail | The domain has been found in any Free Email Service Provider list.

## disposable score

The disposable score contains the information of looking up the domain in the lists of Disposable Email Addresses Providers.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' domain. Neutral or positive means it's a 'clean' domain.
is_disposable | The domain has been found in any Disposable Email Address Providers list.

## email score

The email score contains the information of looking up the email in the blacklists.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' email. Neutral or positive means it's a 'clean' email.
blacklist | Array containing the blacklists where the email was found.
