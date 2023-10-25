To get the month-wise purchase amount from a "transaction" collection in MongoDB, you can use the aggregation framework to group and sum the transactions by month. Assuming your "transaction" collection has a date field, here's an example of how you can achieve this:

```javascript
db.transactions.aggregate([
  {
    $project: {
      year: { $year: "$dateField" },
      month: { $month: "$dateField" },
      amount: 1, // Assuming there is an "amount" field in your documents
    },
  },
  {
    $group: {
      _id: { year: "$year", month: "$month" },
      totalAmount: { $sum: "$amount" },
    },
  },
  {
    $project: {
      _id: 0, // Exclude the "_id" field
      year: "$_id.year",
      month: "$_id.month",
      totalAmount: 1,
    },
  },
  {
    $sort: { year: 1, month: 1 }, // Optionally, sort the results by year and month
  },
]);
```

Here's what each stage of the aggregation pipeline does:

1. `$project`: Extracts the year and month components from the "dateField" and keeps the "amount" field. You should replace "dateField" with the name of the date field in your documents and adjust the "amount" field as needed.

2. `$group`: Groups the documents by year and month, calculating the total purchase amount within each group.

3. `$project`: Renames the fields and excludes the "\_id" field to format the output document.

4. `$sort`: Optionally, sorts the results by year and month to have a chronological order.

This aggregation pipeline will give you the total purchase amount for each month in your "transaction" collection. Be sure to replace "dateField" and "amount" with the actual field names from your documents.
