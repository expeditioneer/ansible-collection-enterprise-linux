# Foreman preparation

The intention of this role is to replace the pre-set password and SSH key which was initially set by the
[Foreman Katello Kickstart](https://github.com/expeditioneer/Foreman-Katello-Kickstart) installation.

To change the root password the variable provide the new password with `root_login_password`.

To change the authorized_key for the _root_ user the variable `root_authorized_key` can be provided.
After the new authorized key is added the pre-set SSH-Key from the kickstart installation is removed 
with the following task.

## Usage

Create an ansible play with the following content

```yaml
---

- hosts: foreman
  vars:
    ansible_ssh_private_key_file: ./foreman_key
    root_authorized_key: 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJxElcUz4fphHg42hQxIsNK4P/3IaVZs4zMjjA+7UQ+g _MY_KEY_COMMENT_'
    root_login_password: _MY_NEW_PASSWORD_
  roles:
    - role: expeditioneer.enterprise_linux.foreman_preparation
```

and replace `root_authorized_key` and `root_login_password` with your values. 

`ansible_ssh_private_key_file` should reference a file, here _foreman_key_ in the same directory as the play, with the private
SSH key for the initially set authorized key by the Foreman Katello Kickstart installation.

File _foreman_key_ content:
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACD8wtbIqjb9IeGLzqyU+xI8WdY3SxdN4RQv8A2JQDuX2QAAAJhd00UfXdNF
HwAAAAtzc2gtZWQyNTUxOQAAACD8wtbIqjb9IeGLzqyU+xI8WdY3SxdN4RQv8A2JQDuX2Q
AAAEBL/pYo4CGMibFrzV2LU5Ka2B2yRtx57uWjSymJ4dh8bvzC1siqNv0h4YvOrJT7EjxZ
1jdLF03hFC/wDYlAO5fZAAAAEmRlbm5pc0B3b3Jrc3RhdGlvbgECAw==
-----END OPENSSH PRIVATE KEY-----
```

This play should only be needed to be executed once.
