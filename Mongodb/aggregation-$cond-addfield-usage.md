In MongoDB, the `$cond` operator is used to add a field with conditional logic based on a single `if-then-else` condition. It allows you to evaluate an expression (`if`) and return one of two values (`then` or `else`) based on whether the condition is true or false.

---

### Syntax of `$cond`

```json
{
  $cond: {
    if: <expression>,
    then: <value>,
    else: <value>
  }
}
```

- `if`: The condition to evaluate.
- `then`: The value to return if the condition evaluates to `true`.
- `else`: The value to return if the condition evaluates to `false`.

---

### Example: Adding a `discount` Field Based on Price

Suppose you have a collection `products`:

```json
[
  { "name": "Apple", "price": 100 },
  { "name": "Banana", "price": 50 },
  { "name": "Laptop", "price": 50000 }
]
```

#### Task: Add a `discount` field:

- If `price > 1000`, the discount is 10%.
- Otherwise, the discount is 5%.

---

### Query

```javascript
db.products.aggregate([
  {
    $addFields: {
      discount: {
        $cond: {
          if: { $gt: ["$price", 1000] }, // Condition: price > 1000
          then: 10, // Discount for expensive items
          else: 5, // Discount for affordable items
        },
      },
    },
  },
]);
```

---

### Output

```json
[
  { "name": "Apple", "price": 100, "discount": 5 },
  { "name": "Banana", "price": 50, "discount": 5 },
  { "name": "Laptop", "price": 50000, "discount": 10 }
]
```

---

### Explanation

1. **`$addFields`**:
   - Adds a new field (`discount`) to each document.
2. **`$cond`**:
   - Evaluates the condition (`price > 1000`):
     - If `true`, assigns a discount of 10.
     - If `false`, assigns a discount of 5.

---

### Use Case: Categorizing Documents

If you want to add a `category` field based on a condition (e.g., price-based categorization):

```javascript
db.products.aggregate([
  {
    $addFields: {
      category: {
        $cond: {
          if: { $gte: ["$price", 1000] }, // Condition: price >= 1000
          then: "Expensive", // Expensive category
          else: "Affordable", // Affordable category
        },
      },
    },
  },
]);
```

#### Output

```json
[
  { "name": "Apple", "price": 100, "category": "Affordable" },
  { "name": "Banana", "price": 50, "category": "Affordable" },
  { "name": "Laptop", "price": 50000, "category": "Expensive" }
]
```

---

### Key Benefits of `$cond`

- **Flexible Logic**: Implements simple `if-then-else` logic directly within the pipeline.
- **Dynamic Calculations**: Enables field value changes based on conditions.
- **Efficiency**: Reduces the need for complex code in application layers by handling logic within the database.

This operator is ideal for scenarios where a single condition determines the output field's value.
