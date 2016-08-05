# Twitter Analytics Infrastructure OSS Ecosystem Governance

### How to commit to, review, and release the Scalding / Summingbird family of projects

This document describes the process for how to commit to, review and release the Scalding / Summingbird OSS family of projects. It does not discuss the mechanics of doing the release, publishing to maven central, etc.

## Projects and Tiers

The Scalding family of projects can be broken coarsely into two tiers:

1. Tier 0: These are the larger top-level projects that are most externally visible.
  * [Scalding](https://github.com/twitter/scalding)
  * [Summingbird](https://github.com/twitter/summingbird)
2. Tier 1: These are the libraries that the Tier 0 projects depend on. These can often be released independently without releasing the Tier 0 projects.
  * [Algebird](https://github.com/twitter/algebird)
  * [Bijection](https://github.com/twitter/bijection)
  * [Chill](https://github.com/twitter/chill)
  * [Storehaus](https://github.com/twitter/storehaus)
  * [Tormenta](https://github.com/twitter/tormenta)

## Contributing and Committership

It is not necessary to become a committer in order to contribute to the Analytics Infrastructure OSS Ecosystem. Anyone is welcome to contribute to Twitter's OSS projects, and pull requests and bug reports are greatly appreciated.
We ask that contributors follow [Twitter's Open Source Code of Conduct](https://engineering.twitter.com/opensource/code-of-conduct). Additionally, see the [Typelevel Code of Conduct](http://typelevel.org/conduct) for specific examples of harassing behavior that are not tolerated.

In addition to contributing, anyone can become a committer. The role of committer is not purely a technical one, it is also a project stewardship and maintenance role. Of course, anyone is welcome to contribute to the project without becoming a committer - you only need to be a committer if you want to play a leadership or stewardship role within the project. Only committers can merge pull requests, because only committers have write access to the repositories.

A prospective committer should demonstrate that they have:

  * Sustained non-trivial, well-tested contributions accepted into the project
  * Community involvement, including reviewing other's contributions (code reviews), participating in design discussions, attending project sync-ups, and contributing to project maintenance (documentation, tech-debt cleanup)

In order to add a new committer to a project, the existing active committers in that project must vote, using the same voting process as the one for releases described below, with at least one -0 or higher vote from a Twitter employee committer.

Each project contains a COMMITTERS.md file at the root of the project that lists the current and emeritus committers. If a committer hasn’t contributed to the project (i.e. by participating in reviews, or by submitting contributions) for a period of ONE year, their status will be moved to Emeritus, and they will no longer have commit/vote/release privileges.

## Code Review and Merge Process

Committers (and other contributors) will review your contribution (i.e. a pull request). Each contribution must get at least one +1 from an active committer (who is not yourself) before merging. If the initial reviewer is unsure whether to +1 something or not, they should involve other committers for testing/validation of the contribution. No PRs should be merged if there are open issues that have not been discussed, or if there are any -1s without further discussion, or if the build is not green. Any committer can merge the change after the review process is complete and the build is green.

## Release Process

We use a separate processes for Tier 0 and Tier 1 projects. We follow [Semver 2.0.0](http://semver.org/spec/v2.0.0.html) rules, but since none of our projects are 1.0 yet, our versions are of the form 0.MAJOR.MINOR. All minor releases should be source and binary compatible with the previous major release. For minor releases of Tier 1 projects (i.e. 0.7.1->0.7.2), any of the committers can release and publish artifacts as long as they including release notes, and all tests are passing.

For major releases of Tier 1 projects, and any release of the Tier 0 projects, we propose the following process inspired by the [Apache release process](http://www.apache.org/dev/release-publishing.html):

1. One of the committers proposes a release (often based on their own requirements, or per the request of some user) - this committer will be designated as the release manager unless they designate another.
2. The release manager prepares and creates a branch for the release with a naming convention of `releases/0.<major>.<minor>-rc<xx>`. This branch includes well-defined release notes. They then publish a release candidate with the same version (with -RCXX at the end) as the branch into maven central for the other voters to test.
3. The release is offered for a binding vote. The voting process is described below.
4. If the vote passes, the release manager publishes the artifacts to the distribution infrastructure without the -RCXX appended (i.e. to maven central).
5. The master branch should always be updated with the latest released version, and tagged with the release number.

RCs should only be published with the intent of having a vote. If committers from different organizations would like newer version of the libraries without a formal release, they should publish to their own internal maven repositories from their own release branches (probably with a non-conflicting version number, like `0.<major>.<minor>-twitter-xx`)

## Voting Process

Voting for releases is a simplified version of the [Apache voting process](http://www.apache.org/foundation/voting.html). After a vote for a release is requested, the voting continues for 3 business days (Mon-Fri, excluding holidays). During this process, committers/users from various organizations can choose to run their own set of integration tests before casting their vote.

Votes can be +1, +0, -0, and -1, as follows:
  * +1 means the committer has validated the release via the method of their choice (e.g. via integration tests), and supports this release.
  * +0 means the committer doesn’t feel strongly about the release, but is OK with it.
  * -0 means the committer has reservations about the release, but won’t stand in the way.
  * -1 means that the committer has strong reservations about the release (e.g. due to a failing integration test), and will block the release. This is essentially a veto, and can’t be overruled or overridden. If there is a veto, it is the responsibility of the other committers who have +1-ed to convince the vetoer to change their vote (e.g. by fixing or providing a solution to the offending issues).

There will only be one quorum rule - every release must have at least a -0 or higher from a Twitter employee committer since these are Twitter owned OSS projects.

If at the end of 3 days, outstanding issues aren’t resolved, then the release is canceled. After fixing the issues discussed leading to the -1 vote, the process may be repeated.