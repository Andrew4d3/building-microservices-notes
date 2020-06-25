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
