# Role: ssh

Configuration of OpenSSH for SOSETH server use. This role uses the security
recommendations of [Mozilla](https://infosec.mozilla.org/guidelines/openssh.html).

## Intro
This role will apply standard ssh hardening procedures to both server and client
configuration. Additionally, by default, root logins are restricted to
passwordless login only.
You can then, among other things:
 * roll out keys using `ssh_authorized_keys`
 * enable Kerberos (GSSAPI) by adding it to `ssh_auth_methods`
 * restrict users that are allowed to use sshd using `sshd_restrict_users`
 * roll out keys or kerberos principals for users other than root using `sshd_additional_user_cfg`

## Configuration
| Name | Default value | Description |
| ---- | ------------- | ----------- |
| `ssh_user_key_loc` | `{{ config_repo_path }}/ssh-public-keys` | Location of user keys in this repository |
| `ssh_authorized_keys` | `[]` | Which of the above keys should we put into root's authorized_keys? |
| `ssh_manage_root_keys_exclusively` | `True` | Manage root's authorized_keys exclusively by this role? |
| `ssh_deploy_root_keys_in_etc` | `False` | Deploy root's authorized keys in /etc/ssh? |
| `ssh_deploy_recovery_key` | `False` | Deploy the recovery key? |
| `ssh_allow_x11_fwd` | `False` | Whether to allow X11 forwarding |
| `ssh_auth_methods` | (see defaults/main.yml) | Which authentication methods to allow. By default, only pubkey is enabled, Kerberos is optionally supported |
| `ssh_publickey_sssd` | `False` | Whether to use sssd to get user's keys |
| `ssh_strict_client` | `False` | Whether to enforce strict security settings for the ssh client. Note that this might break backwards compatibility |
| `sshd_acceptable_environment_variables` | (see defaults/main.yml) | Which environment vairables to accept via ssh |
| `ssh_environment_variables_to_send` | (see defaults/main.yml) | Which environment variables to send via SSH |
| `ssh_deploy_root_authorized_keys` | `True` | Whether to deploy root's authorized keys. This is useful on, say, proxmox hosts where the file is automatically shared |
| `sshd_use_dns` | `False` | Whether to enable reverse DNS lookup for clients
| `sshd_principals_for_root` | `[]` | A list of principals to allow to ssh as root. Only useful for Kerberos |
| `sshd_restrict_users` | `False` | Whether to restrict ssh logins to members of a certain group |
| `sshd_manage_restriction_group` | `True` | Whether to actively manage the ssh restriction group (disable if it is in e.g. LDAP) |
| `sshd_restriction_group` | `sshusers` | Name of the ssh restriction group |
| `sshd_allowed_users` | `["root"]` | User to add to ssh restriction group |
| `sshd_permit_root_login` | `prohibit-password` | Whether to permit root login |
| `sshd_additional_user_cfg` | `[]` | Besides root, additionally deploy keys or kerberos principals for these users. Have a look at (defaults/main.yml) for formatting |
| `sshd_replace_moduli` | `False` | Whether to replace `/etc/ssh/moduli` with user-provided values |
| `sshd_moduli_name` | `ssh.moduli` | File in playbook that contains the moduli |
| `sshd_listen_address` | (see defaults/main.yml) | Which addresses to bind to |

## Notes
To avoid interference with software (Proxmox, PMG) also editing the `.ssh/authorized_keys` file
this role deploys keys at `/etc/ssh/authorized_keys/%u` if `ssh_deploy_root_keys_in_etc` is set.

In particular these are following files:
  * `authorized_keys`: normal user public keys with access to the host
  * `recovery`: special recovery key in case the IAM (FreeIPA) fails. To deploy set `ssh_deploy_recovery_key` to True.

The `backup` key configured in the `sshd_config` is deployed by the **backup_client** role.

## License
GPLv3

