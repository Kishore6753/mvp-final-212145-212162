# Operations and Environment

## Overview
This guide covers environment variables and operational scripts for the database container, including startup, health, visualization, and backup/restore.

## Environment Variables
- PGPORT: PostgreSQL port to listen on.
- PGTRUST_PROXY: Trust reverse proxy headers (where applicable).
- PGLOG_LEVEL: Log verbosity for container-managed services.
- PGHEALTHCHECK_PATH: Path used by health checks (if an HTTP health endpoint is present).
- PGFEATURE_FLAGS: Feature toggles for internal services.
- PGEXPERIMENTS_ENABLED: Enables experimental behavior for internal utilities.

## Startup
- startup.sh initializes and starts PostgreSQL, creates the database and user, grants privileges, and writes a connection string to db_connection.txt.
- The script also writes db_visualizer/postgres.env for convenience.

## Health and Visibility
- Use db_visualizer/server.js to quickly inspect tables and data across supported engines; it reads *.env files and exposes simple endpoints for listing tables and fetching data.

## Backup and Restore
- backup_db.sh detects the running engine and writes a consistent backup file (SQL or archive).
- restore_db.sh detects backup type and restores to the running database.

## Runbook Notes
- Verify port and credentials using the connection string in db_connection.txt.
- When ingestion errors occur, check file_ingestions and audit_logs first; re-run the specific batch as required.
- Ensure environment variables are set non-interactively for CI/CD pipelines.
