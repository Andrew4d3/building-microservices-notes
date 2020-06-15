# Chapter 2: The Evolutionary Architect

---

## What architects should shitft their focus away from? What should they focus on instead?

Thus, our architects need to shift their thinking away from creating the perfect end product, and instead focus on helping create a **framework** in which the right systems can emerge, and continue to grow as we learn more.

---

## Why is "town planner" a more suitable role when refering to software architects?

Erik Doernenburg first shared with me the idea that we should think of our role more as town planners than architects for the built environment. The role of the town planner should be familiar to any of you who have played SimCity before.
The way he influences how the city evolves, though, isinteresting. He does not say, “build this specific building there”; instead, he zones a city.

---

## What a "town planner" should be more worried about? (from the software architecture perspective)

Rather than worrying too much about what happens in one zone (service), the town planner will instead spend far more time working out how people and utilities (data) move from one zone to another.

---

## In software, Why are "cities" better representations than buildings?

The comparison with software should be obvious. As our users use our software, we need to react and change. We cannot foresee everything that will happen, and so rather than plan for any eventuality, we should plan to allow for change by avoiding the urge to overspecify every last thing.

---

## Communication between services can get messy. Why this happens?

Between services is where things can get messy, however. If one service decides to expose REST over HTTP, another makes use of protocol buffers, and a third uses Java RMI, then integration can become a nightmare as consuming services have to understand and support multiple styles of interchange. This is why I try to stick to the guideline that we should “be worried about what happens between the boxes, and be liberal in what happens inside.”

---

## What should we define when making decisions related to microservice archictectures?

In order to frame our decision. We need to define:

-  Strategic Goals
-  Principles
-  Practices

---

## What are Strategic goals?

The role of the architect is already daunting enough, so luckily we usually don’t have to also define strategic goals! Strategic goals should speak to where your company is going, and how it sees itself as best making its customers happy. These will be highlevel goals, and may not include technology at all. They could be defined at a company level or a division level.

---

## What are Principles?

Principles are rules you have made in order to align what you are doing to some larger goal, and will sometimes change.

---

## What's the difference between Principles and Constraints?

A constraint is really something that is very hard (or virtually impossible) to change, whereas principles are things we decide to choose (and can change over time but not so frequently)

---

## What are Practices?

Our practices are how we ensure our principles are being carried out. They are a set of detailed, practical guidance for performing tasks. They will often be technologyspecific, and should be low level enough that any developer can understand them. Practices could include coding guidelines, the fact that all log data needs to be captured centrally, or that HTTP/REST is the standard integration style. Due to their technical nature, practices will often change more often than principles.

---

## Is OK to combine Principles and Practices?

One person’s principles are another’s practices. You might decide to call the use of HTTP/REST a principle rather than a practice, for example. And that would be fine. For a small enough group, perhaps a single team, combining principles and practices might be OK. However, for larger organizations, where the technology and working practices may differ, you may want a different set of practices in different places, as long as they both map to a common set of principles.

---

## Which "clear attribute" should we define for each service?

-  Monitoring
-  Interfaces
-  Architectural Safety

---

## When it comes to Monitoring, what is important to keep in mind?

Whatever you pick, try to keep it standardized. Make the technology inside the box opaque, and don’t require that your monitoring systems change in order to support it. Logging falls into the same category here: we need it in one place.

---

## When it comes to Interfaces, what is important to keep in mind?

Picking a small number of defined interface technologies helps integrate new consumers. Having one standard is a good number. Two isn’t too bad, either. Having 20 different styles of integration is bad. This isn’t just about picking the technology and the protocol. If you pick HTTP/REST, for example, will you use verbs or nouns? How will you handle pagination of resources? How will you handle versioning of end points?

---

## When it comes to Architectural Safety, what is important to keep in mind?

Playing by the rules is important when it comes to response codes, too. If your circuit breakers rely on HTTP codes, and one service decides to send back 2XX codes for errors, or confuses 4XX codes with 5XX codes, then these safety measures can fall apart.

---

## Why "exemplars" are important to apply Principles and Practices?

But developers also like code, and code they can run and explore. If you have a set of standards or best practices you would like to encourage, then having exemplars that you can point people to is useful. The idea is that people can’t go far wrong just by imitating some of the better parts of your system. Ideally, these should be real-world services you have that get things right, rather than isolated services that are just implemented to be perfect examples.

---

## What's a Tailored Service Template? And why are they helpful?

It's a group of libraries/templates/frameworks we implement and encourage to use. In order to make sure that the principles and practices we defined are being met. By using a "Tailored Service Template", Developers can have most of the code in place, which will allow them to implement the core attributes that each service needs. This also ensures that teams can get going faster, and also that developers have to go out of their way to make their services badly behaved.

---

## When it comes to designing a service template, should it be the task of one developer/team?

Ideally, it shouldn't. You do have to be careful that creating the service template doesn’t become the job of a central tools or architecture team who dictates how things should be done, albeit via
code. Defining the practices you use should be a collective activity, so ideally your team(s) should take joint responsibility for updating this template (an internal open source approach works well here).

---

## Should a service template be optional or mandatory?

Ideally, its use should be purely optional, but if you are going to be more forceful in its adoption you need to understand that ease of use for the developers has to be a prime guiding force.

---

## What does it mean "governance through code"? and which ways do we have to ensure it?

It's a way to facilitate the correct fulfillment of our principles and practices by providing tangible examples in code. This code can either come in the form of "exemplars" or "tailored service templates".

---

## What's the governance job?

If one of the architect’s jobs is ensuring there is a technical vision, then governance is about ensuring what we are building matches this vision, and evolving the vision if needed.

---

## How can we organize Governance?

Normally, governance is a group activity. It could be an informal chat with a small enough team, or a more structured regular meeting with formal group membership for a larger scope. This is where I think the principles we covered earlier should be discussed and changed as required. This group needs to be led by a technologist, and to consist predominantly of people who are executing the work being governed. This group should also be responsible for tracking and managing technical risks.
