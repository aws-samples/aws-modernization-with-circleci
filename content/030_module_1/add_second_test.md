---
title: "2.1 Add second test"
chapter: true
weight: ADD WEIGHT
---

# Adding a second test

Let's add another test to `app.test.js` now. We're going to go with a simple function to test state:

```js
describe('Addition', () => {
  it('knows that 2 and 2 make 4', () => {
    expect(2 + 2).toBe(4);
  });
});
```

# Running test locally

Let's run this test locally now with `npm test` and check out the test coverage:

TODO: Add screenshot