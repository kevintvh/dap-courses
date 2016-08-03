Build Simple Webservice using Apache Nifi
=========================================

# What is ?

Build webservice using apache nifi only.

# How to use this repo ?

1. Start Docker containers:
```
# docker-compose up
```
2. Connect to local [Nifi](http://localhost:8080/nifi)
3. Import `SimpleWebservice.xml` template

# Experiments

This scenario uses `StandardHttpContextMap` service. You should enable the service in the `NiFi Flow Settings`'s `Controller Services` tab.

## Sending the HTTP request

1. Start nifi data flow.
2. Sending some data to nifi with `curl`
```
$ curl -X POST -d '{"name":"test","value":1}' http://localhost:11111/item/log
```
3. Confirm 200 OK
4. Check Nifi's Data Provenance.

## Change response

1. Retry the curl and check the response body is same content of request body. 
2. Go Nifi UI and stop flow.
3. Insert `ReplaceText` processor between `funnel` and `HandleHTTPResponse` processor.
4. Configure `ReplaceText`'s `Replacement Value` property to something like ```{"name":0}```
5. Start flow.
6. Resend request. In this time, the response body should contain `Replacement Value`'s content.

## Tweak like real webservice

1. Goto Nifi UI
2. Stop the flow and open configuration panel of `HandleHTTPRequest` processor.
3. Reconfigure `Allowed Paths` using Regex e.g. ```/items/\d{6}/(logs|status)$```. This pattern matches like `/items/123456/logs` or `/items/123456/status`.
4. Start flow
5. Sending data using matched urls. Like
```
$ curl -X POST -d '{"name":"test","value":1}' http://localhost:11111/items/123456/logs
```
or
```
$ curl -X GET -d http://localhost:11111/items/123456/status
```

* Before sending `GET` method, Should enable `GET` method of `HandleHTTPRequest` processor. 

# Further more

* If we use `RouteOnAttribute` and `http.request.uri`(see [HandleHTTPRequest](http://nifi.apache.org/docs/nifi-docs/components/org.apache.nifi.processors.standard.HandleHttpRequest/index.html)'s `Writes Attributes` section.) attribute, then requests like `/items/\d{6}/logs` will route to `HandleHTTPResponse` directly but requests like `/items/\d{6}/status` route to other processor for generating item's status from RDBs. 
* The `MergeContent` processor merges content into one flowfile. It has property for size or count based merge strategy.