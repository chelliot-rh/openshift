# Role: ado.openshift_infrastructure_automation.route_info

This role retrieves all `Route` objects from a specified OpenShift namespace. It can be used for validating route creation, collecting route hostnames, or feeding dependent tasks such as ingress validation or integration checks.

## ✅ Role Requirements

- Requires access to an OpenShift cluster.
- Ensure `kubernetes.core` collection is installed.
- Cluster access credentials should be provided via `KUBECONFIG`, env variables, or the preferred method of using OpenShift bearer credential type in AAP.

## 📦 Role Variables

| Variable     | Description                                  | Required | Default |
|--------------|----------------------------------------------|----------|---------|
| `name_space` | The namespace from which to retrieve Routes  | ✅       | —       |

**Example Usage:**

```yaml
- name: Get all Routes in the Keycloak namespace
  hosts: localhost
  gather_facts: false
  vars:
    name_space: keycloak
  roles:
    - role: ado.openshift_infrastructure_automation.route_info

```

**Structure:**
```
get_routes/
├── defaults/main.yml
├── vars/main.yml
├── tasks/
│   ├── main.yml
│   └── get_routes.yml
├── templates/
├── handlers/main.yml
├── files/
├── tests/
│   ├── inventory
│   └── test.yml
└── README.md