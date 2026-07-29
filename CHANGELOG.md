# Changelog

All notable changes to the `mitm_collector_employee_ora` project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v0.12.0] - 2026-07-29

### Changed

- **Components Logging**: Refactored component version logging mechanism across all layers (Collectors, Transformation, Delivery, Scheduler) to consistently output a clean `Major.Minor.Patch` version format.

## [0.11.0] - 2026-07-24

### Added

- Created `mitm_collector_employee_ora` as an independent collector from `mitm_collector_ora`.
- Implemented an inline SQL `LEFT JOIN` between the primary table and the `org` table to satisfy the 1:N enrichment constraint required for downstream processing.
- Automatically denormalizes `orgid`, `short`, and `parent_org_id` into the Employee fragment payloads.
