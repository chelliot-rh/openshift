# Role: ado.openshift_infrastructure_automation.pod_wait

This role waits for all pods in a specified OpenShift namespace to reach a `Running` state. It is useful for ensuring workloads are fully started before continuing with dependent tasks or post-install validations.

## ✅ Role Requirements

- Requires access to an OpenShift or Kubernetes cluster.
- Ensure `kubernetes.core` collection is installed.
- Cluster access credentials should be provided via `KUBECONFIG`, env variables, or the preferred method of using OpenShift bearer credential type in AAP.

## 📦 Role Variables

| Variable            | Description                                                    | Required | Default |
|---------------------|----------------------------------------------------------------|----------|---------|
| `name_space`        | The name of the namespace to check for running pods           | ✅       | —       |
| `pod_wait_retries`  | How many times to check pod statuses before failing           | ❌       | `20`    |
| `pod_wait_delay`    | Seconds to wait between each retry                            | ❌       | `10`    |

**Example Usage:**

```yaml
- name: Wait for all pods in Keycloak namespace to be running
  hosts: localhost
  gather_facts: false
  vars:
    name_space: keycloak
  roles:
    - role: ado.openshift_infrastructure_automation.pod_wait

- name: Wait for all pods with custom retry settings
  hosts: localhost
  gather_facts: false
  vars:
    name_space: kafka
    pod_wait_retries: 30
    pod_wait_delay: 5
  roles:
    - role: ado.openshift_infrastructure_automation.pod_wait
```

**Structure:**
```
wait_for_pods_running/
├── defaults/main.yml
├── vars/main.yml
├── tasks/
│   ├── main.yml
│   └── wait_for_pods.yml
├── templates/
├── handlers/main.yml
├── files/
├── tests/
│   ├── inventory
│   └── test.yml
└── README.md