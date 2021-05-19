---
title: "2.1 Add a second test"
chapter: true
weight: ADD WEIGHT
---

## Step 1 &mdash; Adding a second test

Now it is time to add another test to `app.test.js`. We are going to use a simple function to test state:

```js
describe('Addition', () => {
  it('knows that 2 and 2 make 4', () => {
    expect(2 + 2).toBe(4);
  });
});
```

## Step 2 &mdash; Running the test locally

Run this test locally with `npm test` and check out the test coverage:

![](https://d585tldpucybw.cloudfront.net/sfimages/default-source/default-album/06-09.png?sfvrsn=149c482_1)