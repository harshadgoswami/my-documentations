In **Move**, `copy` is an **ability** that allows **duplication of values** — meaning you can create **multiple independent instances** of a value without moving ownership.

---

### 📋 What is the `copy` ability?

The `copy` ability in Move lets you do:

```move
let b = a;  // This creates a *copy* of `a`, if `a` has the `copy` ability.
```

If a struct (or type) has `copy`, you can duplicate it freely — **no need to `move`** it.

---

### ✅ Use Case

You can **pass** and **reuse** the same value without ownership transfer:

```move
struct MyData has copy, drop {
    val: u64
}

public fun use_data(data: MyData) {
    let a = data; // This is a copy
    let b = data; // This is another copy
}
```

> If `MyData` **didn't** have the `copy` ability, the second assignment would **fail to compile**, since Move would treat it as a move.

---

### 📌 Default Copy Types

These **primitive types** in Move already have `copy`:

* `bool`
* `u8`, `u64`, `u128`
* `address`
* Fixed-size arrays of `copy` elements (e.g., `[u8; 32]`)

---

### ❌ What Can’t Be Copied?

Any type that **represents ownership or uniqueness**, like tokens, tickets, or session keys, **should not have the `copy` ability**.

```move
struct Token has key {} // ⚠️ No `copy`

public fun fail_to_copy(t: Token) {
    let t2 = t; // ✅ okay, moved
    let t3 = t; // ❌ ERROR: already moved
}
```

---

### 🔐 Why This Matters?

Move enforces **strict control over resources**:

* If something is **copyable**, it can be **duplicated**.
* If something is **not copyable**, it must be **moved** or **explicitly passed**.
* This is how Move ensures **financial security**, **no double-spending**, and **predictable ownership**.

---

### ✅ Summary Table

| Ability | Meaning                                                         |
| ------- | --------------------------------------------------------------- |
| `copy`  | Value can be duplicated freely                                  |
| `drop`  | Value can be discarded                                          |
| `store` | Value can be stored in global storage                           |
| `key`   | Value can live in global storage as a resource under an account |

---

Let me know if you want an example using `copy` + `drop`, or how to design secure types that **avoid copy** for better safety.
