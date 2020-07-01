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

## We have this situation: The finance context wants to get some data from the "Line items table" which belongs to a different bounded contexThe monolith can get access to that table directly but how can we work around this issue using microservices?

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
