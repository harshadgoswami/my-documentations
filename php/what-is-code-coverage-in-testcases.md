Code Coverage HTML Report
Purpose:
The --coverage-html coverage flag generates an interactive HTML report showing which lines of your code are executed by tests and which are not.

What It Shows:
Code Coverage Percentage - Overall percentage of code lines covered by tests

Example: 97%+ coverage means 97% of your code is executed during test runs
Line-by-Line Color Coding:

🟢 Green - Line is tested (executed by at least one test)
🔴 Red - Line is NOT tested (never executed during tests)
🟡 Gray - Line is not executable (comments, blank lines)
Coverage by File - Shows coverage stats for each class/file:

HolidayService.php - 95% coverage
DateValidator.php - 98% coverage
HolidayRepository.php - 92% coverage
Method-Level Breakdown - Shows which methods are fully/partially/not tested

How to View:
After running ./vendor/bin/phpunit --coverage-html coverage, open:

in your browser to see the interactive report.

Why It Matters:
Quality Assurance - Identifies untested code paths (potential bugs)
Risk Assessment - Highlights critical code that needs more testing
Compliance - Many projects require minimum coverage (80-90%)
Refactoring Confidence - High coverage means safe to refactor without breaking functionality
Your command achieved 97%+ coverage, meaning nearly all production code is validated by tests—excellent quality indicator.
