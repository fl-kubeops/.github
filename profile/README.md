<div align="center">

# FL-KubeOps

### Privacy-Preserving Federated Learning for Distributed Healthcare Data

*A Kubernetes-native operator and MLOps pipeline for orchestrating federated learning across distributed hospital nodes — without centralizing patient data.*

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Kubeflow](https://img.shields.io/badge/Kubeflow-00A8E1?style=flat&logo=kubeflow&logoColor=white)
![MLflow](https://img.shields.io/badge/MLflow-0194E2?style=flat&logo=mlflow&logoColor=white)
![K3s](https://img.shields.io/badge/K3s-FFC61C?style=flat&logo=k3s&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)
![Status](https://img.shields.io/badge/Status-Active%20Development-brightgreen?style=flat)
![University](https://img.shields.io/badge/UCP-Spring%202026-navy?style=flat)

</div>

---

## What is FL-KubeOps?

Healthcare institutions hold some of the most valuable data for medical AI — yet patient records cannot leave their facility of origin due to privacy regulations. **FL-KubeOps** solves this at the infrastructure layer: a custom Kubernetes operator that manages the complete federated learning lifecycle declaratively, so hospital IT teams can collaborate on model training without sharing a single patient record.

A single `kubectl apply` of an `FLCluster` manifest deploys the entire federated training infrastructure. The operator handles the rest — client deployment, round scheduling, secure aggregation, failure recovery, and model versioning — autonomously.

---

## Architecture┌─────────────────────────────────────────────────────────┐
│                     MLOps Layer                         │
│   ┌─────────────────────┐   ┌────────────────────────┐  │
│   │  Kubeflow Dashboard │   │        MLflow          │  │
│   │  Pipeline UI &      │   │  Model versioning &    │  │
│   │  round monitoring   │   │  experiment tracking   │  │
│   └─────────────────────┘   └────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
│
▼
┌─────────────────────────────────────────────────────────┐
│                  Orchestration Layer                    │
│  ┌─────────────┐  ┌──────────────────┐  ┌───────────┐   │
│  │    kopf     │  │Kubeflow Pipeline │  │  FastAPI  │   │
│  │  Operator   │  │Training round DAG│  │Aggregator │   │
│  │ FLCluster   │  │Fan-out / fan-in  │  │FedAvg +   │   │
│  │ CRD mgmt    │  │Model broadcast   │  │SMPC       │   │
│  └─────────────┘  └──────────────────┘  └───────────┘   │
└─────────────────────────────────────────────────────────┘
│
(SMPC-encrypted updates only)
▼
┌─────────────────────────────────────────────────────────┐
│            Edge Layer — Hospital Nodes (K3s)            │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐   │
│  │Hospital 1│  │Hospital 2│  │Hospital 3│  │  ...   │   │
│  │ PySyft   │  │ PySyft   │  │ PySyft   │  │ PySyft │   │
│  │ client   │  │ client   │  │ client   │  │ client │   │
│  │📁 local  │  │📁 local │  │📁 local  │  │📁local│   │
│  │  data    │  │  data    │  │  data    │  │  data  │   │
│  └──────────┘  └──────────┘  └──────────┘  └────────┘   │
└─────────────────────────────────────────────────────────┘
---

## Repositories

| Repository | Description | Status |
|---|---|---|
| [operator](https://github.com/fl-kubeops/operator) | kopf-based Kubernetes operator — FLCluster CRD, lifecycle management, failure recovery | 🔨 Active |
| [fl-clients](https://github.com/fl-kubeops/fl-clients) | PySyft FL clients — local training, SMPC secure aggregation | 🔨 Active |
| [pipeline](https://github.com/fl-kubeops/pipeline) | Kubeflow pipeline definitions, FastAPI aggregation server, MLflow integration | 🔨 Active |
| [docs](https://github.com/fl-kubeops/docs) | Architecture, research notes, meeting minutes, proposal | 📖 Public |

---

## Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| Operator | Python + [kopf](https://kopf.readthedocs.io/) | Kubernetes controller for FL lifecycle |
| FL Framework | [PySyft](https://github.com/OpenMined/PySyft) | Local training + SMPC secure aggregation |
| MLOps Pipeline | [Kubeflow Pipelines](https://www.kubeflow.org/) | Reproducible FL round DAG |
| Experiment Tracking | [MLflow](https://mlflow.org/) | Per-round model versioning and metrics |
| Aggregation API | FastAPI | REST interface for model update collection |
| Runtime | [K3s](https://k3s.io/) + Docker | Lightweight Kubernetes on team hardware |

---

## Key Features

- **Declarative FL clusters** — define an `FLCluster` in YAML, the operator manages everything
- **Automatic failure recovery** — detects client pod failures mid-round and recovers within 60 seconds
- **SMPC secure aggregation** — individual client gradients are cryptographically irrecoverable
- **No cloud required** — runs entirely on local K3s hardware
- **Full observability** — every training round versioned in MLflow, visible in Kubeflow UI
- **One-command deployment** — `kubectl apply -f flcluster.yaml` and you're federated

---

## Project Status

| Phase | Deliverable | Target | Status |
|---|---|---|---|
| Phase 1 | Requirements Specification (RS) | April 2026 | ✅ Complete |
| Phase 2 | Software Design Spec (SDS) | June 2026 | 🔄 In progress |
| Phase 3 | Design & Test Spec (DTS) | September 2026 | ⏳ Upcoming |
| Phase 4 | Final System | November 2026 | ⏳ Upcoming |

---

## Team

| Name | Role | GitHub |
|---|---|---|
| Faisal Khalid | Project Lead · Operator Development | [@faisalkhalid](https://github.com/Malik-Faisal-Awan1) |
| Shah Wali | FL Client Development · Aggregation | [@shahwali](https://github.com/syedshahwali07-wq) |
| Wajeeh ur Rehman | Infrastructure · Kubeflow Pipeline | [@wajeeh](https://github.com/wajeeh417) |
| M. Hammad | Testing · Dataset Simulation | [@hammad](https://github.com/l1f23bscs0204) |

**Advisor:** Dr. Saira Gillani
**Institution:** University of Central Punjab, Faculty of IT & CS
**Program:** BSCS Final Year Project — Spring 2026

---

## Research Archive

Key papers and reading notes are maintained in the team's shared research archive (access restricted to team members). Public documentation and architecture diagrams are available in the [docs](https://github.com/fl-kubeops/docs) repository.

---

<div align="center">

*FL-KubeOps is an academic open-source project — MIT licensed, reproducible, and built to be extended.*

**University of Central Punjab · Spring 2026**

</div>
