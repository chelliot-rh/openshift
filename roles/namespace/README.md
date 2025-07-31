# Role: fbi.openshift_infrastructure_automation.namespace

This role manages Kubernetes namespaces (projects) in an OpenShift cluster. It supports creating or deleting namespaces using the `kubernetes.core.k8s` module.

## ✅ Role Requirements

- Requires access to an OpenShift or Kubernetes cluster.
- Ensure `kubernetes.core` collection is installed.
- Cluster access credentials should be provided via `KUBECONFIG`, env variables, or the preferred method of using Openshift bearer credential type in AAP  

## 📦 Role Variables

| Variable    | Description                           | Required | Default |
|-------------|---------------------------------------|----------|---------|
| `name_space` | The name of the namespace to manage   | ✅       | —       |
| `state`      | Whether the namespace should exist (`present`) or be deleted (`absent`) | ✅ | `present` |

**Example Usage:**

```yaml
- name: Create the Keycloak namespace
  hosts: localhost
  gather_facts: false
  vars:
    name_space: keycloak
    state: present
  roles:
    - role: fbi.openshift_infrastructure_automation.namespace

- name: Remove the Keycloak namespace
  hosts: localhost
  gather_facts: false
  vars:
    name_space: keycloak
    state: absent
  roles:
    - role: fbi.openshift_infrastructure_automation.namespace
```

**Structure:**
```
namespace/
├── defaults/main.yml
├── vars/main.yml
├── tasks/
│   ├──main.yml
│   └──namespace.yml
├── templates/
├── handlers/main.yml
├── files/
├── tests/
│   ├──inventory
│   └──test.yml
└── README.md
```

---
