[//]: <> (Chaper 12: Bringing It All Together)

## List the Principles of Microservices (7)

---

![image](https://user-images.githubusercontent.com/1868409/94346320-32f84c80-0002-11eb-8888-10b919c74c91.png)

===

## What can we do to maximize the autonomy that microservices make possible?

---

To maximize the autonomy that microservices make possible, we need to constantly
be looking for the chance to delegate decision making and control to the teams that
own the services themselves. This process starts with embracing self-service wherever
possible, allowing people to deploy software on demand, making development and
testing as easy as possible, and avoiding the need for separate teams to perform these
activities.

===

## If we don’t account for the fact that a downstream call can and will fail, what can happen?

---

If we don’t account for
the fact that a downstream call can and will fail, our systems might suffer catastrophic
cascading failure, and we could find ourselves with a system that is much more fragile
than before.

===

## When taking decisions as to our microservice architecture, we need to accept that we're going to get some things wrong. What are our options?

---

So, knowing we are going to get some things wrong, what are our options? Well, I would suggest
finding ways to make each decision small in scope; that way, if you get it wrong, you
only impact a small part of your system. Learn to embrace the concept of evolutionary architecture, where your system bends and flexes and changes over time as you
learn new things.
