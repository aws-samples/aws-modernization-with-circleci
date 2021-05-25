---
title: "1.2 Adding tests"
chapter: true
weight: 12
---

## Step 1 &mdash; Add tests to your demo app

Now you will create some tests for your application. Go to the `app.test.js` file in the demo application you created in the last step. Add a test:

```js
it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
```

## Step 2 &mdash; Run tests locally

To run your test locally, run `npm test`. The output shouldlook something like this:

```bash
No tests found related to files changed since last commit.
Press `a` to run all tests, or run Jest with `--watchAll`.
----------|---------|----------|---------|---------|-------------------
File      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s 
----------|---------|----------|---------|---------|-------------------
All files |       0 |        0 |       0 |       0 |                   
----------|---------|----------|---------|---------|-------------------

Watch Usage
 › Press a to run all tests.
 › Press f to run only failed tests.
 › Press q to quit watch mode.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press Enter to trigger a test run.
```

Press `a` to run the tests and you should be presented with this output:

```bash
No tests found related to files changed since last commit.
Press `a` to run all tests, or run Jest with `--watchAll`.
----------|---------|----------|---------|---------|-------------------
 PASS  src/App.test.js (18.699 s)
  √ renders without crashing (36 ms)
  √ renders welcome message (36 ms)
  Addition
    √ knows that 2 and 2 make 4 (3 ms)

----------|---------|----------|---------|---------|-------------------
File      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s 
----------|---------|----------|---------|---------|-------------------
All files |       0 |        0 |       0 |       0 |                   
----------|---------|----------|---------|---------|-------------------
Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Snapshots:   0 total
Time:        21.486 s
Ran all test suites.

Watch Usage: Press w to show more.
```

Now that we have our tests passing, let's move on and automate the process of integrating them into our deployments