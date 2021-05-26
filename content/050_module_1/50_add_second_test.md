---
title: "2.1 Add a second test"
chapter: true
weight: 10
---

## Step 1 &mdash; Adding a second test

Now it is time to add another test to `App.test.js`. We are going to use a simple function to test state:

```js
describe('Addition', () => {
  it('knows that 2 and 2 make 4', () => {
    expect(2 + 2).toBe(4);
  });
});
```

## Step 2 &mdash; Running the test locally

Run this test locally with `npm test` and check out the test coverage:

![2 Test Passes](/images/2-test.png)

So now that we've gotten more tests, it's time to add some coverage analysis. In the next section, we're going to learn how to leverage CodeCov to continuously analyze our codes coverage. 