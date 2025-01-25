### `$lookup` with `$pipeline` and `$match` Example

In MongoDB, using `$pipeline` within `$lookup` allows you to add advanced filtering and transformations during the lookup operation. This is particularly useful when you want to filter the joined data before including it in the result.

---

### Example Scenario

#### Collections

1. **`users` Collection**

    ```json
    [
        { "_id": 1, "name": "Alice", "age": 30 },
        { "_id": 2, "name": "Bob", "age": 25 },
        { "_id": 3, "name": "Charlie", "age": 35 }
    ]
    ```

2. **`orders` Collection**
    ```json
    [
        { "userId": 1, "orderAmount": 100, "status": "completed" },
        { "userId": 1, "orderAmount": 200, "status": "pending" },
        { "userId": 2, "orderAmount": 50, "status": "completed" },
        { "userId": 2, "orderAmount": 150, "status": "completed" },
        { "userId": 3, "orderAmount": 300, "status": "pending" }
    ]
    ```

---

### Goal

Fetch users along with their **completed orders** only.

---

### Aggregation Query

```javascript
db.users.aggregate([
    {
        $lookup: {
            from: "orders", // Target collection to join
            let: { userId: "$_id" }, // Pass local fields to the pipeline
            pipeline: [
                {
                    $match: {
                        $expr: {
                            $and: [
                                { $eq: ["$userId", "$$userId"] }, // Match `userId` with the user's `_id`
                                { $eq: ["$status", "completed"] }, // Only include completed orders
                            ],
                        },
                    },
                },
            ],
            as: "completedOrders", // Output field for the joined data
        },
    },
]);
```

---

### Query Breakdown

1. **`$lookup`**:

    - Joins the `users` collection with the `orders` collection.
    - Uses `$pipeline` to filter `orders` during the join.

2. **`let`**:

    - Defines variables (e.g., `$$userId`) that can be accessed inside the `$pipeline`.

3. **`$pipeline`**:

    - Filters `orders` using `$match` to include only those with:
        - `userId` matching the user's `_id`.
        - `status` equal to `"completed"`.

4. **`as`**:
    - Adds the filtered orders as a new array field called `completedOrders`.

---

### Result

The output will look like this:

```json
[
    {
        "_id": 1,
        "name": "Alice",
        "age": 30,
        "completedOrders": [
            { "userId": 1, "orderAmount": 100, "status": "completed" }
        ]
    },
    {
        "_id": 2,
        "name": "Bob",
        "age": 25,
        "completedOrders": [
            { "userId": 2, "orderAmount": 50, "status": "completed" },
            { "userId": 2, "orderAmount": 150, "status": "completed" }
        ]
    },
    {
        "_id": 3,
        "name": "Charlie",
        "age": 35,
        "completedOrders": []
    }
]
```

---

### Notes

1. **Empty Arrays**: If there are no matching orders, the `completedOrders` field will be an empty array (`[]`).
2. **Flexibility**: `$pipeline` gives more control for filtering, projecting, or even sorting the joined data during the lookup.
3. **Performance**: Be cautious when using `$pipeline` in `$lookup`, especially with large datasets, as it can affect performance.

---

Let me know if you'd like further explanation or modifications!
