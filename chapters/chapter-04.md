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
