If you want to calculate compound interest annually, you can use the formula:

\[
A = P \times \left(1 + \frac{r}{n}\right)^{n \times t}
\]

Where:

- \(A\) is the future value of the investment/loan, including interest.
- \(P\) is the principal investment amount (the initial deposit or loan amount).
- \(r\) is the annual interest rate (in decimal).
- \(n\) is the number of times that interest is compounded per unit of time (in this case, annually).
- \(t\) is the time the money is invested/borrowed for, in years.

Here's how you can implement this formula in TypeScript:

```typescript
function calculateFutureValue(
  P: number,
  r: number,
  n: number,
  t: number
): number {
  const A: number = P * Math.pow(1 + r / n, n * t);
  return A;
}

// Example usage
const principal: number = 1000; // Principal investment amount
const annualInterestRate: number = 0.05; // Annual interest rate (as a decimal)
const timesCompoundedPerYear: number = 1; // Compounded annually
const years: number = 5; // Number of years

const futureValue: number = calculateFutureValue(
  principal,
  annualInterestRate,
  timesCompoundedPerYear,
  years
);
console.log(`Future value after ${years} years: ${futureValue}`);
```

This code calculates the future value of an investment compounded annually over a specified number of years. Adjust the `principal`, `annualInterestRate`, `timesCompoundedPerYear`, and `years` variables according to your specific scenario.
