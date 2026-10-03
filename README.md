# pg-backup-ui

A small web UI, shipped as Docker, for PostgreSQL backups you would otherwise type by hand.

It does not implement its own backup format. It runs the PostgreSQL tools and shows the result:

- `pg_dump` / `pg_restore` for one database
- `pg_basebackup` for a full physical backup
- `pg_basebackup --incremental` plus `pg_combinebackup` (PostgreSQL 17+)
- restore into a **new** directory, not over the live data directory

The server must already be able to archive WAL if you want point-in-time recovery. This app does not replace Barman or pgBackRest.

## Status

Design only. No application code yet. Planned work is in [docs/ROADMAP.md](docs/ROADMAP.md).

## Who it is for

Someone who knows `pg_basebackup` and wants a screen for one lab or one cluster: take a full backup, take an incremental, combine them, restore to a spare directory, and confirm the row count.

## What it will not do

- Its own storage format or WAL shipper
- Scheduling, retention, or encryption (later, if ever)
- Many hosts, replicas, or Barman/pgBackRest management
- A button that deletes or replaces the live data directory without typing the cluster name

## Requirements (when the app exists)

- PostgreSQL 17 or 18 (`--incremental` and `pg_combinebackup`)
- `summarize_wal = on` for incremental backup
- Docker, with the backup directory mounted into the container

## License

TBD.
