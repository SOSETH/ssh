# Role: ssh

Configuration of OpenSSH for SOSETH server use. This role uses the security
recommendations of [Mozilla](https://infosec.mozilla.org/guidelines/openssh.html).

## Configuration
| Name | Default value | Description |
| ---- | ------------- | ----------- |
| `ssh_user_key_loc` | `{{ config_repo_path }}/ssh-public-keys` | Location of user keys in this repository |
| `ssh_authorized_keys` | `[]` | Which of the above keys should we put into root's authorized_keys? |
| `ssh_allow_x11_fwd` | `false` | Whether to allow X11 forwarding |
| `ssh_auth_methods` | (see defaults/main.yml) | Which authentication methods to allow. By default, only pubkey is enabled, Kerberos is optionally supported |
| `ssh_publickey_sssd` | `False` | Whether to use sssd to get user's keys |
| `ssh_strict_client` | `False` | Whether to enforce strict security settings for the ssh client. Note that this might break backwards compatibility |
| `sshd_acceptable_environment_variables` | (see defaults/main.yml) | Which environment vairables to accept via ssh |
| `ssh_environment_variables_to_send` | (see defaults/main.yml) | Which environment variables to send via SSH |
| `ssh_deploy_root_authorized_keys` | `True` | Whether to deploy root's authorized keys. This is useful on, say, proxmox hosts where the file is automatically shared |
| `sshd_use_dns` | `no` | Whether to enable reverse DNS lookup for clients
| `sshd_principals_for_root` | `[]` | A list of principals to allow to ssh as root. Only useful for Kerberos |
