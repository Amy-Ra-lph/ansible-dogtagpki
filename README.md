# Ansible Roles for Dogtag PKI

Ansible roles for provisioning a Dogtag Certificate Authority and deploying the Dogtag WebUI container.

## Roles

- **dogtag_ca** — Installs and configures 389 Directory Server + Dogtag CA on a RHEL/Fedora host
- **dogtag_webui** — Builds and runs the Dogtag WebUI container (from [dogtag-webui](https://github.com/Amy-Ra-lph/dogtag-webui))

## Playbooks

| Playbook | Description |
|----------|-------------|
| `site.yml` | Full deployment (CA + WebUI) |
| `provision-ca.yml` | CA provisioning only |
| `deploy-webui.yml` | WebUI container deployment only |

## Prerequisites

- RHEL 9 / Fedora target host
- Ansible 2.14+
- Collections: `containers.podman`, `ansible.posix` (see `requirements.yml`)

```bash
ansible-galaxy collection install -r requirements.yml
```

## Setup

1. Copy the vault example and encrypt it:

```bash
cp group_vars/pki_servers/vault.yml.example group_vars/pki_servers/vault.yml
ansible-vault encrypt group_vars/pki_servers/vault.yml
```

2. Edit `inventory/hosts.yml` with your target host.

3. Run the playbook:

```bash
ansible-playbook site.yml --ask-vault-pass
```

## Security Notes

- All passwords are stored in Ansible Vault (never in plaintext)
- Passwords are passed to commands via temporary files (not shell arguments)
- Sensitive tasks use `no_log: true`
- See [SECURITY.md](https://github.com/Amy-Ra-lph/dogtag-webui/blob/main/SECURITY.md) in the WebUI repo for the full audit
