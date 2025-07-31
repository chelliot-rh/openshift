---

## `operatorgroup`

**Example Usage:**

```yaml
- name: Execute the operatorgroup role
  hosts: localhost
  gather_facts: false
  roles:
    - role: fbi.openshift_infrastructure_automation.operatorgroup
      vars:
        example_variable: example_value
```

> Replace `example_variable` and `example_value` with the appropriate parameters defined in `defaults/main.yml` for the role.


**Description**: Creates OperatorGroups for multi-tenant operator installs

**Structure:**
```
operatorgroup/
├── defaults/main.yml
├── vars/main.yml
├── tasks/main.yml
├── templates/
├── handlers/main.yml
├── files/
├── tests/
│   ├── inventory
│   └── test.yml
└── README.md
```