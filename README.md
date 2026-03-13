# ansible-anysync

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE.md)
[![Platform](https://img.shields.io/badge/platform-Debian%20%7C%20RHEL-lightgrey)](#prerequisites)

Ansible roles for deploying [any-sync](https://tech.anytype.io/) daemons. Supports Debian- and RHEL-based Linux distributions.

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [1. Configure inventory](#1-configure-inventory)
  - [2. Generate network keys](#2-generate-network-keys)
  - [3. Configure roles](#3-configure-roles)
  - [4. Run the playbook](#4-run-the-playbook)
- [Contribution](#contribution)

## Prerequisites

**Infrastructure requirements:**

| Component | Count | Notes |
|-----------|-------|-------|
| [any-sync-node](https://github.com/anyproto/any-sync-node) | 3+ | Document sync nodes |
| [any-sync-coordinator](https://github.com/anyproto/any-sync-coordinator) | 1 | Network coordinator, manages spaces and members |
| [any-sync-filenode](https://github.com/anyproto/any-sync-filenode) | 1 | File storage node |
| [any-sync-consensusnode](https://github.com/anyproto/any-sync-consensusnode) | 1 | Consensus for conflict resolution |
| Redis with [Bloom](https://github.com/RedisBloom/RedisBloom) module | 1 | Filenode index |
| MongoDB ≥4 (latest version is recommended) | 1–3 | Coordinator state; must be in Replica Set mode |
| S3 storage | 1 | Object storage for files — any S3-compatible cloud (AWS S3, GCS, etc.) or self-hosted (e.g. [Minio](https://github.com/minio/minio)) |

**Tooling:**

- Ansible v2.11+
- [any-sync-tools](https://github.com/anyproto/any-sync-tools/blob/main/any-sync-network/README.md) for generating network configuration

## Installation

```bash
git clone https://github.com/anyproto/ansible-anysync.git
```

## Usage

### 1. Configure inventory

Edit `inventory.ini` to match your infrastructure. Example structure:

```ini
[any_sync_node]
any-sync-node1 ansible_host=10.10.0.1
any-sync-node2 ansible_host=10.10.0.2
any-sync-node3 ansible_host=10.10.0.3

[any_sync_filenode]
any-sync-filenode1 ansible_host=10.10.1.1

[any_sync_coordinator]
any-sync-coordinator1 ansible_host=10.10.2.1

[any_sync_consensusnode]
any-sync-consensusnode1 ansible_host=10.10.3.1

[any_sync:children]
any_sync_node
any_sync_filenode
any_sync_coordinator
any_sync_consensusnode

; optional: self-hosted S3 (e.g. Minio)
[s3]
minio ansible_host=10.10.6.1
```

### 2. Generate network keys

Use [any-sync-tools](https://github.com/anyproto/any-sync-tools/blob/main/any-sync-network/README.md) to generate your network configuration, then copy the following values into `group_vars/any_sync.yml`:

- `networkId` — from the generated `client.yml`
- `peerId`, `peerKey`, `signingKey` — for each any-sync-\* node

```yaml
any_sync_config:
  networkId: <your-network-id>

  any-sync-node1:
    peerId: <peerId>
    peerKey: <peerKey>
    signingKey: <signingKey>
    type: tree
    addresses:
      - "{{ hostvars['any-sync-node1'].ansible_host }}:{{ any_sync_node_yamux_port }}"
      - "quic://{{ hostvars['any-sync-node1'].ansible_host }}:{{ any_sync_node_quic_port }}"
  # ... repeat for each node
```

### 3. Configure roles

Tune each daemon's settings by editing `defaults/main.yml` inside the corresponding role directory. This includes MongoDB/Redis URLs, ports, and other options.

The latest compatible component versions are published at the [any-sync versions API](https://puppetdoc.anytype.io/api/v1/prod-any-sync-compatible-versions/).

### 4. Run the playbook

```bash
ansible-playbook any-sync.yml -i inventory.ini
```

After the cluster is up, import the `client.yml` file (generated in step 2) into your Anytype client app.

## Contribution

Thank you for your desire to develop Anytype together!

❤️ This project and everyone involved in it is governed by the [Code of Conduct](https://github.com/anyproto/.github/blob/main/docs/CODE_OF_CONDUCT.md).

🧑‍💻 Check out our [contributing guide](https://github.com/anyproto/.github/blob/main/docs/CONTRIBUTING.md) to learn about asking questions, creating issues, or submitting pull requests.

🫢 For security findings, please email [security@anytype.io](mailto:security@anytype.io) and refer to our [security guide](https://github.com/anyproto/.github/blob/main/docs/SECURITY.md) for more information.

🤝 Follow us on [Github](https://github.com/anyproto) and join the [Contributors Community](https://github.com/orgs/anyproto/discussions).

---
Made by Any — a Swiss association 🇨🇭

Licensed under [MIT](./LICENSE.md).
