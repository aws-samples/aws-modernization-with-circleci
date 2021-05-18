---
title: "3.1 Create new project"
chapter: true
weight: ADD WEIGHT
---

# Create new project

Let's create a new Express project. Open your terminal and run:

```bash
$ npm install -g express-generator
$ express
```

# Add tests

Now let's add some tests. Open your `spec.js` application and add these tests:

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