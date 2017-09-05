+++
date = "2013-04-25T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Async and RESTSharp"
weight = 20
popular = 100
+++

    tl;tr; Examples of using Async method of RESTSharp, and comments

As I mentioned last time I was forced to use async architecture because Windows Phone require it. I was newbie in async programming but when I met RESTSharp, it clear everything, I will show you some code block with my comment for it. Basics, and how to connect to Web API check this blogpost

If we want execute GET.

var request = new RestRequest("api/location", Method.GET);
var asyncHandler = client.ExecuteAsync<List<Location>>(request, r =>
{
  if (r.ResponseStatus == ResponseStatus.Completed)
   {
     Locations = r.Data;
   }
});

We have to remember that async method is runs in brand new thread! First we have to set REST request, type here your api path, and what method, for this case it will be GET. r.Data is deserialised List of Locations.

If we want execute POST

var request = new RestRequest("api/location", Method.POST);
request.RequestFormat = DataFormat.Json;
request.AddBody(new Location
  {
     GeoX = _geoX,
     GeoY = _geoY
  });
var asyncHandler = client.ExecuteAsync(request, r =>
  {
     if (r.ResponseStatus == ResponseStatus.Completed)
           {
              MessageBox.Show("New location added");

           }
   });

Here we fill body of our REST request, we add new object which will provide information that we want to POST.

If we want execute DELETE

var request = new RestRequest("api/location/{id}", Method.DELETE);
request.AddParameter("id", idLocation);

            var asyncHandler = client.ExecuteAsync(request, r =>
            {
                if (r.ResponseStatus == ResponseStatus.Completed)
                {
                    MessageBox.Show("Location is deleted");

                }
            });

Now we have to add parameter to URL so our request for example will be "http://example.com/api/location/1", and method DELETE

If we want to update (PUT)

 RestRequest request = new RestRequest("api/location/{id}", Method.PUT);
                request.AddUrlSegment("id", idLocation);
                request.AddObject(location);

                var asyncHandler = client.ExecuteAsync<Location>(request, r =>
                {
                    if (r.ResponseStatus == ResponseStatus.Completed)
                    {
                       MessageBox.Show("Location is updated");
                    }
                });

In this case we have to provide object which will replace old one, by using of AddObject method. And finally we can execute our request.

Most of you will ask me, how works this part. If we want to execute method ExecuteAsync<T>(). As a first argument we have to provide request. Second argument is CallBack - what we want to do when data will be received, so we can put here some MessageBox, event for logger or simply bind our given data.

(request, r =>
                {
                    if (r.ResponseStatus == ResponseStatus.Completed)
                    {
                       MessageBox.Show("Location is updated");
                    }
                });

In next episode we will be checking, is there any possibility to mix REST a XNA,