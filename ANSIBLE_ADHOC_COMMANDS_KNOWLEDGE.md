# Ansible Ad-hoc Commands Note

Ad-hoc command format:

```bash
ansible -i inventory.ini <host-or-group> -b -m <module> -a "<arguments>"
```

## Create a System User

```bash
ansible -i inventory.ini man -b -m ansible.builtin.user -a "name=jansi system=true shell=/sbin/nologin comment='system user' create_home=false"
```

Meaning:

- `man` targets the `[man]` group from inventory.
- `-b` runs with sudo.
- `user` module creates or manages Linux users.
- `system=true` creates a system user.
- `shell=/sbin/nologin` prevents normal login.

First run can show:

```text
CHANGED
```

Second run can show:

```text
SUCCESS changed=false
```

That is idempotency: Ansible does not repeat the change when the required state already exists.

## Install nginx

```bash
ansible -i inventory.ini local -b -m ansible.builtin.dnf -a "name=nginx state=present"
```

`state=present` means nginx must be installed.

## Start nginx

```bash
ansible -i inventory.ini local -b -m ansible.builtin.service -a "name=nginx state=started"
```

`state=started` means nginx service must be running.

## Verify

```bash
cat /etc/passwd | grep jansi
sudo systemctl status nginx
sudo ss -nltp
```

Main rule:

```text
Ad-hoc commands are good for quick one-time tasks.
Playbooks are better when you want to save and repeat the automation.
```
