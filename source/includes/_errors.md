# Errors

Apility.io uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is probably using parameters with bad format. Check the error details.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- Your API key does not have enough permissions to perform the action requested.
404 | Not Found -- If using Simple Model, then it means the resource is not in any blacklist (and this is good).
405 | Method Not Allowed -- All endpoints only allow GET verbs.
429 | Too Many Requests -- You have ran out of quota. Please consider upgrading your plan.
500 | Internal Server Error -- We had a problem with our server. Please report to our suppor team.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
