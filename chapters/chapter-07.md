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
