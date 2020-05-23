# Chapter 1: Microservices

---
## What are Microservices?
Microservices are small, autonomous services that work together.

---
## What does it mean “Small, and Focused on Doing One Thing Well”? How do we set services boundaries?
Microservices take this same approach to independent services. We focus our service boundaries on business boundaries, making it obvious where code lives for a given piece of functionality. And by keeping this service focused on an explicit boundary, we avoid the temptation for it to grow too large, with all the associated difficulties that this can introduce.

---
## Which is one of the aspects that help us answer the “how small?” question?
A strong factor in helping us answer how small? is how well the service aligns to team structures. If the codebase is too big to be managed by a small team, looking to break it down is very sensible.

---
## What does it mean for a microservice to be autonomous?
Our microservice is a separate entity. It might be deployed as an isolated service on a platform as a service (PAAS), or it might be its own operating system process.

---
## Three properties of an Autonomous microservice.
- These services need to be able to change independently of each other, and be deployed by themselves without requiring consumers to change.
- We need to think about what our services should expose, and what they should allow to be hidden.
- If there is too much sharing, our consuming services become coupled to our internal representations. This decreases our autonomy, as it requires additional coordination with consumers when making changes.

---
## Key Benefit: Technology Heterogeneity?
With a system composed of multiple, collaborating services, we can decide to use different technologies inside each one. This allows us to pick the right tool for each job, rather than having to select a more standardized, one-size-fits-all approach that often ends up being the lowest common denominator.

---
## How can we use the key benefit of  “technology heterogeneity” to try new technologies?
With a system consisting of multiple services, I have multiple new places in which to try out a new piece of technology. I can pick a service that is perhaps lowest risk and use the technology there, knowing that I can limit any potential negative impact.

---
## Key benefit: Resilience?
If one component of a system fails, but that failure doesn’t cascade, you can isolate the problem and the rest of the system can carry on working. Service boundaries become your obvious bulkheads (walls). In a monolithic service, if the service fails, everything stops working.

---
## When it comes to resilience what should be careful of? Any new source of failure?
We do need to be careful, however. To ensure our microservice systems can properly embrace this improved resilience, we need to understand the new sources of failure that distributed systems have to deal with. Networks can and will fail, as will machines.

---
## Key benefit: Scaling?
With a large, monolithic service, we have to scale everything together. One small part of our overall system is constrained in performance, but if that behavior is locked up in a giant monolithic application, we have to handle scaling everything as a piece. With smaller services, we can just scale those services that need scaling, allowing us to run other parts of the system on smaller, less powerful hardware.

---
## What is one of the issues of deploying small changes with monolithic systems? (Key benefit: Ease of deployment)
 A one-line change to a million-line-long monolithic application requires the whole application to be deployed in order to release the change. That could be a large impact, high-risk deployment. In practice, large-impact, high-risk deployments end up happening infrequently due to understandable fear. Unfortunately, this means that our changes build up and build up between releases, until the new version of our application hitting production has masses of changes. And the bigger the delta between releases, the higher the risk that we’ll get something wrong!

---
## Which advantages do we have when using mircroservices for deployment?
With microservices, we can make a change to a single service and deploy it independently of the rest of the system. This allows us to get our code deployed faster. If a problem does occur, it can be isolated quickly to an individual service, making fast rollback easy to achieve. It also means we can get our new functionality out to customers faster.

---
## How do microservices help? (Key benefit: Organizational Alignment)
We know that smaller teams working on smaller codebases tend to be more productive

---
## Which opportunities do we have when using microservices? (Key benefit: Composability)
One of the key promises of distributed systems and service-oriented architectures is that we open up opportunities for reuse of functionality. With microservices, we allow for our functionality to be consumed in different ways for different purposes.

---
## List of all microservices Key Benefits
- Technology Heterogeneity
- Resilience
- Scaling
- Ease of Deployment
- Organizational Alignment
- Composability
- Optimizing for Replaceability

---
## So is the microservice strategy a silver bullet?
Of course it's not. If you’re coming from a monolithic system point of view, you’ll have to get much better at handling deployment, testing, and monitoring to unlock the benefits we’ve covered so far. You’ll also need to think differently about how you scale your systems and ensure that they are resilient. Don’t also be surprised if things like distributed transactions start giving you headaches, either!

---

