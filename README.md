# 🏗️ Infrastructure as Code Manager - Multi-Layer IaC Orchestrator

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg?logo=python&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-Config-623CE4?logo=terraform)
![AWS](https://img.shields.io/badge/CloudFormation-JSON-FF9900?logo=amazonaws&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-YAML-326CE5?logo=kubernetes)
![CI/CD](https://img.shields.io/badge/IaC-Automation-2088FF?logo=githubactions)
![Logging](https://img.shields.io/badge/Logging-Built_in-4CAF50?logo=logstash)
![Dependencies](https://img.shields.io/badge/Dependencies-None-green?logo=python)

**Infrastructure as Code Manager (IaC Manager)** is a **standalone Python 3** utility that generates, tracks, and audits deployments across **Terraform**, **CloudFormation**, and **Kubernetes** — all without external dependencies.

It’s built for DevOps and platform engineers who need a lightweight way to **model**, **simulate**, and **manage** IaC workflows locally or in demo environments.

-----------------------------

## 🛠 Tech & Languages

| Layer | Tech / Format | Purpose |
|------|------|------|
| Language | **Python 3.10+** | Pure standard library, no dependencies |
| IaC Templates | **Terraform / CloudFormation / Kubernetes** | Generate multi-layer infra templates |
| Storage | **JSON Files** | Store deployments, resources, and templates |
| Logging | **Python Logging** | Record operations and audit events |
| Reports | **JSON Drift Report** | Summarize deployments and drift status |

--------

## 🌐 Architecture

<p align="center">
  <img src="assets/architecture.png" alt="IaC Manager Architecture" width="650" />
</p>


Flow:
1. Generates **Terraform**, **CloudFormation**, or **Kubernetes** templates  
2. Simulates deployment and tracks created resources  
3. Persists metadata in local JSON storage  
4. Generates **drift reports** and audit summaries  
5. Exports deployment data for recordkeeping or documentation  

------

## 📦 Repository Structure

iac-manager/
├─ infrastructure/
│ ├─ deployments.json
│ ├─ resources.json
│ ├─ templates.json
│ └─ deployment_export.json
├─ iac_manager.py
└─ README.md

yaml
Copy code

---

## ▶️ Run the Demo

```bash
python iac_manager.py
What it does:

Initializes a local workspace (./infrastructure)

Creates:

A Terraform deployment (production)

A CloudFormation deployment (staging)

A Kubernetes deployment (production)

Lists all deployments and their resources

Generates a drift report

Exports deployment details to deployment_export.json

🧪 Example Output
Drift Report Example
json
Copy code
{
  "total_deployments": 3,
  "by_status": {"applied": 3},
  "by_environment": {"production": 2, "staging": 1},
  "by_template_type": {
    "terraform": 1,
    "cloudformation": 1,
    "kubernetes": 1
  },
  "total_resources": 6,
  "resources_by_type": {
    "aws_instance": 1,
    "aws_security_group": 1,
    "CloudFormation::Stack": 1,
    "kubernetes::Deployment": 1,
    "kubernetes::Service": 1
  }
}
Deployment Export Example (deployment_export.json)
json
Copy code
{
  "deployment": {
    "id": "tf-production-20251013095501",
    "environment": "production",
    "template_name": "terraform",
    "status": "applied",
    "timestamp": "2025-10-13T09:55:01Z",
    "version": "1.0.0",
    "checksum": "8b17c5a..."
  },
  "resources": [
    {
      "id": "aws_instance-1234",
      "type": "aws_instance",
      "name": "aws_instance-1",
      "region": "us-east-1",
      "status": "created"
    }
  ]
}
📄 Supported IaC Generators
Type	Function	Output Example
Terraform	generate_terraform_template()	.tf with provider, EC2, SG, outputs
CloudFormation	generate_cloudformation_template()	JSON with Parameters, Resources, Outputs
Kubernetes	generate_kubernetes_yaml()	Multi-doc YAML (Deployment + Service)

⚙️ Key Classes
Class	Purpose
IaCManager	Core orchestrator for IaC operations
IaCTemplateGenerator	Generates Terraform / CloudFormation / K8s templates
DeploymentRecord	Represents one deployment
ResourceInfo	Represents one resource instance
TemplateFile	Stores and version-tracks templates
DeploymentStatus	Enum for deployment states

🧮 Example Usage (Programmatic)
python
Copy code
from iac_manager import IaCManager

iac = IaCManager("./infra")

config = {
    "environment": "staging",
    "project": "api",
    "region": "us-west-2",
    "instance_type": "t3.micro",
}

deployment_id = iac.create_terraform_deployment("staging", config)
print(f"Created deployment: {deployment_id}")

report = iac.generate_drift_report()
print(report)
📊 Use Cases
Simulate multi-environment IaC workflows locally

Run demos or training sessions without Terraform CLI

Generate ready-to-use IaC templates for documentation

Track infrastructure drift and environment consistency

Export deployment state for audit and compliance

Design Philosophy
Lightweight: 100% Python stdlib

Transparent: All operations logged

Portable: Works offline on any OS

Extendable: Ready for GitOps / CI/CD integration

🐳 Docker (Optional)
Build:

bash
Copy code
docker build -t iac-manager:latest .
Run:

bash
Copy code
docker run --rm -v $(pwd):/work iac-manager:latest python iac_manager.py
👤 Author
Siddharth Raut — DevOps / Platform Engineer
