---
name: babysit-stack
description: Watch a stack of PRs, addressing test failures and responding to comments, until the stack is fully green.
---

# Babysit Stack

## Input

- Accept input as a jj revset. If not given, use `stack`.

## Workflow

1. Use the handle-pr-feedback skill, passing it the input revset, to handle comments on the PRs in the stack.

2. Check CI for each PR in the stack, specifically the `lint` and `test-optimized` jobs. Consider only the most recent run per check. If the `require-tests-pre-merge` job has a failure, add the `pr/force-ci` label to the PR to resolve (and move to the next PR for now since adding the label will trigger test-optimized to start, which will take a while).

3. If there are real failures, investigate and fix.

4. If any changes were made in the earlier steps, push the changes using `jfd`.

5. Exit when: no new unhandled review comments, and the `lint` and `test-optimized` jobs are showing SUCCESS on all the PRs.
