# ZFS Administration Skill for Claude Code

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) that gives Claude deep knowledge of ZFS filesystem administration on OpenZFS (Linux and macOS).

## What It Does

When installed, this skill enables Claude Code to assist with:

- **Pool management** — creating, expanding, and replacing disks in ZFS pools with correct `ashift`, redundancy, and device naming
- **Dataset configuration** — setting recordsize, compression, encryption, quotas, and other properties matched to your workload
- **Snapshots and replication** — `zfs send/recv`, incremental replication, encrypted send, and automated replication with syncoid/sanoid
- **Performance tuning** — ARC sizing, SLOG, L2ARC, special vdevs, and recordsize/compression selection by workload type
- **Troubleshooting** — degraded pool recovery, scrub errors, data corruption, import/export problems, and capacity management
- **Platform-aware guidance** — handles Linux vs macOS differences in device naming, kernel integration, mount behavior, and installation

The skill also enforces best practices, such as never recommending file-backed pools for production and always using `/dev/disk/by-id/` on Linux.

## Repository Structure

```
zfs.skill              # Packaged skill file (installable archive)
zfs/
  SKILL.md             # Main skill definition and quick reference
  references/
    properties.md      # ZFS property reference (pool, dataset, encryption)
    workload-tuning.md # Recordsize, compression, dedup, ARC, SLOG, L2ARC guidance
    replication.md     # Snapshots, send/recv, remote replication, sanoid/syncoid
    troubleshooting.md # Degraded pools, scrub errors, corruption, common mistakes
    platform-notes.md  # Linux vs macOS installation, behavior, and caveats
  scripts/
    zfs_health_check.sh  # Pool health check script (status, capacity, errors, scrub)
```

## Installation

Copy `zfs.skill` into your Claude Code skills directory:

```bash
cp zfs.skill ~/.claude/skills/
```

Or symlink the unpacked `zfs/` directory during development:

```bash
ln -s "$(pwd)/zfs" ~/.claude/skills/zfs
```

## Health Check Script

The skill includes a pool health check script that reports pool state, capacity warnings, device errors, scrub status, and flags file-backed vdevs:

```bash
bash zfs/scripts/zfs_health_check.sh           # all pools
bash zfs/scripts/zfs_health_check.sh tank       # specific pool
```

## License

This project is provided as-is for use with Claude Code.
