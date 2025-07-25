In **Move**, the **Hot Potato pattern** is a **programming pattern used to enforce that a value (often a resource) is always passed along and never accidentally discarded or stored permanently**.

---

### 🔥 What is the "Hot Potato" Pattern?

Just like in the real game of hot potato — where a player must keep passing the potato and can’t hold it — in Move, **a resource is passed from one function or account to another without being dropped, stored, or kept**.

It’s a pattern used to:

* Ensure **temporary ownership**.
* Prevent accidental **resource loss** or **storage**.
* Make sure the resource is **used or returned**.

---

### 📦 Purpose of Hot Potato Pattern

* Enforce **safe handling of resources** (e.g., tokens, session keys, tickets).
* Avoid accidental `drop` or misuse.
* Force the developer to **do something with the value** before the function ends.
* Implement **composable logic** (e.g., pipelines, middleware-style functions).

---

### 🧠 Key Concepts

* The resource does **not** have `drop`.
* The resource is **not stored** in global storage.
* The resource is always **moved** — like a hot potato — and **must be consumed** or passed.

---

### ✅ Example: Hot Potato Resource

```move
module hotpotato::Game {
    struct Potato has key {
        heat: u64,
    }

    // Create a new hot potato
    public fun create_potato(): Potato {
        Potato { heat: 100 }
    }

    // Pass the potato and reduce heat
    public fun pass_potato(potato: Potato): Potato {
        let new_heat = potato.heat - 10;
        assert!(new_heat > 0, 1); // Potato explodes if heat is 0
        Potato { heat: new_heat }
    }

    // Final consume (e.g., winner eats it)
    public fun consume_potato(potato: Potato) {
        // Use the potato. It gets destroyed here.
        // No return = final consumer
    }
}
```

---

### 🔁 Usage Flow

```move
fun play_game() {
    let mut p = Game::create_potato();
    p = Game::pass_potato(p);
    p = Game::pass_potato(p);
    Game::consume_potato(p);
}
```

Notice:

* The potato **must be moved** between functions.
* You **cannot drop it** or leave it unconsumed.
* If you try to end the function without doing anything with it, **Move will throw a compile-time error**.

---

### 🔒 Why it’s important?

Move emphasizes **resource safety**. The Hot Potato pattern:

* Makes ownership and flow **explicit**.
* Prevents bugs where someone **forgets to handle** a resource.
* Helps build **secure financial flows** (like token swaps, NFT passes, or session tokens).

---

Let me know if you want to see how this can apply to **payments**, **NFT redemption**, or **cross-contract resource flow**.
