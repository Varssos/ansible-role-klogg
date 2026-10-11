# klogg

Ansible role to install [klogg](https://klogg.filimonov.dev/) log viewer on Debian/Ubuntu systems.

## Requirements

- Debian or Ubuntu host
- `become: true` privileges (sudo)

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `klogg_package_name` | `klogg` | Package name to install via apt |
| `klogg_gpg_key_url` | `https://klogg.filimonov.dev/klogg.gpg.key` | URL to the GPG signing key |
| `klogg_gpg_keyring` | `/etc/apt/keyrings/klogg.gpg` | Path for the converted keyring file |
| `klogg_repo_codename` | `jammy` | Ubuntu codename used in the apt repository URL |

## Notes

The klogg repository currently does not support codenames newer than `jammy`. The `klogg_repo_codename` variable is set to `jammy` as a workaround. Change it to `{{ ubuntu_codename }}` once support for newer codenames is added upstream.

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: klogg
```

## Manual setup (without Ansible)

The role defaults to the `jammy` repository, including on newer releases:

```bash
sudo apt install curl gnupg
sudo install -d -m 0755 /etc/apt/keyrings
curl -fL https://klogg.filimonov.dev/klogg.gpg.key -o /tmp/klogg.gpg.key
sudo gpg --dearmor --yes --output /etc/apt/keyrings/klogg.gpg /tmp/klogg.gpg.key
sudo chmod 0644 /etc/apt/keyrings/klogg.gpg
rm /tmp/klogg.gpg.key
echo 'deb [arch=amd64 signed-by=/etc/apt/keyrings/klogg.gpg] https://klogg.filimonov.dev/deb/jammy jammy utils' | sudo tee /etc/apt/sources.list.d/klogg.list
sudo apt update
sudo apt install klogg
```

## License

MIT

## Author

Varssos
