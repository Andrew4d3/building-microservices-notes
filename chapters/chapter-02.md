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
## Things between services can get messy. Why this happens?
Between services is where things can get messy, however. If one service decides to expose REST over HTTP, another makes use of protocol buffers, and a third uses Java RMI, then integration can become a nightmare as consuming services have to under‐ stand and support multiple styles of interchange. This is why I try to stick to the guideline that we should “be worried about what happens between the boxes, and be liberal in what happens inside.”
