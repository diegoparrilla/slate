---
title: Apility.io API Reference Guide

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
  - <a href='https://apility.io'>Homepage</a>
  - <a href='https://apility.io/docs'>General Documentation</a>
  - <a href='https://support.apility.io'>Support</a>
  - <a href='https://apility.io/search'>Search Engine</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# What is Apility.io

[Apility.io](https://apility.io) can be defined as Threat Intelligence SaaS for developers and product companies that want to know in realtime if their existing or potential users have been classified as 'abusers' by one or more of these lists.

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
http://api.apility.net
</aside>


But if you care about non using a secured connection you can use SSL-terminated endpoints at:

<aside class="notice">
https://api.apility.net
</aside>

When you perform a request to this endpoint, thanks to the magic of DNS Anycast and latency-based resolution you will be served with the closest server available.

# How to use the API

## Design principles

The access to the API has been developed to be simple, minimalistic and fast. We have followed the Keep it Simple (KISS) approach, with several different ways to access the data.

[To use the API you will need a API Key or Token](https://apility.io/docs/step-2-api-key/) that you will pass along your request. [You need to sign up to get it](https://dashboard.apility.io/#/register), and yes, there is a free plan you can use as long as you want.

API is restricted by this API Key. Each subscription plan is [limited to a number of API requests per day](https://apility.io/docs/difference-hits-requests/). If you exceed that limit in a 24 hour period the service will return a 429 HTTP status code. If you need to make more requests you should consider a paid plan with more quota. You can know in real time the quota consumed daily from the Dashboard.

There is also an ANONYMOUS PLAN. The platform applies this plan if a developer does not pass any API Token when doing a request. So you can test the API right way before signing up!

Please read more details about [Plans, Pricing and limits by day and hour](https://apility.io/docs/step-4-plans-pricing/)


## The Administration Dashboard

To enjoy the API and the different Apility.io services it is necessary for the user to register on our platform. Once registered, the user will have access to a 30-day Trial of the service that will allow you to know without any cost the benefits of our solution. If after these 30 days the user does not enter the method of payment, you will enjoy the service for free for an unlimited period of time but with some restrictions.

By registering on the platform [you have access to the administration dashboard](https://dashboard.apility.io/#/login). This administration console allows you to manage and configure the different services to tailor them to the needs of each client.


## Authentication

> To authorize with a header parameter, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.apility.net"
  -H "X-Auth-Token: UUID"
```

> To authorize with a query string parameter, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.apility.net/?token=UUID"
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
$ curl -i -X GET api.apility.net/badip/1.2.3.4 -H "X-Auth-Token: UUID"
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
$ curl -i -X GET api.apility.net/badip/8.8.8.8 -H "X-Auth-Token: UUID"
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
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET https://api.apility.net/baddomain/gmail.com
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

To use Advanced Model, the developer needs to add to the header of the requests the __application/json__ response __Accept__:

Developers will need to parse and analyze the JSON object returned in their applications.

## JSONP support

> ___Example: Is email marketing@apility.io in any blacklist?___

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/bademail/marketing@apility.io?callback=myfunction"
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
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/baddomain/mailinator.com?callback=myfunction"
```

> The response is:

```shell
myfunction(
{"blacklists": ["IVOLO-DED", "DEA"]}
);
```

> Mailinator is a very well known Disposible Email Address provider, and the response returns as a JSON payload inside a function call as JSONP needs.

JSONP (JSON with Padding or JSON-P) is a technique used by web developers to overcome the cross-domain restrictions imposed by browsers' same-origin policy that limits access to resources retrieved from origins other than the one the page was served by.  [Read more details in the wikipedia](https://en.wikipedia.org/wiki/JSONP)

The developer does not  need to add to the header of the requests the __application/json__ response __Accept__, it will be added implicit when the parameter __callback__ is passed:

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
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/badip/<IP>"
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

`GET https://api.apility.net/badip/<IP>`

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
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/badip/<IP>"
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

`GET https://api.apility.net/badip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | Yes | application/json

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

## Get all abusers' blacklist a set of IP belong to (Bulk request)

```shell
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/badip_batch/<IP1>,<IP2>...<IPn>"
```

>The response can be:

```shell
{
    "response": [
        {
            "ip": "8.8.8.8",
            "blacklists": []
        },
        {
            "ip": "212.231.122.12",
            "blacklists": []
        },
        {
            "ip": "1.2.3.4",
            "blacklists": ["STOPFORUMSPAM-180", "STOPFORUMSPAM-30", "STOPFORUMSPAM-7", "STOPFORUMSPAM-365", "STOPFORUMSPAM-90"]
        }
    ]
}
```

A developer can save time and rate-limit restrictions if she passes a list of comma separated IP in the QueryString. She will get the blacklists for each IP in the response. If the IP is not well formed it will return nothing for that IP, but will perform the lookup for the rest of the valid IP:

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/badip_batch/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | No | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP1,IP2,...IPn | The comma separated list of IP to look up in the system.

### Response

The status code of the response can be a 200 if everything is ok, but it will also return the following JSON structure:

Parameter | Description
--------- | -----------
response | Array with a list with the blacklists for each IP.


<aside class="warning">
The maximum number of IP per bulk request is 1000. The service will return a 400 error code (Bad Request) for arrays larger than 1000.
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
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/baddomain/google.com"
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
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/baddomain/mailinator.com"
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

`GET https://api.apility.net/baddomain/<DOMAIN>`

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
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/baddomain/google.com"
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
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/baddomain/mailinator.com"
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

`GET https://api.apility.net/baddomain/<DOMAIN>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | Yes | application/json

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

## Get all the scores of a set of domains (Bulk request)

> Get all information from a set of domain

```shell
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/baddomain_batch/google.com,moocher.io,apility.io,mailinator.com"
```

>The response can be:

```shell
{
    "response": [
        {
            "domain": "apility.io",
            "scoring": {
                "score": 0,
                "source_ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "127.0.0.1"
                },
                "domain": {
                    "score": 0,
                    "blacklist_ns": [],
                    "ns": [
                        "alex.ns.cloudflare.com.",
                        "pam.ns.cloudflare.com."
                    ],
                    "mx": [
                        "aspmx.l.google.com",
                        "aspmx2.googlemail.com",
                        "aspmx3.googlemail.com",
                        "alt1.aspmx.l.google.com",
                        "alt2.aspmx.l.google.com"
                    ],
                    "blacklist_mx": [],
                    "blacklist": []
                },
                "ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "104.31.76.225"
                }
            }
        },
        {
            "domain": "moocher.io",
            "scoring": {
                "score": 0,
                "source_ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "127.0.0.1"
                },
                "domain": {
                    "score": 0,
                    "blacklist_ns": [],
                    "ns": [
                        "alex.ns.cloudflare.com.",
                        "pam.ns.cloudflare.com."
                    ],
                    "mx": [
                        "aspmx.l.google.com",
                        "alt2.aspmx.l.google.com",
                        "alt4.aspmx.l.google.com",
                        "alt1.aspmx.l.google.com",
                        "alt3.aspmx.l.google.com"
                    ],
                    "blacklist_mx": [],
                    "blacklist": []
                },
                "ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "104.18.45.187"
                }
            }
        },
        {
            "domain": "gmail.com",
            "scoring": {
                "score": -1,
                "source_ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "127.0.0.1"
                },
                "domain": {
                    "score": -1,
                    "blacklist_ns": [],
                    "ns": [
                        "ns4.google.com.",
                        "ns1.google.com.",
                        "ns2.google.com.",
                        "ns3.google.com."
                    ],
                    "mx": [
                        "alt3.gmail-smtp-in.l.google.com",
                        "alt4.gmail-smtp-in.l.google.com",
                        "alt2.gmail-smtp-in.l.google.com",
                        "gmail-smtp-in.l.google.com",
                        "alt1.gmail-smtp-in.l.google.com"
                    ],
                    "blacklist_mx": [],
                    "blacklist": [
                        "FREEMAIL"
                    ]
                },
                "ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "216.58.214.165"
                }
            }
        },
        {
            "domain": "mailinator.com",
            "scoring": {
                "score": -1,
                "source_ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "127.0.0.1"
                },
                "domain": {
                    "score": -1,
                    "blacklist_ns": [],
                    "ns": [
                        "betty.ns.cloudflare.com.",
                        "james.ns.cloudflare.com."
                    ],
                    "mx": [
                        "mail2.mailinator.com",
                        "mail.mailinator.com"
                    ],
                    "blacklist_mx": [],
                    "blacklist": [
                        "MARTENSON-DED",
                        "DEA",
                        "IVOLO-DED"
                    ]
                },
                "ip": {
                    "score": 0,
                    "blacklist": [],
                    "is_quarantined": false,
                    "address": "104.25.198.31"
                }
            }
        }
    ]
}
```

A developer can save time and rate-limit restrictions if she passes a list of comma separated domains in the QueryString. She will get the scoring and full JSON structure for each domain in the response. If the domain is not well formed it will return nothing for that domain, but will perform the lookup for the rest of the valid domains:

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/baddomain_batch/<DOMAIN1>,<DOMAIN2>,...,<DOMAINn>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | No | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
DOMAIN1,DOMAIN2,...,DOMAINn    | The list of Domain names to lookup in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
response  | JSON structure containing a list of JSON structures with the domain name and a '[Domain](#domain)' object.


<aside class="warning">
The maximum number of domains per bulk request is 250. The service will return a 400 error code (Bad Request) for arrays larger than 250.
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
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/bademail/support@apility.io"
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
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/bademail/test@mailinator.com"
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

`GET https://api.apility.net/bademail/<EMAIL>`

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
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/bademail/ceo@apility.io"
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
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/bademail/test@mailinator.com"
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

`GET https://api.apility.net/bademail/<EMAIL>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | Yes | application/json

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


## Get all the scores of a set of emails (Bulk requests)

> Get all information from a set of emails

```shell
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/bademail_batch/test@moocher.io,test@apility.io,test@mailinator.com"
```

>The response can be:

```shell
{
    "response": [
        {
            "email": "test@moocher.io",
            "scoring": {
                "score": 0,
                "smtp": {
                    "score": 0,
                    "exist_address": false,
                    "exist_mx": true,
                    "exist_catchall": false
                },
                "domain": {
                    "score": 0,
                    "blacklist_ns": [],
                    "ns": [
                        "alex.ns.cloudflare.com.",
                        "pam.ns.cloudflare.com."
                    ],
                    "mx": [
                        "aspmx.l.google.com",
                        "alt2.aspmx.l.google.com",
                        "alt4.aspmx.l.google.com",
                        "alt1.aspmx.l.google.com",
                        "alt3.aspmx.l.google.com"
                    ],
                    "blacklist_mx": [],
                    "blacklist": []
                },
                "freemail": {
                    "score": 0,
                    "is_freemail": false
                },
                "ip": {
                    "score": 0,
                    "address": "104.18.44.187",
                    "is_quarantined": false,
                    "blacklist": []
                },
                "source_ip": {
                    "score": 0,
                    "address": "127.0.0.1",
                    "is_quarantined": false,
                    "blacklist": []
                },
                "disposable": {
                    "score": 0,
                    "is_disposable": false
                },
                "email": {
                    "score": 0,
                    "blacklist": []
                },
                "address": {
                    "score": 0,
                    "is_role": false,
                    "is_well_formed": true
                }
            }
        },
        {
            "email": "test@apility.io",
            "scoring": {
                "score": 0,
                "smtp": {
                    "score": 0,
                    "exist_address": false,
                    "exist_mx": true,
                    "exist_catchall": false
                },
                "domain": {
                    "score": 0,
                    "blacklist_ns": [],
                    "ns": [
                        "alex.ns.cloudflare.com.",
                        "pam.ns.cloudflare.com."
                    ],
                    "mx": [
                        "aspmx.l.google.com",
                        "aspmx2.googlemail.com",
                        "aspmx3.googlemail.com",
                        "alt1.aspmx.l.google.com",
                        "alt2.aspmx.l.google.com"
                    ],
                    "blacklist_mx": [],
                    "blacklist": []
                },
                "freemail": {
                    "score": 0,
                    "is_freemail": false
                },
                "ip": {
                    "score": 0,
                    "address": "104.31.77.225",
                    "is_quarantined": false,
                    "blacklist": []
                },
                "source_ip": {
                    "score": 0,
                    "address": "127.0.0.1",
                    "is_quarantined": false,
                    "blacklist": []
                },
                "disposable": {
                    "score": 0,
                    "is_disposable": false
                },
                "email": {
                    "score": 0,
                    "blacklist": []
                },
                "address": {
                    "score": 0,
                    "is_role": false,
                    "is_well_formed": true
                }
            }
        },
        {
            "email": "test@mailinator.com",
            "scoring": {
                "score": -2,
                "smtp": {
                    "score": 1,
                    "exist_address": true,
                    "exist_mx": true,
                    "exist_catchall": false
                },
                "domain": {
                    "score": -1,
                    "blacklist_ns": [],
                    "ns": [
                        "betty.ns.cloudflare.com.",
                        "james.ns.cloudflare.com."
                    ],
                    "mx": [
                        "mail2.mailinator.com",
                        "mail.mailinator.com"
                    ],
                    "blacklist_mx": [],
                    "blacklist": [
                        "MARTENSON-DED",
                        "DEA",
                        "IVOLO-DED"
                    ]
                },
                "freemail": {
                    "score": 0,
                    "is_freemail": false
                },
                "ip": {
                    "score": 0,
                    "address": "104.25.198.31",
                    "is_quarantined": false,
                    "blacklist": []
                },
                "source_ip": {
                    "score": 0,
                    "address": "127.0.0.1",
                    "is_quarantined": false,
                    "blacklist": []
                },
                "disposable": {
                    "score": -1,
                    "is_disposable": true
                },
                "email": {
                    "score": -1,
                    "blacklist": [
                        "STOPFORUMSPAM-365",
                        "STOPFORUMSPAM-180"
                    ]
                },
                "address": {
                    "score": 0,
                    "is_role": false,
                    "is_well_formed": true
                }
            }
        }
    ]
}
```


A developer can save time and rate-limit restrictions if she passes a list of comma separated Emails in the QueryString. She will get the scoring and full JSON structure for each Email in the response. If the Email is not well formed it will return nothing for that email, but will perform the lookup for the rest of the valid emails:



<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/bademail_batch/<EMAIL1>,<EMAIL2>,...,<EMAILn>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | No | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
EMAIL1,EMAIL2,...,EMAILn     | The list of Emails to lookup in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
response  | JSON structure containing a list of JSON structures with the email address and the '[Email](#email)' object.


<aside class="warning">
The maximum number of domains per bulk request is 100. The service will return a 400 error code (Bad Request) for arrays larger than 100.
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
* Time zone
* Geoname numbers

This API request will always return a JSON structure with the information and a 200 HTTP code if the IP can be found in the database. If not, then it will return a 404 HTTP code as usual.


## Get IP geo location.

> Get the geo location data of an IP:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/geoip/9.9.9.9"
```

>The response can be:

```shell
{
    "ip": {
        "time_zone": "Europe/Paris",
        "city": "",
        "continent_geoname_id": 6255148,
        "region": "",
        "accuracy_radius": 1000,
        "latitude": 48.8582,
        "region_geoname_id": -1,
        "region_names": {},
        "postal": "",
        "longitude": 2.3387000000000002,
        "as": {
            "networks": [
                "9.9.9.0/24",
                "149.112.112.0/24",
                "149.112.149.0/24"
            ],
            "name": "QUAD9-AS-1 - Quad9",
            "asn": "19281",
            "country": "US"
        },
        "address": "9.9.9.9",
        "continent": "EU",
        "country_names": {
            "en": "France",
            "pt-BR": "França",
            "fr": "France",
            "ja": "フランス共和国",
            "de": "Frankreich",
            "zh-CN": "法国",
            "es": "Francia",
            "ru": "Франция"
        },
        "city_names": {},
        "hostname": "",
        "country_geoname_id": 3017382,
        "country": "FR",
        "continent_names": {
            "en": "Europe",
            "pt-BR": "Europa",
            "fr": "Europe",
            "ja": "ヨーロッパ",
            "de": "Europa",
            "zh-CN": "欧洲",
            "es": "Europa",
            "ru": "Европа"
        },
        "city_geoname_id": -1
    }
}
```

This endpoint returns a JSON structure with individual details about the geographical location of the IP, and information about the Autonomous System and its networks. The request always returns the status code 200 (HTTP OK) if the IP exists. If the IP is malformed it will return a 400 (HTTP Bad Request) code.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/geoip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | Yes | application/json

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

## Get geo location of a set of IP (Bulk Request).

> Get the geo location data of a set of IP:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/geoip_batch212.231.122.12,8.8.8.8,9.9.9.9"
```

>The response can be:

```shell
{
    "response": [
        {
            "ip": "8.8.8.8",
            "geoip": {
                "time_zone": "",
                "city": "",
                "continent_geoname_id": 6255149,
                "region": "",
                "accuracy_radius": 1000,
                "latitude": 37.751,
                "region_geoname_id": -1,
                "region_names": {},
                "postal": "",
                "longitude": -97.822,
                "as": {
                    "networks": [
                        "8.8.4.0/24",
                        "8.8.8.0/24",
                        ...
                        "216.252.222.0/24"
                    ],
                    "ip": "8.8.8.8",
                    "name": "GOOGLE - Google LLC",
                    "asn": "15169",
                    "country": "US"
                },
                "address": "8.8.8.8",
                "continent": "NA",
                "country_names": {
                    "en": "United States",
                    "pt-BR": "Estados Unidos",
                    "fr": "États-Unis",
                    "ja": "アメリカ合衆国",
                    "de": "USA",
                    "zh-CN": "美国",
                    "es": "Estados Unidos",
                    "ru": "США"
                },
                "city_names": {},
                "hostname": "",
                "country_geoname_id": 6252001,
                "country": "US",
                "continent_names": {
                    "en": "North America",
                    "pt-BR": "América do Norte",
                    "fr": "Amérique du Nord",
                    "ja": "北アメリカ",
                    "de": "Nordamerika",
                    "zh-CN": "北美洲",
                    "es": "Norteamérica",
                    "ru": "Северная Америка"
                },
                "city_geoname_id": -1
            }
        },
        {
            "ip": "212.231.122.12",
            "geoip": {
                "time_zone": "",
                "city": "",
                "continent_geoname_id": 6255148,
                "region": "",
                "accuracy_radius": 500,
                "latitude": 40.4172,
                "region_geoname_id": -1,
                "region_names": {},
                "postal": "",
                "longitude": -3.684,
                "as": {
                    "networks": [
                        "31.222.80.0/20",
                        "46.6.0.0/18",
                        ...
                        "213.195.96.0/19"
                    ],
                    "ip": "212.231.122.12",
                    "name": "AS15704",
                    "asn": "15704",
                    "country": "ES"
                },
                "address": "212.231.122.12",
                "continent": "EU",
                "country_names": {
                    "en": "Spain",
                    "pt-BR": "Espanha",
                    "fr": "Espagne",
                    "ja": "スペイン",
                    "de": "Spanien",
                    "zh-CN": "西班牙",
                    "es": "España",
                    "ru": "Испания"
                },
                "city_names": {},
                "hostname": "",
                "country_geoname_id": 2510769,
                "country": "ES",
                "continent_names": {
                    "en": "Europe",
                    "pt-BR": "Europa",
                    "fr": "Europe",
                    "ja": "ヨーロッパ",
                    "de": "Europa",
                    "zh-CN": "欧洲",
                    "es": "Europa",
                    "ru": "Европа"
                },
                "city_geoname_id": -1
            }
        },
        {
            "ip": "9.9.9.9",
            "geoip": {
                "time_zone": "Europe/Paris",
                "city": "",
                "continent_geoname_id": 6255148,
                "region": "",
                "accuracy_radius": 1000,
                "latitude": 48.8582,
                "region_geoname_id": -1,
                "region_names": {},
                "postal": "",
                "longitude": 2.3387000000000002,
                "as": {
                    "networks": [
                        "9.9.9.0/24",
                        "149.112.112.0/24",
                        "149.112.149.0/24"
                    ],
                    "ip": "9.9.9.9",
                    "name": "QUAD9-AS-1 - Quad9",
                    "asn": "19281",
                    "country": "US"
                },
                "address": "9.9.9.9",
                "continent": "EU",
                "country_names": {
                    "en": "France",
                    "pt-BR": "França",
                    "fr": "France",
                    "ja": "フランス共和国",
                    "de": "Frankreich",
                    "zh-CN": "法国",
                    "es": "Francia",
                    "ru": "Франция"
                },
                "city_names": {},
                "hostname": "",
                "country_geoname_id": 3017382,
                "country": "FR",
                "continent_names": {
                    "en": "Europe",
                    "pt-BR": "Europa",
                    "fr": "Europe",
                    "ja": "ヨーロッパ",
                    "de": "Europa",
                    "zh-CN": "欧洲",
                    "es": "Europa",
                    "ru": "Европа"
                },
                "city_geoname_id": -1
            }
        }
    ]
}
```

A developer can save time and rate-limit restrictions if she passes a list of comma separated IP in the QueryString. She will get the information inside a JSON structure for each IP in the response. If the IP is not well formed it will return nothing for that IP, but will perform the lookup for the rest of the valid IP addresses:



<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/geoip_batch/<IP1>,<IP2>,...,<IPn>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | No | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP1,IP2,...,IPn        | The list  of IP to geo locate in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
response  | JSON structure containing a JSON structure with the IP to geo locate and its '[geoip](#geoip)' object.


<aside class="warning">
The maximum number of IP to geo locate per bulk request is 1000. The service will return a 400 error code (Bad Request) for arrays larger than 1000.
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
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/as/ip/9.9.9.9"
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

`GET https://api.apility.net/as/ip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | Yes | application/json

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

## Get the AS information from a set of IP (Bulk Request).

> Get the AS data of a set of IP addresses:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/as_batch/ip/212.231.122.12,8.8.8.8,9.9.9.9"
```

>The response can be:

```shell
{
    "response": [
        {
            "as": {
                "country": "ES",
                "asn": "15704",
                "networks": [
                    "31.222.80.0/20",
                    "46.6.0.0/18",
                    ...
                    "213.195.96.0/19"
                ],
                "name": "AS15704"
            },
            "ip": "212.231.122.12"
        },
        {
            "as": {
                "country": "US",
                "asn": "15169",
                "networks": [
                    "8.8.4.0/24",
                    "8.8.8.0/24",
                    ...
                    "216.252.222.0/24"
                ],
                "name": "GOOGLE - Google LLC"
            },
            "ip": "8.8.8.8"
        },
        {
            "as": {
                "networks": [
                    "9.9.9.0/24",
                    "149.112.112.0/24",
                    "149.112.149.0/24"
                ],
                "asn": "19281",
                "country": "US",
                "name": "QUAD9-AS-1 - Quad9"
            },
            "ip": "9.9.9.9"
        }
    ]
}
```

A developer can save time and rate-limit restrictions if she passes a list of comma separated IP in the QueryString. She will get the information inside a JSON structure for each IP in the response. If the IP is not well formed it will return nothing for that IP, but will perform the lookup for the rest of the valid IP addresses:


<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/as_batch/ip/<IP1>,<IP2>,...,<IPn>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | No | application/json

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP1,IP2,...,IPn        | The list of IP addresses to lookup the AS in the system.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return the following JSON:

Parameter | Description
--------- | -----------
response  | JSON structure containing a JSON structure with the IP adddress and its ''[as](#as)'' object.


<aside class="warning">
The maximum number of IP addresses to resolve per bulk request is 1000. The service will return a 400 error code (Bad Request) for arrays larger than 1000.
</aside>

## Get the AS information from its number.

> Get the AS data from its number:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/as/num/19281"
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

`GET https://api.apility.net/as/num/<NUMBER>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | Yes | application/json

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

## Get the AS information from a set of AS numbers (Bulk Request).

> Get the AS data from a set of AS numbers:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/as_batch/num/15704,3352,15169"
```

>The response can be:

```shell
{
    "response": [
        {
            "asn": "15704",
            "as": {
                "country": "ES",
                "asn": "15704",
                "networks": [
                    "31.222.80.0/20",
                    "46.6.0.0/18",
                    ...
                    "213.195.96.0/19"
                ],
                "name": "AS15704"
            }
        },
        {
            "asn": "3352",
            "as": {
                "country": "ES",
                "asn": "3352",
                "networks": [
                    "2.136.0.0/16",
                    "2.137.0.0/16",
                    ...
                    "217.127.0.0/16"
                ],
                "name": "TELEFONICA_DE_ESPANA"
            }
        },
        {
            "asn": "15169",
            "as": {
                "country": "US",
                "asn": "15169",
                "networks": [
                    "8.8.4.0/24",
                    "8.8.8.0/24",
                    ...
                    "216.252.222.0/24"
                ],
                "name": "GOOGLE - Google LLC"
            }
        }
    ]
}
```

A developer can save time and rate-limit restrictions if she passes a list of comma separated AS numbers in the QueryString. She will get the information inside a JSON structure for each AS number in the response. If the AS Number is not a correct number or the AS does not exist it will return nothing for it, but will perform the lookup for the rest of the valid AS numbers:


<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/as_batch/num/<NUM1>,<NUM2>,...,<NUMn>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.
Accept | No | application/json

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
response  | JSON structure containing a list of JSON structures with the AS number and its '[as](#as)' object.


<aside class="warning">
The maximum number of AS numbers to resolve per bulk request is 1000. The service will return a 400 error code (Bad Request) for arrays larger than 1000.
</aside>


# Quarantined Objects

[Apility.io](https://apility.io) adds every few weeks new lists from multiple sources with the intention of helping our users to keep away those who want to abuse the services of our clients.
However, it may be the case that our customers want to create their own blacklists based on individual parameters or business logic. This is why a new capability has been implemented to create private exclusion lists based on user IP properties. The properties we can control are the following:

* IP address [QUARANTINE-IP](https://apility.io/list?id=QUARANTINE-IP&type=badip)
* Country [QUARANTINE-COUNTRY](https://apility.io/list?id=QUARANTINE-COUNTRY&type=badip)
* Continent [QUARANTINE-CONTINENT](https://apility.io/list?id=QUARANTINE-CONTINENT&type=badip)
* Autonomous System [QUARANTINE-AS](https://apility.io/list?id=QUARANTINE-AS&type=badip)


For each type of object it's possible to perform four different actions:

* add the object to the blacklist,
* remove it,
* check if the object is in the list or
* get all the elements in the list.

As part of the attributes of the object, the Time To Live of the object in the black list is the most important. The TTL or Time to Live is the number of seconds the object will be in the black list before expiring and disappearing. Hence, it's possible to temporaly ban an IP address or set of IP addreses based on some attributes: for example ban the IP address coming from a toxic Autonomous System. Due to this time-based ban this kind of blacklists are referred as QUARANTINED objects.

Finally, to check if the IP belongs to any of these lists it's as simple as using the [IP Check](#ip-check) services of the API. If the IP matches some of the attributes, then the QUARANTINED blacklist will be shown just like the rest of the public blacklist.


## Add an IP to the private QUARANTINE-IP backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X POST -d'{"ip":<IP>,"ttl":<TTL>}' "https://api.apility.net/quarantine/ip"
```

>If the operation can be performed then the result is:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:01:31 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

200: OK
```


This endpoint add the IP address passed as argument in the body to the QUARANTINE-IP private blacklist. The TTL must be passed too and it permits two options:

- If TTL is 0, then the object will never expire and can only be removed using the remove request of the API or from the user dashboard.
- If TTL is greater than 0 then the object will expire after the number of seconds in TTL.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`POST https://api.apility.net/quarantine/ip`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.

### JSON body

The body must have a valid JSON object composed of two parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
ip | Yes | IP address to add to QUARANTINE-IP blacklist.
ttl | Yes | Time to Live in seconds of the IP in the blacklist. Zero for never expiring.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the IP has been added succesfully to the QUARANTINE-IP blacklist. Any other error message means that the IP could not be added:

* __HTTP/1.1 400 Bad Request__: The JSON object, IP address and/or the TTL are malformed, the IP address cannot be stored. Check the response text for more information about the error.


## Add the Country of an IP address to the private QUARANTINE-COUNTRY backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X POST -d'{"country":<COUNTRY_CODE>,"ttl":<TTL>}' "https://api.apility.net/quarantine/country"
```

>If the operation can be performed then the result is:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:01:31 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

200: OK
```


This endpoint add the country passed as argument in the body to the QUARANTINE-COUNTRY private blacklist. The TTL must be passed too and it permits two options:

- If TTL is 0, then the object will never expire and can only be removed using the remove request of the API or from the user dashboard.
- If TTL is greater than 0 then the object will expire after the number of seconds in TTL.

The service will automatically obtain the country of the IP using the Geo location service. If the country is in the QUARANTINE-COUNTRY blacklist, it will be reported in the [IP Check](#ip-check) services.


<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`POST https://api.apility.net/quarantine/country`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.

### JSON body

The body must have a valid JSON object composed of two parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
country | Yes | Country ISO 3166-1 alfa-2 code to add to the QUARANTINE-COUNTRY blacklist.
ttl | Yes | Time to Live in seconds of the Country in the blacklist. Zero for never expiring.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the Country has been added succesfully to the QUARANTINE-Country blacklist. Any other error message means that the Country could not be added:

* __HTTP/1.1 400 Bad Request__: The JSON object, Country code and/or the TTL are malformed, the Country code cannot be stored. Check the response text for more information about the error.


## Add the Continent of an IP address to the private QUARANTINE-CONTINENT backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X POST -d'{"continent":<CONTINENT_CODE>,"ttl":<TTL>}' "https://api.apility.net/quarantine/continent"
```

>If the operation can be performed then the result is:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:01:31 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

200: OK
```


This endpoint add the continent passed as argument in the body to the QUARANTINE-CONTINENT private blacklist. The TTL must be passed too and it permits two options:

- If TTL is 0, then the object will never expire and can only be removed using the remove request of the API or from the user dashboard.
- If TTL is greater than 0 then the object will expire after the number of seconds in TTL.

The service will automatically obtain the continent of the IP using the Geo location service. If the continent is in the QUARANTINE-CONTINENT blacklist, it will be reported in the [IP Check](#ip-check) services.


<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`POST https://api.apility.net/quarantine/continent`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.

### JSON body

The body must have a valid JSON object composed of two parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
continent | Yes | [Continent codes](https://datahub.io/core/continent-codes) to add to the QUARANTINE-CONTINENT blacklist.
ttl | Yes | Time to Live in seconds of the Continent in the blacklist. Zero for never expiring.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the Continent has been added succesfully to the QUARANTINE-CONTINENT blacklist. Any other error message means that the Continent could not be added:

* __HTTP/1.1 400 Bad Request__: The JSON object, Continent code and/or the TTL are malformed, the Continent code cannot be stored. Check the response text for more information about the error.


## Add the Autonomous System (AS) that owns the IP address to the private QUARANTINE-AS backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X POST -d'{"asn":<AS_NUMBER>,"ttl":<TTL>}' "https://api.apility.net/quarantine/as"
```

>If the operation can be performed then the result is:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:01:31 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

200: OK
```


This endpoint adds the Autonomous System Number passed as argument in the body to the QUARANTINE-AS private blacklist. The TTL must be passed too and it permits two options:

- If TTL is 0, then the object will never expire and can only be removed using the remove request of the API or from the user dashboard.
- If TTL is greater than 0 then the object will expire after the number of seconds in TTL.

The service will automatically obtain the AS number of the IP using the AS resolution service. If the AS is in the QUARANTINE-AS blacklist, it will be reported in the [IP Check](#ip-check) services.


<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`POST https://api.apility.net/quarantine/as`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.

### JSON body

The body must have a valid JSON object composed of two parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
asn | Yes | Autonomous System Number (ASN) to add to the QUARANTINE-AS blacklist.
ttl | Yes | Time to Live in seconds of the AS in the blacklist. Zero for never expiring.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the AS has been added succesfully to the QUARANTINE-AS blacklist. Any other error message means that the AS could not be added:

* __HTTP/1.1 400 Bad Request__: The JSON object, AS number and/or the TTL are malformed, the AS cannot be stored. Check the response text for more information about the error.


## Check if the IP is in the private QUARANTINE-IP backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/ip/<IP>"
```

>If the IP is in the list:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:19:39 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

>If the IP is NOT the list:

```shell
HTTP/2 404
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:20:26 GMT
content-type: text/plain; charset=utf-8
content-length: 14

```

This endpoint check if the IP address passed as argument in the query string is in the QUARANTINE-IP private blacklist. If exists, it returns a HTTP 200 OK, and if not, a HTTP 404 Not Found error. If you want to perform a full test on the IP address including public blacklists you should better use the [IP Check](#ip-check) API services.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/ip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
IP | Yes | IP address to check if it is in the QUARANTINE-IP


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the IP is in the QUARANTINE-IP blacklist. A HTTP/1.1 404 Not Found means that the IP address is not in the QUARANTINE-IP blacklist. Any other error message means that the IP could not be parsed:

* __HTTP/1.1 400 Bad Request__: The IP address is malformed. Check the response text for more information about the error.


## Check if the Country is in the private QUARANTINE-COUNTRY backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/country/<COUNTRY_CODE>"
```

>If the Country is in the list:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:19:39 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

>If the Country is NOT the list:

```shell
HTTP/2 404
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:20:26 GMT
content-type: text/plain; charset=utf-8
content-length: 14

```

This endpoint checks if the Country code passed as argument in the query string is in the QUARANTINE-COUNTRY private blacklist. If exists, it returns a HTTP 200 OK, and if not, a HTTP 404 Not Found error. If you want to perform a full test on the IP address including public blacklists you should better use the [IP Check](#ip-check) API services.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/country/<COUNTRY_CODE>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
COUNTRY_CODE | Yes | Country ISO 3166-1 alfa-2 code to check if it is in the QUARANTINE-COUNTRY list.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the Country code is in the QUARANTINE-COUNTRY blacklist. A HTTP/1.1 404 Not Found means that the Conutry code is not in the QUARANTINE-COUNTRY blacklist. Any other error message means that the Country Code could not be parsed:

* __HTTP/1.1 400 Bad Request__: The Country code is malformed. Check the response text for more information about the error.


## Check if the Continent is in the private QUARANTINE-CONTINENT backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/continent/<CONTINENT_CODE>"
```

>If the Continent is in the list:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:19:39 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

>If the Continent is NOT in the list:

```shell
HTTP/2 404
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:20:26 GMT
content-type: text/plain; charset=utf-8
content-length: 14

```

This endpoint checks if the Continent code passed as argument in the query string is in the QUARANTINE-CONTINENT private blacklist. If exists, it returns a HTTP 200 OK, and if not, a HTTP 404 Not Found error. If you want to perform a full test on the IP address including public blacklists you should better use the [IP Check](#ip-check) API services.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/continent/<CONTINENT_CODE>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
CONTINENT_CODE | Yes | [Continent codes](https://datahub.io/core/continent-codes) to check if it is in the QUARANTINE-CONTINENT list.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the Continent code is in the QUARANTINE-CONTINENT blacklist. A HTTP/1.1 404 Not Found means that the Continent code is not in the QUARANTINE-CONTINENT blacklist. Any other error message means that the Continent Code could not be parsed:

* __HTTP/1.1 400 Bad Request__: The Continent code is malformed. Check the response text for more information about the error.


## Check if the AS is in the private QUARANTINE-AS backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/as/<AS_NUM>"
```

>If the AS is in the list:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:19:39 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

>If the AS is NOT in the list:

```shell
HTTP/2 404
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:20:26 GMT
content-type: text/plain; charset=utf-8
content-length: 14

```

This endpoint checks if the AS Number passed as argument in the query string is in the QUARANTINE-AS private blacklist. If exists, it returns a HTTP 200 OK, and if not, a HTTP 404 Not Found error. If you want to perform a full test on the IP address including public blacklists you should better use the [IP Check](#ip-check) API services.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/as/<AS_NUM>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
AS_NUM | Yes | Autonmous System Number (ASN) to check if it is in the QUARANTINE-AS list.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the AS is in the QUARANTINE-AS blacklist. A HTTP/1.1 404 Not Found means that the AS is not in the QUARANTINE-AS blacklist. Any other error message means that the AS number could not be parsed:

* __HTTP/1.1 400 Bad Request__: The AS number (ASN) is malformed. Check the response text for more information about the error.


## Get full list of IP addresses in the private QUARANTINE-IP backlist

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/ip"
```

>The response will always return a JSON object, even when there is no elements in the list:

```shell
{
   "quarantined":[
      {
         "ip":"8.8.4.4",
         "ttl":7185
      },
      {
         "ip":"9.9.9.9",
         "ttl":1717
      },
      {
         "ip":"8.8.8.8",
         "ttl":3571
      }
   ]
}
```

This endpoint returns the full list of the IP addresses and the corresponding TTL in the QUARANTINE-IP private blacklist as a JSON object.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/ip`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error should be considered a platform problem and you can report it to our suppor team.

The JSON response is composed of:

Parameter     | Description
------------- | -----------
quarantined | List containing the pair of IP addresses and TTL.


## Get full list of Country codes in the private QUARANTINE-COUNTRY backlist

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/country"
```

>The response will always return a JSON object, even when there is no elements in the list:

```shell
{
   "quarantined":[
      {
         "country":"ES",
         "ttl":7182
      },
      {
         "country":"EE",
         "ttl":7171
      },
      {
         "country":"US",
         "ttl":7195
      }
   ]
}

```

This endpoint returns the full list of the Country codes and the corresponding TTL in the QUARANTINE-COUNTRY private blacklist as a JSON object.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/country`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error should be considered a platform problem and you can report it to our suppor team.

The JSON response is composed of:

Parameter     | Description
------------- | -----------
quarantined | List containing the pair of Country codes and TTL.


## Get full list of Continent codes in the private QUARANTINE-CONTINENT backlist

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/continent"
```

>The response will always return a JSON object, even when there is no elements in the list:

```shell
{
   "quarantined":[
      {
         "continent":"EU",
         "ttl":7179
      },
      {
         "continent":"AS",
         "ttl":3592
      }
   ]
}

```

This endpoint returns the full list of the Continent codes and the corresponding TTL in the QUARANTINE-CONTINENT private blacklist as a JSON object.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/continent`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error should be considered a platform problem and you can report it to our suppor team.

The JSON response is composed of:

Parameter     | Description
------------- | -----------
quarantined | List containing the pair of [Continent codes](https://datahub.io/core/continent-codes) and TTL.


## Get full list of AS in the private QUARANTINE-AS backlist

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/quarantine/as"
```

>The response will always return a JSON object, even when there is no elements in the list:

```shell
{
   "quarantined":[
      {
         "asn":"3352",
         "ttl":3559
      },
      {
         "asn":"15169",
         "ttl":7191
      }
   ]
}
```

This endpoint returns the full list of the AS Numbers and the corresponding TTL in the QUARANTINE-AS private blacklist as a JSON object.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/quarantine/as`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error should be considered a platform problem and you can report it to our suppor team.

The JSON response is composed of:

Parameter     | Description
------------- | -----------
quarantined | List containing the pair of AS Numbers and TTL.




## Delete an IP address if it is in the private QUARANTINE-IP backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X DELETE "https://api.apility.net/quarantine/ip/<IP>"
```

>The response will always be HTTP 200 OK no matter if the IP to delete exists or not:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:45:14 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

This endpoint delete the IP address passed as argument in the query string if it exists in the QUARANTINE-IP private blacklist. This API call will delete the IP address no matter if it will expire or not.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`DELETE https://api.apility.net/quarantine/ip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
IP | Yes | IP address to remove from the QUARANTINE-IP blacklist


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the operation was performed succesufully. Any other error should be considered a platform problem and you can report it to our suppor team.

* __HTTP/1.1 400 Bad Request__: The IP address is malformed. Check the response text for more information about the error.


## Delete a Country Code if it is in the private QUARANTINE-COUNTRY backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X DELETE "https://api.apility.net/quarantine/country/<COUNTRY_CODE>"
```

>The response will always be HTTP 200 OK no matter if the Country code to delete exists or not:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:45:14 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

This endpoint delete the Country code passed as argument in the query string if it exists in the QUARANTINE-COUNTRY private blacklist. This API call will delete the Country code no matter if it will expire or not.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`DELETE https://api.apility.net/quarantine/country/<COUNTRY_CODE>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
COUNTRY_CODE | Yes | Country Code to remove from the QUARANTINE-COUNTRY blacklist


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the operation was performed succesufully. Any other error should be considered a platform problem and you can report it to our suppor team.

* __HTTP/1.1 400 Bad Request__: The Country code is malformed. Check the response text for more information about the error.



## Delete a Continent Code if it is in the private QUARANTINE-CONTINENT backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X DELETE "https://api.apility.net/quarantine/continent/<CONTINENT_CODE>"
```

>The response will always be HTTP 200 OK no matter if the Continent code to delete exists or not:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:45:14 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

This endpoint delete the Continent code passed as argument in the query string if it exists in the QUARANTINE-CONTINENT private blacklist. This API call will delete the Continent code no matter if it will expire or not.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`DELETE https://api.apility.net/quarantine/continent/<CONTINENT_CODE>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
CONTINENT_CODE | Yes | [Continent code](https://datahub.io/core/continent-codes) to remove from the QUARANTINE-CONTINENT blacklist


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the operation was performed succesufully. Any other error should be considered a platform problem and you can report it to our suppor team.

* __HTTP/1.1 400 Bad Request__: The Continent code is malformed. Check the response text for more information about the error.


## Delete an AS if it is in the private QUARANTINE-AS backlist

```shell
$ curl -i -H "X-Auth-Token: UUID" -X DELETE "https://api.apility.net/quarantine/AS/<AS_NUM>"
```

>The response will always be HTTP 200 OK no matter if the AS to delete exists or not:

```shell
HTTP/2 200
server: nginx/1.10.3 (Ubuntu)
date: Mon, 29 Jan 2018 17:45:14 GMT
content-type: text/plain; charset=utf-8
content-length: 7
strict-transport-security: max-age=63072000; includeSubdomains
x-frame-options: DENY
x-content-type-options: nosniff

```

This endpoint delete the AS passed as argument in the query string if it exists in the QUARANTINE-AS private blacklist. This API call will delete the AS no matter if it will expire or not.

<aside class="success">
This is a paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`DELETE https://api.apility.net/quarantine/as/<AS_NUM>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
callback | Yes | Function to invoke when using JSONP model.
AS_NUM | Yes | AS Number to remove from the QUARANTINE-AS blacklist


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok and means that the operation was performed succesufully. Any other error should be considered a platform problem and you can report it to our suppor team.

* __HTTP/1.1 400 Bad Request__: The AS Number is malformed. Check the response text for more information about the error.




















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
continent | 2 letter code of the continent.
country | ISO 3166-1 Country code.
region | Name of the region, by default the english translation in 'region_names'.
city | Name of the city, by default the english translation in 'city_names'.
postal | Postal code or Zip code
time_zone | Time zone of the location
accuracy_radius | The approximate radius in kilometers around the latitude and longitude for the geographical entity. -1 if unknown.
continent_geoname_id | Id of the continent in the [geonames.org](http://www.geonames.org) database. -1 if the continent cannot be geolocated.
country_geoname_id | Id of the country in the [geonames.org](http://www.geonames.org) database. -1 if the country cannot be geolocated.
region_geoname_id | Id of the region in the [geonames.org](http://www.geonames.org) database. -1 if the region cannot be geolocated.
city_geoname_id | Id of the city in the [geonames.org](http://www.geonames.org) database. -1 if the city cannot be geolocated.
continent_names | JSON structure containing the different names of the continent in different languages. Languages are in ISO 639-1. Empty if continent cannot be geolocated.
country_names | JSON structure containing the different names of the country in different languages. Languages are in ISO 639-1. Empty if country cannot be geolocated.
region_names | JSON structure containing the different names of the region in different languages. Languages are in ISO 639-1. Empty if region cannot be geolocated.
city_names | JSON structure containing the different names of the city in different languages. Languages are in ISO 639-1. Empty if city cannot be geolocated.
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
