# Chapter 3: How to Model Services

---

## What is Loose Coupling?

When services are loosely coupled, a change to one service should not require a change to another. The whole point of a microservice is being able to make a change to one service and deploy it, without needing to change any other part of the system. This is really quite important.

---

## What sort of things cause tight coupling?

A classic mistake is to pick an integration style that tightly binds one service to another, causing changes inside the service to require a change to consumers.

---

## What is desireble about the number of calls between microservices?

This also means we probably want to limit the number of different types
of calls from one service to another, because beyond the potential performance prob‐
lem, chatty communication can lead to tight coupling.

---

## (High Cohesion) We want related behavior to sit together, and unrelated behavior to sit elsewhere. Why?

Well, if we want to change behavior, we want to be able to change it in one place, and release that change as soon as possible. If we have to change that behavior in lots of different places, we’ll have to release lots of different services (perhaps at the same time) to deliver that change. Making changes in lots of different places is slower, and deploying lots of services at once is risky—both of which we want to avoid.

---

## In the MusicCorp example. Which is the domain? And Which are the bounded contexts?

Let’s return for a moment to the MusicCorp business. Our domain is the whole busiess in which we are operating. It covers everything from the warehouse to the reception desk, from finance to ordering. We may or may not model all of that in our
software, but that is nonetheless the domain in which we are operating. Let’s think
about parts of that domain that look like the bounded contexts that Evans refers to.
At MusicCorp, our warehouse is a hive of activity—managing orders being shipped
out (and the odd return), taking delivery of new stock, having forklift truck races, and
so on. Elsewhere, the finance department is perhaps less fun-loving, but still has a
very important function inside our organization. These employees manage payroll,
keep the company accounts, and produce important reports. Lots of reports. They
probably also have interesting desk toys.

---

## Why are shared models important? (Example: Warehouse and Finance departments)

To be able to work out the valuation of the company, though, the finance employees
need information about the stock we hold. The stock item then becomes a shared
model between the two contexts. However, note that we don’t need to blindly expose
everything about the stock item from the warehouse context. For example, although
internally we keep a record on a stock item as to where it should live within the warehouse, that doesn’t need to be exposed in the shared model. So there is the internalonly representation, and the external representation we expose.

---

## We need to clearly think what models should be shared. Why?

By thinking clearly about what models should be shared, and not sharing our internal
representations, we avoid one of the potential pitfalls that can result in tight coupling
(the opposite of what we want). We have also identified a boundary within our
domain where all like-minded business capabilities should live, giving us the high
cohesion we want.

---

## Why is not a good idea to decompose your system into microservices from the beginning?

Prematurely decomposing a system into microservices can be costly, especially if you are new to the domain. In many ways, having an existing codebase you want to decompose into microservices is much easier than trying to go to microservices from the beginning.

---

## What questions do we have to make when deciding which models we should share?

So ask first “What does this context do?”, and then “So what data
does it need to do that?” When modeled as services, these capabilities become the key operations that will be exposed over the wire to other collaborators.

---

## What do we have to do when deciding the boundaries of our microservices?

When considering the boundaries of your microservices, first think in terms of the larger, coarser-grained contexts, and then subdivide along these nested contexts when you’re looking for the benefits of splitting out these seams.

---

## Nested approach or full separation approach? What should we consider to decide which one of those two?

In general, there isn’t a hard-and-fast rule as to what approach makes the most sense.
However, whether you choose the nested approach over the full separation approach
should be based on your organizational structure. If order fulfillment, inventory
management, and goods receiving are managed by different teams, they probably
deserve their status as top-level microservices. If, on the other hand, all of them are
managed by one team, then the nested model makes more sense.

---

## When modeling microservices, what is important to keep in mind when it comes to communication and how it relates to the business concepts?

The same terms and ideas that are shared between parts of your organization should be reflected in your interfaces. It can be useful to think of forms being sent between these microservices, much as forms are sent around an organization.

---

## Is it correct to model service boundaries along technical seams?

Making decisions to model service boundaries along technical seams isn’t always
wrong. I have certainly seen this make lots of sense when an organization is looking
to achieve certain performance objectives, for example. However, it should be your
secondary driver for finding these seams, not your primary one.
