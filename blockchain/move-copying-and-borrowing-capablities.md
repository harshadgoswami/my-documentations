In the **Move language**, *copying*, *moving*, and *borrowing* are **core concepts** that control how values are used, passed, and owned — especially important for writing secure smart contracts.

---

## 🔁 1. Copying

**Copying** means creating a **new value** that is **identical to an existing one**, without consuming the original.

* ✅ Allowed **only if** the type has the `copy` ability.
* 🔐 Use when data can safely be duplicated (e.g., numbers, addresses).

### ✅ Example

```move
fun test_copy(x: u64): (u64, u64) {
    let y = x; // copy happens here
    let z = x; // another copy
    (y, z)
}
```

> You **cannot copy** a resource like `Coin` or `NFT`, unless it explicitly has `copy` ability — which it usually doesn't for safety.

---

## 🔀 2. Moving

**Moving** transfers **ownership** of a value. After a move, the original **can no longer be used**.

* ✅ Used for **resources** (non-copyable, non-duplicable data).
* ⛔ Prevents double-spending, accidental reuse.

### ✅ Example

```move
struct Token has key {}

fun move_example(t: Token) {
    let t2 = t; // `t` has been moved to `t2`
    // use t; ❌ ERROR: `t` was moved
}
```

---

## 🔗 3. Borrowing

**Borrowing** lets you temporarily reference a value **without taking ownership**.

* ✅ You can borrow either:

  * **immutably** (`&T`) — read-only access
  * **mutably** (`&mut T`) — read/write access

* 🔄 Borrowing ensures **safe sharing** while enforcing **exclusive write access**.

---

### 🧊 Immutable Borrow Example:

```move
fun read_balance(balance: &u64): u64 {
    *balance // dereference and read
}
```

---

### 🔥 Mutable Borrow Example:

```move
fun increment(balance: &mut u64) {
    *balance = *balance + 1;
}
```

---

### 🔒 Borrowing Rules (like Rust):

* At any time, you can have either:

  * **multiple immutable borrows**, OR
  * **one mutable borrow**
* You **cannot** move or copy a value while it is borrowed.

---

## 📦 Summary Table

| Concept   | Transfers Ownership | Can Duplicate? | Use Case                      |
| --------- | ------------------- | -------------- | ----------------------------- |
| Copying   | ❌ No                | ✅ Yes          | Primitive types (e.g., u64)   |
| Moving    | ✅ Yes               | ❌ No           | Resources, tokens             |
| Borrowing | ❌ No                | ❌ No           | Temporary access (read/write) |

---

## 💡 When to Use What?

* Use **copy** for simple data like numbers.
* Use **move** for anything valuable or non-duplicable (tokens, NFTs).
* Use **borrow** for inspection/modification without changing ownership.

---

Let me know if you want diagrams or a real Move contract using these concepts!
