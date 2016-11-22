# SimVersion

A simple, pragmatic versioning convention.

In a sense, this is a subset of [SemVer](http://semver.org/), but with slightly different semantics.

## Series

An API has two series of releases: feature-complete, and feature-incomplete.

### Feature-incomplete

Feature-incomplete releases use a version numbering scheme `0.MAJOR.UPDATE`, where:

  * `0` indicates a feature-incomplete release
  * `MAJOR` increases for releases with *breaking* changes
  * `UPDATE` increases for releases with *compatible* changes

Increase of the `MAJOR` version number indicates backwards-incompatible API changes.

Increase of the `UPDATE` number indicates any other minor change, e.g. bugfixes, new compatible features, documentation updates, etc.

Example:

  * `0.1.0` is the initial, incomplete release
  * `0.1.1` is the next compatible, still-incomplete release
  * `0.2.0` is the next incompatible, still-incomplete release

Note that, because the `0.x` series is by definition feature-incomplete, the addition of new features is expected, and
therefore is not indicated any differently from any other change.

### Feature-complete

The first feature-complete release is indicated by the release of version `1.0.0`.

Feature-complete releases use a version numbering scheme `MAJOR.MINOR.PATCH`, where:

  * `MAJOR` increases for releases with *breaking* changes
  * `MINOR` increases for releases introducing *new features*
  * `PATCH` increases for releases with *compatible* changes

Increase of the `MAJOR` version number indicates backwards-incompatible API changes.

Increase of the `MINOR` version number indicates compatible API changes introducing *new features*.

Increase of the `PATCH` number indicates any other minor change, e.g. bugfixes, documentation updates, etc.

Example:

  * `1.0.0` is the first feature-complete release
  * `1.0.1` is the next compatible release with minor changes or bugfixes
  * `1.1.0` is the next compatible release introducing new features
  * `2.0.0` is the next incompatible release with breaking changes

## Guidelines

Every release, whether or not it's feature-complete, is considered "stable", in the sense
that it's expected to work - in terms of source-control, an unstable release should *never*
be tagged, but may exist on a pre-release branch, until it's ready for release.

The `0.x` release series indicates that the API is still incomplete and doesn't yet include
or support all of the planned features. Increase the version number to `1.0.0` when the API
is feature-complete.

*Always* increase the `MAJOR` version number for any breaking change, regardless of whether the
release is feature-complete or not.

*Never* increase the `MAJOR` version number for backwards-compatible changes.

The initial `1.0.0` release *may* have breaking changes - your change log should indicate if it does.

Deprecation is a minor change. Removal is a breaking change.

Accidental release of a breaking change with a minor version number increase must be corrected
by an immediate minor release reverting the change, and a subsequent `MAJOR` release.

A major update to an external dependency imported by your package does not automatically lead
to a major version increase in your package - while it could lead to blockage (in a package
with a conflicting version constraint for the same external dependency) it doesn't always,
for example, if changes to an external dependency do *not* result in any breaking changes to
your public API.

Breaking changes to *internal* APIs with a minor version increase are permitted, for classes
(and other source elements) that have been *explicitly* marked as implementation details intended
for internal use only, e.g. with a formal annotation or doc-block.

## Differences from SemVer

This convention uses two distinct release series with slightly different semantics and scope,
which match the caret operator for version constraints, e.g. as implemented by package management
tools such as [Composer](https://getcomposer.org/) and [npm](https://www.npmjs.com/).

Unlike SemVer, the feature-incomplete `0.x` series does *not* allow breaking changes from `0.1.0`
to `0.1.1` - in practice, this is generally how the caret `^` operator version constraint works
with most package managers, e.g. in `composer.json` with Composer and `package.json` with npm.

This convention does not permit the use of pre-release identifiers, as in `1.0.0-alpha`, etc. -
the notion of pre-releases is outside the scope of this convention, which recognizes that this
problem can solved in a number of ways, for example (typically) by treating e.g. a `1.0.0` *branch*
(under source-control) as a pre-release, and a `1.0.0` *tag* as a release.
