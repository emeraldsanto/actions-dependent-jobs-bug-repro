# actions-dependent-jobs-bug-repro

A reproduction of possible unintended behaviour when configuring an Actions workflow job to depend on another.

## Summary

You'll find a single GitHub Actions workflow in this repository [here](./.github/workflows/broken.yml) which is meant to run in this order:

1. Parallel
  1. Run `succeeding` and report a status of success
  2. Run `skipped` and report a status of skipped
2. Run `summary` and report a status of success
3. Run `afterall` and report a status of success

However, you'll notice that instead the following occurs:

1. Parallel
  1. Run `succeeding` and report a status of success
  2. Run `skipped` and report a status of skipped
2. Run `summary` and report a status of success
3. Run `afterall` and report a status of skipped

I don't think this is right, given `afterall` defines `needs: summary` which does succeed? The problem goes away when `skipped` is removed from `summary`'s `needs`, but that is not a viable solution for the underlying use case.
