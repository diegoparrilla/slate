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

* __IP__: IP address that has been used in any abusing activities like spam, attacks, hacking activities and others.
* __Domains__: Domain names used as email registration, source of attacks and others.
* __Emails__: Emails used in spam and fraudulent activities.
* __IP Geolocation__: For Geolocation activities, access to a Geolocation Rest API based in MaxMind GeoIP Lite database is also available.
* __Autonomous Systems__: Get the Autonomous System and network by the IP or the number.
* __Resource History__:  Historical activity of these resources. Complete access to the history of changes.
* __Whois data__: Database query of RFC 3912 protocol.

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

[To use the API you will need a API Key or Token](https://apility.io/docs/step-2-api-key/) that you will pass along your request. [You need to sign up to get it](https://dashboard.apility.io/#/register).

API is restricted by this API Key. The access is [limited to a number of API requests per day](https://apility.io/docs/difference-hits-requests/). If you exceed that limit in a 24 hour period the service will return a 429 HTTP status code. If you need to make more requests you should consider contact us and tell us why you need a higher limit. You can know in real time the quota consumed daily from the Dashboard.

There is also an ANONYMOUS PLAN. The platform applies this plan if a developer does not pass any API Token when doing a request. So you can test the API right way before signing up!

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

## Consuming the API with libraries and tools.

There are several tools and libraries created to consume the API directly with extensions to Google Spreadsheet, Python libraries or command line tools. You can learn more about all these options in the [Tools and Services section](https://apility.io/docs/category/tools-services/) of our documentation.

# IP Check

Apility.io tracks multiple abuse blacklists and consolidates them in a single database you can look up with our minimalist API.

<aside class="warning">
Each IP Check request consumes <a href="https://apility.io/docs/difference-hits-requests/">1 hit</a>.
</aside>

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

* __HTTP/1.1 200 OK__ : 200 (OK) means that the IP address is in any blacklist and then it is a __bad__ IP
* __HTTP/1.1 404 Not Found__: and 404 (OK) means that the IP address is a clean IP.

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


# Full IP address reputation

From API version 2.0 a new endpoint has been added that allows to obtain a score of an IP address not only studying if it is in a black list. Also the following checks are made:

* A reverse DNS lookup is performed and a hostname is obtained. The service checks if the domain is in any blacklist.
* Check if the IP address was in any blacklist in the past. The service checks the historical information of the IP address.

These three checks -IP address blacklist, Domain blacklist and IP address historical blacklist- are summarized and returned as a global score for the IP address.

This API call also returns detailed information about the IP address from different sources:

* IP Geolocation
* AS information
* WHOIS informaton
* Reverse DNS lookup to obtain the Hostname
* Blacklists where the IP address was found (if any).
* Blacklists where the Hostname was found (if any).
* IP address historical activity: what blacklists did the IP address was found in?

<aside class="success">
This is a free and paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

<aside class="warning">
Each Full IP address reputation request consumes <a href="https://apility.io/docs/difference-hits-requests/">5 hits</a>. You can save some quota using this request instead of making different request to different services.
</aside>

## Get full IP address reputation info

```shell
$ curl -H "Accept: application/json" -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/ip/<IP>"
```

>The response can be:

```shell
{
    "fullip": {
        "geo": { ... },
        "hostname": "abts-tn-static-035.5.165.122.airtelbroadband.in",
        "baddomain": {
            "domain": {
                "blacklist": [],
                "blacklist_mx": [],
                "blacklist_ns": [],
                "mx": [],
                "ns": [],
                "score": 0
            },
            "ip": {
                "address": "",
                "blacklist": "",
                "is_quarantined": false,
                "score": 0
            },
            "source_ip": {
                "address": "71.152.251.222",
                "blacklist": [],
                "is_quarantined": false,
                "score": 0
            },
            "score": 0
        },
        "badip": {
            "score": 0,
            "blacklists": []
        },
        "history": {
            "score": -1,
            "activity": [
                {
                    "ip": "122.165.5.35",
                    "timestamp": 1537000113218,
                    "command": "rem",
                    "blacklists": "",
                    "blacklist_change": "UCEPROTECT-LEVEL1"
                },
                ...
            ],
            "score_1day": false,
            "score_7days": false,
            "score_30days": true,
            "score_90days": true,
            "score_180days": true,
            "score_1year": true
        },
        "score": -1,
        "whois": { ... }
    }
}
```

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/v2.0/ip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
timestamp | No | UNIX time in seconds to filter the search in the database. If ignored, then current UNIX time is taken.
page | No | Page number to paginate the result. Always start at 1. If ignored, then search for page one.
items | No | Number of items per page. Can be in the range of 5 to 200. If ignored, then return 5 items.
callback | No | Function to invoke when using JSONP model.

### URL Parameters

Parameter | Description
--------- | -----------
IP | The IP to look up in the system.

### Response

The status code of the response will be a 200 HTTP code if everything is ok and it will also return the following JSON structure.:

Parameter | Description
--------- | -----------
fullip | '[FullIP](#fullip)' JSON object cointaining all the information gathered from the different sources.



# Domain Check

Domain check implements an algorithm that assigns a score to the domain based on several checks done by the system:

* Is the domain in any of the domain blacklists?
* Is the domain MX records in any of the domain blacklists?
* Is the domain NS records in any of the domain blacklists?
* Is the IP of domain in any of the IP blacklists?

By default if some of these tests success, a -1 score is added to the overall score of the domain. So an overall score of -3 means the chances of being a bad domain are higher than -1.

<aside class="warning">
Each Domain check request consumes <a href="https://apility.io/docs/difference-hits-requests/">5 hit</a>.
</aside>

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
quarantine_ip | No | This IP address will be added to the quarantine blacklist of the user if the domain has a negative score.
quarantine_ttl | No | The TTL in seconds to store the IP address passed in 'quarantine_ip'. If not passed, default TTL is 3600.

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
quarantine_ip | No | This IP address will be added to the quarantine blacklist of the user if the domain has a negative score.
quarantine_ttl | No | The TTL in seconds to store the IP address passed in 'quarantine_ip'. If not passed, default TTL is 3600.

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

<aside class="warning">
Each Email Check request consumes <a href="https://apility.io/docs/difference-hits-requests/">10 hits</a>.
</aside>

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
alias | No | If 'True', the service will allow email aliases using the '+' sign. If 'False' then the service will not allow email aliases and will return a 400 - Bad Request error. The default value is 'False', so if no paremeter found the service will assume that email aliases are not allowed.
callback | No | Function to invoke when using JSONP model.
quarantine_ip | No | This IP address will be added to the quarantine blacklist of the user if the domain has a negative score.
quarantine_ttl | No | The TTL in seconds to store the IP address passed in 'quarantine_ip'. If not passed, default TTL is 3600.
timeout | No | Number of seconds before the process give up testing the email server. If timeout is 0, then the email server testing is fully ignored. If timeout is not passed in the query string or negative the default timeout is 20 seconds. If the timeout is greater than 60 seconds the value is ignored and the timeout is set to 60 seconds.
token | No | API Key of the owner.

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
         "exist_catchall":false,
         "graylisted":false,
         "timedout":false
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
         "exist_catchall":false,
         "graylisted":false,
         "timedout":false
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
alias | No | If 'True', the service will allow email aliases using the '+' sign. If 'False' then the service will not allow email aliases and will return a 400 - Bad Request error. The default value is 'False', so if no paremeter found the service will assume that email aliases are not allowed.
callback | No | Function to invoke when using JSONP model.
quarantine_ip | No | This IP address will be added to the quarantine blacklist of the user if the domain has a negative score.
quarantine_ttl | No | The TTL in seconds to store the IP address passed in 'quarantine_ip'. If not passed, default TTL is 3600.
timeout | No | Number of seconds before the process give up testing the email server. If timeout is 0, then the email server testing is fully ignored. If timeout is not passed in the query string or negative the default timeout is 20 seconds. If the timeout is greater than 60 seconds the value is ignored and the timeout is set to 60 seconds.
token | No | API Key of the owner.


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
                    "exist_catchall": false,
                    "graylisted":false,
                    "timedout":false
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
                    "exist_catchall": false,
                    "graylisted":false,
                    "timedout":false
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
                    "exist_catchall": false,
                    "graylisted":false,
                    "timedout":false
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
alias | No | If 'True', the service will allow email aliases using the '+' sign. If 'False' then the service will not allow email aliases and will return a 400 - Bad Request error. The default value is 'False', so if no paremeter found the service will assume that email aliases are not allowed.
callback | No | Function to invoke when using JSONP model.
timeout | No | Number of seconds before the process give up testing the email server. If timeout is 0, then the email server testing is fully ignored. If timeout is not passed in the query string or negative the default timeout is 20 seconds. If the timeout is greater than 60 seconds the value is ignored and the timeout is set to 60 seconds.
token | No | API Key of the owner.

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

<aside class="warning">
Each Geo IP look up request consumes <a href="https://apility.io/docs/difference-hits-requests/">1 hit</a>. You can save some quota using this request instead of making different request to different services.
</aside>

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

This service has two different versións. Version 1 returns the following Autonomous System information of an IP or ASN:

* ASN
* Country
* Name
* IPv4 Networks, as obtained from the BGP tables.

Version 2.0 adds to all these fields more information:

* Description in public databases
* Date
* Registry
* IPv4 Networks with the maintainers and last update.
* IPv6 Networks with the maintainers and last update.

This API request will always return a JSON structure with the information and a 200 HTTP code if the IP or AS number can be found in the system. If not, then it will return a 404 HTTP code as usual.

<aside class="warning">
Each Autonomous System look up request consumes <a href="https://apility.io/docs/difference-hits-requests/">1 hit</a>.
</aside>

## Get the AS information from an IP (V1.0).

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

## Get the AS information from an IP (V2.0).

> Get the AS data of an IP:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/as/ip/9.9.9.9"
```

>The response can be:

```shell
{
{
    "as": {
        "asn_description": "QUAD9-AS-1 - Quad9",
        "networks": [],
        "networks_v4": [
            {
                "cidr": "216.58.198.0/24",
                "description": "Proxy-registered route object",
                "maintainer": "MAINT-AS3491",
                "updated": "sajwani@pccwbtn.com 20040404"
            },
            ...,
            {
                "cidr": "216.58.192.0/19",
                "description": "route register for foxcomm",
                "maintainer": "FOXCOMM-MNT",
                "updated": "michael.renner@level3.com 20031104"
            }
        ],
        "name": "QUAD9-AS-1 - Quad9",
        "asn_date": "",
        "asn_registry": "",
        "networks_v6": [
            {
                "cidr": "2620:FE::/48",
                "description": "Quad9 - Global Public Recursive DNS Resolver Service",
                "maintainer": "MAINT-AS3856",
                "updated": "kabindra@pch.net 20171105"
            }
        ],
        "asn_country_code": "US",
        "country": "US",
        "asn": "19281"
    }
}
```

This endpoint returns a JSON structure that includes all the information in Version 1 of the API endpoint about the Autonomous System that owns the IP, and information about its networks. The request always returns the status code 200 (HTTP OK) if the IP exists. If the IP is malformed it will return a 400 (HTTP Bad Request) code.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/v2.0/as/ip/<IP>`

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
as  | JSON structure containing the ''[asv2](#asv2)'' object.


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>



## Bulk Request - Get the AS information from a set of IP (V1.0).

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

## Bulk Request - Get the AS information from a set of IP (V2.0).

> Get the AS data of a set of IP addresses:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/as_batch/ip/212.231.122.12,8.8.8.8,9.9.9.9"
```

>The response can be:

```shell
{
    "response": [
        {
            "ip": "212.231.122.12",
            "as": {
                "asn_description": "AS15704, ES",
                "networks": [
                    "31.222.80.0/20",
                    ...,
                    "217.113.240.0/20"
                ],
                "networks_v4": [
                    {
                        "cidr": "212.230.0.0/15",
                        "description": "Global ISP by PriorityTelecom Spain, S.A.",
                        "maintainer": "MUNDI-MNT",
                        "updated": null
                    },
                    ...,
                    {
                        "cidr": "195.160.224.0/22",
                        "description": null,
                        "maintainer": "MUNDI-MNT",
                        "updated": null
                    }
                ],
                "name": "Xtra Telecom S.A.",
                "asn_date": "2011-05-13",
                "asn_registry": "ripencc",
                "networks_v6": [
                    {
                        "cidr": "2a01:8480::/32",
                        "description": "MasMovil",
                        "maintainer": "MUNDI-MNT",
                        "updated": null
                    }
                ],
                "asn_country_code": "ES",
                "country": "ES",
                "asn": "15704"
            }
        },
        {
            "ip": "8.8.8.8",
            "as": {
                "asn_description": "GOOGLE - Google LLC, US",
                "networks": [
                    "8.8.4.0/24",
                    "8.8.8.0/24",
                    ...,
                    "216.252.222.0/24"
                ],
                "networks_v4": [
                    {
                        "cidr": "66.249.64.0/20",
                        "description": "Google",
                        "maintainer": "MAINT-AS15169",
                        "updated": "noc@google.com 20110301"
                    {
                        "cidr": "104.132.222.0/24",
                        "description": "ADDED FOR - AS15169",
                        "maintainer": "I123-MNT",
                        "updated": "rpd@123.net 20160516"
                    }
                ],
                "name": "Google LLC",
                "asn_date": "1992-12-01",
                "asn_registry": "arin",
                "networks_v6": [
                    {                    {
                        "cidr": "2a03:ace0::/32",
                        "description": "Google",
                        "maintainer": "MAINT-AS15169",
                        "updated": "jschiller@google.com 20160510  #16:41:45Z"
                    },
                    ...,
                    {
                        "cidr": "2604:31C0::/32",
                        "description": "Google",
                        "maintainer": "MAINT-AS15169",
                        "updated": "raybennett@google.com 20170412  #18:54:33Z"
                    }
                ],
                "asn_country_code": "US",
                "country": "US",
                "asn": "15169"
            }
        },
        {
            "ip": "9.9.9.9",
            "as": {
                "asn_description": "QUAD9-AS-1 - Quad9",
                "networks": [],
                "networks_v4": [
                    {
                        "cidr": "216.58.198.0/24",
                        "description": "Proxy-registered route object",
                        "maintainer": "MAINT-AS3491",
                        "updated": "sajwani@pccwbtn.com 20040404"
                    },
                    ...,
                    {
                        "cidr": "216.58.192.0/19",
                        "description": "route register for foxcomm",
                        "maintainer": "FOXCOMM-MNT",
                        "updated": "michael.renner@level3.com 20031104"
                    }
                ],
                "name": "QUAD9-AS-1 - Quad9",
                "asn_date": "",
                "asn_registry": "",
                "networks_v6": [
                    {
                        "cidr": "2620:FE::/48",
                        "description": "Quad9 - Global Public Recursive DNS Resolver Service",
                        "maintainer": "MAINT-AS3856",
                        "updated": "kabindra@pch.net 20171105"
                    }
                ],
                "asn_country_code": "US",
                "country": "US",
                "asn": "19281"
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

`GET https://api.apility.net/v2.0/as_batch/ip/<IP1>,<IP2>,...,<IPn>`

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
response  | JSON structure containing a JSON structure with the IP adddress and its ''[asv2](#asv2)'' object.


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

## Get the AS information from its number (V2.0)

> Get the AS data from its number:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/as/num/19281"
```

>The response can be:

```shell
{
    "as": {
        "asn_description": "QUAD9-AS-1 - Quad9",
        "networks": [],
        "networks_v4": [
            {
                "cidr": "216.58.198.0/24",
                "description": "Proxy-registered route object",
                "maintainer": "MAINT-AS3491",
                "updated": "sajwani@pccwbtn.com 20040404"
            },
            ...,
            {
                "cidr": "216.58.192.0/19",
                "description": "route register for foxcomm",
                "maintainer": "FOXCOMM-MNT",
                "updated": "michael.renner@level3.com 20031104"
            }
        ],
        "name": "QUAD9-AS-1 - Quad9",
        "asn_date": "",
        "asn_registry": "",
        "networks_v6": [
            {
                "cidr": "2620:FE::/48",
                "description": "Quad9 - Global Public Recursive DNS Resolver Service",
                "maintainer": "MAINT-AS3856",
                "updated": "kabindra@pch.net 20171105"
            }
        ],
        "asn_country_code": "US",
        "country": "US",
        "asn": "19281"
    }
}
```

This endpoint returns a JSON structure that includes all the information in Version 1 of the API endpoint about the Autonomous System that owns the IP, and information about its networks.  The request always returns the status code 200 (HTTP OK) if the IP exists. If the IP is malformed it will return a 400 (HTTP Bad Request) code.

<aside class="success">
You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

### HTTP Request

`GET https://api.apility.net/v2.0/as/num/<NUMBER>`

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
as  | JSON structure containing the '[asv2](#asv2)' object.


<aside class="warning">
You should double check that the endpoint of the URL is correct, since a wrong URL can return a 404 error too.
</aside>

## Bulk Request - Get the AS information from a set of AS numbers (V1.0).

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
                "networks": [
                    "31.222.80.0/20",
                    ...,
                    "217.113.240.0/20"
                ],
                "name": "Xtra Telecom S.A.",
                "country": "ES",
                "asn": "15704"
            }
        },
        {
            "asn": "3352",
            "as": {
                "networks": [
                    "2.136.0.0/16",
                    ...,
                    "217.127.0.0/16"
                ],
                "name": "Telefonica De Espana",
                "country": "ES",
                "asn": "3352"
            }
        },
        {
            "asn": "15169",
            "as": {
                "name": "Google LLC",
                "country": "US",
                "asn": "15169"
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

## Bulk Request - Get the AS information from a set of AS numbers (V2.0).

> Get the AS data from a set of AS numbers:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/as_batch/num/15704,3352,15169"
```

>The response can be:

```shell
{
    "response": [
        {
            "asn": "15704",
            "as": {
                "asn_description": "AS15704, ES",
                "networks": [
                    "31.222.80.0/20",
                    ...,
                    "217.113.240.0/20"
                ],
                "networks_v4": [
                    {
                        "cidr": "212.230.0.0/15",
                        "description": "Global ISP by PriorityTelecom Spain, S.A.",
                        "maintainer": "MUNDI-MNT",
                        "updated": null
                    },
                    ...,
                    {
                        "cidr": "195.160.224.0/22",
                        "description": null,
                        "maintainer": "MUNDI-MNT",
                        "updated": null
                    }
                ],
                "name": "Xtra Telecom S.A.",
                "asn_date": "2011-05-13",
                "asn_registry": "ripencc",
                "networks_v6": [
                    {
                        "cidr": "2a01:8480::/32",
                        "description": "MasMovil",
                        "maintainer": "MUNDI-MNT",
                        "updated": null
                    }
                ],
                "asn_country_code": "ES",
                "country": "ES",
                "asn": "15704"
            }
        },
        {
            "asn": "3352",
            "as": {
                "asn_description": "TELEFONICA_DE_ESPANA, ES",
                "networks": [
                    "2.136.0.0/16",
                    ...,
                    "217.127.0.0/16"
                ],
                "networks_v4": [
                    {
                        "cidr": "194.179.0.0/17",
                        "description": "TDENET (Red de servicios IP)",
                        "maintainer": "MAINT-AS3352",
                        "updated": null
                    },
                    ...,
                    {
                        "cidr": "212.170.0.0/17",
                        "description": "Proxy-registered route object",
                        "maintainer": "LEVEL3-MNT",
                        "updated": "roy@Level3.net 20070810"
                    }
                ],
                "name": "Telefonica De Espana",
                "asn_date": "2010-11-05",
                "asn_registry": "ripencc",
                "networks_v6": [
                    {
                        "cidr": "2a02:9000::/23",
                        "description": "ES-TELEFONICA-20110302",
                        "maintainer": "MAINT-AS3352",
                        "updated": null
                    }
                ],
                "asn_country_code": "ES",
                "country": "ES",
                "asn": "3352"
            }
        },
        {
            "asn": "15169",
            "as": {
                "asn_description": "GOOGLE - Google LLC, US",
                "networks": [
                    "8.8.4.0/24",
                    ...,
                    "216.252.222.0/24"
                ],
                "networks_v4": [
                    {
                        "cidr": "66.249.64.0/20",
                        "description": "Google",
                        "maintainer": "MAINT-AS15169",
                        "updated": "noc@google.com 20110301"
                    },
                    ...,
                    {
                        "cidr": "104.132.222.0/24",
                        "description": "ADDED FOR - AS15169",
                        "maintainer": "I123-MNT",
                        "updated": "rpd@123.net 20160516"
                    }
                ],
                "name": "Google LLC",
                "asn_date": "1992-12-01",
                "asn_registry": "arin",
                "networks_v6": [
                    {
                        "cidr": "2401:fa00::/42",
                        "description": "Google",
                        "maintainer": "MAINT-AS36385",
                        "updated": "noc@google.com 20100512"
                    },
                    ...,
                    {
                        "cidr": "2604:31C0::/32",
                        "description": "Google",
                        "maintainer": "MAINT-AS15169",
                        "updated": "raybennett@google.com 20170412  #18:54:33Z"
                    }
                ],
                "asn_country_code": "US",
                "country": "US",
                "asn": "15169"
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

`GET https://api.apility.net/v2.0/as_batch/num/<NUM1>,<NUM2>,...,<NUMn>`

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
response  | JSON structure containing a list of JSON structures with the AS number and its '[asv2](#asv2)' object.


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


## Add an IP to the private QUARANTINE-IP blacklist

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


## Add the Country of an IP address to the private QUARANTINE-COUNTRY blacklist

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


## Add the Continent of an IP address to the private QUARANTINE-CONTINENT blacklist

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


## Add the Autonomous System (AS) that owns the IP address to the private QUARANTINE-AS blacklist

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


## Check if the IP is in the private QUARANTINE-IP blacklist

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


## Check if the Country is in the private QUARANTINE-COUNTRY blacklist

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


## Check if the Continent is in the private QUARANTINE-CONTINENT blacklist

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


## Check if the AS is in the private QUARANTINE-AS blacklist

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


## Get full list of IP addresses in the private QUARANTINE-IP blacklist

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


## Get full list of Country codes in the private QUARANTINE-COUNTRY blacklist

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


## Get full list of Continent codes in the private QUARANTINE-CONTINENT blacklist

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


## Get full list of AS in the private QUARANTINE-AS blacklist

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




## Delete an IP address if it is in the private QUARANTINE-IP blacklist

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


## Delete a Country Code if it is in the private QUARANTINE-COUNTRY blacklist

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



## Delete a Continent Code if it is in the private QUARANTINE-CONTINENT blacklist

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


## Delete an AS if it is in the private QUARANTINE-AS blacklist

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




## Add an IP address automatically to QUARANTINE-IP blacklist if a domain or an email has a negative score

> Check a "bad" domain and add IP address 8.8.8.8 to QUARANTINE-IP if found malicious

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/baddomain/mailinator.com?quarantine_ip=8.8.8.8&quarantine_ttl=86400"
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

> Check a "bad" email and add IP address 8.8.8.8 to QUARANTINE-IP if found malicious

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/bademail/test@mailinator.com?quarantine_ip=8.8.8.8&quarantine_ttl=86400"
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


Sometimes a developer wants the IP address from which the checked domain or email was sent to be stored in the quarantine blacklist to prevent abuse from the same IP address over and over again.

This can be done by adding the `quarantine_ip` and `quarantine_ttl` parameters to the [existing API request for domain](#check-if-a-domain-scores-a-negative-or-neutral-score) and [existing API request for email](#check-if-an-email-scores-a-negative-or-neutral-score). In `quarantine_ip` you should pass the source IP address of the request, and in `quarantine_ttl` you can optionally pass the Time to Live in seconds that should remain the IP address in quarantine. If this parameter is not added, the default Time to Live is 3600 seconds.


<aside class="success">
You always have to pass the API key because quarantine needs to identify as a valid user in the platform. You can pass it as a header parameter or a query string parameter.
</aside>



# Resource History

Our databases contain several million active records of IP addresses, domains and emails. But this is just the tip of the iceberg because every day we process more than a million transactions on this database. A resource such as an IP address can enter and exit a blocking list on multiple occasions, which added to the fact that it can enter and exit different lists at the same time allows our users to get an idea of the magnitude of the information we handle.

For those cybersecurity experts who wish to know the historical activity of these resources in our database, we now make available to our users complete access to the history we have available for each one.

The resources available are:

* IP address
* Domains
* Emails

Because there may be a large amount of data available, it is possible to restrict queries by providing:

* Unix time in seconds from which the query will be made.
* Number of items to be returned per page.
* The page number.

Calling this API always returns items from the most recent to the oldest, so Unix time always indicates the freshsest transaction. If none of these parameters are provided, the API call will return the history from the current date with a maximum of 10 items.

Every call made to the API will count as a new HIT in the quota of the user.

<aside class="success">
This is a free and paid plan feature only. You always have to pass the API key. You can pass it as a header parameter or a query string parameter.
</aside>

<aside class="warning">
Each Resource History request consumes <a href="https://apility.io/docs/difference-hits-requests/">1 hit</a>.
</aside>



## Get IP address history

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/changes/ip/<IP>?timestamp=<TIMESTAMP>&page=<PAGE>&items=<ITEMS>"
```

>No matter if the IP has information in the database, it will always return the changes_ip JSON object. If it has information in the database:

```shell
{
    "changes_ip": [
        {
            "blacklist_change": "FAIL2BAN-ALL,FAIL2BAN-SSH",
            "blacklists": "",
            "timestamp": 1520187309162,
            "command": "rem",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-ALL",
            "blacklists": "FAIL2BAN-ALL,FAIL2BAN-SSH",
            "timestamp": 1520108359925,
            "command": "add",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-ALL",
            "blacklists": "FAIL2BAN-SSH",
            "timestamp": 1520104871843,
            "command": "rem",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-SSH",
            "blacklists": "FAIL2BAN-ALL,FAIL2BAN-SSH",
            "timestamp": 1520072166966,
            "command": "add",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-SSH",
            "blacklists": "FAIL2BAN-ALL",
            "timestamp": 1520068580059,
            "command": "rem",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-SSH,FAIL2BAN-ALL",
            "blacklists": "FAIL2BAN-SSH,FAIL2BAN-ALL",
            "timestamp": 1519946120669,
            "command": "add",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-SSH,FAIL2BAN-ALL",
            "blacklists": "FAIL2BAN-SSH,FAIL2BAN-ALL",
            "timestamp": 1519946118892,
            "command": "add",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-SSH,FAIL2BAN-ALL",
            "blacklists": "",
            "timestamp": 1519661715742,
            "command": "rem",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-SSH",
            "blacklists": "FAIL2BAN-SSH,FAIL2BAN-ALL",
            "timestamp": 1519647423138,
            "command": "add",
            "ip": "XX.XX.XX.XX"
        },
        {
            "blacklist_change": "FAIL2BAN-SSH",
            "blacklists": "FAIL2BAN-ALL",
            "timestamp": 1519643865538,
            "command": "rem",
            "ip": "XX.XX.XX.XX"
        }
    ]
}
```

>If there is no information in the database:

```shell
{
    "changes_ip": []
}
```


The changes_ip JSON object contains a list of transaction_ip objects. Each transaction IP object will return:

* timestamp: The UNIX time in seconds when the transaction was performed.
* command: Type of transaction in the database: ADD to the blacklist or REMove of the blacklist.
* ip: IP address of the transaction
* blacklist_change: Blackist added or removed thanks to the transaction.
* blacklists: List of blacklists after the execution of the command and the blacklist change.

### HTTP Request

`GET https://api.apility.net/metadata/changes/ip/<IP>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
timestamp | No | UNIX time in seconds to filter the search in the database. If ignored, then current UNIX time is taken.
page | No | Page number to paginate the result. Always start at 1. If ignored, then search for page one.
items | No | Number of items per page. Can be in the range of 5 to 200. If ignored, then return 10 items.
callback | Yes | Function to invoke when using JSONP model.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error message means something is wrong in the system and you should contact support.

If everything goes fine then it will also return the following JSON object:

Parameter | Description
--------- | -----------
changes_ip  | JSON object containing a list of JSON '[transaction ip](#transaction-ip)' objects.


## Get Domain history

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/changes/domain/<DOMAIN>?timestamp=<TIMESTAMP>&page=<PAGE>&items=<ITEMS>"
```

>No matter if the Domain has information in the database, it will always return the changes_domain JSON object. If it has information in the database:

```shell
{
    "changes_domain": [
        {
            "blacklist_change": "LISINGE-DED",
            "blacklists": "LISINGE-DED,MARTENSON-DED,IVOLO-DED,DEA",
            "command": "add",
            "domain": "XXX.XXX.XXX",
            "timestamp": 1519836616549
        },
        {
            "blacklist_change": "MARTENSON-DED",
            "blacklists": "MARTENSON-DED,IVOLO-DED,DEA",
            "command": "add",
            "domain": "XXX.XXX.XXX",
            "timestamp": 1519708978688
        },
        {
            "blacklist_change": "IVOLO-DED",
            "blacklists": "IVOLO-DED,DEA",
            "command": "add",
            "domain": "XXX.XXX.XXX",
            "timestamp": 1519708958303
        },
        {
            "blacklist_change": "DEA",
            "blacklists": "DEA",
            "command": "add",
            "domain": "XXX.XXX.XXX",
            "timestamp": 1519707737136
        }
    ]
}
```

>If there is no information in the database:

```shell
{
    "changes_domain": []
}
```


The changes_domain JSON object contains a list of transaction_domain objects. Each transaction Domain object will return:

* timestamp: The UNIX time in seconds when the transaction was performed.
* command: Type of transaction in the database: ADD to the blacklist or REMove of the blacklist.
* domain: Domain of the transaction
* blacklist_change: Blackist added or removed thanks to the transaction.
* blacklists: List of blacklists after the execution of the command and the blacklist change.

### HTTP Request

`GET https://api.apility.net/metadata/changes/domain/<DOMAIN>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
timestamp | No | UNIX time in seconds to filter the search in the database. If ignored, then current UNIX time is taken.
page | No | Page number to paginate the result. Always start at 1. If ignored, then search for page one.
items | No | Number of items per page. Can be in the range of 5 to 200. If ignored, then return 10 items.
callback | Yes | Function to invoke when using JSONP model.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error message means something is wrong in the system and you should contact support.

If everything goes fine then it will also return the following JSON object:

Parameter | Description
--------- | -----------
changes_domain  | JSON object containing a list of JSON '[transaction domain](#transaction-domain)' objects.

## Get Email history

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/changes/email/<EMAIL>?timestamp=<TIMESTAMP>&page=<PAGE>&items=<ITEMS>"
```

>No matter if the Email has information in the database, it will always return the changes_email JSON object. If it has information in the database:

```shell
{
    "changes_email": [
        {
            "blacklist_change": "STOPFORUMSPAM-365",
            "blacklists": "",
            "command": "rem",
            "email": "XXXXX@XXXXX.COM",
            "timestamp": 1518666525299
        },
        {
            "blacklist_change": "STOPFORUMSPAM-365",
            "blacklists": "STOPFORUMSPAM-365",
            "command": "add",
            "email": "XXXXX@XXXXX.COM",
            "timestamp": 1518461696289
        }
    ]
}
```

>If there is no information in the database:

```shell
{
    "changes_email": []
}
```


The changes_email JSON object contains a list of transaction_email objects. Each transaction Email object will return:

* timestamp: The UNIX time in seconds when the transaction was performed.
* command: Type of transaction in the database: ADD to the blacklist or REMove of the blacklist.
* email: Email of the transaction
* blacklist_change: Blackist added or removed thanks to the transaction.
* blacklists: List of blacklists/blacklists after the execution of the command and the blacklist change.

### HTTP Request

`GET https://api.apility.net/metadata/changes/email/<EMAIL>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner. You can choose to pass the API Key in the header or in the Query String.
timestamp | No | UNIX time in seconds to filter the search in the database. If ignored, then current UNIX time is taken.
page | No | Page number to paginate the result. Always start at 1. If ignored, then search for page one.
items | No | Number of items per page. Can be in the range of 5 to 200. If ignored, then return 10 items.
callback | Yes | Function to invoke when using JSONP model.


### Response

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error message means something is wrong in the system and you should contact support.

If everything goes fine then it will also return the following JSON object:

Parameter | Description
--------- | -----------
changes_email  | JSON object containing a list of JSON '[transaction email](#transaction-email)' objects.


# WHOIS query

WHOIS is a query and response protocol that is widely used for querying databases that store the registered users or assignees of an Internet resource, such as a domain name, an IP address block, or an autonomous system, but is also used for a wider range of other information. The protocol stores and delivers database content in a human-readable format. The WHOIS protocol is documented in RFC 3912.

This endpoint implements a WHOIS query service and returns as much information as possible for a given resource  in JSON format.

The resources currently available are (more in the future):

* IP address


The API call will try to return the WHOIS information of the resource cached in our databases. If the cache has expired or the resource is not cached then it will perform a rountrip to the Regional Internet Registry (RIR) to which the resource belongs.

<aside class="warning">
Each WHOIS query request consumes <a href="https://apility.io/docs/difference-hits-requests/">1 hit</a>.
</aside>


## Lookup WHOIS IP address

```shell
$ curl -i -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/whois/ip/<IP>"
```

>No matter if the IP address has information in any RIR, it will always return the whois JSON object. If the IP address has information in any RIR:

```shell
{
    "whois": {
        "asn_country_code": "US",
        "objects": {
            "ABUSE5250-ARIN": {
                "handle": "ABUSE5250-ARIN",
                "events": [
                    {
                        "action": "last changed",
                        "actor": null,
                        "timestamp": "2017-12-04T10:49:20-05:00"
                    },
                    {
                        "action": "registration",
                        "actor": null,
                        "timestamp": "2015-11-06T15:36:35-05:00"
                    }
                ],
                "roles": [
                    "abuse"
                ],
                "status": [
                    "validated"
                ],
                "contact": {
                    "email": [
                        {
                            "type": null,
                            "value": "network-abuse@google.com"
                        }
                    ],
                    "phone": [
                        {
                            "type": [
                                "work",
                                "voice"
                            ],
                            "value": "+1-650-253-0000"
                        }
                    ],
                    "role": null,
                    "name": "Abuse",
                    "address": [
                        {
                            "type": null,
                            "value": "1600 Amphitheatre Parkway\nMountain View\nCA\n94043\nUnited States"
                        }
                    ],
                    "title": null,
                    "kind": "group"
                },
                "remarks": [
                    {
                        "links": null,
                        "title": "Registration Comments",
                        "description": "Please note that the recommended way to file abuse complaints are located in the following links.\r\n\r\nTo report abuse and illegal activity: https://www.google.com/intl/en_US/goodtoknow/online-safety/reporting-abuse/ \r\n\r\nFor legal requests: http://support.google.com/legal \r\n\r\nRegards,\r\nThe Google Team"
                    }
                ],
                "links": [
                    "https://rdap.arin.net/registry/entity/ABUSE5250-ARIN",
                    "https://whois.arin.net/rest/poc/ABUSE5250-ARIN"
                ],
                "events_actor": null,
                "notices": [
                    {
                        "links": [
                            "https://www.arin.net/whois_tou.html"
                        ],
                        "title": "Terms of Service",
                        "description": "By using the ARIN RDAP/Whois service, you are agreeing to the RDAP/Whois Terms of Use"
                    }
                ],
                "raw": null,
                "entities": null
            },
            "GOGL": {
                "handle": "GOGL",
                "events": [
                    {
                        "action": "last changed",
                        "actor": null,
                        "timestamp": "2017-12-21T13:24:44-05:00"
                    },
                    {
                        "action": "registration",
                        "actor": null,
                        "timestamp": "2000-03-30T00:00:00-05:00"
                    }
                ],
                "roles": [
                    "registrant"
                ],
                "status": null,
                "contact": {
                    "email": null,
                    "phone": null,
                    "role": null,
                    "name": "Google LLC",
                    "address": [
                        {
                            "type": null,
                            "value": "1600 Amphitheatre Parkway\nMountain View\nCA\n94043\nUnited States"
                        }
                    ],
                    "title": null,
                    "kind": "org"
                },
                "remarks": null,
                "links": [
                    "https://rdap.arin.net/registry/entity/GOGL",
                    "https://whois.arin.net/rest/org/GOGL"
                ],
                "events_actor": null,
                "notices": null,
                "raw": null,
                "entities": [
                    "ABUSE5250-ARIN",
                    "ZG39-ARIN"
                ]
            },
            "ZG39-ARIN": {
                "handle": "ZG39-ARIN",
                "events": [
                    {
                        "action": "last changed",
                        "actor": null,
                        "timestamp": "2017-10-17T06:35:04-04:00"
                    },
                    {
                        "action": "registration",
                        "actor": null,
                        "timestamp": "2000-11-30T13:54:08-05:00"
                    }
                ],
                "roles": [
                    "administrative",
                    "technical"
                ],
                "status": [
                    "validated"
                ],
                "contact": {
                    "email": [
                        {
                            "type": null,
                            "value": "arin-contact@google.com"
                        }
                    ],
                    "phone": [
                        {
                            "type": [
                                "work",
                                "voice"
                            ],
                            "value": "+1-650-253-0000"
                        }
                    ],
                    "role": null,
                    "name": "Google LLC",
                    "address": [
                        {
                            "type": null,
                            "value": "1600 Amphitheatre Parkway\nMountain View\nCA\n94043\nUnited States"
                        }
                    ],
                    "title": null,
                    "kind": "group"
                },
                "remarks": null,
                "links": [
                    "https://rdap.arin.net/registry/entity/ZG39-ARIN",
                    "https://whois.arin.net/rest/poc/ZG39-ARIN"
                ],
                "events_actor": null,
                "notices": [
                    {
                        "links": [
                            "https://www.arin.net/whois_tou.html"
                        ],
                        "title": "Terms of Service",
                        "description": "By using the ARIN RDAP/Whois service, you are agreeing to the RDAP/Whois Terms of Use"
                    }
                ],
                "raw": null,
                "entities": null
            }
        },
        "asn_cidr": "8.8.8.0/24",
        "nir": null,
        "entities": [
            "GOGL"
        ],
        "network": {
            "handle": "NET-8-8-8-0-1",
            "status": null,
            "type": null,
            "start_address": "8.8.8.0",
            "end_address": "8.8.8.255",
            "remarks": null,
            "events": [
                {
                    "action": "last changed",
                    "actor": null,
                    "timestamp": "2014-03-14T15:52:05-04:00"
                },
                {
                    "action": "registration",
                    "actor": null,
                    "timestamp": "2014-03-14T15:52:05-04:00"
                }
            ],
            "parent_handle": "NET-8-0-0-0-1",
            "cidr": "8.8.8.0/24",
            "country": null,
            "raw": null,
            "name": "LVLT-GOGL-8-8-8",
            "notices": [
                {
                    "links": [
                        "https://www.arin.net/whois_tou.html"
                    ],
                    "title": "Terms of Service",
                    "description": "By using the ARIN RDAP/Whois service, you are agreeing to the RDAP/Whois Terms of Use"
                }
            ],
            "ip_version": "v4",
            "links": [
                "https://rdap.arin.net/registry/ip/8.8.8.0",
                "https://whois.arin.net/rest/net/NET-8-8-8-0-1",
                "https://rdap.arin.net/registry/ip/8.0.0.0/8"
            ]
        },
        "asn_description": "GOOGLE - Google LLC, US",
        "asn_date": "1992-12-01",
        "query": "8.8.8.8",
        "raw": null,
        "asn_registry": "arin",
        "asn": "15169"
    }
}```

>If there is no information in any RIR:

```shell
{
    "whois": []
}
```

The whois JSON object contains all the information returned by the RIR about the object. See a full description of the objet whois.


### HTTP Request

`GET https://api.apility.net/whois/ip/<IP>`

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

The response should be a __HTTP/1.1 200 OK__ if everything is ok. Any other error message means something went wrong in the system and you should contact support.

If everything goes fine then it will also return the following JSON object:

Parameter | Description
--------- | -----------
whois  | JSON object containing all the information returned by the RIR '[whois](#whois)' object.

# Apility API metadata

Metadata services allow developers to access internal information about Apility.io service blacklists, databases, and repositories.

Among the data that can be obtained to metadata calls are:

* List of all blacklists of IP addresses (badip).
* List of all blacklists of Domain addresses (baddomain).
* List of all blacklists of Email addresses (bademail).
* Detail of a blacklist of type badip.
* Detail of a blacklist of type baddomain.
* Detail of a blacklist of type bademail.
* Sum of all elements of badip, domain and bademail.


This API request will always return a JSON structure with the information and a 200 HTTP code if the list can be found in the database. If not, then it will return a 404 HTTP code as usual.

<aside class="warning">
Each metadata request consumes <a href="https://apility.io/docs/difference-hits-requests/">1 hit</a>.
</aside>

## List of all blackists by type.

> Get the list of blacklists type badip:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/badip/lists"
```

>The response can be:

```shell
{
    "NIXSPAM-IP": {
        "group": "abuse",
        "refresh": "Every 15 minutes",
        "last_update": "1543164604",
        "visibility": "Public",
        "site": "http://www.nixspam.org",
        "type": "badip",
        "description": "The iX blacklist is made of over 500,000 automatically generated entries per day without distinguishing open proxies from relays, dialup gateways, and so on. After 12 hours the IP address will be removed if there is no new spam from there.",
        "source": "NiX Spam IP DNSBL and blacklist",
        "name": "NiX Spam IP blacklist",
        "count": "40645",
        "problem": "It lists any active address instantly whilst removing older entries. That's the idea of the iX blacklist.",
        "enabled": "True"
    },
    ...
    ,
    "ZEUS-BADIP-IP": {
        "group": "abuse",
        "refresh": "Every 60 minutes",
        "last_update": "1543163417",
        "visibility": "Public",
        "site": "https://zeustracker.abuse.ch",
        "type": "badip",
        "description": "ZeuS Tracker offers various IP-blocklists that contains known ZeuS Command&Control server (C&C) assocaited with the ZeuS crimeware. ZeuS Tracker offers blocklists in various formats and for different purposes.",
        "source": "ZEUS-BADIP-IP Blacklist - IP of ZeuS Command&Control",
        "name": "ZEUS-BADIP-IP Blacklist - IP of ZeuS Command&Control",
        "count": "107",
        "problem": "This blocklists only includes IPv4 addresses that are used by the ZeuS trojan. It is the recommened blocklist if you want to block only ZeuS IPs. It excludes IP addresses that ZeuS Tracker believes to be hijacked (level 2) or belong to a free web hosting provider (level 3). Hence the false postive rate should be much lower compared to the standard ZeuS Standard IP blocklist.",
        "enabled": "True"
    }
}
```

> Get the list of blacklists type baddomain:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/baddomain/lists"
```

>The response can be:

```shell
{
    "FREEMAIL": {
        "group": "anonymizer",
        "refresh": "Everyday",
        "last_update": "1543164938",
        "visibility": "Public",
        "site": "",
        "type": "baddomain",
        "description": "Free email services give users the ability to send and receive emails plus more services like a calendar, instant messaging and mobile apps for your smartphone, among other features that can be beneficial for individuals. These services can be free of charge or advertised supported, and this is what makes them suitable for users that want to hide their identity using multiple free email accounts.",
        "source": "Several sources.",
        "name": "Free Email Providers blacklist",
        "count": "4317",
        "problem": "Free email can be used to mask the identities of abusers, but free email users cannot be classified as abusers by default. Still, some companies reject to accept free email addresses when registering.",
        "enabled": "True"
    },
    ...
    ,
    "ETHERSCAMDB-DOMAINS": {
        "group": "anonymizer",
        "refresh": "Everyday",
        "last_update": "1543118542",
        "visibility": "Public",
        "site": "https://etherscamdb.info",
        "type": "baddomain",
        "description": "Ethereum Scam Database or EtherScamDB is an open source project and website that combines all the information that's available. It also has an easy to use reporting function and add them to the database.",
        "source": "Github EtherScamDB account.",
        "name": "Ethereum Scam Database Domain blacklist",
        "count": "6059",
        "problem": "There is a lot of bad guys out there that will try to steal your cryptocurrencies using fake domains. This list will help you to avoid them.",
        "enabled": "True"
    }
}
```

> Get the list of blacklists type bademail:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/bademail/lists"
```

>The response can be:

```shell
{
    "STOPFORUMSPAM-90": {
        "group": "abuse",
        "refresh": "Every day",
        "last_update": "1543115919",
        "visibility": "Public",
        "site": "http://www.stopforumspam.com",
        "type": "bademail",
        "description": "StopForumSpamprovide lists of spammers that persist in abusing forums and blogs with their scams, ripoffs, exploits and other annoyances*. They provide these lists so that you don't have to endure the never ending job of having to moderate, filter and delete their rubbish. Stop Forum Spam is one of the biggest and more detailed list of abusers.",
        "source": "StopForumSpam 90 days Email blacklist",
        "name": "StopForumSpam Email 90d",
        "count": "574535",
        "problem": "Emails in this list includes new entries in the main list in the last 90 days.",
        "enabled": "True"
    },
    ...
    ,
    "STOPFORUMSPAM-7": {
        "group": "abuse",
        "refresh": "Every day",
        "last_update": "1543115779",
        "visibility": "Public",
        "site": "http://www.stopforumspam.com",
        "type": "bademail",
        "description": "StopForumSpamprovide lists of spammers that persist in abusing forums and blogs with their scams, ripoffs, exploits and other annoyances*. They provide these lists so that you don't have to endure the never ending job of having to moderate, filter and delete their rubbish. Stop Forum Spam is one of the biggest and more detailed list of abusers.",
        "source": "StopForumSpam 7 days Email blacklist",
        "name": "StopForumSpam Email 7d",
        "count": "56253",
        "problem": "Emails in this list includes new entries in the main list in the last 7 days.",
        "enabled": "True"
    }
}
```

This endpoint returns a JSON structure with a list of blacklist objects information indexed by the list Id. The request always returns the status code 200 (HTTP OK).

<aside class="success">
This API endpoint does not need API Key.
</aside>

### HTTP Request

`GET https://api.apility.net/metadata/<BLACKLIST_TYPE>/lists`

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
BLACKLIST_TYPE | The blacklist type: badip, baddomain or bademail.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return a JSON with a list of blacklist Id names and its '[blacklist](#blacklist)' object.

## Get full details of a blacklist

> Get the details of a blacklist by type badip and blacklist id:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/badip/lists/NIXSPAM-IP"
```

>The response can be:

```shell
{
        "group": "abuse",
        "refresh": "Every 15 minutes",
        "last_update": "1543164604",
        "visibility": "Public",
        "site": "http://www.nixspam.org",
        "type": "badip",
        "description": "The iX blacklist is made of over 500,000 automatically generated entries per day without distinguishing open proxies from relays, dialup gateways, and so on. After 12 hours the IP address will be removed if there is no new spam from there.",
        "source": "NiX Spam IP DNSBL and blacklist",
        "name": "NiX Spam IP blacklist",
        "count": "40645",
        "problem": "It lists any active address instantly whilst removing older entries. That's the idea of the iX blacklist.",
        "enabled": "True"
}
```

> Get the details of a blacklist by type baddomain and blacklist id:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/baddomain/lists/FREEMAIL"
```

>The response can be:

```shell
{
        "group": "anonymizer",
        "refresh": "Everyday",
        "last_update": "1543164938",
        "visibility": "Public",
        "site": "",
        "type": "baddomain",
        "description": "Free email services give users the ability to send and receive emails plus more services like a calendar, instant messaging and mobile apps for your smartphone, among other features that can be beneficial for individuals. These services can be free of charge or advertised supported, and this is what makes them suitable for users that want to hide their identity using multiple free email accounts.",
        "source": "Several sources.",
        "name": "Free Email Providers blacklist",
        "count": "4317",
        "problem": "Free email can be used to mask the identities of abusers, but free email users cannot be classified as abusers by default. Still, some companies reject to accept free email addresses when registering.",
        "enabled": "True"
}
```

> Get the details of a blacklist by type bademail and blacklist id:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/bademail/lists/STOPFORUMSPAM-90"
```

>The response can be:

```shell
{
        "group": "abuse",
        "refresh": "Every day",
        "last_update": "1543115919",
        "visibility": "Public",
        "site": "http://www.stopforumspam.com",
        "type": "bademail",
        "description": "StopForumSpamprovide lists of spammers that persist in abusing forums and blogs with their scams, ripoffs, exploits and other annoyances*. They provide these lists so that you don't have to endure the never ending job of having to moderate, filter and delete their rubbish. Stop Forum Spam is one of the biggest and more detailed list of abusers.",
        "source": "StopForumSpam 90 days Email blacklist",
        "name": "StopForumSpam Email 90d",
        "count": "574535",
        "problem": "Emails in this list includes new entries in the main list in the last 90 days.",
        "enabled": "True"
}
```

This endpoint returns a JSON structure with a list of blacklist objects information indexed by the list Id. The request always returns the status code 200 (HTTP OK).

<aside class="success">
This API endpoint does not need API Key.
</aside>

### HTTP Request

`GET https://api.apility.net/metadata/<BLACKLIST_TYPE>/lists/<BLACKLIST_ID>`

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
BLACKLIST_TYPE | The blacklist type: badip, baddomain or bademail.
BLACKLIST_ID | The alphanumeric blacklist id.

### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return a JSON with a list of blacklist Id names and its '[blacklist](#blacklist)' object.

## Get statistics of all the blacklists

> Get the statistics of all the blacklists:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata"
```

>The response can be:

```shell
{
    "badip": {
        "items": 1313983954,
        "lists": 102
    },
    "baddomain": {
        "items": 318413,
        "lists": 25
    },
    "bademail": {
        "items": 4677067,
        "lists": 6
    }
}
```

This endpoint returns a JSON structure with a list of blacklist objects information indexed by the list Id. The request always returns the status code 200 (HTTP OK).

<aside class="success">
This API endpoint does not need API Key.
</aside>

### HTTP Request

`GET https://api.apility.net/metadata/<BLACKLIST_TYPE>/lists/<BLACKLIST_ID>`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.


### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return a JSON with an '[blackliststats](#blackliststats)' object.

## Get statistics of the number of IP addresses processed

> Get statistics of the number IP addresses processed:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/metadata/processed/ip"
```

>The response can be:

```shell
{ 
   "last_badip_log":{ 
      "last_hour":4960,
      "last_24_hours":289253,
      "last_7_days":1472441
   }
}
```

This endpoint returns a JSON structure with a the number of IP addresses processed by the engine during the last hour, 24 hours and 7 day. This information is updated once every 60 minutes, so don't expect more than one change in the interval. The request always returns the status code 200 (HTTP OK).

<aside class="success">
This API endpoint does need an API Key.
</aside>

### HTTP Request

`GET https://api.apility.net/metadata/processed/ip`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | No | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | No | API Key of the owner.
callback | No | Function to invoke when using JSONP model.


### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return a JSON with an '[last_badip_log](#last_badip_log)' object.


# Apility API account information

Account information services allow developers to get read-only about the existing quota, the active blacklists and the configuration of the services.

The information that can be obtained is:

* Number of hits consumed in the existing 24 hour period.
* Time To Live (TTL) until the existing 24 hour period expires.
* Active blacklists of IP addresses.
* Active blacklists of Domains.
* Internal parameters configuration.
* Source IP or Header restrictions.


This API request will always return a JSON structure with the information and a 200 HTTP code if the API TOKEN belongs to an active user. If not, then it will return a:
* 401 HTTP Unauthorized if no API KEY is passed or API KEY is well formed but not recognized (user does not exists), for example doing an anonymous request.
* 400 HTTP Bad Request if the API KEY passed is malformed.


## Get the current number of hits and TTL.

> Get the current number of hits and TTL:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/account/quota"
```

>The response can be:

```shell
{
    "hits": 70876, 
    "ttl": 2628
}
```

This endpoint returns a JSON structure with the number of hits consumed in the current 24 hour period, and the Time to Live until the quota is restarted after this 24 hout period. The request always returns the status code 200 (HTTP OK).

<aside class="success">
This API endpoint needs API Key.
</aside>

### HTTP Request

`GET https://api.apility.net/v2.0/account/quota`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | Yes | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | Yes | API Key of the owner.


### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return a JSON with a 'hits' and 'ttl' objects. 'ttl' is measured in seconds.

## Get the active blacklists.

> Get the active blacklists:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/account/blacklists"
```

>The response can be:

```shell
{ 
   "ip_addresses":[ 
      "ALIENVAULT-REPUTATION",
      "BBCAN177-MS1",
      "BBCAN177-MS3",
      "BLOCKLISTNET-UA",
      "BOTSCOUT-1D",
      "BOTSCOUT-30D",
      "BOTSCOUT-7D",
      "BOTSCOUT-LATEST",
      "BRUTEFORCEBLOCKER",
      "CLEANTALK-ORG-1D",
      "CLEANTALK-ORG-30D",
      "CLEANTALK-ORG-7D",
      "CLEANTALK-ORG-LATEST",
      "DEA-IP",
      "EXPRESSVPN-EXIT-IP",
      "FAIL2BAN-APACHE",
      "FAIL2BAN-BOTS",
      "FAIL2BAN-BRUTEFORCELOGIN",
      "FAIL2BAN-FTP",
      "FAIL2BAN-IMAP",
      "FAIL2BAN-IRCBOT",
      "FAIL2BAN-MAIL",
      "FAIL2BAN-SIP",
      "FAIL2BAN-SSH",
      "FAIL2BAN-STRONGIPS",
      "IDCLOACK-COM-1D",
      "IDCLOACK-COM-30D",
      "IDCLOACK-COM-7D",
      "IDCLOACK-COM-LATEST",
      "IPVANISHVPN-EXIT-IP",
      "IVOLO-DED-IP",
      "LISINGE-DED-IP",
      "MARTENSON-DED-IP",
      "NIXSPAM-IP",
      "NORDVPN-EXIT-IP",
      "SPYS-ONE-1D",
      "SPYS-ONE-30D",
      "SPYS-ONE-7D",
      "SPYS-ONE-LATEST",
      "SSLBL-IP",
      "STOPFORUMSPAM-1",
      "STOPFORUMSPAM-30",
      "STOPFORUMSPAM-7",
      "STOPFORUMSPAM-90",
      "TOP100-LATEST-IP",
      "TOR",
      "TOR-BLUTMAGIE-FULL",
      "UCEPROTECT-BACKSCATTERER",
      "UCEPROTECT-LEVEL1",
      "ZEUS-BADIP-IP",
      "ZEUS-STANDARD-IP"
   ],
   "domains":[ 
      "AA419-ORG-1D-DOMAINS",
      "AA419-ORG-30D",
      "AA419-ORG-LATEST-DOMAINS",
      "COINBLOCKER-7D-DOMAINS",
      "COINBLOCKER-LATEST-DOMAINS",
      "DEA",
      "EAL-DOMAINS",
      "ETHERSCAMDB-DOMAINS",
      "ISC-DOMAINS-HIGH",
      "ISC-DOMAINS-MEDIUM",
      "IVOLO-DED",
      "LISINGE-DED",
      "MALWAREDOMAINS-COM",
      "MARTENSON-DED",
      "METAMASK-DOMAINS",
      "RANSOMWARE-ABUSE-CH",
      "SBLACK-HOSTS-DOMAINS",
      "SQUIDBLACKLIST-MALICIOUS-DOMAINS"
   ]
}
```

This endpoint returns a JSON structure with two lists of strings: the list of active blacklists of IP addresses and the list of active blacklists of domains. The request always returns the status code 200 (HTTP OK).

<aside class="success">
This API endpoint needs API Key.
</aside>

### HTTP Request

`GET https://api.apility.net/v2.0/account/blacklists`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | Yes | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | Yes | API Key of the owner.


### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return a JSON with an 'ip_address' array of strings and 'domains' array of strings.

## Get the config information.

> Get the config information:

```shell
$ curl -H "X-Auth-Token: UUID" -X GET "https://api.apility.net/v2.0/account/config"
```

>The response can be:

```shell
{
    "config": "{}", 
    "restriction": "None", 
    "restriction_values": ""
}
```

This endpoint returns a JSON structure with three values: a dictionary with internal configuration parameters (if any), the type of access restriction to the API (None, IP or Header) and the values depending on the restriction type (IP addresses or domains). The request always returns the status code 200 (HTTP OK).

<aside class="success">
This API endpoint needs API Key.
</aside>

### HTTP Request

`GET https://api.apility.net/v2.0/account/config`

### Header Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
X-Auth-Token | Yes | API Key of the owner.

### QueryString Parameters

Parameter    | Mandatory | Description
------------ | --------- | -----------
token | Yes | API Key of the owner.


### Response

The status code of the response is 200 (HTTP OK) if everything was ok, but it will also return a JSON with a 'config' dictionary, the 'restriction' string with three possible values: None, IP or Header and the 'restriction_values' with the list of the IP addresses or domains to allow access, depending on the restriction type chosen.

# Objects

## IP

The ip score contains the information of looking up the IP in the blacklists.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means 'suspicious' or 'bad' IP. Neutral or positive means it's a 'clean' IP.
blacklist | Array containing the blacklists where the IP was found.

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
networks | Array with the lists of networks of the AS obtained from BGP tables.

## asv2

The AS object contains the information of Autonmous System for the v2.0 of the API endpoint.

Parameter     | Description
------------- | -----------
asn | AS number
name | name of the AS
asn_description | Full description of the AS
asn_date | Date when the AS was registered
asn_registry | Local registry that maintains the AS information.
country | ISO 3166-1 Country code
networks | Array with the lists of networks of the AS obtained from BGP tables.
networks_v4 | Array with the lists of ipv4 cidr information. See [AS_NETWORK](#as-network) object.
networks_v6 | Array with the lists of ipv6 cidr information. See [AS_NETWORK](#as-network) object.

## as-network

The AS object that contains the information of the CIDR block and the maintainer.

Parameter     | Description
------------- | -----------
cidr | IPv4 or IPv6 prefix of a block managed by the AS.
description | Human readable description of the object.
maintainer | Who is the maintainer of the object. It can reference other AS or itself.
updated | Who made the last change and when.

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
graylisted | The SMTP service implements a graylisting feature, so the data in this object should be analyzed before considering it valid.
timedout | The SMTP service timed out before completing all the tests. Hence, the data in this object should be considered not valid.

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

## transaction ip

The transaction ip object contains information about what action was performed on the blacklists/blacklists in the database.

Parameter     | Description
------------- | -----------
timestamp | The UNIX time in seconds when the transaction was performed.
command | 'add' or 'rem'. Type of transaction in the database: ADD to the blacklist or REMove of the blacklist.
ip | IP address of the transaction
blacklist_change | Blackist added or removed thanks to the transaction.
blacklists | List of blacklists after the execution of the command and the blacklist change.

## transaction domain

The transaction domain object contains information about what action was performed on the blacklists in the database.

Parameter     | Description
------------- | -----------
timestamp | The UNIX time in seconds when the transaction was performed.
command | 'add' or 'rem'. Type of transaction in the database: ADD to the blacklist or REMove of the blacklist.
domain | Domain of the transaction
blacklist_change | Blackist added or removed thanks to the transaction.
blacklists | List of blacklists after the execution of the command and the blacklist change.

## transaction email

The transaction email object contains information about what action was performed on the blacklists in the database.

Parameter     | Description
------------- | -----------
timestamp | The UNIX time in seconds when the transaction was performed.
command | 'add' or 'rem'. Type of transaction in the database: ADD to the blacklist or REMove of the blacklist.
email | Email of the transaction
blacklist_change | Blackist added or removed thanks to the transaction.
blacklists | List of blacklists after the execution of the command and the blacklist change.

## whois

Contains many nested lists and objects, detailed below.

Parameter     | Description
------------- | -----------
query |	The IP address
asn	| Globally unique identifier used for routing information exchange with Autonomous Systems.
asn_cidr | Network routing block assigned to an ASN.
asn_country_code | ASN assigned country code in ISO 3166-1 format.
asn_date | ASN allocation date in ISO 8601 format.
asn_registry | ASN assigned regional internet registry.
asn_description | The ASN description
network | The assigned network for an IP address. May be a parent or child network. See [Network](#whois-network) object.
entities | list of object names referenced by an RIR network. Map these to the objects keys.
objects | The objects (entities) referenced by an RIR network or by other entities (depending on depth parameter). Keys are the object names with values as [Object](#whois-object).

## whois network

The parameters mapped to the network in the objects list within the [whois](#whois) object.

Parameter     | Description
------------- | -----------
cidr	|	Network routing block an IP address belongs to.
country	|	Country code registered with the RIR in ISO 3166-1 format.
end_address	|	The last IP address in a network block.
events	|	List of events. See [Events](#whois-event) object.
handle	|	Unique identifier for a registered object.
ip_version	|	IP protocol version (v4 or v6) of an IP address.
links	|	HTTP/HTTPS links provided for an RIR object.
name	|	The identifier assigned to the network registration for an IP address.
notices	|	List of notice objects. See [Notices](#whois-notice) object.
parent_handle	|	Unique identifier for the parent network of a registered network.
remarks	|	List of remark (notice) dictionaries. See [Notices](#whois-notice) object.
start_address	|	The first IP address in a network block.
status	|	List indicating the state of a registered object.
type	|	The RIR classification of a registered network.

## whois object

The parameters mapped to the object (entity) in the objects list within the [whois](#whois).

Parameter     | Description
------------- | -----------
contact	|	Contact information registered with an RIR object. See [Object Contact](#whois-object_contact).
entities	|	List of object names referenced by an RIR object. Map these to other objects keys.
events	|	List of event dictionaries. See [Events](#whois-event) object.
events_actor	|	List of event (no actor) dictionaries. See [Events](#whois-event) object.
handle	|	Unique identifier for a registered object.
links	|	List of HTTP/HTTPS links provided for an RIR object.
notices	|	List of notice dictionaries. See [Notices](#whois-notice) object.
remarks	|	List of remark (notice) dictionaries. See [Notices](#whois-notice) object.
roles	|	List of roles assigned to a registered object.
status	|	List indicating the state of a registered object.

## whois object contact

The contact information registered to an RIR object. This is the contact key contained in [Object](#whois-object).

Parameter     | Description
------------- | -----------
address	|	List of contact postal address dictionaries. Contains key type and value.
email	|	List of contact email address dictionaries. Contains key type and value.
kind	|	The contact information kind (individual, group, org).
name	|	The contact name.
phone	|	List of contact phone number dictionaries. Contains key type and value.
role	|	The contact’s role.
title	|	The contact’s position or job title.

## whois event

Common to lists of events in the registry.

Parameter     | Description
------------- | -----------
action	|	The reason for an event.
timestamp	|	The date an event occured in ISO 8601 format.
actor	|	The identifier for an event initiator (if any).


## whois notice

Information contained in notices and remarks.

Parameter     | Description
------------- | -----------
title	|	The title/header for a notice.
description	|	The description/body of a notice.
links	|	list of HTTP/HTTPS links provided for a notice.

## fullip

Information gathered from different sources to describe the reputation of the IP Address.

Parameter     | Description
------------- | -----------
geo	|	IP Geolocation object as described in [GeoIP](#geoip)
hostname	|	String with the name of a hostname result of a reverse DNS lookup on the IP Address.
baddomain	|	The [Domain](#domain) object contains the information obtained in the scoring algorithm of the hostname.
badip  |   The [IP](#ip) object contains the information obtained in the scoring algorithm of the IP address.
history | List of transactions as objects [History IP](#history-ip) in the IP address blacklist database and the scoring.
whois | Full WHOIS information as described in the object [WHOIS](#whois)
score | Number describing the result of summarizing each individual scores.

## history ip

List of transactions in the blacklist database and the scoring based on when it was inserted.

Parameter     | Description
------------- | -----------
score | Number describing the result of the algorithm. Negative means the IP was add to any blacklist in the specified time range.
activity | List of [Transaction IP](#transaction-ip) objects with the activity in the database.
score_1day | True if the IP was added in any blacklist in the last 24 hours.
score_7days | True if the IP was added in any blacklist in the last 7 days.
score_30days | True if the IP was added in any blacklist in the last 30 days.
score_90days | True if the IP was added in any blacklist in the last 90 days.
score_180days | True if the IP was added in any blacklist in the last 180 days.
score_1year | True if the IP was added in any blacklist in the last 365 days.

## blacklist

Details about a given blacklist.

Parameter     | Description
------------- | -----------
group | Sub-group to classify the blacklists.
refresh | How often the list is updated. It's a human readable text.
last_update | When was the last time the blacklist was updated. Expressed in seconds since 00:00:00 UTC, 1 January 1970 (Unix Time).
visibility | If this list is Public or Private.
site | The website or internet endpoint where the list was obtained.
type | Blacklist type: badip, baddomain or bademail.
description | A human readable description of what is this list and its purpose.
source | Generic name of the data source.
name | Human readable name of the list.
count | Number of items in the list.
problem | A human readable description of why a user should use the list.
enabled | Internal information. Should always be Enabled for end users.

## blackliststats

Statistics of the blacklists.

Parameter     | Description
------------- | -----------
badip | A [blackliststatsinfo](#blackliststatsinfo) object with the number of items and lists of type badip.
baddomain | A [blackliststatsinfo](#blackliststatsinfo) object with the number of items and lists of type baddomain.
bademail | A [blackliststatsinfo](#blackliststatsinfo) object with the number of items and lists of type bademail.

## blackliststatsinfo

Information detail of the blacklists statistics.

Parameter     | Description
------------- | -----------
items | Number of items in the chosen type.
lists | Number of lists in the chosen type.

## last_badip_log

Number of IP addresses processed in the last hour, 24 hours and 7 days.

Parameter     | Description
------------- | -----------
last_hour | An integer with the number of IP addresses processed in the last 60 minutes.
last_24_hours | An integer with the number of IP addresses processed in the last 24 hours.
last_7_days | An integer with the number of IP addresses processed in the last 7 days.
