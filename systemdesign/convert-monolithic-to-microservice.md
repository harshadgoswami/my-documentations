Yes — there **is** a proper approach name for converting a **monolithic application** into **microservices**.

And this is a **very common system design / architecture interview topic**.

---

# Main approach name:

# **Strangler Fig Pattern**

(also called **Strangler Pattern**)

---

## Simple meaning:

Instead of rewriting the whole monolith at once, you:

- keep the monolith running
- slowly extract features one by one
- move them into microservices
- eventually replace the monolith gradually

---

# Why is it called “Strangler Fig”?

Because in nature, a **strangler fig tree** grows around an existing tree and slowly replaces it over time.

Same in software:

- new microservices grow around the old monolith
- old parts are removed gradually

---

# One-line definition

> **Strangler Fig Pattern is an incremental migration approach used to gradually transform a monolith into microservices without a full rewrite.**

---

# Why this approach is preferred?

Because full rewrite is usually a trap 😄

If you try:

> “Let’s rewrite entire monolith into microservices in 6 months”

That often becomes:

- expensive
- risky
- slow
- bug-prone
- never-ending

So instead:

## Migrate gradually.

That is much safer.

---

# How it works (simple flow)

Suppose your monolith has:

- Auth
- Orders
- Payments
- Notifications
- Inventory

Initially everything is inside one app.

---

## Step 1:

Keep monolith as it is

```text id="2k6s7f"
Users -> Monolith
```

---

## Step 2:

Extract one bounded feature first

Example:
Take **Notifications** out first

```text id="g6d8we"
Users -> Monolith -> Notification Service
```

Now notification logic is outside.

---

## Step 3:

Route that feature to microservice

Example:

- monolith no longer sends emails directly
- it calls Notification Service

---

## Step 4:

Repeat feature by feature

Then extract:

- Payments
- Orders
- Inventory

Over time:

```text id="9d2lzf"
Users -> API Gateway -> Auth Service
                     -> Order Service
                     -> Payment Service
                     -> Notification Service
                     -> Inventory Service
```

Monolith becomes smaller and smaller.

Eventually maybe only legacy parts remain.

That is **Strangler Pattern**.

---

# Best migration order (very important)

Do **not** start with the most critical part first.

Bad idea:
❌ Extract Payments first if architecture is unstable

Better:
✅ Start with low-risk modules

---

## Good candidates to extract first:

- Notifications
- Search
- Reporting
- Recommendation
- File Uploads
- Audit Logs

Because these are often:

- loosely coupled
- easier to separate
- lower blast radius

---

# Harder modules to extract later:

- Auth
- Orders
- Payments
- Inventory
- Billing

Because these usually have:

- strong DB dependencies
- transactional complexity
- many integrations

---

# Important supporting concepts

To do this properly, you usually use:

---

## 1) **Domain-Driven Design (DDD)**

Helps identify **bounded contexts**.

Meaning:
Break system by **business capability**, not by technical layer.

### Good split:

- User Service
- Order Service
- Payment Service

### Bad split:

- Controller Service
- Utils Service
- DB Service 😅

So DDD helps decide:
**what should become a microservice**

---

## 2) **Anti-Corruption Layer (ACL)**

Very useful during migration.

This means:
Create a translation layer between old monolith and new services.

Why?
Because old system data/models may be messy.

ACL prevents the new microservice from becoming infected by bad legacy design.

---

## 3) **API Gateway / Routing Layer**

During strangler migration, traffic often needs to be routed like this:

- `/orders` → monolith
- `/notifications` → microservice
- `/payments` → microservice

So gateway/proxy helps send requests to the correct place.

Example tools:

- NGINX
- Kong
- API Gateway
- Envoy

---

# Real-world Example

Suppose you have an **e-commerce monolith**.

Inside one app:

- Login
- Product catalog
- Cart
- Orders
- Payments
- Email notifications

You decide to migrate.

---

## Phase 1

Extract **Notification Service**

- email sending
- SMS sending
- push notifications

This is easy and low-risk.

---

## Phase 2

Extract **Catalog Service**

- products
- categories
- search

---

## Phase 3

Extract **Order Service**

---

## Phase 4

Extract **Payment Service**

---

## Final result

Monolith slowly shrinks.

This is exactly how many real companies do it.

---

# Common migration approaches used together

Strangler Pattern is the **main name**, but these are often used with it:

---

## 1) **Incremental Migration**

General strategy:
Move piece by piece, not all at once.

👉 Strangler Pattern is a form of incremental migration.

---

## 2) **Branch by Abstraction**

Very useful engineering technique.

You insert an abstraction layer first, then switch implementation gradually.

### Example:

Instead of directly calling monolith payment code:

```js
paymentService.processPayment();
```

Then behind the scenes:

- old implementation = monolith
- new implementation = microservice

You can switch safely later.

This is a great migration technique.

---

## 3) **Database Decomposition**

One of the hardest parts.

In monolith:

- often one big shared DB

In microservices:

- each service should ideally own its own data

So you gradually split DB ownership.

This is difficult and must be done carefully.

---

# Very Important Warning ⚠️

## Don’t start by only splitting code.

You must split by:

# **Business capability / bounded context**

Otherwise you create:

> **Distributed Monolith**

And that is worse than a monolith 😄

---

# What is Distributed Monolith?

When you have many services, but they are still:

- tightly coupled
- dependent on each other
- sharing same DB
- requiring synchronous calls everywhere

Then you didn’t really fix the architecture.

You just made it harder.

---

# Interview-ready answer

If interviewer asks:

## “What approach is used to migrate from monolith to microservices?”

You can say:

> The most common approach is the **Strangler Fig Pattern**, where we gradually extract modules from the monolith into independent microservices instead of rewriting everything at once. This reduces migration risk, allows incremental delivery, and helps validate service boundaries over time.

That is a strong answer.

---

# Better interview answer (advanced)

> I would use the **Strangler Fig Pattern** combined with **DDD for identifying bounded contexts**, **API gateway-based routing**, and possibly **Branch by Abstraction** to gradually replace monolithic functionality with microservices while minimizing risk and avoiding a big-bang rewrite.

That sounds very solid in interviews.

---

# One-line memory trick

## Monolith → Microservices

# = **Strangler Pattern**

---

# Super Short Summary

| Concept                    | Meaning                                       |
| -------------------------- | --------------------------------------------- |
| **Strangler Fig Pattern**  | Gradually replace monolith with microservices |
| **DDD**                    | Identify service boundaries                   |
| **Branch by Abstraction**  | Switch implementation safely                  |
| **ACL**                    | Protect new service from legacy mess          |
| **Database Decomposition** | Split shared DB carefully                     |

---

If you want, I can also give you:

### **Monolith to Microservices migration diagram (Mermaid)**

or

### **Real interview answer: “How would you migrate an e-commerce monolith?”**

That would be very useful for your prep.
