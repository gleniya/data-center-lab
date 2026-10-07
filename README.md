# Data Center Lab

A hands-on project about keeping data safe: virtualization, containers, RAID storage, backups, restores and monitoring.

It has two parts:

| Folder | What it is |
|---|---|
| [`demo/`](demo) | A browser-based simulator of a small data center. Open it to see the concepts in action. |
| [`real-lab/`](real-lab) | A real Docker setup (website + PostgreSQL) with automated backups, verified restores and health checks. |

**Live demo:**
https://github.com/gleniya/data-center-lab/blob/main/mini-datacenter.html

---

## 1. Demo (simulator)

A single HTML page, no install needed. It simulates:

- **Virtual machines:** start, stop, snapshot, wipe data
- **Containers:** run apps that add load to their host VM
- **RAID storage:** RAID 0/1/5/6, disk failure and rebuild
- **Backups:** SHA-256 checksums, restore, corrupt-backup detection, scheduled jobs
- **Monitoring:** live CPU graph and alerts
- **Shell:** Linux-style commands such as `vm list` and `docker ps`

This is a simulation of the ideas. It does not manage real servers.

## 2. Real lab (Docker)

A real stack that runs on your own machine.

**Stack:** Docker Compose, Bash, PostgreSQL, nginx, cron, SHA-256

**What it does:**
- Runs a website and a database in containers, with a database health check
- Backs up website files and the database, then verifies each archive
- Keeps the newest 7 backups
- Restores everything from backup after a simulated disaster
- Checks health with `monitor.sh`, which returns an exit code for cron or CI

**Run it:**

    cd real-lab
    docker compose up -d     # website at http://localhost:8080
    ./test.sh                # backup, destroy, restore, verify (prints PASS)
    ./monitor.sh             # health check

See [`real-lab/README.md`](real-lab/README.md) for details.

## What I learned

- A backup is only useful if a restore has been tested.
- RAID protects against disk failure, not against deleted files or ransomware.
- Snapshots on the same disks as the VM are not a backup.
- Checksums catch corrupt backups before they overwrite good data.

## Next steps

- Copy backups to a second machine with rsync
- Add Prometheus and Grafana dashboards
- Run backups from a systemd timer or GitHub Actions
