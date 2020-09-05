# Chapter 11: Microservices at Scale

---

## Prevent failure or deal with it gracefully?

We can also spend a bit less of our time trying to stop the inevitable, and a bit more of
our time dealing with it gracefully. I’m amazed at how many organizations put processes and controls in place to try to stop failure from occurring, but put little to no
thought into actually making it easier to recover from failure in the first place.

---

## Which 3 requirements do you need to understand to handle failure?

-  Response time/latency
-  Availability
-  Durability of data

---

## What question do you have to make about response time or latency? and why is important to ask such question?

How long should various operations take? It can be useful here to measure this
with different numbers of users to understand how increasing load will impact
the response time.

---

## When it comes to "Avaiability", what's important to consider? (3 questions)

Can you expect a service to be down? Is this considered a 24/7 service? Some
people like to look at periods of acceptable downtime when measuring availability, but how useful is this to someone calling your service? I should either be able
to rely on your service responding or not. Measuring periods of downtime is
really more useful from a historical reporting angle.

---

## When talking about Durability of data, What 2 questions are important to answer? and what's important to consider?

How much data loss is acceptable? How long should data be kept for? This is
highly likely to change on a case-by-case basis. For example, you might choose to
keep user session logs for a year or less to save space, but your financial transaction records might need to be kept for many years.

---

## When and how should you "degrade functionality" in your microservices?

What we need to do is understand the impact of each outage, and work out how to
properly degrade functionality. If the shopping cart service is unavailable, we’re probably in a lot of trouble, but we could still show the web page with the listing. Perhaps
we just hide the shopping cart or replace it with an icon saying “Be Back Soon!”

---

## What do you have to ask yourself when dealing with multiple microservices that depend on multiple downstream collaborator? (Resilience) (2)

But for every customer-facing
interface that uses multiple microservices, or every microservice that depends on
multiple downstream collaborators, you need to ask yourself, “What happens if this is
down?” and know what to do.

---

## Systems that act slow or systems that fail fast? Which is the worst?

When you get down to it, we discovered the hard way that systems
that just act slow are much harder to deal with than systems that just fail fast. In a
distributed system, latency kills.

---

## What three (3) fixes did the Sam's team implement in the ads website project where he was the technical lead?

We ended up implementing three fixes to avoid this happening again: getting our timeouts right,
implementing bulkheads to separate out different connection pools, and implementing a circuit breaker to avoid sending calls to an unhealthy system in the first place.

---

## Explain Chaos Monkey

The most famous of these programs is the Chaos Monkey, which during certain
hours of the day will turn off random machines. Knowing that this can and will happen in production means that the developers who create the systems really have to be
prepared for it.

---

## Explain the trade-offs when deciding "Timeouts" (3)

Wait too long to decide that a call has failed, and you can slow the whole system
down. Time out too quickly, and you’ll consider a call that might have worked as
failed. Have no timeouts at all, and a downstream system being down could hang
your whole system.

---

## Where do you have to put timeouts? and what do you have to do with them?

Put timeouts on all out-of-process calls, and pick a default timeout for everything.
Log when timeouts occur, look at what happens, and change them accordingly

---

## Explain "Circuit Breakers"

With a circuit breaker, after a certain number of requests to the downstream resource
have failed, the circuit breaker is blown. All further requests fail fast while the circuit
breaker is in its blown state. After a certain period of time, the client sends a few
requests through to see if the downstream service has recovered, and if it gets enough
healthy responses it resets the circuit breaker. You can see an overview of this process
in Figure 11-2.

![image](https://user-images.githubusercontent.com/1868409/92310004-12dfeb00-ef78-11ea-8a72-e2350a00e4d6.png)

---

## What do you have to do with a request when a circuit breaker is blown (open)? (synchronous and asynchronous operations)

While the circuit breaker is blown, you have some options. One is to queue up the
requests and retry them later on. For some use cases, this might be appropriate, especially if you’re carrying out some work as part of a asynchronous job. If this call is
being made as part of a synchronous call chain, however, it is probably better to fail
fast. This could mean propagating an error up the call chain, or a more subtle degrading of functionality.

---

## Explain Bulkheads

We should have used different connection pools for each downstream connection. That way, if one connection pool gets exhausted, the other connections aren’t
impacted, as we see in Figure 11-3. This would ensure that if a downstream service
started behaving slowly in the future, only that one connection pool would be impac‐
ted, allowing other calls to proceed as normal.

![image](https://user-images.githubusercontent.com/1868409/92310219-30ae4f80-ef7a-11ea-9284-10c24c66b99a.png)

---

## How can we combine circuit breakers with bulk-heads?

We can think of our circuit breakers as an automatic mechanism to seal a bulkhead,
to not only protect the consumer from the downstream problem, but also to potentially protect the downstream service from more calls that may be having an adverse impact.

---

## Why is important to keep isolation in our services?

The more one service depends on another being up, the more the health of one
impacts the ability of the other to do its job. If we can use integration techniques that
allow a downstream server to be offline, upstream services are less likely to be affected by outages, planned or unplanned
