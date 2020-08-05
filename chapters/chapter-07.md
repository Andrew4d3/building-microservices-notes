# Chapter 7: Testing

---

## How has the trend been when it comes to testing? (manual or automated)

The trend
recently has been away from any large-scale manual testing, in favor of automating as
much as possible, and I certainly agree with this approach. If you currently carry out
large amounts of manual testing, I would suggest you address that before proceeding
too far down the path of microservices, as you won’t get many of their benefits if you
are unable to validate your software quickly and efficiently

---

## When it comes to the testing pyramid, what happens when we go up?

When you’re reading the pyramid, the key thing to take away is that as you go up the
pyramid, the test scope increases, as does our confidence that the functionality being
tested works. On the other hand, the feedback cycle time increases as the tests take
longer to run, and when a test fails it can be harder to determine which functionality
has broken

---

## When it comes to the testing pyramid, what happens when we go down?

As you go down the pyramid, in general the tests become much faster, so
we get much faster feedback cycles. We find broken functionality faster, our continuous integration builds are faster, and we are less likely to move on to a new task
before finding out we have broken something. When those smaller-scoped tests fail,
we also tend to know what broke, often exactly what line of code. On the flipside, we
don’t get a lot of confidence that our system as a whole works if we’ve only tested one
line of code!

---

## Explain stubbing downstream collaborators.

When I talk about stubbing downstream collaborators, I mean that we create a stub
service that responds with canned responses to known requests from the service
under test. For example, I might tell my stub points bank that when asked for the balance of customer 123, it should return 15,000. The test doesn’t care if the stub is
called 0, 1, or 100 times.

---

## Explain mocking in comparison to stubbing

When using a mock, I actually go further and make sure the call was made. If the
expected call is not made, the test fails. Implementing this approach requires more
smarts in the fake collaborators that we create, and if overused can cause tests to
become brittle. As noted, however, a stub doesn’t care if it is called 0, 1, or many
times

---

## What's Mountebank?

You can think of Mountebank as a small software appliance that is programmable via
HTTP. The fact that it happens to be written in NodeJS is completely opaque to any
calling service. When it launches, you send it commands telling it what port to stub
on, what protocol to handle (currently TCP, HTTP, and HTTPS are supported, with
more planned), and what responses it should send when requests are sent. It also sup‐
ports setting expectations if you want to use it as a mock. You can add or remove
these stub endpoints at will, making it possible for a single Mountebank instance to
stub more than one downstream dependency.

---

## What happens when you have more moving parts in your tests?

The more moving parts, the more brittle our tests may be, and the less deterministic
they are. If you have tests that sometimes fail, but everyone just re-runs them because
they may pass again later, then you have flaky tests. It isn’t only tests covering lots of
different process that are the culprit here.

---

## According to Martin Fowlen, what should we do when we have flaky tests?

In “Eradicating Non-Determinism in Tests”, Martin Fowler advocates the approach
that if you have flaky tests, you should track them down and if you can’t immediately
fix them, remove them from the suite so you can treat them.

---

## When is it good to remove a e2e test?

When it comes to the
larger-scoped test suites, however, this is exactly what we need to be able to do. If the
same feature is covered in 20 different tests, perhaps we can get rid of half of them, as
those 20 tests take 10 minutes to run!

---

## Instead of including a e2e test for each new piece of functionality, which other approach can we take?

The best way to counter this is to focus on a small number of core journeys to test for
the whole system. Any functionality not covered in these core journeys needs to be
covered in tests that analyze services in isolation from each other. These journeys
need to be mutually agreed upon, and jointly owned. For our music shop, we might
focus on actions like ordering a CD, returning a product, or perhaps creating a new
customer—high-value interactions and very few in number.

---

## Explain CDC (Consumer-Driven Contracts)

With CDCs, we are defining the expectations of a consumer on a service (or producer). The expectations of the consumers are captured in code form as tests, which
are then run against the producer. If done right, these CDCs should be run as part of
the CI build of the producer, ensuring that it never gets deployed if it breaks one of
these contracts.

---

## So Should You Use End-to-End Tests?

You can view running end-to-end tests prior to production deployment as training
wheels. While you are learning how CDCs work, and improving your production
monitoring and deployment techniques, these end-to-end tests may form a useful
safety net, where you are trading off cycle time for decreased risk. But as you improve
those other areas, you can start to reduce your reliance on end-to-end tests to the
point where they are no longer needed.

---

## What's a smoke test suite?

A common example of this is
the smoke test suite, a collection of tests designed to be run against newly deployed
software to confirm that the deployment worked. These tests help you pick up any
local environmental issues.

---

## What's a blue/green deployment?

Another example of this is what is called blue/green deployment. With blue/green, we
have two copies of our software deployed at a time, but only one version of it is
receiving real requests.
Let’s consider a simple example, seen in Figure 7-12. In production, we have v123 of
the customer service live. We want to deploy a new version, v456. We deploy this
alongside v123, but do not direct any traffic to it. Instead, we perform some testing in
situ against the newly deployed version. Once the tests have worked, we direct the
production load to the new v456 version of the customer service. It is common to
keep the old version around for a short period of time, allowing for a fast fallback if
you detect any errors.

![image](https://user-images.githubusercontent.com/1868409/89364152-da5ea000-d69f-11ea-85c3-bb6b400676ae.png)

---

## What's canary releasing?

With canary releasing, we are verifying our newly deployed software by directing
amounts of production traffic against the system to see if it performs as expected.
“Performing as expected” can cover a number of things, both functional and non‐
functional. For example, we could check that a newly deployed service is responding
to requests within 500ms, or that we see the same proportional error rates from the
new and the old service.

---

## What do you need to decide when considering canary releasing? (about the requests)

When considering canary releasing, you need to decide if you are going to divert a
portion of production requests to the canary or just copy production load. Some
teams are able to shadow production traffic and direct it to their canary. In this way,
the existing production and canary versions can see exactly the same requests, but
only the results of the production requests are seen externally. This allows you to do a
side-by-side comparison while eliminating the chance that a failure in the canary can
be seen by a customer request.

---

## Explain the trade-offs between MTBF and MTTR?

Sometimes expending the same effort into getting better at remediation of a release
can be significantly more beneficial than adding more automated functional tests. In
the web operations world, this is often referred to as the trade-off between optimizing
for mean time between failures (MTBF) and mean time to repair (MTTR).

---
