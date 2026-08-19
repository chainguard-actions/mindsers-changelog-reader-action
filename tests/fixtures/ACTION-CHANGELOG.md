# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [2.4.0] - 2026-05-20

### Added

- New `changes_file` output: a path to a temporary file containing the matched entry's text.
- New `version_scheme` input enabling extraction and validation of Python PEP 440 version identifiers.

### Security

- Harden the reference-link parsing regex against catastrophic backtracking.

## [2.3.0] - 2026-05-19

### Changed

- Use Node 24 as the action runtime.
- Refactor the internal entry, validation, and pipeline modules for type safety.

[Unreleased]: https://github.com/mindsers/changelog-reader-action/compare/v2.4.0...HEAD
[2.4.0]: https://github.com/mindsers/changelog-reader-action/compare/v2.3.0...v2.4.0
[2.3.0]: https://github.com/mindsers/changelog-reader-action/releases/tag/v2.3.0
