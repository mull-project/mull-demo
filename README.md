# mull-demo

A minimal demo of [Mull](https://mull-project.com) mutation testing, based on the [Quick Primer](https://mull-project.com/getting-started/primer/).

## What this project does

`main.c` implements a (buggy) `in_range` function that checks whether a value falls within a closed interval `[min, max]`, along with three assertions that serve as its test suite:

```c
int in_range(int value, int min, int max) {
  return value >= min && value < max;
}
```

The tests cover below-min, mid-range, and above-max cases — achieving 100% line coverage. Despite this, Mull reveals that two mutations survive:

- `>=` changed to `>` (the boundary at `min` is not tested)
- `<` changed to `<=` (the boundary at `max` is not tested)

This demonstrates a core limitation of coverage-based testing: coverage does not guarantee that the tests actually validate the semantics of the code.

## Running locally

Install mull (see [installation docs](https://mull-project.com/getting-started/installation/)), then:

```bash
clang-22 \
  -fpass-plugin=/usr/lib/mull-ir-frontend-22 \
  -g -grecord-command-line \
  main.c -o range_tests

mull-runner-22 range_tests
```
