
# Role: ado.openshift_infrastructure_automation.grafana_service_account

This role provisions a dedicated ServiceAccount in OpenShift for Grafana to securely access Prometheus metrics. It creates the ServiceAccount, binds it to a ClusterRole, manually creates a token Secret, and extracts the bearer token for use in Grafana. When `state: absent`, it cleanly deletes all associated resources.

---

## ✅ Role Requirements

- Red Hat OpenShift 4.x cluster with cluster-admin access
- The `kubernetes.core` collection must be installed
- The Prometheus ClusterRole `cluster-monitoring-view` must exist
- Namespace (e.g., `openshift-monitoring`) must already be created

---

## 📦 Role Variables

| Variable                    | Description                                                                 | Required | Default |
|-----------------------------|-----------------------------------------------------------------------------|----------|---------|
| `service_account_name`      | Name of the ServiceAccount to create                                        | ✅       | —       |
| `service_account_namespace` | Namespace where the ServiceAccount is created                               | ✅       | —       |
| `binding_name`              | Name of the ClusterRoleBinding that links the SA to the ClusterRole         | ✅       | —       |
| `role_ref`                  | Name of the ClusterRole (e.g. `cluster-monitoring-view`)                    | ✅       | —       |
| `state`                     | Whether to `present` (create) or `absent` (delete) the resources            | ❌       | `present` |
| `prometheus_bearer_token`   | (Output) Token used by Grafana to authenticate with Prometheus              | Output   | —       |

---

## 📘 Example Usage

### Create ServiceAccount

```yaml
- name: Create Grafana Prometheus ServiceAccount
  hosts: localhost
  gather_facts: false
  vars:
    service_account_name: grafana-prometheus-sa
    service_account_namespace: openshift-monitoring
    binding_name: grafana-prometheus-sa-binding
    role_ref: cluster-monitoring-view
    state: present
  roles:
    - role: ado.openshift_infrastructure_automation.grafana_service_account

- name: Delete Grafana Prometheus ServiceAccount
  hosts: localhost
  gather_facts: false
  vars:
    service_account_name: grafana-prometheus-sa
    service_account_namespace: openshift-monitoring
    binding_name: grafana-prometheus-sa-binding
    role_ref: cluster-monitoring-view
    state: absent
  roles:
    - role: ado.openshift_infrastructure_automation.grafana_service_account
```

create_service_account/
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