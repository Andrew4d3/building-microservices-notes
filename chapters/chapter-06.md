# Chapter 6: Deployment

---

## What's the core goal of CI?

With CI, the core goal is to keep everyone in sync with each other, which we achieve
by making sure that newly checked-in code properly integrates with existing code. To
do this, a CI server detects that the code has been committed, checks it out, and carries out some verification like making sure the code compiles and that tests pass.

---

## What benefits do we get with CI? (Hint: there are 3)

CI has a number of benefits:

-  We get some level of fast feedback as to the quality of our code.
-  It allows us to automate the creation of our binary artifacts. All the code
   required to build the artifact is itself version controlled, so we can re-create the artifact if needed.
-  We also get some level of traceability from a deployed artifact back to
   the code, and depending on the capabilities of the CI tool itself, can see what tests
   were run on the code and artifact too

---

## What questions do we have to ask ourserlf to see if we uderstand CI?

-  Do you check in to mainline once per day?
-  Do you have a suite of tests to validate your changes?
-  When the build is broken, is it the #1 priority of the team to fix it?

---

## According to Jez Humble and Dave Farley’s book. What's Continuos Delivery (CD)?

Continuous delivery (CD) builds on this concept, and then some. As outlined in Jez
Humble and Dave Farley’s book of the same name, continuous delivery is the
approach whereby we get constant feedback on the production readiness of each and
every check-in, and furthermore treat each and every check-in as a release candidate.

---

## What's "configuration drift"?

By storing all our configuration in source control, we are trying to ensure that we can
automatically reproduce services and hopefully entire environments at will. But once
we run our deployment process, what happens if someone comes along, logs into the
box, and changes things independently of what is in source control? This problem is
often called conguration drift — the code in source control no longer reflects the configuration of the running host.

---

## How does PaaS work?

When using a platform as a service (PaaS), you are working at a higher-level abstraction than at a single host. Most of these platforms rely on taking a technology-specific
artifact, such as a Java WAR file or Ruby gem, and automatically provisioning and
running it for you. Some of these platforms will transparently attempt to handle scaling the system up and down for you, although a more common (and in my experience less error-prone) way will allow you some control over how many nodes your
service might run on, but it handles the rest.

---

## Disavantages of using PaaS solutions

When PaaS solutions work well, they work very well indeed. However, when they
don’t quite work for you, you often don’t have much control in terms of getting under
the hood to fix things. This is part of the trade-off you make. I would say that in my
experience the smarter the PaaS solutions try to be, the more they go wrong. I’ve used
more than one PaaS that attempts to autoscale based on application use, but does it
badly.

---

## Why slicing up a phisical machine into multiple VM's might not be a good idea?

Well, for some people, you can. However, slicing up the machine into ever increasing
VMs isn’t free. Think of our physical machine as a sock drawer. If we put lots of
wooden dividers into our drawer, can we store more socks or fewer? The answer is
fewer: the dividers themselves take up room too! Our drawer might be easier to deal
with and organize, and perhaps we could decide to put T-shirts in one of the spaces
now rather than just socks, but more dividers means less overall space.

---
