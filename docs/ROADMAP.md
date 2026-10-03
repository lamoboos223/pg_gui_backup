# Roadmap

This is the feature backlog. Open-source projects usually keep it in `docs/ROADMAP.md` (or `ROADMAP.md`). A private team more often says "backlog" and tracks the same items as GitHub issues.

Nothing below is built yet.

## Now

One cluster, local Docker, no login.

- [ ] Connect to one Postgres instance (host, port, user, password or socket) and show version, data directory, and `summarize_wal`
- [ ] `pg_dump` one database to a chosen folder (custom format)
- [ ] `pg_restore` that dump into a **new** database name, not over the source database
- [ ] Full `pg_basebackup` (`-Fp`) into a named folder; keep `backup_manifest`
- [ ] List backup folders and whether each one is full, incremental, or combined
- [ ] Refuse to write into the live data directory

## Next

Incremental backups and a safe restore drill.

- [ ] Require `summarize_wal = on` before an incremental; explain how to turn it on
- [ ] Incremental `pg_basebackup --incremental=<previous backup_manifest>`
- [ ] Chain: full, then incremental, then another incremental off the latest manifest
- [ ] `pg_combinebackup` of a chosen chain into a new output directory
- [ ] Restore that combined directory as a **new** cluster path (never `mv` onto the running `main`)
- [ ] Show a PITR hint: archive command, `recovery_target_time`, `recovery.signal` (the app does not perform PITR by itself in this phase)
- [ ] Plain-language log of the exact command that ran, plus success or the tool's stderr

## Later

Only after the items above are boring and correct.

- [ ] Optional login in front of the UI
- [ ] More than one saved connection
- [ ] Simple schedule (cron inside the container) for full and incremental
- [ ] Retention: keep the last N full backups and their incrementals
- [ ] WAL archive directory browser (list segments, do not delete them from the UI at first)

## Out of scope

- Replacing Barman, pgBackRest, or WAL-G
- Replica promotion, failover, or Patroni
- Editing `postgresql.conf` from the UI
- A one-click restore onto the live data directory
