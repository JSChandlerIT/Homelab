# Lightning Node Recovery

## Purpose

This document describes how to recover the Lightning Network node after hardware failure, operating system corruption, or accidental deletion.

---

# Required Recovery Items

The following items must be available before recovery.

- 24-word wallet seed
- Static Channel Backup (`channel.backup`)
- Docker Compose configuration
- Bitcoin blockchain data (optional but recommended)

---

# Current Node Information

Host:
Dell Precision T5820

Operating System:
Ubuntu Server LTS

Lightning Implementation:
LND (Docker)

Management Interface:
ThunderHub

Bitcoin Backend:
Bitcoin Core

---

# Backup Locations

## Wallet Seed

Stored offline.

Never stored digitally.

---

## Static Channel Backup

Current location:

```
/opt/stacks/lightning/backups/channel.backup
```

Additional copy:

- USB Drive
- (Add additional locations here)

---

# Recovery Procedure

## 1. Install Ubuntu Server

Install the operating system.

Update packages.

Install Docker and Docker Compose.

---

## 2. Restore Docker Configuration

Restore the Lightning stack.

Example:

```
docker compose up -d
```

---

## 3. Restore Wallet

Start LND.

Choose:

Restore Existing Wallet

Enter the 24-word seed.

---

## 4. Restore Static Channel Backup

Provide:

```
channel.backup
```

LND will recover any channels.

Channels may be force-closed.

This is expected.

---

## 5. Wait for Bitcoin Sync

Verify:

```
lncli getinfo
```

Confirm:

```
synced_to_chain: true
synced_to_graph: true
```

---

## 6. Verify Services

Verify:

- Bitcoin Core
- LND
- ThunderHub
- Grafana
- Homepage

---

# Post-Recovery Checklist

- Wallet balance verified
- Channels restored
- Force closes complete
- New Static Channel Backup created
- Backup copied to external storage

---

# Notes

The Static Channel Backup changes whenever channels are opened or closed.

After any channel change:

1. Copy the new backup.

```
docker cp lnd:/root/.lnd/data/chain/bitcoin/mainnet/channel.backup \
/opt/stacks/lightning/backups/channel.backup
```

2. Copy it to external storage.

Never rely on a backup stored only on the server.
