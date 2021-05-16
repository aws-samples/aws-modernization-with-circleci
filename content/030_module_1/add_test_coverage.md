---
title: "2.2 Implement continuous test coverage analysis"
chapter: true
weight: ADD WEIGHT
---

# Add step for test coverage

Now that we've added another test, let's add a `store_artifacts` step to track our test coverage:

```YAML
    steps:
      - store_artifacts:
          path: coverage
```

# Setting a threshold for out test coverage

