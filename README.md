# Collection: ado.openshift_infrastructure_automation

This Ansible collection provides modular roles to automate infrastructure setup in Red Hat OpenShift. It includes tasks for namespace creation, Operator installation, Keycloak deployment, route introspection, and workload readiness validation.

The collection is designed to support secure, automated platform bootstrapping in enterprise and air-gapped environments.

---

## ✅ Collection Requirements

- Red Hat OpenShift cluster (tested on 4.x)
- Ansible 2.14+ or AAP 2.4+
- Installed collections:
  - `community.kubernetes`
  - `kubernetes.core`
- Cluster credentials (via `KUBECONFIG`, environment, or OpenShift bearer token in AAP)

---

## 📦 Included Roles

| Role Name                                       | Purpose                                                                 |
|------------------------------------------------|-------------------------------------------------------------------------|
| `namespace`                                     | Creates or deletes OpenShift projects (namespaces)                     |
| `operatorgroup`                                 | Creates an OperatorGroup in the target namespace                       |
| `subscription_operator`                         | Subscribes to an OperatorHub source for a given Operator               |
| `rhbk`                                          | Deploys Red Hat Build of Keycloak, Secrets, DB, and CR configuration   |
| `get_routes`                                    | Retrieves all OpenShift routes from a specified namespace              |
| `wait_for_pods_running`                         | Waits until all pods in a namespace are in `Running` state             |

---

## 🔧 Global Variables Example (from `vault.yaml`)

| Variable                    | Description                                                  | Required |
|----------------------------|--------------------------------------------------------------|----------|
| `keycloak_namespace`       | Namespace where Keycloak will be deployed                    | ✅       |
| `keycloak_admin_user`      | Username for Keycloak admin account                          | ✅       |
| `keycloak_admin_password`  | Password for the admin account                               | ✅       |
| `tls_crt`, `tls_key`       | TLS certificate and key for securing Keycloak                | ✅       |
| `keycloak_hostname`        | Hostname where Keycloak will be exposed                      | ✅       |

---

## 🚀 Example: Deploy Red Hat Build of Keycloak on OpenShift

```yaml
- name: Deploy Red Hat build of keycloak
  hosts: localhost
  gather_facts: false
  collections:
    - ado.openshift_infrastructure_automation

  vars_files:
    - vault.yaml

  vars:
    name_space: keycloak
    state: present

    # OperatorGroup settings
    operatorgroup: rhbk-operator

    # Subscription settings
    operator_name: rhbk-operator
    operator_channel: stable-v26.0
    operator_source: redhat-operators
    operator_source_namespace: openshift-marketplace
    install_rhbk_operator: true

    # RHBK settings
    keycloak_hostname: keycloak.apps.server.lab
    create_rhbk_admin_user: true

  roles:
    - role: ado.openshift_infrastructure_automation.namespace
    - role: ado.openshift_infrastructure_automation.operatorgroup
    - role: ado.openshift_infrastructure_automation.subscription_operator
    - role: ado.openshift_infrastructure_automation.rhbk
    - role: ado.openshift_infrastructure_automation.get_routes
```

**Structure**
```
├── defaults
│   └── main.yml
├── files
│   └── oc
├── galaxy.yml
├── handlers
│   └── main.yml
├── main.yml
├── meta
│   ├── main.yml
│   └── runtime.yml
├── README.md
├── roles
│   ├── install_subscription
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── files
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── meta
│   │   │   └── main.yml
│   │   ├── README.md
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── templates
│   │   ├── tests
│   │   │   ├── inventory
│   │   │   └── test.yml
│   │   └── vars
│   │       └── main.yml
│   ├── namespace
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── meta
│   │   │   └── main.yml
│   │   ├── README.md
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── tests
│   │   │   ├── inventory
│   │   │   └── test.yml
│   │   └── vars
│   │       └── main.yml
│   ├── operatorgroup
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── meta
│   │   │   └── main.yml
│   │   ├── README.md
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── tests
│   │   │   ├── inventory
│   │   │   └── test.yml
│   │   └── vars
│   │       └── main.yml
│   ├── rhbk
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── meta
│   │   │   └── main.yml
│   │   ├── README.md
│   │   ├── tasks
│   │   │   ├── create-rhbk-admin-user.yaml
│   │   │   ├── install-rhbk-operator.yaml
│   │   │   └── main.yml
│   │   ├── tests
│   │   │   ├── inventory
│   │   │   └── test.yml
│   │   └── vars
│   │       └── main.yml
│   ├── status_message
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── files
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── meta
│   │   │   └── main.yml
│   │   ├── README.md
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── templates
│   │   ├── tests
│   │   │   ├── inventory
│   │   │   └── test.yml
│   │   └── vars
│   │       └── main.yml
│   └── wait_for_subscription
│       ├── defaults
│       │   └── main.yml
│       ├── files
│       ├── handlers
│       │   └── main.yml
│       ├── meta
│       │   └── main.yml
│       ├── README.md
│       ├── tasks
│       │   └── main.yml
│       ├── templates
│       ├── tests
│       │   ├── inventory
│       │   └── test.yml
│       └── vars
│           └── main.yml
├── tasks
├── templates
│   └── rhbk-crd.yaml.j2
├── tests
│   └── inventory
├── vars
│   └── main.yml
└── vault.yml
