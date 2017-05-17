+++
date = "2013-03-18T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Introduction to RESTSharp"
weight = 20

+++

I would like to show you, good library to working with REST architecture. Nowadays REST is getting more popular because it's very simple API, even ASP.NET MVC support this as default API service.

RESTSharp, is powerful library for any kind of .NET technology. Especially for Windows Phone where you have to use REST or SOAP to communicate with your external data. I came across RESTSharp when I was working on Project for Imagine Cup 2013. It saves me, a lot of time. RestSharp support asynchronously actions as well as windows phone because it is the main presumption of programming on that platform. I would like to show you few code block of using RESTSharp. At the begging basics and then in the next blog post some advance methods of using library with async.

var client = new RestClient("192.168.0.1");
var request = new RestRequest("api/item/", Method.GET);
var queryResult = client.Execute<List<Items>>(request).Data;

First, we have to make a connection with server where API is located. Then we add physical path to specific API controller, and choose method of connection (GET, POST, DELETE, PUT etc.). Next step is create a query to our REST API services, our result will be list of Item, it's highly recommended to use that type deserialization, now we can easily get access just adding .Data at the end of statement.

Now let's look at Method.POST.

var client = new RestClient("http://192.168.0.1");
var request = new RestRequest("api/item/", Method.POST);
request.RequestFormat = DataFormat.Json;
request.AddBody(new Item
{
ItemName = someName,
Price = 19.99
});
client.Execute(request);

client and request stay same like in first example, but there is one change in method of connection, please set Method.POST. Next step is add body to our request create model that is expected in API controller, and finally last line, execute our request.

If we want run delete method, just use Method.DELETE

var item = new Item(){//body};
var client = new RestClient("http://192.168.0.1");
var request = new RestRequest("api/item/{id}", Method.DELETE);
request.AddParameter("id", idItem);
client.Execute(request)

Declaring client stay same like in other cases, request has small change, first you have to add path with parameter and define method of connection Method.GET. Moreover, we have to add to request one more property, define parameter {id} and what values its get. Finally execute request.