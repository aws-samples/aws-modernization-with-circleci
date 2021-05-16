---
title: "1.2 Adding tests"
chapter: true
weight: ADD WEIGHT
---

## Step 1 – Add tests to your demo app

Now you will create some tests for your application. Navigate to the `app.test.js` file in the demo application your created in the last step and add a test:

```js
it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
```

# Step 2. Running tests locally

To run your test locally, run `npm test`