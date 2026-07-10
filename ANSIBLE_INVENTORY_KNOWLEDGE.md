# Ansible Inventory Knowledge Note

This note explains the difference between these two commands:

```bash
ansible-inventory --list
ansible-inventory -i inventory.ini --list
```

## What happened

When this command was run:

```bash
ansible-inventory --list
```

Ansible printed:

```text
[WARNING]: No inventory was parsed, only implicit localhost is available
```

Meaning:

- Ansible did not automatically read `inventory.ini` from the current folder.
- No configured inventory file was found.
- Ansible only used its built-in implicit `localhost`.
- Remote groups like `man` were not available.

This is why the output only showed:

```text
all
ungrouped
```

## Why this command worked

When this command was run:

```bash
ansible-inventory -i inventory.ini --list
```

Ansible was told exactly which inventory file to use.

The inventory file contains:

```ini
[man]
3.85.237.223
54.85.107.119

[local]
localhost ansible_connection=local
```

So Ansible correctly found:

- group `man`
- group `local`
- host `3.85.237.223`
- host `54.85.107.119`
- host `localhost` with local connection

## Important concept

The inventory file tells Ansible which machines exist.

The playbook tells Ansible what to do on those machines.

Example from `f1.yaml`:

```yaml
hosts: man
```

This means:

- Look inside the inventory.
- Find the group named `man`.
- Run the playbook tasks on the hosts inside that group.

If Ansible cannot read the inventory, then `hosts: man` has nothing to match.

## Correct command for this project

Use this command when checking inventory:

```bash
ansible-inventory -i inventory.ini --list
```

Use this command when running the playbook:

```bash
ansible-playbook -i inventory.ini f1.yaml
```

## Optional default setup

If you want Ansible to use `inventory.ini` automatically, create an `ansible.cfg`
file in the same directory:

```ini
[defaults]
inventory = inventory.ini
```

After that, this command can work without `-i inventory.ini`:

```bash
ansible-inventory --list
```

## Quick troubleshooting rule

If you see:

```text
No inventory was parsed, only implicit localhost is available
```

First check:

```bash
pwd
ls
ansible-inventory -i inventory.ini --list
```

Do not start debugging the playbook first. Confirm that Ansible can read the
inventory before checking tasks.
