---
title: "1.2 Adding a test"
chapter: true
weight: 12
---

## Step 1 &mdash; Add test to your demo app

1. Now you will create some tests for your application. Go to the `App.test.js` file.
2. You can select this file in your Cloud9 as such
3. ![Select App.test.js here](/images/app-test-file.png)
4. Add this test:

```js
it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
```

## Step 2 &mdash; Run test locally

To run your test locally, run `npm test`. The output should look similar to this:

```bash
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
 PASS  src/App.test.js (9.49 s)
  √ renders without crashing (40 ms)

----------|---------|----------|---------|---------|-------------------
File      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s 
----------|---------|----------|---------|---------|-------------------
All files |     100 |      100 |     100 |     100 |                   
 App.js   |     100 |      100 |     100 |     100 |                   
----------|---------|----------|---------|---------|-------------------
Test Suites: 1 passed, 1 total       
Tests:       1 passed, 1 total       
Snapshots:   0 total
Time:        14.877 s, estimated 19 s
Ran all test suites related to changed files.

Watch Usage
 › Press a to run all tests.
 › Press f to run only failed tests.
 › Press q to quit watch mode.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press Enter to trigger a test run.
```

Now that we know we have a passing test, let's move on and automate the process of integrating it into our CI/CD Pipeline.