# Ansible-anysync

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installing](#installing)
- [Usage](#usage)
- [Contribution](#contribution)

## Getting Started

Ansible role for any-sync daemons. Currently, this works on Debian and RHEL based linux systems. Tested platforms are:
* Ubuntu 22.04
* Amazon Linux 2

You can read the documentation on using any-sync [here](https://tech.anytype.io/).

### Prerequisites

* Minimum Ansible v2.11 recommended
* [Redis with Bloom module](https://github.com/RedisBloom/RedisBloom)
* MongoDB (version ≥6 recommended)
* S3 object storage: AWS S3 or self-hosted solution (for example [minio](https://github.com/minio/minio))

### Installing

Clone the current repository with roles

```
git clone https://github.com/anyproto/ansible-anysync.git
```

Use the playbook:

```
ansible-playbook any-sync.yml
```

## Usage

1. To start, fill in the account information and network configuration in the `group_vars/any_sync.yml` file, replacing the example data with your own.
2. Define the structure of your any-sync cluster in the `inventory.yml` file.
3. You can then make changes to the configuration of each any-sync-* daemon by editing its respective `default/main.yml` inside each role.

⚠️ Use [any-sync-tools](https://github.com/anyproto/any-sync-tools) to generate account and network configuration.

## Contribution
Thank you for your desire to develop Anytype together!

❤️ This project and everyone involved in it is governed by the [Code of Conduct](https://github.com/anyproto/.github/blob/main/docs/CODE_OF_CONDUCT.md).

🧑‍💻 Check out our [contributing guide](https://github.com/anyproto/.github/blob/main/docs/CONTRIBUTING.md) to learn about asking questions, creating issues, or submitting pull requests.

🫢 For security findings, please email [security@anytype.io](mailto:security@anytype.io) and refer to our [security guide](https://github.com/anyproto/.github/blob/main/docs/SECURITY.md) for more information.

🤝 Follow us on [Github](https://github.com/anyproto) and join the [Contributors Community](https://github.com/orgs/anyproto/discussions).

---
Made by Any — a Swiss association 🇨🇭

Licensed under [MIT](./LICENSE.md).
