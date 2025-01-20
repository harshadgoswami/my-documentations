MongoDB provides the `$toLower` operator, which is used to convert a string to lowercase in an aggregation pipeline. This operator is particularly useful for case-insensitive text comparisons, filtering, or formatting string data.

---

### Syntax of `$toLower`

```json
{
  $toLower: <expression>
}
```

-   `<expression>`: The string or field to convert to lowercase.

---

### Example 1: Convert a Field to Lowercase

Suppose you have a collection `users`:

```json
[{ "name": "Alice" }, { "name": "Bob" }, { "name": "CHARLIE" }]
```

#### Query: Add a `lowercaseName` Field

```javascript
db.users.aggregate([
    {
        $addFields: {
            lowercaseName: { $toLower: "$name" },
        },
    },
]);
```

#### Output:

```json
[
    { "name": "Alice", "lowercaseName": "alice" },
    { "name": "Bob", "lowercaseName": "bob" },
    { "name": "CHARLIE", "lowercaseName": "charlie" }
]
```

---

### Example 2: Case-Insensitive Filtering

If you want to filter documents where the `name` matches "alice" (case-insensitive):

```javascript
db.users.aggregate([
    {
        $match: {
            $expr: {
                $eq: [{ $toLower: "$name" }, "alice"],
            },
        },
    },
]);
```

#### Output:

```json
[{ "name": "Alice" }]
```

---

### Example 3: Combine `$toLower` with `$project`

To create a result that only includes the original name and its lowercase version:

```javascript
db.users.aggregate([
    {
        $project: {
            _id: 0,
            name: 1,
            lowercaseName: { $toLower: "$name" },
        },
    },
]);
```

#### Output:

```json
[
    { "name": "Alice", "lowercaseName": "alice" },
    { "name": "Bob", "lowercaseName": "bob" },
    { "name": "CHARLIE", "lowercaseName": "charlie" }
]
```

---

### Notes:

-   If the input to `$toLower` is `null`, `undefined`, or not a string, it returns `null`.
-   The operator works only on string fields. For non-string fields, you can combine `$toLower` with `$convert` or `$toString` to ensure compatibility.

---

### Use Cases:

1. **Case-Insensitive Filtering**: Normalize strings for comparison or matching.
2. **Data Formatting**: Standardize string data for consistent storage or display.
3. **Search Functionality**: Enhance search operations by converting user input and fields to lowercase.

The `$toLower` operator is simple yet powerful for text processing in MongoDB.
