# Chapter 10: Conway’s Law and System Design

---

## How does S. Raymond sumarize the Conway's law?

This statement is often quoted, in various forms, as Conway’s law. Eric S. Raymond
summarized this phenomenon in The New Hacker’s Dictionary (MIT Press) by stating
“If you have four groups working on a compiler, you’ll get a 4-pass compiler.”

---

## What rules does Amazon use to manage the size of its teans?

... It wanted teams to own and operate the systems they looked after, managing the entire
lifecycle. But Amazon also knew that small teams can work faster than large teams.
This led famously to its two-pizza teams, where no team should be so big that it could
not be fed with two pizzas.

---

## What does happen when the cost of coordinating change increases?

When the cost of coordinating change increases, one of two things happen. Either
people find ways to reduce the coordination/communication costs, or they stop making changes. The latter is exactly how we end up with large, hard-to-maintain
codebases.

---

## When it comes to defining teams based on their geographic location, what's a good advice?

So where does this leave us when considering evolving our own service design? Well,
I would suggest that geographical boundaries between people involved with the
development of a system can be a great way to drive when services should be decomposed, and that in general, you should look to assign ownership of a service to a single, colocated team who can keep the cost of change low.

---

## Explain having a core service as an internal open source project. What would the responsability of the core committers be(the ones with the ownership)?

With normal open source, a small group of people are considered core committers.
They are the custodians of the code. If you want a change to an open source project,
you either ask one of the committers to make the change for you, or else you make
the change yourself and send them a pull request. The core committers are still in
charge of the codebase; they are the owners.

---

## What would the process of vetting a approving changes be for the core team?

The core team needs to have some way of vetting and approving the changes. It needs
to make sure the changes are idiomatically consistent—that is, that they follow the
general coding guidelines of the rest of the codebase. The people doing the vetting are
therefore going to have to spend time working with the submitters to make sure the
change is of sufficient quality.

---

## When working with open source projects, when is it good to take submissions and when it's not?

Most open source projects tend to not take submissions from a wider group of
untrusted committers until the core of the first version is done. Following a similar
model for your own organizations makes sense. If a service is pretty mature, and is
rarely changed—for example, our cart service—then perhaps that is the time to open
it up for other contributions.

---

## What benefits do we get by drawing our service boundaries around our bounded contexts? (3)

This has multiple benefits. First, a team will find it easier to grasp domain
concepts within a bounded context, as they are interrelated. Second, services within a
bounded context are more likely to be services that talk to each other, making system
design and release coordination easier. Finally, in terms of how the delivery team
interacts with the business stakeholders, it becomes easier for the team to create good
relationships with the one or two experts in that area.

---

## What about those services that are not changed frequently, how can we work around those when our team structures are aligned along the bounded contexts?

If your team structures are aligned along the bounded contexts of your organization,
then even services that are not changed frequently still have a de facto owner. Imagine
a team that is aligned with the consumer web sales context. It might handle the website, cart, and recommendation services. Even if the cart service hasn’t been changed
in months, it would naturally fall to this team to make the change.

---

## Explain the "Line of Business" implemented by RealState.au

Each squad inside a line of business is expected to own the entire lifecycle of the serv‐
ices it creates, including building, testing and releasing, supporting, and even decom‐
missioning. A core delivery services team provides advice and guidance to these
teams, as well as tooling to help it get the job done.

---

## Coming up with a vision for how things should be done without considering how your current staff will feel about this or without considering what capabilities they have is likely to lead to a bad place. How can we adress this issue?

Each organization has its own set of dynamics around this topic. Understand your
staff ’s appetite to change. Don’t push them too fast! Maybe you still have a separate
team handle frontline support or deployment for a short period of time, giving your
developers time to adjust to other new practices.

---

## In summary, How should we align our teams?

This leads us to trying to align service ownership to colocated teams, which themselves are aligned around the same bounded contexts of the
organization.
