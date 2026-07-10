# f21.yaml - List Filters Practice

This playbook practices common Ansible/Jinja list filters.

## Run

```bash
ansible-playbook -i inventory.ini f21.yaml
```

## Variables Used

```yaml
list1: [1, 2, 3]
list2: [3, 6, 1]
list3: [1, 4, 9, 3, 4, 9]
```

## Filters Practiced

```yaml
{{ list1 | first }}
```

Gets the first value from the list.

```yaml
{{ list1 | last }}
```

Gets the last value from the list.

```yaml
{{ list3 | length }}
```

Counts how many items are in the list.

```yaml
{{ list3 | sort }}
```

Sorts the list values.

```yaml
{{ list3 | unique }}
```

Removes duplicate values from the list.

## Important Note

This playbook uses only `debug`, so it does not change the managed nodes. It is
for practicing how to read and transform list variables.
