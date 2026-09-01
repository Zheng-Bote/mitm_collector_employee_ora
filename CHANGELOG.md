# Changelog

All notable changes to the `mitm_collector_employee_ora` project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v0.15.2] - 2026-09-01

### Fixed

- **IPC SSLMode Type Fix**: Changed `SSLMode` field in JSON parsing struct from `string` to `bool` to correctly unmarshal boolean values (`true`/`false`) sent by the scheduler.

## [v0.15.1] - 2026-09-01

### Fixed

- **IPC SSLMode Fix**: Fixed an issue where `SSLMode` was not correctly parsed from the scheduler's JSON configuration and improved the `MITM_DB_SSLMODE` fallback logic to support proper PostgreSQL sslmode strings (e.g., `require`, `verify-full`).

## [v0.15.0] - 2026-08-31

### Added

- **IPC Socket as Credential Broker**: The collector now fetches database credentials and the master key at runtime from the Scheduler via a Unix Domain Socket request (`get_credentials` with `RUN_ID` and `SCHEDULER_SOCKET_PATH`), instead of holding them locally. Removed the previously committed binary from the repository.

### Changed

- **Atomic Batch Transactions**: Refactored row ingestion and cursor persistence into a shared `executeBatch` transaction (`pgx.Tx`). Batch inserts and the cursor upsert now commit or roll back atomically, with correct `recordsIngested`/`recordsFailed` accounting.

## [v0.14.0] - 2026-08-29

### Changed/Added

- Configured `pgxpool` connection limits (`MaxConns=20`, `MaxConnIdleTime=5m`, `MaxConnLifetime=1h`).
- Implemented graceful shutdown with context cancellation on `SIGINT`/`SIGTERM`.
- Optimized performance with batched operations.

## [v0.13.0] - 2026-08-29

### Fixed

- **Oracle Connection**: Fixed an issue where the connection string was incorrectly formulated when using a SID instead of a Service Name.

## [v0.12.0] - 2026-07-29

### Changed

- **Components Logging**: Refactored component version logging mechanism across all layers (Collectors, Transformation, Delivery, Scheduler) to consistently output a clean `Major.Minor.Patch` version format.

## [0.11.0] - 2026-07-24

### Added

- Created `mitm_collector_employee_ora` as an independent collector from `mitm_collector_ora`.
- Implemented an inline SQL `LEFT JOIN` between the primary table and the `org` table to satisfy the 1:N enrichment constraint required for downstream processing.
- Automatically denormalizes `orgid`, `short`, and `parent_org_id` into the Employee fragment payloads.
