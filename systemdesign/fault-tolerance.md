**Maintain data integrity** means:

# **Keep the data correct, consistent, valid, and trustworthy.**

In simple words:

> **Whatever is stored in the database should stay “right” and should not become broken or contradictory.**

---

# Very Simple Example

Suppose your database says:

- User bought **1 iPhone**
- Inventory still says **same stock unchanged**
- Payment says **failed**
- Order says **completed**

This is messy and contradictory.

That means:

❌ **data integrity is broken**

---

# If data integrity is maintained, then:

All related data should match properly, like:

- Payment = Success
- Order = Confirmed
- Inventory = Reduced by 1
- Invoice = Created

Now data is:

✅ **correct and consistent**

That means:

# **data integrity is maintained**

---

# One-line definition

> **Data integrity means ensuring that data remains accurate, consistent, complete, and valid throughout its lifecycle.**

---

# Simple Real-Life Example — Bank

Suppose you transfer ₹1000 from Account A to B.

Correct result should be:

- A balance decreases by ₹1000
- B balance increases by ₹1000

If only A is reduced but B is not increased:

❌ Data integrity broken

Because database now has **wrong money records**.

---

# What does “integrity” practically mean?

It means data should not become:

- wrong
- incomplete
- duplicate incorrectly
- mismatched
- invalid
- contradictory

---

# Examples of Broken Data Integrity

---

## 1) **Order exists but user does not**

Suppose:

### Orders table:

| order_id | user_id |
| -------- | ------- |
| 101      | 999     |

But in **Users table**, user 999 does not exist.

That is bad.

❌ Broken integrity

---

## 2) **Negative account balance where not allowed**

Suppose:

- balance = **-5000**
- but business rule says balance cannot go below 0

❌ Broken integrity

---

## 3) **Duplicate email in users table**

Suppose:

| id  | email                           |
| --- | ------------------------------- |
| 1   | [a@test.com](mailto:a@test.com) |
| 2   | [a@test.com](mailto:a@test.com) |

If system requires unique email:

❌ Broken integrity

---

## 4) **Inventory mismatch**

Suppose:

- Order says product sold
- Inventory says stock not reduced

❌ Broken integrity

---

# How do we maintain data integrity?

This is very important.

---

## 1) **Constraints**

Database rules that prevent invalid data.

Examples:

- **PRIMARY KEY**
- **FOREIGN KEY**
- **UNIQUE**
- **NOT NULL**
- **CHECK**

### Example:

- email must be unique
- user_id in orders must exist in users table

These help maintain integrity.

---

## 2) **Transactions (ACID)**

If multiple DB operations belong together, they should happen safely.

Example:

- deduct money
- add money

Both should succeed together.

This protects data integrity.

---

## 3) **Validation**

Before saving data, check whether it is valid.

Example:

- age should not be negative
- quantity should be > 0
- required fields should not be empty

---

## 4) **Referential Integrity**

Relationships between tables should stay correct.

Example:

- every `order.user_id` should point to a real user

This is usually enforced using **foreign keys**.

---

## 5) **Concurrency Control**

If two users update same data at the same time, system should avoid corruption.

Example:

- two people booking last seat
- only one should get it

This also protects integrity.

---

# Best interview explanation

If interviewer asks:

## “What does maintaining data integrity mean?”

You can say:

> **Maintaining data integrity means ensuring that data remains accurate, consistent, valid, and reliable across all operations, so that the database never enters an incorrect or contradictory state.**

That is a very good answer.

---

# Easy analogy 📚

Imagine a school register.

If it says:

- Student roll no. 12 is present
- but student name is missing
- marks are assigned to wrong student
- same student has two different DOBs

Then register is corrupted.

That means:

❌ integrity broken

If all entries are correct and matching:

✅ integrity maintained

---

# Data Integrity vs Data Security

These are different.

---

## Data Integrity

Means:

- data is **correct**

## Data Security

Means:

- data is **protected from unauthorized access**

### Example:

- Secure but wrong data = still bad
- Correct but hacked data = also bad

So both matter.

---

# Super Simple Memory Trick

## Data Integrity =

# **“Database should not lie.”**

That’s the easiest way to remember it 😄

---

# Final one-line summary

> **Maintaining data integrity means making sure the data in the system always stays correct, complete, valid, and consistent, even after updates, failures, or concurrent operations.**

---

If you want, I can next explain:

### **Referential Integrity vs Data Integrity**

This is another very common interview confusion topic.
