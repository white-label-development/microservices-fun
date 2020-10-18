# ASP.NET Core Microservices - Getting Started

GloboTicket app. forked [https://github.com/white-label-development/ps-microservices-getting-started]

+ Event Catalog
+ Shopping Basket
+ Payment

requires: Flexible scaling, deploy independently using continous delivery/ Separated teams, easy to expand. Reliable.

A distributed architecture of small services.
Each service is independent.
Multiple teams can work independently
Each service must be responisble for one task.
Frequently refactor architecture to ensure service is still independent.

Defining the boundaries and identifying the components / services is half the trick.

Request-Response is too brittle. If one response fails, the others cannot continue.

Called "service" rather than "API" as other technologies are an option (gRPC or GrapgQL instead of REST for example)

## A first microservice

(in the demo it's all in one solution. This is not how to really do it)

MVC front end to Event Catalog (RESt based interface).
The event catalog is a standard REST API (except it has a unique database)
The MVC front end is standard too. The only thing that makes it a "microservice" is the architecture. No shared libraries (ie: common models)

## Code generation using OPEN API (Swagger)

The API uses Swagger, so the client can discover the options.

NSwag for api client generation.
In VS19, right click `Connected Services` in the project and choose `Add Connected Service`. Service References > Add > OpenAPI > url

## gRPC

Much faster. Http2 + binary serialization and streaming.
But behavioral coupling: the client has to know the implementation details of the service. name, parameters, types etc. Better for non-public communications. 

see eventcatalog.proto in demo. Needs connected client defined in Client AND server.

## 3 Connecting microservices sync and async

demo adds GloboTicket.Services.ShoppingBasket so we can explore service to servce communication.
Add ShoppingBasketService in MVC proj.

Around 3.3 the issue of adding a new event comes up, namely: if we add a new event to the EventCatalog servce, the ShoppingBasket services database knows nothing about it.

The simple solution provided is that ShoppingBasket looks to see if event in a request exists, if not it requests (with it's own httpClient) event data from the EventCatalog and updates it's own database. `Event` in the EventCatalog might have many fields, while the Shopping Basket event might only have a few properties so they are not really duplicated, but contain what is relevant to the domain. Ids match ofc.



