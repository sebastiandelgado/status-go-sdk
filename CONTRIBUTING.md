# Contributing to status-go

First of all, thank you for taking time to add more value to Status, we really appreciate it!
If you just have a question, open an issue or ask us on our Slack channel: https://slack.status.im/

## Starter Links

TBD

## Workflow

1. Pick an [issue](https://github.com/status-go-sdk/status-go/issues) to work on and drop a line there that you're working on that.
2. Wait for an approve from one of core contributors.
3. Fork the project and work right in the `develop` branch.
4. Work on the issue and file a PR back into `develop`.
5. Wait until your PR is [reviewed](https://hackmd.io/s/B1AenvFU-) by 2 core developers and merged.

## Code Style

Please, note that we follow [Effective Go](https://golang.org/doc/effective_go.html) and
[CodeReviewComments](https://github.com/golang/go/wiki/CodeReviewComments) in our code.

You have a quick script for checking styles by running `make lint`

## Keep history clean

1. Squash PR before merging.
   You can do it either with GitHub API by merging with "Squash and merge" or locally if you want to preserve your signature.
   It is ok to merge multiple commits with "Rebase and merge" if they are logically separate.

2. Preserve as much history as possible.
   If you need to re-name file use `git mv` - it will preserve git history.
