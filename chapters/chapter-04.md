# Chapter 4: Integration

---

## How should we keep the APIs used for communication between microservices?

It is also why I think it is very important to ensure that you keep the APIs used
for communication between microservices technology-agnostic. This means avoiding
integration technology that dictates what technology stacks we can use to implement
our microservices.

---

## What are the problems of "Database Integration" when using microservices?

Remember when we talked about the core principles behind good microservices?
Strong cohesion and loose coupling—with database integration, we lose both things.
Database integration makes it easy for services to share data, but does nothing about
sharing behavior. Our internal representation is exposed over the wire to our consumers, and it can be very difficult to avoid making breaking changes, which inevitably
leads to a fear of any change at all. Avoid at (nearly) all costs.

---

## How can we classify the types of communication we use with microservices?

-  Synchronous communication
-  Asynchronous communication
   -  Including: event-based collaboration

---

## Explain Orchestration and Choreography of microservices

With orchestration, we rely on a central brain to guide and drive the
process, much like the conductor in an orchestra. With choreography, we inform
each part of the system of its job, and let it work out the details, like dancers all finding their way and reacting to others around them in a ballet.

---

## Which is the downside of the orchestration approach?

The downside to this orchestration approach is that the customer service can become
too much of a central governing authority. It can become the hub in the middle of a
web, and a central point where logic starts to live. I have seen this approach result in a
small number of smart “god” services telling anemic CRUD-based services what to
do.

---

## How can we apply the choreography approach?

With a choreographed approach, we could instead just have the customer service
emit an event in an asynchronous manner, saying Customer created. The email service, postal service, and loyalty points bank then just subscribe to these events and
react accordingly. This approach is significantly more decoupled. If
some other service needed to reach to the creation of a customer, it just needs to subscribe to the events and do its job when needed.

---

## Which is the downside of the choreography approach?

The downside is that the explicit
view of the business process we see before is now only implicitly reflected in
our system. This means additional work is needed to ensure that you can monitor and track that
the right things have happened

---

## How can we tackle the downsides of the choreography approach?

One approach I like for dealing with this is to build a monitoring system that explicitly matches the view of the business process, but then tracks what each of the services does as independent entities, letting you see odd exceptions mapped onto the more
explicit process flow

---

## When using microservices, should we trust networks?

You need to think about the network itself. Famously, the first of the fallacies of distributed computing is “The network is reliable”. Networks aren’t reliable. They can
and will fail, even if your client and the server you are speaking to are fine. They can
fail fast, they can fail slow, and they can even malform your packets. You should
assume that your networks are plagued with malevolent entities ready to unleash
their ire on a whim.

---

## Which is the key challeng when using RPC mechanisms?

This is a key challenge with any RPC mechanism that promotes the
use of binary stub generation: you don’t get to separate client and server deployments.
If you use this technology, lock-step releases may be in your future. (This means: Tight-coupling)

---

## How should we design our remote calls when applying RPC?

Don’t abstract your remote calls to the point where the network is
completely hidden, and ensure that you can evolve the server interface without having to insist on lock-step upgrades for clients (tight-coupling). Finding the right balance for your client code is important, for example. Make sure your clients aren’t oblivious to the fact
that a network call is going to be made

---

## In REST, what does a "Resource" mean?

Most important is the concept of resources. You can think of a resource as a thing
that the service itself knows about, like a `Customer`. The server creates different representations of this `Customer` on request. How a resource is shown externally is completely decoupled from how it is stored internally. A client might ask for a JSON
representation of a `Customer`, for example, even if it is stored in a completely different
format. Once a client has a representation of this `Customer`, it can then make requests
to change it, and the server may or may not comply with them.

---

## What's the idea behind HATEOAS?

The idea behind HATEOAS is that clients should perform interactions (potentially leading to state transitions) with the server via these links (hypermedia controls) to other resources. It doesn’t need to know where exactly customers live on the server by knowing which URI to hit; instead, the client looks for and navigates links to find what it needs.

## Explain this HEATEOAS example...

```
<album>
   <name>Give Blood</name>
   <link rel="/artist" href="/artist/theBrakes" />
   <description>
      Awesome, short, brutish, funny and loud. Must buy!
   </description>
   <link rel="/instantpurchase" href="/instantPurchase/1234" />
</album>
```

-  This hypermedia control shows us where to find information about the artist.
-  And if we want to purchase the album, we now know where to go.

---

## What's one of the benefits of using HEATEOAS?

Using these controls to decouple the client and server yields significant benefits over
time that greatly offset the small increase in the time it takes to get these protocols up
and running. By following the links, the client gets to progressively discover the API,
which can be a really handy capability when we are implementing new clients.

---

## What's one of the downsides of using HEATEOAS?

One of the downsides is that this navigation of controls can be quite chatty, as the
client needs to follow links to find the operation it wants to perform.

---

## What pattern can we use when deciding how to store our data?

There is a more general problem at play here. How we decide to store our data, and
how we expose it to our consumers, can easily dominate our thinking. One pattern I
saw used effectively by one of our teams was to delay the implementation of proper
persistence for the microservice, until the interface had stabilized enough.

---

## Book for learning more about REST

Despite these disadvantages, REST over HTTP is a sensible default choice for serviceto-service interactions. If you want to know more, I recommend REST in Practice
(O’Reilly), which covers the topic of REST over HTTP in depth.

---

## How should we keep our middlewares? (message brokers, queues)

However, vendors tend to want to package lots of software with them, which can lead
to more and more smarts being pushed into the middleware, as evidenced by things
like the Enterprise Service Bus. Make sure you know what you’re getting: keep your
middleware dumb, and keep the smarts in the endpoints.

---

## Which strategy did Sam Newman use to view bad messages in the pricing system he worked on?

Aside from the bug itself, we’d failed to specify a maximum retry limit for the job on
the queue. We fixed the bug itself, and also configured a maximum retry. But we also
realized we needed a way to view, and potentially replay, these bad messages. We
ended up having to implement a message hospital (or dead letter queue), where messages got sent if they failed. We also created a UI to view those messages and retry
them if needed. These sorts of problems aren’t immediately obvious if you are only
familiar with synchronous point-to-point communication.

---

## What should we ensure to have if we end up adopting event-driven architectures?

The associated complexity with event-driven architectures and asynchronous programming in general leads me to believe that you should be cautious in how eagerly
you start adopting these ideas. Ensure you have good monitoring in place, and
strongly consider the use of correlation IDs, which allow you to trace requests across
process boundaries

---

## What does happen when you introduce shared code outside your service boundary and how does RealEstate.com.au deal with it?

If your use of shared code ever leaks outside your service boundary, you have introduced a potential form of coupling. Using common code like logging libraries is fine,
as they are internal concepts that are invisible to the outside world. RealEstate.com.au
makes use of a tailored service template to help bootstrap new service creation.
Rather than make this code shared, the company copies it for every new service to
ensure that coupling doesn’t leak in.

---

## What's the general rule of thumb when sharing code in microservices? (DRY)

My general rule of thumb: don’t violate DRY within a microservice, but be relaxed
about violating DRY across all services

---

## What's the problem of creeping logic into a client library?

The more logic that creeps into the client library, the more
cohesion starts to break down, and you find yourself having to change multiple clients to roll out fixes to your server.

---

## What's the model of AWS when using client libraries?

A model for client libraries I like is the one for Amazon Web Services (AWS). The
underlying SOAP or REST web service calls can be made directly, but everyone ends
up using just one of the various software development kits (SDKs) that exist, which
provide abstractions over the underlying API. These SDKs, though, are written by the
community or AWS people other than those who work on the API itself. This degree
of separation seems to work, and avoids some of the pitfalls of client libraries. Part of
the reason this works so well is that the client is in charge of when the upgrade happens. If you go down the path of client libraries yourself, make sure this is the case.

---

## What could happen after we retrieve a resource from a specific service? (e.g Customer resource)

When we retrieve a given Customer resource from the customer service, we get to see
what that resource looked like when we made the request. It is possible that after we
requested that Customer resource, something else has changed it. What we have in
effect is a memory of what the Customer resource once looked like. The longer we
hold on to this memory, the higher the chance that this memory will be false.

---

## In event-based collaboration. What could be valuable to have if we want to know what happend with a specific resource? (e.g Customer resource)

With events, we’re saying this happened, but we need to know what happened. If we’re
receiving updates due to a Customer resource changing, for example, it could be valuable to us to know what the Customer looked like when the event occurred. As long
as we also get a reference to the entity itself so we can look up its current state, then
we can get the best of both worlds.

---

## Which mechanisms can we used to reduce load in our services when we need to retrieve some resource information?

If we provide additional information when the resource is retrieved, letting us know at what
time the resource was in the given state and perhaps how long we can consider this
information to be fresh, then we can do a lot with caching to reduce load.

---

## What does the "Postel's law consist in?

The example of a client trying to be as flexible as possible in consuming a service
demonstrates Postel’s Law (otherwise known as the robustness principle), which states:
“Be conservative in what you do, be liberal in what you accept from others.”

---

## Explain semantic versioning. What is about?

With semantic versioning, each version number is in the form
MAJOR.MINOR.PATCH. When the MAJOR number increments, it means that backward
incompatible changes have been made. When MINOR increments, new functionality
has been added that should be backward compatible. Finally, a change to PATCH states
that bug fixes have been made to existing functionality

---

## Explain a simple use case when working with semantic versioning

Our helpdesk application is built to work against version 1.2.0 of the customer service. If a
new feature is added, causing the customer service to change to 1.3.0, our helpdesk
application should see no change in behavior and shouldn’t be expected to make any
changes. We couldn’t guarantee that we could work against version 1.1.0 of the customer service, though, as we may rely on functionality added in the 1.2.0 release. We
could also expect to have to make changes to our application if a new 2.0.0 release of
the customer service comes out.

---

## When introducing a breaking interface change, how can we use our endpoints to handle this issue?

One approach I have used successfully to handle this is to coexist both the old and new interfaces in the same running service. So if we want to release a breaking change, we deploy a new version of the service that exposes both the old and new versions of the endpoint.

---

## When coexisting different endpoint versions, things can get messy, with a lot of duplicated code, additional tests, etc. How can we work around this?

One approach I have used successfully to handle this is to coexist both the old and new interfaces in the same running service. So if we want to release a breaking change, we deploy a new version of the service that exposes both the old and new versions of the endpoint.

---

## When introducing a breaking interface change, how can we use our services to handle this issue?

Another versioning solution often cited is to have different versions of the service live
at once, and for older consumers to route their traffic to the older version, with newer
versions seeing the new one.

---

## What problems could we have when using multiple concurrent service versions?

if I need to fix an internal bug in my service, I now have to fix and deploy two different sets of services. This would probably mean I have to branch the codebase for my
service, and this is always problematic. Second, it means I need smarts to handle
directing consumers to the right microservice.

---

## Which could be a good rule when deciding which approach to take when dealing with breaking interface changes?

The longer it takes for you to get consumers upgraded to the newer version and released,
the more you should look to coexist different endpoints in the same microservice
rather than coexist entirely different versions.

---

## When it comes to our user interfaces. How should we adapt our core services?

So, although our core services —our core offering— might be the same, we need a way
to adapt them for the different constraints that exist for each type of interface.

---

## Explain UI Fragment Composition

Rather than having our UI make API calls and map everything back to UI controls,
we could have our services provide parts of the UI directly, and then just pull these
fragments in to create a UI. Imagine, for example, that the recommendation service provides a recommendation widget that is combined with other
controls or UI fragments to create an overall UI. It might get rendered as a box on a
web page along with other content.

---

## What's an API gateway?

A common solution to the problem of chatty interfaces with backend services, or the
need to vary content for different types of devices, is to have a server-side aggregation
endpoint, or API gateway. This can marshal multiple backend calls, vary and aggregate content if needed for different devices, and serve it up.

---

## What problems could we have when using unique API gateways and which pattern can we use to resolve them?

The problem that can occur is that normally we’ll have one giant layer for all our
services. This leads to everything being thrown in together, and suddenly we start to
lose isolation of our various user interfaces, limiting our ability to release them independently. A model I prefer and that I’ve seen work well is to restrict the use of these
backends to one specific user interface or application, as we see in Figure 4-10.

![image](https://user-images.githubusercontent.com/1868409/85929062-b11c4a00-b87f-11ea-9d0e-9019a71740da.png)

---

## Which dangers do we have when using BFF's?

The danger with this approach is the same as with any aggregating layer; it can take
on logic it shouldn’t. The business logic for the various capabilities these backends use
should stay in the services themselves. These BFFs should only contain behavior specific to delivering a particular user experience.

---

## When deciding the inclusion of new software... “Should I build, or should I buy?”

My clients often struggle with the question “Should I build, or should I buy?” In general, the advice I and my colleagues give when having this conversation with the average enterprise organization boils down to “Build if it is unique to what you do, and
can be considered a strategic asset; buy if your use of the tool isn’t that special.”

---

## How can we work around CMS's in our microservice systems?

The answer? Front the CMS with your own service that provides the website to the
outside world, as shown in Figure 4-11. Treat the CMS as a service whose role is to
allow for the creation and retrieval of content. In your own service, you write the
code and integrate with services how you want. You have control over scaling the
website (many commercial CMSes provide their own proprietary add-ons to handle
load), and you can pick the templating system that makes sense.

![image](https://user-images.githubusercontent.com/1868409/85929253-5388fd00-b881-11ea-99c7-5614f6c4e879.png)

---

## How can we work around CRM's in our microservice systems?

The first thing we did was identify the core concepts to our domain that the CRM
system currently owned. One of these was the concept of a project — that is, something
to which a member of staff could be assigned. Multiple other systems needed project
information. What we did was instead create a project service. This service exposed
projects as RESTful resources, and the external systems could move their integration
points over to the new, easier-to-work-with service. Internally, the project service was
just a façade, hiding the detail of the underlying integration (the CRM itself). You can see this in
Figure 4-12.

![image](https://user-images.githubusercontent.com/1868409/85929360-1bce8500-b882-11ea-966d-c705e266fbf1.png)

---

## What is the Strangler Application Pattern?

A useful pattern here is the Strangler Application Pattern.
Much like with our example of fronting the CMS system with our own code, with a
strangler you capture and intercept calls to the old system. This allows you to decide
if you route these calls to existing, legacy code, or direct them to new code you may have written. This allows you to replace functionality over time without requiring a
big bang rewrite.

---
