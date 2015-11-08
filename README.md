# kitakaze
The Ultrastructure/Northwind Mashup on Heroku

This project is still a sketch at this point, kind of an end user testing 
ground wrt [Ultrastructure as a Service](https://github.com/ChiralBehaviors/Ultrastructure/wiki).  Consequently, 
things may seem a bit more tedious than what I'm ultimately targeting.  

First, you'll need to get a [Heroku](http://heroku.com) account.  It's a bit of a thing, but it's free and 
they do have extensive documentation and a wonderful "hello world" Java application that can teach you
what you need to know before deploying Kitakaze.

Once you have a Heroku account, you can clone this repository and deploy it as a Heroku application.  There's nothing 
you should need to do other than the magic:

    git push heroku master
    
Unfortunately, the actual process we're deploying will fail to come up because the PostgreSQL database has not been 
initialized as an Ultrastructure instance.  First, shutdown the dyno:

     heroku ps:scale web=0

## Initialization

To initialize the Ultrastructure instance, we do (in your local Kitikaze directory for this project)

    heroku run bash
    
which will give you a bash shell running remotely on AWS.  In this shell, do:

    java -jar target/navi.jar bootstrap
    
You will then see the boot sequence of the Ultrastructure instance into the PostgreSQL database for your Heroku application.  
Note that this will take a lot of time if you're using the free version of Heroku.  The application keeps bumping up against memory limits and 
it doesn't run all that great on purpose - they want you to upgrade and pay them cash.  But it will eventually finish and when you do, you have an
initialized Ultrastructure instance.  
    
## Bring Your Dyno Back Up

     heroku ps:scale web=1

Your Ultrastructure instance should now be up and available for your use.

## Loading Northwind

To load the Northwind ontology, in the same bash process as above, do:

    java -jar target/navi.jar load data/northwind.1.json
    
This loads the base ontology of the Northwind/Ultrastructure mashup.

## Loading Demo Scenario Ontology

You can load the demo scenario ontology in the same bash process as above by doing:

    java -jar target/navi.jar load data/scenario.1.json
    
## Loading Demo Scenario State

Finally, you can load the snapshot state of the demo by:

    java -jar target/navi.jar load-snap data/demo-data.json
    
## Browsing the demo

At the moment, there's not a lot of exciting things you can do with this at this point.  This is entirely due to my
poor UI skills, however.  The GraphQL and JSON-LD APIs are fully functional, and using the tools available you can
produce useable Ultrastructure applications.  But currently this is quite awkward and more work than is necessary - by far.

The Customer/Order master detail view is avaiable at the URI: /northwind.html.  The GraphiQL IDE is available by default, and is 
rooted to the base kernel workspace of the Ultrastructure instance.  To see the Northwind workspace, use the URI:

    /?workspace=uri:http://ultrastructure.me/ontology/com.chiralbehaviors/demo/northwind
    
A lot more exciting than the kernel workspace, but the kernel workspace isn't too shabby I must say.  You can use the [GraphiQL IDE](https://github.com/graphql/graphiql)
to explore the system, much like an application would use the GraphQL api.