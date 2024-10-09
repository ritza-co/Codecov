# Generate Code Coverage Reports with Jest

If you’re using [Jest](https://jestjs.io/) for testing in your JavaScript projects, you may have heard about code coverage reports. These reports give insight into how much of your codebase is being tested, and can be a valuable tool for improving the quality and reliability of your code.

In this article, we’ll walk through how to set up, generate, and interpret Jest code coverage reports and extend their functionality to better share and make decisions. All to help you maximize the effectiveness of your tests.

## What is Code Coverage?

Code coverage measures the percentage of your code that is executed when tests are run. It helps identify untested parts of your codebase, guiding you on where to focus additional testing. Coverage metrics often include:

* **Statement Coverage:** Checks if each statement in your code has been executed
* **Branch Coverage:** Tests whether every possible branch (e.g., `if/else`) has been executed
* **Function Coverage:** Ensures each function in your code has been invoked
* **Line Coverage:** Determines if each line of code has been executed

Jest, a popular JavaScript testing framework, supports code coverage out of the box, making it simple to get started.

---

## Setting Up Jest for Code Coverage

Before generating coverage reports, ensure you have Jest installed in your project. If you don’t have it yet, you can install it using npm:

```bash
npm install --save-dev jest
```

To enable code coverage, you can modify the `package.json` file or pass a flag directly when running Jest.

### Add Code Coverage Command in package.json

You can modify the test script in your `package.json` file to include coverage by default:

```json
{
  "scripts": {
    "test": "jest --coverage"
  }
}
```

### Run Jest with Coverage Flag

Alternatively, you can run the command directly from your terminal:

```bash
npx jest --coverage
```

This will generate a detailed code coverage report each time you run your tests.

## Understanding the Coverage Report

Once the tests run, Jest outputs a coverage summary to the console, and it creates a coverage directory in your project with an HTML report. Following is an explanation of what the outputs mean.

### Console Output

The console output will look something like this:

```bash
C:\path\to\Jest\repo>npx jest --coverage
 PASS  ./index.test.js
  Index.html Tests
    √ true (4 ms)
    √ false (1 ms)
    √ true
    √ true (1 ms)

----------|---------|----------|---------|---------|-------------------
File      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
----------|---------|----------|---------|---------|-------------------
All files |    90.9 |    83.33 |     100 |    90.9 |
 index.js |    90.9 |    83.33 |     100 |    90.9 | 12
----------|---------|----------|---------|---------|-------------------
Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        0.626 s, estimated 1 s
Ran all test suites.
```

* **Stmts:** Percentage of statements executed.
* **Branch:** Percentage of branch paths (e.g., if/else conditions) tested.
* **Funcs:** Percentage of functions called.
* **Lines:** Percentage of lines executed.

Uncovered lines are listed on the right, helping you pinpoint areas that may need additional tests.

### HTML Report

For a more detailed analysis, open the HTML report found in `coverage/lcov-report/index.html` in your browser. This visual representation allows you to:

* Navigate through files and folders
* See color-coded indicators showing covered (green) and uncovered (red) lines of code
* Inspect each file for specific areas where tests are missing

![Image of the Jest coverage report UI](./images/Jest/jest_html_report.png)

---

## Improving Code Coverage

Once you have a report, you may find gaps in your tests. Here are some tips to properly address them

### Increase Branch Coverage

Branch coverage often lags because tests may not cover all conditions. Consider testing both true and false outcomes for every conditional statement.

### Test Edge Cases

Think about edge cases, such as:

* Empty inputs
* Extremely large values
* Unexpected user behaviors

Adding these tests can increase your statement, branch, and line coverage.

### Mock Dependencies

For functions that rely on external APIs or other modules, use mocks to simulate different scenarios. Jest has powerful mocking capabilities through functions like jest.fn() and jest.mock(), which help test your code in isolation.

### Advanced Configuration for Jest Code Coverage

You can customize Jest’s code coverage behavior using the `jest.config.js` file or configuration options within your `package.json`. Below are some useful settings.

#### Exclude Files and Directories

If there are files or directories you want to exclude from coverage reports (e.g., configuration files, mocks, or utilities), you can use the coveragePathIgnorePatterns option:

```js
// jest.config.js
module.exports = {
  collectCoverage: true,
  coveragePathIgnorePatterns: [
    "/node_modules/",
    "/src/utils/"
  ]
};
```

#### Set Thresholds

To enforce a minimum level of coverage, use the coverageThreshold option. If coverage falls below the defined threshold, Jest will fail the test run:

```js
// jest.config.js
module.exports = {
  collectCoverage: true,
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 90,
      lines: 90,
      statements: 90
    }
  }
};
```

#### Coverage Report Types

Jest supports multiple report types, such as text, html, lcov, and json. You can specify which reports Jest should generate using the coverageReporters option:

```js
// jest.config.js
module.exports = {
    collectCoverage: true,
    coverageReporters: ["json", "lcov", "text", "html"]
};
```

This flexibility allows you to integrate Jest reports with CI/CD tools like Codecov, which visualizes and tracks coverage over time.

---

## Automating Coverage Reports in CI/CD

If you use a CI/CD service like GitHub Actions, GitLab CI, or Jenkins, you can automate code coverage checks to ensure code quality continuously. Here's a basic example using GitHub Actions:

```yaml
name: CI
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'
          
      - name: Install dependencies
        run: npm install
        
      - name: Run tests with coverage
        run: npm test -- --coverage
```

---

## Conclusion

Jest code coverage reports are powerful tools for enhancing your testing strategy and maintaining high code quality. By understanding how to set up, interpret, and optimize these reports, you can pinpoint gaps in your tests and ensure your codebase remains robust and reliable.

This guide should give you a solid foundation in Jest code coverage, helping you implement best practices and achieve greater test coverage in your projects.
