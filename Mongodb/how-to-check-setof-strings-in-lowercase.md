If you want to perform a **case-insensitive match** on the `depositAddress` field (which is a single string) against an array of input values, you can use `$toLower` with `$expr` and `$in` in an aggregation query. Here's how you can do it:

---

### Example Collection

```json
[
    { "depositAddress": "OxSDF3R" },
    { "depositAddress": "xyz987" },
    { "depositAddress": "mnop456" }
]
```

---

### Query

Let's assume your query input is an array of deposit addresses, e.g., `["oxsdf3r", "mnop456"]`. You want to match documents where the `depositAddress` (case-insensitively) is in this array.

```javascript
const inputAddresses = ["oxsdf3r", "mnop456"];

db.collection.aggregate([
    {
        $match: {
            $expr: {
                $in: [
                    { $toLower: "$depositAddress" },
                    inputAddresses.map((address) => address.toLowerCase()),
                ],
            },
        },
    },
]);
```

---

### Explanation

1. **`$toLower` on `depositAddress`**:

    - Converts the `depositAddress` field to lowercase during the comparison.

2. **Transforming `inputAddresses`**:

    - The input array is converted to lowercase using JavaScript before passing it to the query to ensure consistent comparison.

3. **`$expr` and `$in`**:
    - `$expr` allows the use of aggregation operators in the `$match` stage.
    - `$in` checks if the transformed `depositAddress` exists in the lowercase `inputAddresses` array.

---

### Output

For the given collection and input, the output will be:

```json
[{ "depositAddress": "OxSDF3R" }, { "depositAddress": "mnop456" }]
```

---

### Notes

-   If the input addresses array is large, ensure the transformation to lowercase is done efficiently before the query.
-   For better performance, consider storing a pre-lowered version of `depositAddress` in the database if case-insensitive lookups are frequent.
