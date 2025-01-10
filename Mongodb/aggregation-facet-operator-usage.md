The `$facet` stage in MongoDB's aggregation framework allows you to run multiple sub-pipelines in parallel within a single query. Each sub-pipeline produces a separate result, and the outputs are returned as fields in a single document. This is useful when you need to perform multiple analyses on the same dataset simultaneously.

### Key Points

- Each sub-pipeline works independently on the same input dataset.
- The results of all sub-pipelines are combined into a single document.
- The output is always an array with fields corresponding to the names of the sub-pipelines.

### Syntax

```javascript
{
  $facet: {
    <outputField1>: [ <stage1>, <stage2>, ... ],
    <outputField2>: [ <stage1>, <stage2>, ... ],
    ...
  }
}
```

### Simple Example

Consider a collection `products` with the following documents:

```json
[
  { "name": "Apple", "category": "Fruit", "price": 1.5 },
  { "name": "Banana", "category": "Fruit", "price": 1.0 },
  { "name": "Carrot", "category": "Vegetable", "price": 0.8 },
  { "name": "Broccoli", "category": "Vegetable", "price": 1.2 }
]
```

You want to perform two analyses on this dataset:

1. Count the total number of documents.
2. Calculate the average price of all products.

Here’s how you can use `$facet`:

```javascript
db.products.aggregate([
  {
    $facet: {
      totalProducts: [
        { $count: "count" }, // Count the total number of documents
      ],
      averagePrice: [
        { $group: { _id: null, avgPrice: { $avg: "$price" } } },
        { $project: { _id: 0, avgPrice: 1 } }, // Calculate the average price
      ],
    },
  },
]);
```

### Output

```json
[
  {
    "totalProducts": [{ "count": 4 }],
    "averagePrice": [{ "avgPrice": 1.125 }]
  }
]
```

### Explanation

1. **Input Data**: All documents in the `products` collection are passed to the `$facet` stage.
2. **Sub-Pipeline `totalProducts`**:
   - Uses `$count` to determine the total number of documents.
3. **Sub-Pipeline `averagePrice`**:
   - Uses `$group` to calculate the average price.
   - `$project` removes unnecessary fields.
4. **Combined Output**: The results of both sub-pipelines are returned as an array with separate fields.

### Use Case Example

The `$facet` stage is commonly used in:

- **Dashboard queries**: Simultaneously calculating totals, averages, or distributions.
- **Search results**: Providing search results alongside aggregations like counts or groupings.
- **Data analytics**: Performing multiple summaries in a single query.

By using `$facet`, you avoid making multiple queries for each type of analysis, improving efficiency and consistency.
