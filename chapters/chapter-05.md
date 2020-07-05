# Chapter 5: Splitting the Monolith

---

## What are seams and why are they important to identify?

In his book Working Eectively with Legacy Code (Prentice-Hall), Michael Feathers
defines the concept of a seam — that is, a portion of the code that can be treated in
isolation and worked on without impacting the rest of the codebase. We also want to
identify seams. But rather than finding them for the purpose of cleaning up our codebase, we want to identify seams that can become service boundaries.

---

## Which are the two main steps that we need to take when breaking apart a Monolith?

Imagine we have a large backend monolithic service that represents a substantial
amount of the behavior of MusicCorp’s online systems. To start, we should identify
the high-level bounded contexts that we think exist in our organization, as we discussed in Chapter 3. Then we want to try to understand what bounded contexts the
monolith maps to.

---

## What tool can we use to identify some database-level constraints?

This doesn’t give us the whole story, however. For example, we may be able to tell that
the finance code uses the ledger table, and that the catalog code uses the line item
table, but it might not be clear that the database enforces a foreign key relationship
from the ledger table to the line item table. To see these database-level constraints,
which may be a stumbling block, we need to use another tool to visualize the data. A
great place to start is to use a tool like the freely available SchemaSpy, which can generate graphical representations of the relationships between tables.

---

## We have this situation: The finance context wants to get some data from the "Line items table" which belongs to a different bounded contex The monolith can get access to that table directly but how can we work around this issue using microservices?

![image](https://user-images.githubusercontent.com/1868409/86134257-64bb4f00-bab7-11ea-983a-9565f67f51d1.png)

So how do we fix things here? Well, we need to make a change in two places. First, we
need to stop the finance code from reaching into the line item table, as this table
really belongs to the catalog code, and we don’t want database integration happening
once catalog and finance are services in their own rights. The quickest way to address
this is rather than having the code in finance reach into the line item table, we’ll
expose the data via an API call in the catalog package that the finance code can call.
This API call will be the forerunner of a call we will make over the wire, as we see in
Figure 5-3.

![image](https://user-images.githubusercontent.com/1868409/86134441-9fbd8280-bab7-11ea-8a0f-5d49571d913a.png)

---

## How can we share static data (e.g country codes) using microservices? (Hint: There are three ways)

-  Well, we have a few options. One is to duplicate this table for each of our packages, with the long-term view that it will be duplicated within each service also. (Downside: updating multiple tables when new static data is avalable)
-  A second option is to instead treat this shared, static data as code. Perhaps it could be in a property file deployed as part of the service, or perhaps just as an enumeration. (Downside: same as before, but at least updating code has shown to be easier than updating a DB table/collection)
-  A third option, which may well be extreme, is to push this static data into a service of its own right. (Downside: Overkill solution when the amount of static data is not that big and not that complex)

---

## We have the following situation: the finance and the warehouse code are writing to, and probably occasionally reading from, the same table. How can we tease this apart?

![image](https://user-images.githubusercontent.com/1868409/86135819-4d7d6100-bab9-11ea-831b-96113067b387.png)

We need to make the current abstract concept of the customer concrete. As a transient step, we create a new package called Customer. We can then use an API to expose
Customer code to other packages, such as finance or warehouse. Rolling this all the way forward, we may now end up with a distinct customer service (Figure 5-6).

![image](https://user-images.githubusercontent.com/1868409/86136008-8a495800-bab9-11ea-9ec7-f3ba7125c2c0.png)

---

## We have the following situation: Our catalog needs to store the name and price of the records we sell, and the warehouse needs to keep an electronic record of inventory. We decide to keep these two things in the same place in a generic line item table. How can we break this down?

![image](https://user-images.githubusercontent.com/1868409/86195684-7df7e600-bb1f-11ea-87b5-8847e4ac01f5.png)

The answer here is to split the table in two as we have in Figure 5-8, perhaps creating
a stock list table for the warehouse, and a catalog entry table for the catalog details.

![image](https://user-images.githubusercontent.com/1868409/86195790-ac75c100-bb1f-11ea-92ef-2b52e7d8593a.png)

---

## What's the best way to split our schemas so that we can avoid doing a "big-bang release"?

What next? Do you do a big-bang release, going from one monolithic service with a single schema to two services, each with its own schema? I would actually
recommend that you split out the schema but keep the service together before splitting the application code out into separate microservices, as shown in Figure 5-9.

![image](https://user-images.githubusercontent.com/1868409/86196115-856bbf00-bb20-11ea-939a-aa8a6b935646.png)

---

## Which benefits do we get by spliting our schemas first (staging the break)?

By splitting the schemas out but keeping the application code together, we give ourselves the ability to revert our changes or continue to
tweak things without impacting any consumers of our service. Once we are satisfied
that the DB separation makes sense, we can then think about splitting out the application code into two services.

---

## What's eventual consistency?

In many ways, this is another form of what is called eventual consistency. Rather than
using a transactional boundary to ensure that the system is in a consistent state when
the transaction completes, instead we accept that the system will get itself into a consistent state at some point in the future. This approach is especially useful with business operations that might be long-lived.

---

## What's the "two-phase commit" algorithmn about?

The most common algorithm for handling distributed transactions — especially shortlived transactions, as in the case of handling our customer order — is to use a two phase commit. With a two-phase commit, first comes the voting phase. This is where
each participant (also called a cohort in this context) in the distributed transaction
tells the transaction manager whether it thinks its local transaction can go ahead. If
the transaction manager gets a yes vote from all participants, then it tells them all to
go ahead and perform their commits. A single no vote is enough for the transaction
manager to send out a rollback to all parties.

---

## What's one the downsides of using the "two-phase commit" algorithmn?

This approach relies on all parties halting until the central coordinating process tells
them to proceed. This means we are vulnerable to outages. If the transaction manager
goes down, the pending transactions never complete. If a cohort fails to respond during voting, everything blocks. And there is also the case of what happens if a commit
fails after voting.

---

## Should we implement our own algorithmn for distributed transactions?

I’d suggest you avoid trying to create your own.
Instead, do lots of research on this topic if this seems like the route you want to take,
and see if you can use an existing implementation.

---

## How do we do when we encounter a state that we really want to be kept consistent?

If you do encounter state that really, really wants to be kept consistent, do everything
you can to avoid splitting it up in the first place. Try really hard. If you really need to
go ahead with the split, think about moving from a purely technical view of the process (e.g, a database transaction) and actually create a concrete concept to represent
the transaction itself.

---

## (Reporting) What is the "Data Retrieval via Service Calls" strategy about?

There are many variants of this model, but they all rely on pulling the required data
from the source systems via API calls. For a very simple reporting system, like a dashboard that might just want to show the number of orders placed in the last 15
minutes, this might be fine. To report across data from two or more systems, you
need to make multiple calls to assemble this data.

---

## When processing big volumes of data. Which approach can we use to avoid returning a huge data response?

For example, the customer service might expose something like a
`BatchCustomerExport` resource endpoint. The calling system would POST a
`BatchRequest`, perhaps passing in a location where a file can be placed with all the
data. The customer service would return an HTTP 202 response code, indicating that
the request was accepted but has not yet been processed. The calling system could
then poll the resource waiting until it retrieves a 201 Created status, indicating that
the request has been fulfilled, and then the calling system could go and fetch the data.
This would allow potentially large data files to be exported without the overhead of
being sent over HTTP; instead, the system could simply save a CSV file to a shared
location.

---

## (Reporting) What is the "Data Pump" strategy about?

An alternative option is to have a standalone program that directly accesses
the database of the service that is the source of data, and pumps it into a reporting
database, as shown in Figure 5-13.

![image](https://user-images.githubusercontent.com/1868409/86518917-58aaf680-be03-11ea-8a98-9cade67ad130.png)

---

## How can we mitigate the tight-coupling and low-cohesion downsides that comes with using the "Data pump" strategy?

To start with, the data pump should be built and managed by the same team that
manages the service. We try to reduce the problems with coupling to the service’s
schema by having the same team that manages the service also manage the pump. I
would suggest, in fact, that you version-control these together, and have builds of the
data pump created as an additional artifact as part of the build of the service itself,
with the assumption that whenever you deploy one of them, you deploy them both.

---

## (Reporting) What is the "Event Data Pump" strategy about?

In Chapter 4, we touched on the idea of microservices emitting events based on the
state change of entities that they manage. For example, our customer service may
emit an event when a given customer is created, or updated, or deleted. For those
microservices that expose such event feeds, we have the option of writing our own
event subscriber that pumps data into the reporting database, as shown in
Figure 5-15.

![image](https://user-images.githubusercontent.com/1868409/86519011-79278080-be04-11ea-8ac8-00d32ed53347.png)

---

## What's the main downside of using the "Event Data Pump" strategy?

The main downsides to this approach are that all the required information must be
broadcast as events, and it may not scale as well as a data pump for larger volumes of
data that has the benefit of operating directly at the database level. Nonetheless, the
looser coupling and fresher data available via such an approach makes it strongly
worth considering if you are already exposing the appropriate events.

---

## What solution does Netflix implement using "Backup Data Pumps"?

In the end, Netflix ended up implementing a pipeline capable of processing large amounts of data using this approach, which it then open sourced as the
**Aegisthus project**.

---

## Which technique can we use to mitigate the cost of change asossiated with spliting a monolith?

A great technique here is to adapt an approach more typically taught for the design of
object-oriented systems: class-responsibility-collaboration (CRC) cards. With CRC
cards, you write on one index card the name of the class, what its responsibilities are,
and who it collaborates with. When working through a proposed design, for each service I list its responsibilities in terms of the capabilities it provides, with the collaboators specified in the diagram. As you work through more use cases, you start to get
a sense as to whether all of this hangs together properly.

---

## Part of the problem is knowing where to start, and I’m hoping this chapter has helped. But another challenge is the cost associated with splitting out services. Finding somewhere to run the service, spinning up a new service stack, and so on, are non‐trivial tasks. So how do we address this?

Well, if doing something is right but difficult,
we should strive to make things easier. Investment in libraries and lightweight service
frameworks can reduce the cost associated with creating the new service. Giving people access to self-service provision virtual machines or even making a platform as a
service (PaaS) available will make it easier to provision systems and test them.
