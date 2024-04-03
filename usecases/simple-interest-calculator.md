Here's how you can implement the interest calculator in TypeScript:

```typescript
function calculateFutureValue(
  contribution: number,
  years: number,
  annualInterestRate: number
): number {
  let total: number = 0;
  for (let year = 1; year <= years; year++) {
    const futureValue: number =
      contribution * Math.pow(1 + annualInterestRate, year);
    total += futureValue;
  }
  return total;
}

// Example usage
const contribution: number = 1000; // Contribution made at the beginning of each year
const years: number = 5; // Number of years
const annualInterestRate: number = 0.05; // 5% annual interest rate (as a decimal)

const futureValue: number = calculateFutureValue(
  contribution,
  years,
  annualInterestRate
);
console.log(`Future value after ${years} years: ${futureValue}`);
```

In this TypeScript version:

- We define a function `calculateFutureValue` that takes the contribution amount, the number of years, and the annual interest rate as inputs and returns the total future value of the investment.
- The future value for each year is calculated using the formula `contribution * Math.pow(1 + annualInterestRate, year)`.
- We accumulate the future values for each year to get the total future value after the specified number of years.

You can use this TypeScript code in your project, and it will work similarly to the Python version provided earlier.
