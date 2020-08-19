# Chapter 8: Monitoring

---

## What two thing do you need to monitor in your services?

-  First, we’ll want to monitor the host itself. CPU, memory, etc.
-  Secondly, we might want to monitor the application itself. At a bare minimum, monitoring the response time of the service is a good idea.

---

## What tool can we use to avoid that our logs take up all our disk space?

We may even get advanced and use logrotate to move old logs out of the way and avoid them taking up all our disk space.

---

## When it comes to monitoring our service resources (CPU, memory), what tool can we use?

We’ll want to know what they should be when things are healthy, so we can alert
when they go out of bounds. If we want to run our own monitoring software, we
could use something like Nagios to do so, or else use a hosted service like New Relic.

---

## When monitoring a single service across multiple services. What strategy and tool should we use?

So at this point, we still want to track the host-level metrics, and alert on them. But
now we want to see what they are across all hosts, as well as individual hosts. In other
words, we want to aggregate them up, and still be able to drill down. Nagios lets us
group our hosts like this—so far, so good. A similar approach will probably suffice for
our application.

---

## What tool can we use to view aggregated logs?

Kibana is an ElasticSearch-backed system for viewing logs, illustrated in Figure 8-4.
You can use a query syntax to search through logs, allowing you to do things like
restrict time and date ranges or use regular expressions to find matching strings.
Kibana can even generate graphs from the logs you send it, allowing you to see at a
glance how many errors have been generated over time, for example.

---

## What's the secrect to knowing when to paninc and when to relax?

Our website is seeing nearly 50 4XX HTTP error codes per second. Is that bad? The CPU load on the catalog service has increased by 20% since lunch; has something gone wrong? The secret to knowing when to panic and when to relax is to gather metrics about how your sys‐
tem behaves over a long-enough period of time that clear patterns emerge.

---

## What's a good way to collect metrics across multiple services? And which would it be a good tool for that?

We’ll want to be able to look at a metric aggregated for the whole
system—for example, the avergage CPU load—but we’ll also want to aggregate that
metric for all the instances of a given service, or even for a single instance of that service. That means we’ll need to be able to associate metadata with the metric to allow
us to infer this structure. Graphite is one such system that makes this very easy.

---

## What metrics are good to expose for our services?

I would strongly suggest having your services expose basic metrics themselves. At a
bare minimum, for a web service you should probably expose metrics like response
times and error rates — vital if your server isn’t fronted by a web server that is doing
this for you. But you should really go further. For example, our accounts service may
want to expose the number of times customers view their past orders, or your web
shop might want to capture how much money has been made during the last day.

---

## Why do we care about knowing which features the final user is using? (2 reasons)

Why do we care about this? Well, for a number of reasons.

-  First, there is an old adage
   that 80% of software features are never used. Now I can’t comment on how accurate
   that figure is, but as someone who has been developing software for nearly 20 years, I
   know that I have spent a lot of time on features that never actually get used. Wouldn’t
   it be nice to know what they are?
-  Second, we are getting better than ever at reacting to how our users are using our sys‐
   tem to work out how to improve it. Metrics that inform us of how our systems behave
   can only help us here. We push out a new version of the website, and find that the
   number of searches by genre has gone up significantly on the catalog service. Is that a
   problem, or expected?

---

## How do we call the fake events that we generate periodically to verify if a service is working correctly? And which technice is associated with it?

This fake event we created is an example of synthetic transaction. We used this syn‐
thetic transaction to ensure the system was behaving semantically, which is why this
technique is often called semantic monitoring.

---

## What do we need to be careful when applying semantic monitoring?

Likewise, we have to make sure we don’t accidentally trigger unforeseen side effects.
A friend told me a story about an ecommerce company that accidentally ran its tests
against its production ordering systems. It didn’t realize its mistake until a large num‐
ber of washing machines arrived at the head office.

---

## How can we trace a transaction or request that goes across multiple services?

One approach that can be useful here is to use correlation IDs. When the first call is
made, you generate a GUID for the call. This is then passed along to all subsequent
calls, and can be put into your logs in a structured way, much as
you’ll already do with components like the log level or date. With the right log aggregation tooling, you’ll then be able to trace that event all the way through your system.

---

## When taling about our development cycles, when is a good moment to start including correlation IDs?

Although it might seem like additional work up front, I
would strongly suggest you consider putting them in as soon as you can, especially if
your system will make use of event-drive architecture patterns, which can lead to
some odd emergent behavior.

---

## When talking about monitoring, why is important to have some level of standardization?

You should try to write your logs out in a standard format. You definitely want to
have all your metrics in one place, and you may want to have a list of standard names
for your metrics too; it would be very annoying for one service to have a metric called
ResponseTime, and another to have one called RspTimeSecs, when they mean the
same thing.

---

## What questions do we have to make when thinking about our monitoring metrics?

What our people want to see and react to right now is different than what they need
when drilling down. So, for the type of person who will be looking at this data, consider the following:

-  What they need to know right now
-  What they might want later
-  How they like to consume data

---

## Good book for the graphical display of quantitative information

A discussion about all the nuances involved in the graphical display of quantita‐
tive information is certainly outside the scope of this book, but a great place to start is
Stephen Few’s excellent book Information Dashboard Design: Displaying Data for Ata-Glance Monitoring (Analytics Press).

---

## Two things to consider when monitoring one service

-  Track inbound response time at a bare minimum. Once you’ve done that, follow
   with error rates and then start working on application-level metrics.
-  Track the health of all downstream responses, at a bare minimum including the
   response time of downstream calls, and at best tracking error rates.

---

## Four things to consider when monitoring the whole system

-  Ensure your metric storage tool allows for aggregation at a system or service
   level, and drill down to individual hosts.
-  Ensure your metric storage tool allows you to maintain data long enough to
   understand trends in your system.
-  Understand what requires a call to action, and structure alerting and dashboards
   accordingly.
-  Investigate the possibility of unifying how you aggregate all of your various met‐
   rics by seeing if a tool like Suro or Riemann makes sense for you.
