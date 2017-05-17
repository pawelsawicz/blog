+++
date = "2013-07-07T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "MongoDB and Web API, power of NoSQL philosophy"
weight = 20

+++

Few days ago I had to create very simple database, and there was a need to expose API, so first thought was no RDBM! (because there wasn't any relation). @mfranc has showed me this powerful diagram, where you can easily decide which database engine will be the best for your solution. In my case I had choose MongoDB, it's document-oriented database, tree of MongoDB is quite similar to RDBM, engine->database->collections->documents (so each document represent record of our data).

Now let's go to see some code, first of all you have to download MongoDB, and then install it. If you did it, now we can run our VS, and select new Web API project.

First of all you need to add Package of MongoDB C# Driver.mongodb_package

Now we can write some code, so first we need model of our data. Speaker.cs, this class represents model of speaker.

        [ScaffoldColumn(false)]
        [BsonId]
        public ObjectId SpeakerId { get; set; }

        public string FullName { get; set; }        

        public string Description { get; set; }

        public string TwitterAccount { get; set; }

        public IList<Presentation> Presentations { get; set; }

And here is example data inserted to this model, as you can see ILIst<Presentation> create us a list of presentation. Moreover this record is one document inside of speaker collection.

mongodb_speaker_tree

Presentation.cs model of presentation.

        [ScaffoldColumn(false)]
        [BsonId]
        public ObjectId PresentationId { get; set; }

        public string Name { get; set; }

        public string Description { get; set; }

        public string Classroom { get; set; }

        public string Time { get; set; }        

        public string FullNameOfPresenter { get; set; }

Example data based on this model,

mongodb_presentation_tree

You have to know that Presentation Collection isn't member of Speaker IList<Presentation> data! This isn't a relation!

After these steps, now we can create our api controllers, Controllers->Add->New Item->Web API Controller, PresentationController.cs

        private readonly MongoHelper<Presentation> _presentation;

        public PresentationController()
        {
            _presentation = new MongoHelper<Presentation>();
        }

        public List<Presentation> Get()
        {
            var collection = _presentation.Collection.FindAll().ToList();

            if (collection == null)
            {
                throw new HttpResponseException(Request.CreateResponse(HttpStatusCode.NotFound));
            }
            else
            {
                return collection;
            }
        }

        public Presentation Get(string classColor)
        {
            var collection = _presentation.Collection.Find(Query.EQ("Classroom", classColor)).Single();

            if (collection == null)
            {
                throw new HttpResponseException(Request.CreateResponse(HttpStatusCode.NotFound));
            }
            else
            {
                return collection;
            }
        }

As you can see there is only implemented Get Method because in my case I need only this, MongoDB queries are somehow similar to ORM's, just look at this method where you want to get only one specific Presentation, you have to use Query.EQ - In this case every record are tested, are they equal to specific value. Also I use .Single() to cut off collection to single record.

SpeakerController.cs, Here is a dynamic method to show you that you can form your api as you wish

 private readonly MongoHelper<Speaker> _speaker;

        public SpeakerController()
        {
            _speaker = new MongoHelper<Speaker>();   
        }

        // GET api/<controller>
        public dynamic Get()
        {

            var listOfSpeakers = _speaker.Collection.FindAll().Select(i => new
            {
                FullName = i.FullName,
                Description = i.Description,
                TwitterAccount = i.TwitterAccount,
                Presentations = i.Presentations
            });

            if (listOfSpeakers == null)
            {
                throw new HttpResponseException(Request.CreateResponse(HttpStatusCode.NotFound));
            }
            else
            {
                return listOfSpeakers;
            }
        }

        public dynamic Get(string name)
        {
            var speaker = _speaker.Collection.Find(Query.EQ("FullName", name)).Select(i => new
            {
                FullName = i.FullName,
                Description = i.Description,
                TwitterAccount = i.TwitterAccount,
                Presentations = i.Presentations
            });

            if (speaker == null)
            {
                throw new HttpResponseException(Request.CreateResponse(HttpStatusCode.NotFound));
            }
            else
            {
                return speaker;
            }
        }

Conclusion : NoSQL - not only sql, it is philosphy to not use only sql in your solutions, in this case using MySQL or MSSQL will overpower my project, moreover Mongodb has better time r/w than typical RDBM. I recommend hereby to use as simple as possible solution, if there is no need of using specific product. At least look at this side, how fast you create and expose this api for me it was a 15-20 minutes.

Comments: When you are creating MongoDB model, it should represent a specific view of our data, it’s very helpful and required. In this example there is no need to add relation at this two models, Speaker.cs and Presentation.cs.