To add a field based on a switch-case condition in MongoDB, you can use the `$switch` operator in the aggregation pipeline. The `$switch` operator allows you to evaluate multiple conditions and return a result based on the first condition that matches.

---

### Syntax of `$switch`

```json
{
  $switch: {
    branches: [
      { case: <expression>, then: <value> },
      { case: <expression>, then: <value> },
      ...
    ],
    default: <value>
  }
}
```

- `branches`: An array of conditions (`case`) and their corresponding result (`then`).
- `default`: A value returned if none of the conditions are met.

---

### Example: Adding a `category` Field

Suppose you have a collection `products`:

```json
[
  { "name": "Apple", "price": 100 },
  { "name": "Banana", "price": 50 },
  { "name": "Laptop", "price": 50000 }
]
```

#### Task: Add a `category` field based on the `price`:

- If `price > 1000`, category is "Expensive".
- If `price <= 100`, category is "Cheap".
- Otherwise, category is "Moderate".

---

### Query

```javascript
db.products.aggregate([
  {
    $addFields: {
      category: {
        $switch: {
          branches: [
            { case: { $gt: ["$price", 1000] }, then: "Expensive" },
            { case: { $lte: ["$price", 100] }, then: "Cheap" },
          ],
          default: "Moderate",
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
  { "name": "Apple", "price": 100, "category": "Cheap" },
  { "name": "Banana", "price": 50, "category": "Cheap" },
  { "name": "Laptop", "price": 50000, "category": "Expensive" }
]
```

---

### Explanation

1. **`$addFields`**:
   - Adds a new field (`category`) to each document.
2. **`$switch`**:
   - Evaluates conditions in the `branches` array:
     - `price > 1000` results in "Expensive".
     - `price <= 100` results in "Cheap".
   - If no condition matches, the `default` value "Moderate" is used.

---

### Use Cases

- **Categorization**: Assigning labels or tags based on values.
- **Dynamic Field Assignment**: Setting values dynamically based on conditions.
- **Complex Calculations**: Implementing multi-condition logic in aggregation.

By using `$switch`, you can handle conditional logic cleanly and flexibly within the MongoDB aggregation pipeline.
