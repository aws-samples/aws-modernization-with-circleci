---
title: "3.1 Create a new project"
chapter: true
weight: 10
---

## Step 1 &mdash; Create an Express project

For this module, we need to create a new Express project. Open your terminal and run:

```bash
$ npm install -g express-generator
$ express
```

## Step 2 &mdash; Add tests

To add some tests, open your `spec.js` application and add:

```javascript
var request = require('supertest');
describe('loading express', function () {
  var server;
  beforeEach(function () {
    server = require('./server');
  });
  afterEach(function () {
    server.close();
  });
  it('responds to /', function testSlash(done) {
  request(server)
    .get('/')
    .expect(200, done);
  });
  it('404 everything else', function testPath(done) {
    request(server)
      .get('/foo/bar')
      .expect(404, done);
  });
});
```

Now that we've learned to create a project, let's set up the variable we need to this module in the next section.